---
title: "WebSocket连接管理与Redis集成实现"
date: 2025-08-21T16:04:23+08:00
categories: ["Python"]
tags: ["Python", "Web"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

# WebSocket连接管理与Redis集成实现

在构建分布式聊天系统时，我们面临着一个核心挑战：如何在多个服务器实例之间有效管理WebSocket连接，并确保消息能够准确传递到正确的机器人客户端。本文详细介绍了我们的解决方案，包括Redis集成、跨实例消息转发、心跳机制等关键技术实现。

## 系统架构概述

我们的系统采用了混合存储架构：本地内存存储活跃的WebSocket连接，Redis存储全局连接状态信息。这种设计既保证了本地访问的高性能，又实现了跨实例的状态共享。

## Redis配置与连接管理

首先，我们在配置文件中定义了Redis相关配置：

```python
# config.py
class RedisConfig:
    REDIS_HOST = os.getenv('REDIS_HOST', 'localhost')
    REDIS_PORT = int(os.getenv('REDIS_PORT', 6379))
    REDIS_DB = int(os.getenv('REDIS_DB', 0))
    REDIS_PASSWORD = os.getenv('REDIS_PASSWORD', None)
    
    # WebSocket连接相关配置
    WEBSOCKET_CONNECTION_PREFIX = "ws_conn:"
    HEARTBEAT_INTERVAL = 30  # 心跳间隔（秒）
    CONNECTION_TIMEOUT = 90  # 连接超时时间（秒）
```

为了确保每个服务器实例都有唯一标识，我们实现了基于主机名、端口和进程ID的实例ID生成机制：

```python
class InstanceConfig:
    @staticmethod
    def get_instance_id():
        # 优先使用环境变量
        instance_id = os.getenv('SERVER_INSTANCE_ID')
        if instance_id:
            return instance_id
        
        # 基于主机名、端口和进程ID生成唯一标识
        hostname = socket.gethostname()
        port = os.getenv('PORT', '8000')
        pid = os.getpid()
        
        # 生成哈希值作为实例ID
        hash_input = f"{hostname}:{port}:{pid}"
        return hashlib.md5(hash_input.encode()).hexdigest()[:16]
```

这个设计解决了同一台机器上运行多个服务实例时的身份识别问题。

## Redis管理器实现

我们创建了一个专门的Redis管理器来处理连接状态的存储和查询：

```python
# app/core/redis_manager.py
import redis
import json
import logging
from datetime import datetime
from typing import Optional, Dict, List
from config import RedisConfig, InstanceConfig

class RedisManager:
    def __init__(self):
        self.redis_client = redis.Redis(
            host=RedisConfig.REDIS_HOST,
            port=RedisConfig.REDIS_PORT,
            db=RedisConfig.REDIS_DB,
            password=RedisConfig.REDIS_PASSWORD,
            decode_responses=True
        )
        self.instance_id = InstanceConfig.get_instance_id()
        
    def store_connection(self, robot_id: str, additional_data: Dict = None):
        """存储机器人连接信息到Redis"""
        connection_data = {
            'instance_id': self.instance_id,
            'robot_id': robot_id,
            'connected_at': datetime.now().isoformat(),
            'last_heartbeat': datetime.now().isoformat(),
            **(additional_data or {})
        }
        
        key = f"{RedisConfig.WEBSOCKET_CONNECTION_PREFIX}{robot_id}"
        self.redis_client.hset(key, mapping=connection_data)
        logging.info(f"存储机器人 {robot_id} 连接信息到Redis，实例: {self.instance_id}")
        
    def get_connection_info(self, robot_id: str) -> Optional[Dict]:
        """获取机器人连接信息"""
        key = f"{RedisConfig.WEBSOCKET_CONNECTION_PREFIX}{robot_id}"
        data = self.redis_client.hgetall(key)
        return data if data else None
        
    def remove_connection(self, robot_id: str):
        """从Redis中移除连接信息"""
        key = f"{RedisConfig.WEBSOCKET_CONNECTION_PREFIX}{robot_id}"
        result = self.redis_client.delete(key)
        if result:
            logging.info(f"从Redis中移除机器人 {robot_id} 的连接信息")
        return result
        
    def update_heartbeat(self, robot_id: str):
        """更新心跳时间戳"""
        key = f"{RedisConfig.WEBSOCKET_CONNECTION_PREFIX}{robot_id}"
        if self.redis_client.exists(key):
            self.redis_client.hset(key, 'last_heartbeat', datetime.now().isoformat())
            
    def get_all_connections(self) -> List[Dict]:
        """获取所有连接信息"""
        pattern = f"{RedisConfig.WEBSOCKET_CONNECTION_PREFIX}*"
        keys = self.redis_client.keys(pattern)
        connections = []
        
        for key in keys:
            data = self.redis_client.hgetall(key)
            if data:
                connections.append(data)
                
        return connections

# 创建全局实例
redis_manager = RedisManager()
```

这个Redis管理器提供了完整的连接生命周期管理功能，包括存储、查询、更新和清理。

## WebSocket连接管理器

核心的WebSocket连接管理器集成了本地内存和Redis存储：

```python
# app/external_service/websocket_channel.py
class ConnectionManager:
    def __init__(self):
        # 本地内存存储活跃连接
        self.active_connections: Dict[str, WebSocket] = {}
        self.instance_id = InstanceConfig.get_instance_id()
        
    async def connect(self, websocket: WebSocket, robot_id: str):
        """建立WebSocket连接"""
        await websocket.accept()
        self.active_connections[robot_id] = websocket
        
        # 同时存储到Redis
        redis_manager.store_connection(robot_id, {
            'status': 'connected',
            'websocket_state': 'active'
        })
        
        logging.info(f"机器人 {robot_id} 已连接到实例 {self.instance_id}")
        
    def disconnect(self, robot_id: str):
        """断开连接并清理"""
        if robot_id in self.active_connections:
            del self.active_connections[robot_id]
            
        # 从Redis中移除
        redis_manager.remove_connection(robot_id)
        logging.info(f"机器人 {robot_id} 已断开连接并清理")
```

## 跨实例消息转发机制

最复杂的部分是实现跨实例的消息转发。当目标机器人不在当前实例时，我们需要找到正确的实例并转发消息：

```python
async def send_personal_message(self, message: str, robot_id: str):
    """发送个人消息，支持跨实例转发"""
    # 首先检查本地连接
    if robot_id in self.active_connections:
        websocket = self.active_connections[robot_id]
        
        # 验证连接是否真实有效
        if await self.is_websocket_alive(robot_id):
            try:
                await websocket.send_text(message)
                redis_manager.update_heartbeat(robot_id)
                return True
            except Exception as e:
                logging.warning(f"发送消息到 {robot_id} 失败: {e}")
                # 清理无效连接
                self.disconnect(robot_id)
        else:
            # 本地连接失效，清理并重新查询Redis
            logging.warning(f"机器人 {robot_id} 本地连接失效，清理Redis记录")
            redis_manager.remove_connection(robot_id)
            
            # 重新查询Redis，寻找其他实例的连接
            connection_info = redis_manager.get_connection_info(robot_id)
            if connection_info and connection_info.get('instance_id') != self.instance_id:
                # 尝试跨实例转发
                return await self.forward_message_to_instance(
                    message, robot_id, connection_info.get('instance_id')
                )
            return False
    
    # 本地没有连接，查询Redis
    connection_info = redis_manager.get_connection_info(robot_id)
    if not connection_info:
        return False
        
    target_instance = connection_info.get('instance_id')
    if target_instance == self.instance_id:
        # 应该在本地但找不到，可能是状态不一致
        logging.warning(f"Redis显示机器人 {robot_id} 在当前实例，但本地未找到连接")
        redis_manager.remove_connection(robot_id)
        return False
    else:
        # 跨实例转发
        return await self.forward_message_to_instance(message, robot_id, target_instance)
```

## WebSocket连接状态检测

由于Starlette的WebSocket类没有ping方法，我们实现了适配的连接状态检测：

```python
async def is_websocket_alive(self, robot_id: str) -> bool:
    """检查WebSocket连接是否真实存活"""
    if robot_id not in self.active_connections:
        return False
    
    websocket = self.active_connections[robot_id]
    try:
        # 检查WebSocket状态
        if hasattr(websocket, 'client_state') and websocket.client_state.name == 'DISCONNECTED':
            logging.warning(f"WebSocket连接 {robot_id} 已断开")
            return False
        
        # 检查应用状态
        if hasattr(websocket, 'application_state'):
            if websocket.application_state.name == 'DISCONNECTED':
                logging.warning(f"WebSocket连接 {robot_id} 应用状态已断开")
                return False
        
        return True
    except Exception as e:
        logging.warning(f"WebSocket连接 {robot_id} 检测失败: {e}")
        return False
```

## 心跳机制实现

我们实现了基于时间戳的心跳机制来维护连接状态：

```python
async def validate_and_cleanup_connections(self):
    """验证并清理无效连接"""
    current_time = datetime.now()
    expired_robots = []
    
    for robot_id in list(self.active_connections.keys()):
        connection_info = redis_manager.get_connection_info(robot_id)
        if not connection_info:
            expired_robots.append(robot_id)
            continue
            
        # 检查心跳超时
        last_heartbeat = datetime.fromisoformat(connection_info.get('last_heartbeat', ''))
        if (current_time - last_heartbeat).total_seconds() > RedisConfig.CONNECTION_TIMEOUT:
            expired_robots.append(robot_id)
            
    # 清理过期连接
    for robot_id in expired_robots:
        self.disconnect(robot_id)
        logging.info(f"清理过期连接: {robot_id}")
```

## 消息发送接口

最终，我们提供了统一的消息发送接口：

```python
async def send_message_to_robot(robot_id: str, message_data: dict, db: Session):
    """发送消息给指定机器人"""
    try:
        message_json = json.dumps(message_data, ensure_ascii=False)
        success = await manager.send_personal_message(message_json, robot_id)
        
        if not success:
            logging.warning(f"未找到机器人 {robot_id} 的连接或发送失败")
            
            # 更新机器人状态为离线
            robot = db.query(Robot).filter(Robot.robot_id == robot_id).first()
            if robot:
                robot.status = 0  # 离线状态
                db.commit()
                
        return success
    except Exception as e:
        logging.error(f"发送消息到机器人 {robot_id} 时发生错误: {e}")
        return False
```

## 技术特点总结

这套实现方案具有以下特点：

**高可用性**：通过Redis实现跨实例状态共享，单个实例故障不影响整体服务。

**性能优化**：本地内存存储活跃连接，避免频繁的Redis查询。

**状态一致性**：通过心跳机制和连接验证，确保连接状态的准确性。

**故障恢复**：当检测到连接异常时，自动清理无效状态并尝试重新路由。

**扩展性**：支持水平扩展，新增实例可以无缝接入现有集群。

这个方案在生产环境中运行稳定，有效解决了分布式WebSocket连接管理的各种挑战。通过合理的架构设计和细致的异常处理，我们构建了一个可靠的实时通信基础设施。