---
title: "单例模式以及事件管理器在 Cocos 中的应用"
date: 2025-03-13T07:30:38Z
lastmod: 2025-03-13T07:30:38Z
categories: ["Cocos"]
tags: ["Cocos"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
disableHLJS: true
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

## 单例模式

单例模式是设计模式中最常用的模式之一，它确保一个类只有一个实例，并提供一个全局访问点。在游戏开发中，单例模式尤其有用，因为它能够有效地管理全局资源和状态。

### 单例模式的核心特点

1. **全局唯一实例** - 确保类在整个应用程序中只存在一个实例
2. **全局访问点** - 提供统一的方式获取该实例
3. **延迟初始化** - 第一次使用时才创建实例（懒加载）
4. **避免资源浪费** - 防止创建多个相同功能的对象

### Cocos 中的单例模式实现

```typescript
export default class Singleton {
  private static _instance: any = null

  static GetInstance<T>(): T {
    if (this._instance === null) {
      this._instance = new this()
    }
    return this._instance
  }
}
```

> 这是一个基础的单例模式实现，使用静态属性和方法确保只创建一个实例。通过泛型支持，使得不同类继承时能够保持类型安全。

## 事件管理器

事件管理器是游戏开发中不可或缺的组件，它通过发布-订阅模式（也称为观察者模式）实现游戏各模块间的松耦合通信。

### 事件管理器的作用

1. **解耦游戏组件** - 使发送者和接收者无需直接引用对方
2. **简化通信** - 统一的事件处理机制
3. **跨场景交互** - 即使在不同场景也能轻松通信
4. **模块化设计** - 有助于分离关注点，提高代码可维护性

### 基于单例模式的事件管理器实现

```typescript
import Singleton from '../Base/Singleton';

export type EventCallback = (...args: any[]) => void;

export default class EventManager extends Singleton {
  private eventMap: Map<string, EventCallback[]> = new Map();
  
  static get Instance() {
    return EventManager.GetInstance<EventManager>();
  }
  
  // 添加事件监听
  on(eventName: string, callback: EventCallback, context?: any) {
    if (!this.eventMap.has(eventName)) {
      this.eventMap.set(eventName, []);
    }
    
    const callbacks = this.eventMap.get(eventName);
    const boundCallback = context ? callback.bind(context) : callback;
    callbacks.push(boundCallback);
  }
  
  // 移除事件监听
  off(eventName: string, callback?: EventCallback, context?: any) {
    if (!this.eventMap.has(eventName)) {
      return;
    }
    
    if (!callback) {
      // 如果没有指定回调函数，移除该事件的所有监听器
      this.eventMap.delete(eventName);
      return;
    }
    
    const callbacks = this.eventMap.get(eventName);
    if (!callbacks || callbacks.length === 0) {
      return;
    }
    
    if (context) {
      // 如果指定了上下文，需要考虑上下文比较
      const boundCallback = callback.bind(context);
      const index = callbacks.findIndex(cb => cb === boundCallback);
      if (index !== -1) {
        callbacks.splice(index, 1);
      }
    } else {
      // 移除特定的回调函数
      const index = callbacks.indexOf(callback);
      if (index !== -1) {
        callbacks.splice(index, 1);
      }
    }
    
    if (callbacks.length === 0) {
      this.eventMap.delete(eventName);
    }
  }
  
  // 触发事件
  emit(eventName: string, ...args: any[]) {
    if (!this.eventMap.has(eventName)) {
      return;
    }
    
    const callbacks = this.eventMap.get(eventName).slice();
    for (const callback of callbacks) {
      callback(...args);
    }
  }
  
  // 清空所有事件
  clear() {
    this.eventMap.clear();
  }
}
```

### 在游戏开发中的应用

#### 1. 玩家输入与角色控制

```typescript
// 控制器代码
function handleKeyPress(key: string) {
  switch (key) {
    case 'ArrowUp':
      EventManager.Instance.emit(EVENT_ENUM.PLAYER_CTRL, CONTROLLER_ENUM.TOP);
      break;
    case 'ArrowDown':
      EventManager.Instance.emit(EVENT_ENUM.PLAYER_CTRL, CONTROLLER_ENUM.BOTTOM);
      break;
    // 其他按键...
  }
}

// 角色管理器代码
class PlayerManager {
  init() {
    // 监听控制事件
    EventManager.Instance.on(EVENT_ENUM.PLAYER_CTRL, this.move, this);
  }
  
  move(direction: CONTROLLER_ENUM) {
    // 处理角色移动...
  }
}
```

## 单例与事件管理器的优势

1. **一致的接口** - 所有组件使用相同的方式进行通信
2. **避免全局变量滥用** - 提供可控的全局访问点
3. **提高性能** - 避免重复创建多个相同功能的实例
4. **简化对象关系** - 不再需要复杂的对象引用关系
5. **灵活的通信模式** - 一对多、多对一、多对多的通信都能轻松实现

## 最佳实践与注意事项

1. **避免过度使用单例** - 不是所有对象都需要是单例
2. **防止内存泄漏** - 记得在对象销毁前取消事件监听
3. **事件命名规范** - 使用枚举或常量定义事件名称，避免字符串拼写错误
4. **合理划分事件作用域** - 避免过多全局事件，适当使用局部事件管理器
5. **明确的事件数据结构** - 定义清晰的事件参数结构，便于维护和扩展

> 通过结合单例模式和事件管理器，游戏代码可以实现高内聚、低耦合的设计，大大提高开发效率和代码可维护性。在复杂的游戏项目中，这种模式尤为重要，能让代码结构更加清晰，扩展性更强。