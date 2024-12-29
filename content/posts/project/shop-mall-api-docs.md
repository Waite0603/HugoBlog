---
title: "电商后台管理系统后端api接口文档"
date: 2023-04-05T16:04:23+08:00
categories: ["Project"]
tags: ["Project"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---


## 一、API V2 接口说明

- 接口基准地址：`http://www.tangxiaoyang.vip:8888/api/v2/`

- 服务端已开启 CORS 跨域支持

- API V2 认证统一使用 Token 认证

- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌

- 使用 HTTP Status Code 标识状态

- 数据返回格式统一使用 JSON

### 1\. 支持的请求方法

- GET（SELECT）：从服务器取出资源（一项或多项）。

- POST（CREATE）：在服务器新建一个资源。

- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

- DELETE（DELETE）：从服务器删除资源。

- HEAD：获取资源的元数据。

- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

### 2\. 通用返回状态说明

| 状态码 | 含义 | 说明 |
| --- | --- | --- |
| 200 | OK | 请求成功 |
| 201 | CREATED | 创建成功 |
| 204 | DELETED | 删除成功 |
| 400 | BAD REQUEST | 请求的地址不存在或者包含不支持的参数 |
| 401 | UNAUTHORIZED | 未授权 |
| 403 | FORBIDDEN | 被禁止访问 |
| 404 | NOT FOUND | 请求的资源不存在 |
| 422 | Unprocesable entity | \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误 |
| 500 | INTERNAL SERVER ERROR | 内部错误 |

## 二、登录

### 1\. 登录验证接口

- 请求路径：login

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名 | 不能为空 |
| password | 密码 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |
| token | 令牌 | 基于 jwt 的令牌 |

- 响应数据

```
{
    "data": {
        "id": 500,
        "rid": 0,
        "username": "admin",
        "mobile": "123",
        "email": "123@qq.com",
        "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzMDY5OX0.eGrsrvwHm-tPsO9r_pxHIQ5i5L1kX9RX444uwnRGaIM"
    },
    "meta": {
        "msg": "登录成功",
        "status": 200
    }
}
```

## 三、用户管理

### 1\. 用户数据列表

- 请求路径：users

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总记录数 |  |
| pagenum | 当前页码 |  |
| users | 用户数据集合 |  |

- 响应数据

```
{
    "data": {
        "total": 5,
        "pagenum": 1,
        "users": [
            {
                "id": 25,
                "username": "tom",
                "mobile": "13951783475",
                "type": 1,
                "email": "1049901079@qq.com",
                "create_time": "2020-11-09T20:36:26.000Z",
                "mg_state": true, // 当前用户的状态
                "role_name": "超级管理员"
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加用户

- 请求路径：users

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名称 | 不能为空 |
| password | 用户密码 | 不能为空 |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 28,
        "username": "tom",
        "mobile": "test",
        "type": 1,
        "openid": "",
        "email": "test@test.com",
        "create_time": "2020-11-10T03:47:13.533Z",
        "modify_time": null,
        "is_delete": false,
        "is_active": false
    },
    "meta": {
        "msg": "用户创建成功",
        "status": 201
    }
}
```

### 3\. 修改用户状态

- 请求路径：users/:uId/state/:type

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| uId | 用户 ID | 不能为空`携带在url中` |
| type | 用户状态 | 不能为空`携带在url中`，值为 true 或者 false |

- 响应数据

```
{
  "data": {
    "id": 566,
    "rid": 30,
    "username": "admin",
    "mobile": "123456",
    "email": "bb@itany.com",
    "mg_state": 0
  },
  "meta": {
    "msg": "设置状态成功",
    "status": 200
  }
}
```

### 4\. 根据 ID 查询用户信息

- 请求路径：users/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "00000",
        "email": "new@new.com"
    },
    "meta": {
        "msg": "查询成功",
        "status": 200
    }
}
```

### 5\. 编辑提交用户

- 请求路径：users/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空 `参数是url参数:id` |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
/* 200表示成功，500表示失败 */
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "111",
        "email": "123@123.com"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 6\. 删除单个用户

- 请求路径：users/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空`参数是url参数:id` |

- 响应参数

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 7\. 分配用户角色

- 请求路径：users/:id/role

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`参数是url参数:id` |
| rid | 角色 id | 不能为空`参数body参数` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 508,
        "rid": "30",
        "username": "asdf1",
        "mobile": "123123",
        "email": "adfsa@qq.com"
    },
    "meta": {
        "msg": "设置角色成功",
        "status": 200
    }
}
```

## 四、权限管理

### 1\. 所有权限列表

- 请求路径：rights/:type

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | 类型 | 值 list 或 tree , list 列表显示权限, tree 树状显示权限,`参数是url参数:type` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 权限 ID |  |
| authName | 权限说明 |  |
| level | 权限层级 |  |
| pid | 权限父 ID |  |
| path | 对应访问路径 |  |

- 响应数据 type=list

```
  {
    "data": [
        {
            "id": 101,
            "authName": "商品管理",
            "level": "0",
            "pid": 0,
            "path": null
        },
        {
            "id": 102,
            "authName": "订单管理",
            "level": "0",
            "pid": 0,
            "path": null
        }
    ],
    "meta": {
        "msg": "获取权限列表成功",
        "status": 200
    }
}
```

type=tree

```
  {
    data: [
      {
        id: 101,
        authName: '商品管理',
        path: null,
        pid: 0,
        children: [
          {
            id: 104,
            authName: '商品列表',
            path: null,
            pid: 101,
            children: [
              {
                id: 105,
                authName: '添加商品',
                path: null,
                pid: '104,101'
              }
            ]
          }
        ]
      }
    ],
    meta: {
      msg: '获取权限列表成功',
      status: 200
    }
  }
```

### 2\. 左侧菜单权限

- 请求路径：menus

- 请求方法：get

- 响应数据

```
{
    "data":
        {
            "id": 101,
            "authName": "商品管理",
            "path": null,
            "children": [
                {
                    "id": 104,
                    "authName": "商品列表",
                    "path": null,
                    "children": []
                }
            ]
        }
    "meta": {
        "msg": "获取菜单列表成功",
        "status": 200
    }
}
```

## 五、角色管理

### 1\. 角色列表

- 请求路径：roles

- 请求方法：get

- 响应数据说明
    - 第一层为角色信息
    
    - 第二层开始为权限说明，权限一共有 3 层权限
    
    - 最后一层权限，不包含 `children` 属性

- 响应数据

```
{
    "data": [
        {
            "id": 30,
            "roleName": "主管",
            "roleDesc": "技术负责人",
            "children": [
                {
                    "id": 101,
                    "authName": "商品管理",
                    "path": null,
                    "children": [
                        {
                            "id": 104,
                            "authName": "商品列表",
                            "path": null,
                            "children": [
                                {
                                    "id": 105,
                                    "authName": "添加商品",
                                    "path": null
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加角色

- 请求路径：roles

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 40,
        "roleName": "admin2",
        "roleDesc": "admin2Desc"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询角色

- 请求路径：roles/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试负责人"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交角色

- 请求路径：roles/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试角色描述"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 5\. 删除角色

- 请求路径：roles/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 6\. 角色授权

- 请求路径：roles/:roleId/rights

- 请求方法：post

- 请求参数：通过 `请求体` 发送给后端

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :roleId | 角色 ID | 不能为空`携带在url中` |
| rids | 权限 ID 列表（字符串） | 以 `,` 分割的权限 ID 列表（获取所有被选中、叶子节点的key和半选中节点的key, 包括 1，2，3级节点） |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 7\. 删除角色指定权限

- 请求路径：roles/:roleId/rights/:rightId

- 请求方法：delete

- 请求参数 参数名 参数说明 备注 :roleId 角色 ID 不能为空`携带在url中` :rightId 权限 ID 不能为空`携带在url中`

- 响应数据说明
    - 返回的data, 是当前角色下最新的权限数据

- 响应数据

```
  {
      "data": [
          {
              "id": 101,
              "authName": "商品管理",
              "path": null,
              "children": [
                  {
                      "id": 104,
                      "authName": "商品列表",
                      "path": null,
                      "children": [
                          {
                              "id": 105,
                              "authName": "添加商品",
                              "path": null
                          },
                          {
                              "id": 116,
                              "authName": "修改",
                              "path": null
                          }
                      ]
                  }
              ]
          }
      ],
      "meta": {
          "msg": "取消权限成功",
          "status": 200
      }
  }
```

## 六、商品分类管理

### 1\. 商品分类数据列表

- 请求路径：categories

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | \[1,2,3\] | 值：1，2，3 分别表示显示一层二层三层分类列表  
【可选参数】如果不传递，则默认获取所有级别的分类 |
| pagenum | 当前页码值 | 【可选参数】如果不传递，则默认获取所有分类 |
| pagesize | 每页显示多少条数据 | 【可选参数】如果不传递，则默认获取所有分类 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_id | 分类 ID |  |
| cat\_name | 分类名称 |  |
| cat\_pid | 分类父 ID |  |
| cat\_level | 分类当前层级 |  |

- 响应数据

```
{
    "data": [
        {
            "cat_id": 1,
            "cat_name": "大家电",
            "cat_pid": 0,
            "cat_level": 0,
            "cat_deleted": false,
            "children": [
                {
                    "cat_id": 3,
                    "cat_name": "电视",
                    "cat_pid": 1,
                    "cat_level": 1,
                    "cat_deleted": false,
                    "children": [
                        {
                            "cat_id": 6,
                            "cat_name": "曲面电视",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        },
                        {
                            "cat_id": 7,
                            "cat_name": "海信",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加分类

- 请求路径：categories

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_pid | 分类父 ID | 不能为空，如果要添加一级分类，则父分类Id应该设置为 `0` |
| cat\_name | 分类名称 | 不能为空 |
| cat\_level | 分类层级 | 不能为空，`0`表示一级分类；`1`表示二级分类；`2`表示三级分类 |

- 响应数据

```
{
    "data": {
        "cat_id": 62,
        "cat_name": "相框",
        "cat_pid": "1",
        "cat_level": "1"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 id 查询分类

- 请求路径：categories/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "cat_id": 3,
        "cat_name": "厨卫电器",
        "cat_pid": 0,
        "cat_level": 0
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交分类

- 请求路径：categories/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| cat\_name | 分类名称 | 不能为空【此参数，放到请求体中】 |

- 响应数据

```
{
    "data": {
        "cat_id": 22,
        "cat_name": "自拍杆",
        "cat_pid": 7,
        "cat_level": 2
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除分类

- 请求路径：categories/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 七、分类参数管理

### 1\. 参数列表

- 请求路径：categories/:id/attributes

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| sel | many或only | 不能为空，many表示动态参数，only表示静态参数（也称为静态属性） |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| attr\_id | 分类参数 ID |  |
| attr\_name | 分类参数名称 |  |
| cat\_id | 分类参数所属分类 |  |
| attr\_sel | 分类参数的类型，many表示动态参数，only表示静态属性 |  |
| attr\_write | list表示从列表选择（动态参数），manual表示手工录入（静态属性） |  |
| attr\_vals | 分类参数的明细，如果是动态参数，则该值是以`空格`分隔的字符串 |  |

- 响应数据

```
{
    "data": [
        {
            "attr_id": 1,
            "attr_name": "cpu",
            "cat_id": 22,
            "attr_sel": "many",
            "attr_write": "list",
            "attr_vals": "4K高清 5K高清 6K高清"
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加动态参数或静态属性

- 请求路径：categories/:id/attributes

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空 |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 44,
        "attr_name": "测试参数",
        "cat_id": "1",
        "attr_sel": "many",
        "attr_write": "list",
        "attr_vals": "a,b,c"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询参数

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 属性 ID | 不能为空`携带在url中` |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 1,
        "attr_name": "cpu",
        "cat_id": 22,
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "ffff"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 更新参数及明细

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 参数 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空，携带在`请求体`中 |
| attr\_sel | many或only | 不能为空，携带在`请求体`中 |
| attr\_vals | 参数的明细 | 可选参数，携带在`请求体`中 |

- 响应数据

```
{
    "data": {
        "attr_id": 9,
        "attr_name": "测试更新",
        "cat_id": "43",
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "abc"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除参数

- 请求路径： categories/:id/attributes/:attrid

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrid | 参数 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 八、商品管理

### 1\. 商品列表数据

- 请求路径：goods

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |

- 响应数据

```
{
    "data": {
        "total": 50,
        "pagenum": "1",
        "goods": [
            {
                "goods_id": 144,
                "goods_name": "iphone",
                "goods_price": 1,
                "goods_number": 1,
                "goods_weight": 1,
                "goods_state": null,
                "add_time": 1512954923,
                "upd_time": 1512954923,
                "hot_mumber": 0,
                "is_promote": false
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加商品

- 请求路径：goods

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以`,`逗号分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组），包含 `动态参数` 和 `静态属性` | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_cat": "1,2,3",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_cat | 以为','分割的分类列表 |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "cat_id": 1,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "创建商品成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询商品

- 请求路径：goods/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| goods\_introduce | 介绍 |  |
| goods\_cat | 以为','分割的分类列表 |  |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交商品

- 请求路径：goods/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以为','分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组） | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选, |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新商品成功",
        "status": 200
    }
}
```

### 5\. 删除商品

- 请求路径：goods/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

###同步商品图片

- 请求路径：goods/:id/pics

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| pics | 商品图片集合 | 如果有 pics\_id 字段会保留该图片，如果没有 pics\_id 但是有 pic 字段就会新生成图片数据 |

- 请求数据

```
;[
  { pic: 'tmp_uploads/db28f6316835836e97653b5c75e418be.png' },
  {
    pics_id: 397,
    goods_id: 145,
    pics_big: 'uploads/goodspics/big_30f08d52c551ecb447277eae232304b8',
    pics_mid: 'uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8',
    pics_sma: 'uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###同步商品属性

- 请求路径：goods/:id/attributes

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 请求数据

```
;[
  {
    attr_id: 15,
    attr_value: 'ddd'
  },
  {
    attr_id: 15,
    attr_value: 'eee'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###商品图片处理必须安装 GraphicsMagick

- linux

```
apt-get install GraphicsMagick
```

- Mac OS X

```
brew install GraphicsMagick
```

- Windows [点击下载](https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick-binaries/1.3.27/GraphicsMagick-1.3.27-Q8-win64-dll.exe/download)

### 6\. 图片上传

- 请求路径：upload

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| file | 上传文件 |  |

- 响应数据

```
{
    "data": {
        "tmp_path": "tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png",
        "url": "http://127.0.0.1:8888tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png"
    },
    "meta": {
        "msg": "上传成功",
        "status": 200
    }
}
```

## 九、订单管理

### 1\. 订单数据列表

- 请求路径：orders

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 可以为空，默认为1 |
| pagesize | 每页显示条数 | 可以为空，省略时返回所有订单 |
| user\_id | 用户 ID | 可以为空 |
| pay\_status | 支付状态 | 可以为空 |
| is\_send | 是否发货 | 可以为空 |
| order\_fapiao\_title | \['个人','公司'\] | 可以为空 |
| order\_fapiao\_company | 公司名称 | 可以为空 |
| order\_fapiao\_content | 发票内容 | 可以为空 |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {
        "total": 1,
        "pagenum": "1",
        "goods": [
            {
                "order_id": 47,
                "user_id": 133,
                "order_number": "59e7502d7993d",
                "order_price": 322,
                "order_pay": "1",
                "is_send": "是",
                "trade_no": "",
                "order_fapiao_title": "个人",
                "order_fapiao_company": "",
                "order_fapiao_content": "办公用品",
                "consignee_addr": "江苏省南京市秦淮区龙蟠中路666号",
                "pay_status": "1",
                "create_time": 1508331565,
                "update_time": 1508331565
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 修改订单状态

- 请求路径：orders/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| is\_send | 订单是否发货 | 1:已经发货，0:未发货 |
| order\_pay | 订单支付 | 支付方式 0 未支付 1 支付宝 2 微信 3 银行卡 |
| order\_price | 订单价格 |  |
| order\_number | 订单数量 |  |
| pay\_status | 支付状态 | 订单状态： 0 未付款、1 已付款 |

- 请求数据说明
    - 所有请求数据都是增量更新，如果参数不填写，就不会更新该字段

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 3\. 查看订单详情

- 请求路径：orders/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 修改地址

- 请求路径：orders/:id/address

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {},
    "meta": {
        "msg": "修改地址成功",
        "status": 200
    }
}
```

### 5\. 查看物流信息

- 请求路径：/kuaidi/:id

- 请求方法：get

- 物流单号：815294206237577

- 响应数据：

```
  {
      "meta":{
          "status":200,
          "message":"获取物流信息成功！"
      },
      "data":[
          {
              "time":"2020-11-15 12:39:56",
              "ftime":"2020-11-15 12:39:56",
              "context":"已签收,签收人是 汤小洋 先生/女士，如有疑问请联系派件员阿奇(13805148888)，如您未收到此快递，请拨打投诉电话：15294207777，感谢使用申通快递，期待再次为您服务",
              "location":null
          },
          {
              "time":"2020-11-15 08:46:54",
              "ftime":"2020-11-15 08:46:54",
              "context":"上海浦东寒亭营业厅-寒亭阿奇(13805148888)-派件中",
              "location":null
          },
          {
              "time":"2020-11-15 08:38:57",
              "ftime":"2020-11-15 08:38:57",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-15 06:38:13",
              "ftime":"2020-11-15 06:38:13",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-14 20:56:45",
              "ftime":"2020-11-14 20:56:45",
              "context":"上海浦东转运中心-已发往-上海浦东寒亭公司",
              "location":null
          },
          {
              "time":"2020-11-14 20:52:44",
              "ftime":"2020-11-14 20:52:44",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 17:43:48",
              "ftime":"2020-11-14 17:43:48",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:53:46",
              "ftime":"2020-11-14 10:53:46",
              "context":"上海浦东转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:43:31",
              "ftime":"2020-11-14 10:43:31",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:43:20",
              "ftime":"2020-11-14 02:43:20",
              "context":"江苏苏州转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:41:40",
              "ftime":"2020-11-14 02:41:40",
              "context":"已到达-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 16:28:13",
              "ftime":"2020-11-13 16:28:13",
              "context":"江苏南京转运中心-已发往-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 15:03:30",
              "ftime":"2020-11-13 15:03:30",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:47:56",
              "ftime":"2020-11-13 14:47:56",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:37:06",
              "ftime":"2020-11-13 14:37:06",
              "context":"南京IT教育公司-城东汪小主宠物店-已收件",
              "location":null
          }
      ]
  }
```

## 一、API V2 接口说明

- 接口基准地址：`http://www.tangxiaoyang.vip:8888/api/v2/`

- 服务端已开启 CORS 跨域支持

- API V2 认证统一使用 Token 认证

- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌

- 使用 HTTP Status Code 标识状态

- 数据返回格式统一使用 JSON

### 1\. 支持的请求方法

- GET（SELECT）：从服务器取出资源（一项或多项）。

- POST（CREATE）：在服务器新建一个资源。

- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

- DELETE（DELETE）：从服务器删除资源。

- HEAD：获取资源的元数据。

- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

### 2\. 通用返回状态说明

| 状态码 | 含义 | 说明 |
| --- | --- | --- |
| 200 | OK | 请求成功 |
| 201 | CREATED | 创建成功 |
| 204 | DELETED | 删除成功 |
| 400 | BAD REQUEST | 请求的地址不存在或者包含不支持的参数 |
| 401 | UNAUTHORIZED | 未授权 |
| 403 | FORBIDDEN | 被禁止访问 |
| 404 | NOT FOUND | 请求的资源不存在 |
| 422 | Unprocesable entity | \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误 |
| 500 | INTERNAL SERVER ERROR | 内部错误 |

## 二、登录

### 1\. 登录验证接口

- 请求路径：login

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名 | 不能为空 |
| password | 密码 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |
| token | 令牌 | 基于 jwt 的令牌 |

- 响应数据

```
{
    "data": {
        "id": 500,
        "rid": 0,
        "username": "admin",
        "mobile": "123",
        "email": "123@qq.com",
        "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzMDY5OX0.eGrsrvwHm-tPsO9r_pxHIQ5i5L1kX9RX444uwnRGaIM"
    },
    "meta": {
        "msg": "登录成功",
        "status": 200
    }
}
```

## 三、用户管理

### 1\. 用户数据列表

- 请求路径：users

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总记录数 |  |
| pagenum | 当前页码 |  |
| users | 用户数据集合 |  |

- 响应数据

```
{
    "data": {
        "total": 5,
        "pagenum": 1,
        "users": [
            {
                "id": 25,
                "username": "tom",
                "mobile": "13951783475",
                "type": 1,
                "email": "1049901079@qq.com",
                "create_time": "2020-11-09T20:36:26.000Z",
                "mg_state": true, // 当前用户的状态
                "role_name": "超级管理员"
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加用户

- 请求路径：users

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名称 | 不能为空 |
| password | 用户密码 | 不能为空 |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 28,
        "username": "tom",
        "mobile": "test",
        "type": 1,
        "openid": "",
        "email": "test@test.com",
        "create_time": "2020-11-10T03:47:13.533Z",
        "modify_time": null,
        "is_delete": false,
        "is_active": false
    },
    "meta": {
        "msg": "用户创建成功",
        "status": 201
    }
}
```

### 3\. 修改用户状态

- 请求路径：users/:uId/state/:type

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| uId | 用户 ID | 不能为空`携带在url中` |
| type | 用户状态 | 不能为空`携带在url中`，值为 true 或者 false |

- 响应数据

```
{
  "data": {
    "id": 566,
    "rid": 30,
    "username": "admin",
    "mobile": "123456",
    "email": "bb@itany.com",
    "mg_state": 0
  },
  "meta": {
    "msg": "设置状态成功",
    "status": 200
  }
}
```

### 4\. 根据 ID 查询用户信息

- 请求路径：users/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "00000",
        "email": "new@new.com"
    },
    "meta": {
        "msg": "查询成功",
        "status": 200
    }
}
```

### 5\. 编辑提交用户

- 请求路径：users/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空 `参数是url参数:id` |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
/* 200表示成功，500表示失败 */
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "111",
        "email": "123@123.com"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 6\. 删除单个用户

- 请求路径：users/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空`参数是url参数:id` |

- 响应参数

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 7\. 分配用户角色

- 请求路径：users/:id/role

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`参数是url参数:id` |
| rid | 角色 id | 不能为空`参数body参数` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 508,
        "rid": "30",
        "username": "asdf1",
        "mobile": "123123",
        "email": "adfsa@qq.com"
    },
    "meta": {
        "msg": "设置角色成功",
        "status": 200
    }
}
```

## 四、权限管理

### 1\. 所有权限列表

- 请求路径：rights/:type

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | 类型 | 值 list 或 tree , list 列表显示权限, tree 树状显示权限,`参数是url参数:type` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 权限 ID |  |
| authName | 权限说明 |  |
| level | 权限层级 |  |
| pid | 权限父 ID |  |
| path | 对应访问路径 |  |

- 响应数据 type=list

```
  {
    "data": [
        {
            "id": 101,
            "authName": "商品管理",
            "level": "0",
            "pid": 0,
            "path": null
        },
        {
            "id": 102,
            "authName": "订单管理",
            "level": "0",
            "pid": 0,
            "path": null
        }
    ],
    "meta": {
        "msg": "获取权限列表成功",
        "status": 200
    }
}
```

type=tree

```
  {
    data: [
      {
        id: 101,
        authName: '商品管理',
        path: null,
        pid: 0,
        children: [
          {
            id: 104,
            authName: '商品列表',
            path: null,
            pid: 101,
            children: [
              {
                id: 105,
                authName: '添加商品',
                path: null,
                pid: '104,101'
              }
            ]
          }
        ]
      }
    ],
    meta: {
      msg: '获取权限列表成功',
      status: 200
    }
  }
```

### 2\. 左侧菜单权限

- 请求路径：menus

- 请求方法：get

- 响应数据

```
{
    "data":
        {
            "id": 101,
            "authName": "商品管理",
            "path": null,
            "children": [
                {
                    "id": 104,
                    "authName": "商品列表",
                    "path": null,
                    "children": []
                }
            ]
        }
    "meta": {
        "msg": "获取菜单列表成功",
        "status": 200
    }
}
```

## 五、角色管理

### 1\. 角色列表

- 请求路径：roles

- 请求方法：get

- 响应数据说明
    - 第一层为角色信息
    
    - 第二层开始为权限说明，权限一共有 3 层权限
    
    - 最后一层权限，不包含 `children` 属性

- 响应数据

```
{
    "data": [
        {
            "id": 30,
            "roleName": "主管",
            "roleDesc": "技术负责人",
            "children": [
                {
                    "id": 101,
                    "authName": "商品管理",
                    "path": null,
                    "children": [
                        {
                            "id": 104,
                            "authName": "商品列表",
                            "path": null,
                            "children": [
                                {
                                    "id": 105,
                                    "authName": "添加商品",
                                    "path": null
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加角色

- 请求路径：roles

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 40,
        "roleName": "admin2",
        "roleDesc": "admin2Desc"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询角色

- 请求路径：roles/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试负责人"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交角色

- 请求路径：roles/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试角色描述"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 5\. 删除角色

- 请求路径：roles/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 6\. 角色授权

- 请求路径：roles/:roleId/rights

- 请求方法：post

- 请求参数：通过 `请求体` 发送给后端

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :roleId | 角色 ID | 不能为空`携带在url中` |
| rids | 权限 ID 列表（字符串） | 以 `,` 分割的权限 ID 列表（获取所有被选中、叶子节点的key和半选中节点的key, 包括 1，2，3级节点） |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 7\. 删除角色指定权限

- 请求路径：roles/:roleId/rights/:rightId

- 请求方法：delete

- 请求参数 参数名 参数说明 备注 :roleId 角色 ID 不能为空`携带在url中` :rightId 权限 ID 不能为空`携带在url中`

- 响应数据说明
    - 返回的data, 是当前角色下最新的权限数据

- 响应数据

```
  {
      "data": [
          {
              "id": 101,
              "authName": "商品管理",
              "path": null,
              "children": [
                  {
                      "id": 104,
                      "authName": "商品列表",
                      "path": null,
                      "children": [
                          {
                              "id": 105,
                              "authName": "添加商品",
                              "path": null
                          },
                          {
                              "id": 116,
                              "authName": "修改",
                              "path": null
                          }
                      ]
                  }
              ]
          }
      ],
      "meta": {
          "msg": "取消权限成功",
          "status": 200
      }
  }
```

## 六、商品分类管理

### 1\. 商品分类数据列表

- 请求路径：categories

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | \[1,2,3\] | 值：1，2，3 分别表示显示一层二层三层分类列表  
【可选参数】如果不传递，则默认获取所有级别的分类 |
| pagenum | 当前页码值 | 【可选参数】如果不传递，则默认获取所有分类 |
| pagesize | 每页显示多少条数据 | 【可选参数】如果不传递，则默认获取所有分类 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_id | 分类 ID |  |
| cat\_name | 分类名称 |  |
| cat\_pid | 分类父 ID |  |
| cat\_level | 分类当前层级 |  |

- 响应数据

```
{
    "data": [
        {
            "cat_id": 1,
            "cat_name": "大家电",
            "cat_pid": 0,
            "cat_level": 0,
            "cat_deleted": false,
            "children": [
                {
                    "cat_id": 3,
                    "cat_name": "电视",
                    "cat_pid": 1,
                    "cat_level": 1,
                    "cat_deleted": false,
                    "children": [
                        {
                            "cat_id": 6,
                            "cat_name": "曲面电视",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        },
                        {
                            "cat_id": 7,
                            "cat_name": "海信",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加分类

- 请求路径：categories

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_pid | 分类父 ID | 不能为空，如果要添加一级分类，则父分类Id应该设置为 `0` |
| cat\_name | 分类名称 | 不能为空 |
| cat\_level | 分类层级 | 不能为空，`0`表示一级分类；`1`表示二级分类；`2`表示三级分类 |

- 响应数据

```
{
    "data": {
        "cat_id": 62,
        "cat_name": "相框",
        "cat_pid": "1",
        "cat_level": "1"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 id 查询分类

- 请求路径：categories/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "cat_id": 3,
        "cat_name": "厨卫电器",
        "cat_pid": 0,
        "cat_level": 0
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交分类

- 请求路径：categories/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| cat\_name | 分类名称 | 不能为空【此参数，放到请求体中】 |

- 响应数据

```
{
    "data": {
        "cat_id": 22,
        "cat_name": "自拍杆",
        "cat_pid": 7,
        "cat_level": 2
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除分类

- 请求路径：categories/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 七、分类参数管理

### 1\. 参数列表

- 请求路径：categories/:id/attributes

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| sel | many或only | 不能为空，many表示动态参数，only表示静态参数（也称为静态属性） |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| attr\_id | 分类参数 ID |  |
| attr\_name | 分类参数名称 |  |
| cat\_id | 分类参数所属分类 |  |
| attr\_sel | 分类参数的类型，many表示动态参数，only表示静态属性 |  |
| attr\_write | list表示从列表选择（动态参数），manual表示手工录入（静态属性） |  |
| attr\_vals | 分类参数的明细，如果是动态参数，则该值是以`空格`分隔的字符串 |  |

- 响应数据

```
{
    "data": [
        {
            "attr_id": 1,
            "attr_name": "cpu",
            "cat_id": 22,
            "attr_sel": "many",
            "attr_write": "list",
            "attr_vals": "4K高清 5K高清 6K高清"
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加动态参数或静态属性

- 请求路径：categories/:id/attributes

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空 |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 44,
        "attr_name": "测试参数",
        "cat_id": "1",
        "attr_sel": "many",
        "attr_write": "list",
        "attr_vals": "a,b,c"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询参数

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 属性 ID | 不能为空`携带在url中` |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 1,
        "attr_name": "cpu",
        "cat_id": 22,
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "ffff"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 更新参数及明细

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 参数 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空，携带在`请求体`中 |
| attr\_sel | many或only | 不能为空，携带在`请求体`中 |
| attr\_vals | 参数的明细 | 可选参数，携带在`请求体`中 |

- 响应数据

```
{
    "data": {
        "attr_id": 9,
        "attr_name": "测试更新",
        "cat_id": "43",
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "abc"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除参数

- 请求路径： categories/:id/attributes/:attrid

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrid | 参数 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 八、商品管理

### 1\. 商品列表数据

- 请求路径：goods

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |

- 响应数据

```
{
    "data": {
        "total": 50,
        "pagenum": "1",
        "goods": [
            {
                "goods_id": 144,
                "goods_name": "iphone",
                "goods_price": 1,
                "goods_number": 1,
                "goods_weight": 1,
                "goods_state": null,
                "add_time": 1512954923,
                "upd_time": 1512954923,
                "hot_mumber": 0,
                "is_promote": false
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加商品

- 请求路径：goods

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以`,`逗号分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组），包含 `动态参数` 和 `静态属性` | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_cat": "1,2,3",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_cat | 以为','分割的分类列表 |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "cat_id": 1,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "创建商品成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询商品

- 请求路径：goods/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| goods\_introduce | 介绍 |  |
| goods\_cat | 以为','分割的分类列表 |  |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交商品

- 请求路径：goods/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以为','分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组） | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选, |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新商品成功",
        "status": 200
    }
}
```

### 5\. 删除商品

- 请求路径：goods/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

###同步商品图片

- 请求路径：goods/:id/pics

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| pics | 商品图片集合 | 如果有 pics\_id 字段会保留该图片，如果没有 pics\_id 但是有 pic 字段就会新生成图片数据 |

- 请求数据

```
;[
  { pic: 'tmp_uploads/db28f6316835836e97653b5c75e418be.png' },
  {
    pics_id: 397,
    goods_id: 145,
    pics_big: 'uploads/goodspics/big_30f08d52c551ecb447277eae232304b8',
    pics_mid: 'uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8',
    pics_sma: 'uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###同步商品属性

- 请求路径：goods/:id/attributes

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 请求数据

```
;[
  {
    attr_id: 15,
    attr_value: 'ddd'
  },
  {
    attr_id: 15,
    attr_value: 'eee'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###商品图片处理必须安装 GraphicsMagick

- linux

```
apt-get install GraphicsMagick
```

- Mac OS X

```
brew install GraphicsMagick
```

- Windows [点击下载](https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick-binaries/1.3.27/GraphicsMagick-1.3.27-Q8-win64-dll.exe/download)

### 6\. 图片上传

- 请求路径：upload

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| file | 上传文件 |  |

- 响应数据

```
{
    "data": {
        "tmp_path": "tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png",
        "url": "http://127.0.0.1:8888tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png"
    },
    "meta": {
        "msg": "上传成功",
        "status": 200
    }
}
```

## 九、订单管理

### 1\. 订单数据列表

- 请求路径：orders

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 可以为空，默认为1 |
| pagesize | 每页显示条数 | 可以为空，省略时返回所有订单 |
| user\_id | 用户 ID | 可以为空 |
| pay\_status | 支付状态 | 可以为空 |
| is\_send | 是否发货 | 可以为空 |
| order\_fapiao\_title | \['个人','公司'\] | 可以为空 |
| order\_fapiao\_company | 公司名称 | 可以为空 |
| order\_fapiao\_content | 发票内容 | 可以为空 |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {
        "total": 1,
        "pagenum": "1",
        "goods": [
            {
                "order_id": 47,
                "user_id": 133,
                "order_number": "59e7502d7993d",
                "order_price": 322,
                "order_pay": "1",
                "is_send": "是",
                "trade_no": "",
                "order_fapiao_title": "个人",
                "order_fapiao_company": "",
                "order_fapiao_content": "办公用品",
                "consignee_addr": "江苏省南京市秦淮区龙蟠中路666号",
                "pay_status": "1",
                "create_time": 1508331565,
                "update_time": 1508331565
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 修改订单状态

- 请求路径：orders/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| is\_send | 订单是否发货 | 1:已经发货，0:未发货 |
| order\_pay | 订单支付 | 支付方式 0 未支付 1 支付宝 2 微信 3 银行卡 |
| order\_price | 订单价格 |  |
| order\_number | 订单数量 |  |
| pay\_status | 支付状态 | 订单状态： 0 未付款、1 已付款 |

- 请求数据说明
    - 所有请求数据都是增量更新，如果参数不填写，就不会更新该字段

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 3\. 查看订单详情

- 请求路径：orders/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 修改地址

- 请求路径：orders/:id/address

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {},
    "meta": {
        "msg": "修改地址成功",
        "status": 200
    }
}
```

### 5\. 查看物流信息

- 请求路径：/kuaidi/:id

- 请求方法：get

- 物流单号：815294206237577

- 响应数据：

```
  {
      "meta":{
          "status":200,
          "message":"获取物流信息成功！"
      },
      "data":[
          {
              "time":"2020-11-15 12:39:56",
              "ftime":"2020-11-15 12:39:56",
              "context":"已签收,签收人是 汤小洋 先生/女士，如有疑问请联系派件员阿奇(13805148888)，如您未收到此快递，请拨打投诉电话：15294207777，感谢使用申通快递，期待再次为您服务",
              "location":null
          },
          {
              "time":"2020-11-15 08:46:54",
              "ftime":"2020-11-15 08:46:54",
              "context":"上海浦东寒亭营业厅-寒亭阿奇(13805148888)-派件中",
              "location":null
          },
          {
              "time":"2020-11-15 08:38:57",
              "ftime":"2020-11-15 08:38:57",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-15 06:38:13",
              "ftime":"2020-11-15 06:38:13",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-14 20:56:45",
              "ftime":"2020-11-14 20:56:45",
              "context":"上海浦东转运中心-已发往-上海浦东寒亭公司",
              "location":null
          },
          {
              "time":"2020-11-14 20:52:44",
              "ftime":"2020-11-14 20:52:44",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 17:43:48",
              "ftime":"2020-11-14 17:43:48",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:53:46",
              "ftime":"2020-11-14 10:53:46",
              "context":"上海浦东转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:43:31",
              "ftime":"2020-11-14 10:43:31",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:43:20",
              "ftime":"2020-11-14 02:43:20",
              "context":"江苏苏州转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:41:40",
              "ftime":"2020-11-14 02:41:40",
              "context":"已到达-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 16:28:13",
              "ftime":"2020-11-13 16:28:13",
              "context":"江苏南京转运中心-已发往-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 15:03:30",
              "ftime":"2020-11-13 15:03:30",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:47:56",
              "ftime":"2020-11-13 14:47:56",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:37:06",
              "ftime":"2020-11-13 14:37:06",
              "context":"南京IT教育公司-城东汪小主宠物店-已收件",
              "location":null
          }
      ]
  }
```

## 一、API V2 接口说明

- 接口基准地址：`http://www.tangxiaoyang.vip:8888/api/v2/`

- 服务端已开启 CORS 跨域支持

- API V2 认证统一使用 Token 认证

- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌

- 使用 HTTP Status Code 标识状态

- 数据返回格式统一使用 JSON

### 1\. 支持的请求方法

- GET（SELECT）：从服务器取出资源（一项或多项）。

- POST（CREATE）：在服务器新建一个资源。

- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

- DELETE（DELETE）：从服务器删除资源。

- HEAD：获取资源的元数据。

- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

### 2\. 通用返回状态说明

| 状态码 | 含义 | 说明 |
| --- | --- | --- |
| 200 | OK | 请求成功 |
| 201 | CREATED | 创建成功 |
| 204 | DELETED | 删除成功 |
| 400 | BAD REQUEST | 请求的地址不存在或者包含不支持的参数 |
| 401 | UNAUTHORIZED | 未授权 |
| 403 | FORBIDDEN | 被禁止访问 |
| 404 | NOT FOUND | 请求的资源不存在 |
| 422 | Unprocesable entity | \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误 |
| 500 | INTERNAL SERVER ERROR | 内部错误 |

## 二、登录

### 1\. 登录验证接口

- 请求路径：login

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名 | 不能为空 |
| password | 密码 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |
| token | 令牌 | 基于 jwt 的令牌 |

- 响应数据

```
{
    "data": {
        "id": 500,
        "rid": 0,
        "username": "admin",
        "mobile": "123",
        "email": "123@qq.com",
        "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzMDY5OX0.eGrsrvwHm-tPsO9r_pxHIQ5i5L1kX9RX444uwnRGaIM"
    },
    "meta": {
        "msg": "登录成功",
        "status": 200
    }
}
```

## 三、用户管理

### 1\. 用户数据列表

- 请求路径：users

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总记录数 |  |
| pagenum | 当前页码 |  |
| users | 用户数据集合 |  |

- 响应数据

```
{
    "data": {
        "total": 5,
        "pagenum": 1,
        "users": [
            {
                "id": 25,
                "username": "tom",
                "mobile": "13951783475",
                "type": 1,
                "email": "1049901079@qq.com",
                "create_time": "2020-11-09T20:36:26.000Z",
                "mg_state": true, // 当前用户的状态
                "role_name": "超级管理员"
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加用户

- 请求路径：users

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名称 | 不能为空 |
| password | 用户密码 | 不能为空 |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 28,
        "username": "tom",
        "mobile": "test",
        "type": 1,
        "openid": "",
        "email": "test@test.com",
        "create_time": "2020-11-10T03:47:13.533Z",
        "modify_time": null,
        "is_delete": false,
        "is_active": false
    },
    "meta": {
        "msg": "用户创建成功",
        "status": 201
    }
}
```

### 3\. 修改用户状态

- 请求路径：users/:uId/state/:type

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| uId | 用户 ID | 不能为空`携带在url中` |
| type | 用户状态 | 不能为空`携带在url中`，值为 true 或者 false |

- 响应数据

```
{
  "data": {
    "id": 566,
    "rid": 30,
    "username": "admin",
    "mobile": "123456",
    "email": "bb@itany.com",
    "mg_state": 0
  },
  "meta": {
    "msg": "设置状态成功",
    "status": 200
  }
}
```

### 4\. 根据 ID 查询用户信息

- 请求路径：users/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "00000",
        "email": "new@new.com"
    },
    "meta": {
        "msg": "查询成功",
        "status": 200
    }
}
```

### 5\. 编辑提交用户

- 请求路径：users/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空 `参数是url参数:id` |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
/* 200表示成功，500表示失败 */
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "111",
        "email": "123@123.com"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 6\. 删除单个用户

- 请求路径：users/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空`参数是url参数:id` |

- 响应参数

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 7\. 分配用户角色

- 请求路径：users/:id/role

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`参数是url参数:id` |
| rid | 角色 id | 不能为空`参数body参数` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 508,
        "rid": "30",
        "username": "asdf1",
        "mobile": "123123",
        "email": "adfsa@qq.com"
    },
    "meta": {
        "msg": "设置角色成功",
        "status": 200
    }
}
```

## 四、权限管理

### 1\. 所有权限列表

- 请求路径：rights/:type

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | 类型 | 值 list 或 tree , list 列表显示权限, tree 树状显示权限,`参数是url参数:type` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 权限 ID |  |
| authName | 权限说明 |  |
| level | 权限层级 |  |
| pid | 权限父 ID |  |
| path | 对应访问路径 |  |

- 响应数据 type=list

```
  {
    "data": [
        {
            "id": 101,
            "authName": "商品管理",
            "level": "0",
            "pid": 0,
            "path": null
        },
        {
            "id": 102,
            "authName": "订单管理",
            "level": "0",
            "pid": 0,
            "path": null
        }
    ],
    "meta": {
        "msg": "获取权限列表成功",
        "status": 200
    }
}
```

type=tree

```
  {
    data: [
      {
        id: 101,
        authName: '商品管理',
        path: null,
        pid: 0,
        children: [
          {
            id: 104,
            authName: '商品列表',
            path: null,
            pid: 101,
            children: [
              {
                id: 105,
                authName: '添加商品',
                path: null,
                pid: '104,101'
              }
            ]
          }
        ]
      }
    ],
    meta: {
      msg: '获取权限列表成功',
      status: 200
    }
  }
```

### 2\. 左侧菜单权限

- 请求路径：menus

- 请求方法：get

- 响应数据

```
{
    "data":
        {
            "id": 101,
            "authName": "商品管理",
            "path": null,
            "children": [
                {
                    "id": 104,
                    "authName": "商品列表",
                    "path": null,
                    "children": []
                }
            ]
        }
    "meta": {
        "msg": "获取菜单列表成功",
        "status": 200
    }
}
```

## 五、角色管理

### 1\. 角色列表

- 请求路径：roles

- 请求方法：get

- 响应数据说明
    - 第一层为角色信息
    
    - 第二层开始为权限说明，权限一共有 3 层权限
    
    - 最后一层权限，不包含 `children` 属性

- 响应数据

```
{
    "data": [
        {
            "id": 30,
            "roleName": "主管",
            "roleDesc": "技术负责人",
            "children": [
                {
                    "id": 101,
                    "authName": "商品管理",
                    "path": null,
                    "children": [
                        {
                            "id": 104,
                            "authName": "商品列表",
                            "path": null,
                            "children": [
                                {
                                    "id": 105,
                                    "authName": "添加商品",
                                    "path": null
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加角色

- 请求路径：roles

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 40,
        "roleName": "admin2",
        "roleDesc": "admin2Desc"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询角色

- 请求路径：roles/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试负责人"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交角色

- 请求路径：roles/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试角色描述"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 5\. 删除角色

- 请求路径：roles/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 6\. 角色授权

- 请求路径：roles/:roleId/rights

- 请求方法：post

- 请求参数：通过 `请求体` 发送给后端

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :roleId | 角色 ID | 不能为空`携带在url中` |
| rids | 权限 ID 列表（字符串） | 以 `,` 分割的权限 ID 列表（获取所有被选中、叶子节点的key和半选中节点的key, 包括 1，2，3级节点） |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 7\. 删除角色指定权限

- 请求路径：roles/:roleId/rights/:rightId

- 请求方法：delete

- 请求参数 参数名 参数说明 备注 :roleId 角色 ID 不能为空`携带在url中` :rightId 权限 ID 不能为空`携带在url中`

- 响应数据说明
    - 返回的data, 是当前角色下最新的权限数据

- 响应数据

```
  {
      "data": [
          {
              "id": 101,
              "authName": "商品管理",
              "path": null,
              "children": [
                  {
                      "id": 104,
                      "authName": "商品列表",
                      "path": null,
                      "children": [
                          {
                              "id": 105,
                              "authName": "添加商品",
                              "path": null
                          },
                          {
                              "id": 116,
                              "authName": "修改",
                              "path": null
                          }
                      ]
                  }
              ]
          }
      ],
      "meta": {
          "msg": "取消权限成功",
          "status": 200
      }
  }
```

## 六、商品分类管理

### 1\. 商品分类数据列表

- 请求路径：categories

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | \[1,2,3\] | 值：1，2，3 分别表示显示一层二层三层分类列表  
【可选参数】如果不传递，则默认获取所有级别的分类 |
| pagenum | 当前页码值 | 【可选参数】如果不传递，则默认获取所有分类 |
| pagesize | 每页显示多少条数据 | 【可选参数】如果不传递，则默认获取所有分类 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_id | 分类 ID |  |
| cat\_name | 分类名称 |  |
| cat\_pid | 分类父 ID |  |
| cat\_level | 分类当前层级 |  |

- 响应数据

```
{
    "data": [
        {
            "cat_id": 1,
            "cat_name": "大家电",
            "cat_pid": 0,
            "cat_level": 0,
            "cat_deleted": false,
            "children": [
                {
                    "cat_id": 3,
                    "cat_name": "电视",
                    "cat_pid": 1,
                    "cat_level": 1,
                    "cat_deleted": false,
                    "children": [
                        {
                            "cat_id": 6,
                            "cat_name": "曲面电视",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        },
                        {
                            "cat_id": 7,
                            "cat_name": "海信",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加分类

- 请求路径：categories

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_pid | 分类父 ID | 不能为空，如果要添加一级分类，则父分类Id应该设置为 `0` |
| cat\_name | 分类名称 | 不能为空 |
| cat\_level | 分类层级 | 不能为空，`0`表示一级分类；`1`表示二级分类；`2`表示三级分类 |

- 响应数据

```
{
    "data": {
        "cat_id": 62,
        "cat_name": "相框",
        "cat_pid": "1",
        "cat_level": "1"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 id 查询分类

- 请求路径：categories/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "cat_id": 3,
        "cat_name": "厨卫电器",
        "cat_pid": 0,
        "cat_level": 0
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交分类

- 请求路径：categories/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| cat\_name | 分类名称 | 不能为空【此参数，放到请求体中】 |

- 响应数据

```
{
    "data": {
        "cat_id": 22,
        "cat_name": "自拍杆",
        "cat_pid": 7,
        "cat_level": 2
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除分类

- 请求路径：categories/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 七、分类参数管理

### 1\. 参数列表

- 请求路径：categories/:id/attributes

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| sel | many或only | 不能为空，many表示动态参数，only表示静态参数（也称为静态属性） |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| attr\_id | 分类参数 ID |  |
| attr\_name | 分类参数名称 |  |
| cat\_id | 分类参数所属分类 |  |
| attr\_sel | 分类参数的类型，many表示动态参数，only表示静态属性 |  |
| attr\_write | list表示从列表选择（动态参数），manual表示手工录入（静态属性） |  |
| attr\_vals | 分类参数的明细，如果是动态参数，则该值是以`空格`分隔的字符串 |  |

- 响应数据

```
{
    "data": [
        {
            "attr_id": 1,
            "attr_name": "cpu",
            "cat_id": 22,
            "attr_sel": "many",
            "attr_write": "list",
            "attr_vals": "4K高清 5K高清 6K高清"
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加动态参数或静态属性

- 请求路径：categories/:id/attributes

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空 |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 44,
        "attr_name": "测试参数",
        "cat_id": "1",
        "attr_sel": "many",
        "attr_write": "list",
        "attr_vals": "a,b,c"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询参数

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 属性 ID | 不能为空`携带在url中` |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 1,
        "attr_name": "cpu",
        "cat_id": 22,
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "ffff"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 更新参数及明细

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 参数 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空，携带在`请求体`中 |
| attr\_sel | many或only | 不能为空，携带在`请求体`中 |
| attr\_vals | 参数的明细 | 可选参数，携带在`请求体`中 |

- 响应数据

```
{
    "data": {
        "attr_id": 9,
        "attr_name": "测试更新",
        "cat_id": "43",
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "abc"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除参数

- 请求路径： categories/:id/attributes/:attrid

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrid | 参数 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 八、商品管理

### 1\. 商品列表数据

- 请求路径：goods

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |

- 响应数据

```
{
    "data": {
        "total": 50,
        "pagenum": "1",
        "goods": [
            {
                "goods_id": 144,
                "goods_name": "iphone",
                "goods_price": 1,
                "goods_number": 1,
                "goods_weight": 1,
                "goods_state": null,
                "add_time": 1512954923,
                "upd_time": 1512954923,
                "hot_mumber": 0,
                "is_promote": false
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加商品

- 请求路径：goods

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以`,`逗号分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组），包含 `动态参数` 和 `静态属性` | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_cat": "1,2,3",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_cat | 以为','分割的分类列表 |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "cat_id": 1,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "创建商品成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询商品

- 请求路径：goods/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| goods\_introduce | 介绍 |  |
| goods\_cat | 以为','分割的分类列表 |  |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交商品

- 请求路径：goods/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以为','分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组） | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选, |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新商品成功",
        "status": 200
    }
}
```

### 5\. 删除商品

- 请求路径：goods/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

###同步商品图片

- 请求路径：goods/:id/pics

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| pics | 商品图片集合 | 如果有 pics\_id 字段会保留该图片，如果没有 pics\_id 但是有 pic 字段就会新生成图片数据 |

- 请求数据

```
;[
  { pic: 'tmp_uploads/db28f6316835836e97653b5c75e418be.png' },
  {
    pics_id: 397,
    goods_id: 145,
    pics_big: 'uploads/goodspics/big_30f08d52c551ecb447277eae232304b8',
    pics_mid: 'uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8',
    pics_sma: 'uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###同步商品属性

- 请求路径：goods/:id/attributes

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 请求数据

```
;[
  {
    attr_id: 15,
    attr_value: 'ddd'
  },
  {
    attr_id: 15,
    attr_value: 'eee'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###商品图片处理必须安装 GraphicsMagick

- linux

```
apt-get install GraphicsMagick
```

- Mac OS X

```
brew install GraphicsMagick
```

- Windows [点击下载](https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick-binaries/1.3.27/GraphicsMagick-1.3.27-Q8-win64-dll.exe/download)

### 6\. 图片上传

- 请求路径：upload

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| file | 上传文件 |  |

- 响应数据

```
{
    "data": {
        "tmp_path": "tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png",
        "url": "http://127.0.0.1:8888tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png"
    },
    "meta": {
        "msg": "上传成功",
        "status": 200
    }
}
```

## 九、订单管理

### 1\. 订单数据列表

- 请求路径：orders

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 可以为空，默认为1 |
| pagesize | 每页显示条数 | 可以为空，省略时返回所有订单 |
| user\_id | 用户 ID | 可以为空 |
| pay\_status | 支付状态 | 可以为空 |
| is\_send | 是否发货 | 可以为空 |
| order\_fapiao\_title | \['个人','公司'\] | 可以为空 |
| order\_fapiao\_company | 公司名称 | 可以为空 |
| order\_fapiao\_content | 发票内容 | 可以为空 |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {
        "total": 1,
        "pagenum": "1",
        "goods": [
            {
                "order_id": 47,
                "user_id": 133,
                "order_number": "59e7502d7993d",
                "order_price": 322,
                "order_pay": "1",
                "is_send": "是",
                "trade_no": "",
                "order_fapiao_title": "个人",
                "order_fapiao_company": "",
                "order_fapiao_content": "办公用品",
                "consignee_addr": "江苏省南京市秦淮区龙蟠中路666号",
                "pay_status": "1",
                "create_time": 1508331565,
                "update_time": 1508331565
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 修改订单状态

- 请求路径：orders/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| is\_send | 订单是否发货 | 1:已经发货，0:未发货 |
| order\_pay | 订单支付 | 支付方式 0 未支付 1 支付宝 2 微信 3 银行卡 |
| order\_price | 订单价格 |  |
| order\_number | 订单数量 |  |
| pay\_status | 支付状态 | 订单状态： 0 未付款、1 已付款 |

- 请求数据说明
    - 所有请求数据都是增量更新，如果参数不填写，就不会更新该字段

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 3\. 查看订单详情

- 请求路径：orders/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 修改地址

- 请求路径：orders/:id/address

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {},
    "meta": {
        "msg": "修改地址成功",
        "status": 200
    }
}
```

### 5\. 查看物流信息

- 请求路径：/kuaidi/:id

- 请求方法：get

- 物流单号：815294206237577

- 响应数据：

```
  {
      "meta":{
          "status":200,
          "message":"获取物流信息成功！"
      },
      "data":[
          {
              "time":"2020-11-15 12:39:56",
              "ftime":"2020-11-15 12:39:56",
              "context":"已签收,签收人是 汤小洋 先生/女士，如有疑问请联系派件员阿奇(13805148888)，如您未收到此快递，请拨打投诉电话：15294207777，感谢使用申通快递，期待再次为您服务",
              "location":null
          },
          {
              "time":"2020-11-15 08:46:54",
              "ftime":"2020-11-15 08:46:54",
              "context":"上海浦东寒亭营业厅-寒亭阿奇(13805148888)-派件中",
              "location":null
          },
          {
              "time":"2020-11-15 08:38:57",
              "ftime":"2020-11-15 08:38:57",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-15 06:38:13",
              "ftime":"2020-11-15 06:38:13",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-14 20:56:45",
              "ftime":"2020-11-14 20:56:45",
              "context":"上海浦东转运中心-已发往-上海浦东寒亭公司",
              "location":null
          },
          {
              "time":"2020-11-14 20:52:44",
              "ftime":"2020-11-14 20:52:44",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 17:43:48",
              "ftime":"2020-11-14 17:43:48",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:53:46",
              "ftime":"2020-11-14 10:53:46",
              "context":"上海浦东转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:43:31",
              "ftime":"2020-11-14 10:43:31",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:43:20",
              "ftime":"2020-11-14 02:43:20",
              "context":"江苏苏州转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:41:40",
              "ftime":"2020-11-14 02:41:40",
              "context":"已到达-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 16:28:13",
              "ftime":"2020-11-13 16:28:13",
              "context":"江苏南京转运中心-已发往-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 15:03:30",
              "ftime":"2020-11-13 15:03:30",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:47:56",
              "ftime":"2020-11-13 14:47:56",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:37:06",
              "ftime":"2020-11-13 14:37:06",
              "context":"南京IT教育公司-城东汪小主宠物店-已收件",
              "location":null
          }
      ]
  }
```

## 一、API V2 接口说明

- 接口基准地址：`http://www.tangxiaoyang.vip:8888/api/v2/`

- 服务端已开启 CORS 跨域支持

- API V2 认证统一使用 Token 认证

- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌

- 使用 HTTP Status Code 标识状态

- 数据返回格式统一使用 JSON

### 1\. 支持的请求方法

- GET（SELECT）：从服务器取出资源（一项或多项）。

- POST（CREATE）：在服务器新建一个资源。

- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

- DELETE（DELETE）：从服务器删除资源。

- HEAD：获取资源的元数据。

- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

### 2\. 通用返回状态说明

| 状态码 | 含义 | 说明 |
| --- | --- | --- |
| 200 | OK | 请求成功 |
| 201 | CREATED | 创建成功 |
| 204 | DELETED | 删除成功 |
| 400 | BAD REQUEST | 请求的地址不存在或者包含不支持的参数 |
| 401 | UNAUTHORIZED | 未授权 |
| 403 | FORBIDDEN | 被禁止访问 |
| 404 | NOT FOUND | 请求的资源不存在 |
| 422 | Unprocesable entity | \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误 |
| 500 | INTERNAL SERVER ERROR | 内部错误 |

## 二、登录

### 1\. 登录验证接口

- 请求路径：login

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名 | 不能为空 |
| password | 密码 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |
| token | 令牌 | 基于 jwt 的令牌 |

- 响应数据

```
{
    "data": {
        "id": 500,
        "rid": 0,
        "username": "admin",
        "mobile": "123",
        "email": "123@qq.com",
        "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzMDY5OX0.eGrsrvwHm-tPsO9r_pxHIQ5i5L1kX9RX444uwnRGaIM"
    },
    "meta": {
        "msg": "登录成功",
        "status": 200
    }
}
```

## 三、用户管理

### 1\. 用户数据列表

- 请求路径：users

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总记录数 |  |
| pagenum | 当前页码 |  |
| users | 用户数据集合 |  |

- 响应数据

```
{
    "data": {
        "total": 5,
        "pagenum": 1,
        "users": [
            {
                "id": 25,
                "username": "tom",
                "mobile": "13951783475",
                "type": 1,
                "email": "1049901079@qq.com",
                "create_time": "2020-11-09T20:36:26.000Z",
                "mg_state": true, // 当前用户的状态
                "role_name": "超级管理员"
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加用户

- 请求路径：users

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| username | 用户名称 | 不能为空 |
| password | 用户密码 | 不能为空 |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| rid | 用户角色 ID |  |
| username | 用户名 |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 28,
        "username": "tom",
        "mobile": "test",
        "type": 1,
        "openid": "",
        "email": "test@test.com",
        "create_time": "2020-11-10T03:47:13.533Z",
        "modify_time": null,
        "is_delete": false,
        "is_active": false
    },
    "meta": {
        "msg": "用户创建成功",
        "status": 201
    }
}
```

### 3\. 修改用户状态

- 请求路径：users/:uId/state/:type

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| uId | 用户 ID | 不能为空`携带在url中` |
| type | 用户状态 | 不能为空`携带在url中`，值为 true 或者 false |

- 响应数据

```
{
  "data": {
    "id": 566,
    "rid": 30,
    "username": "admin",
    "mobile": "123456",
    "email": "bb@itany.com",
    "mg_state": 0
  },
  "meta": {
    "msg": "设置状态成功",
    "status": 200
  }
}
```

### 4\. 根据 ID 查询用户信息

- 请求路径：users/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "00000",
        "email": "new@new.com"
    },
    "meta": {
        "msg": "查询成功",
        "status": 200
    }
}
```

### 5\. 编辑提交用户

- 请求路径：users/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空 `参数是url参数:id` |
| email | 邮箱 | 可以为空 |
| mobile | 手机号 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
/* 200表示成功，500表示失败 */
{
    "data": {
        "id": 503,
        "username": "admin3",
        "role_id": 0,
        "mobile": "111",
        "email": "123@123.com"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 6\. 删除单个用户

- 请求路径：users/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 id | 不能为空`参数是url参数:id` |

- 响应参数

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 7\. 分配用户角色

- 请求路径：users/:id/role

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID | 不能为空`参数是url参数:id` |
| rid | 角色 id | 不能为空`参数body参数` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 用户 ID |  |
| role\_id | 角色 ID |  |
| mobile | 手机号 |  |
| email | 邮箱 |  |

- 响应数据

```
{
    "data": {
        "id": 508,
        "rid": "30",
        "username": "asdf1",
        "mobile": "123123",
        "email": "adfsa@qq.com"
    },
    "meta": {
        "msg": "设置角色成功",
        "status": 200
    }
}
```

## 四、权限管理

### 1\. 所有权限列表

- 请求路径：rights/:type

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | 类型 | 值 list 或 tree , list 列表显示权限, tree 树状显示权限,`参数是url参数:type` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 权限 ID |  |
| authName | 权限说明 |  |
| level | 权限层级 |  |
| pid | 权限父 ID |  |
| path | 对应访问路径 |  |

- 响应数据 type=list

```
  {
    "data": [
        {
            "id": 101,
            "authName": "商品管理",
            "level": "0",
            "pid": 0,
            "path": null
        },
        {
            "id": 102,
            "authName": "订单管理",
            "level": "0",
            "pid": 0,
            "path": null
        }
    ],
    "meta": {
        "msg": "获取权限列表成功",
        "status": 200
    }
}
```

type=tree

```
  {
    data: [
      {
        id: 101,
        authName: '商品管理',
        path: null,
        pid: 0,
        children: [
          {
            id: 104,
            authName: '商品列表',
            path: null,
            pid: 101,
            children: [
              {
                id: 105,
                authName: '添加商品',
                path: null,
                pid: '104,101'
              }
            ]
          }
        ]
      }
    ],
    meta: {
      msg: '获取权限列表成功',
      status: 200
    }
  }
```

### 2\. 左侧菜单权限

- 请求路径：menus

- 请求方法：get

- 响应数据

```
{
    "data":
        {
            "id": 101,
            "authName": "商品管理",
            "path": null,
            "children": [
                {
                    "id": 104,
                    "authName": "商品列表",
                    "path": null,
                    "children": []
                }
            ]
        }
    "meta": {
        "msg": "获取菜单列表成功",
        "status": 200
    }
}
```

## 五、角色管理

### 1\. 角色列表

- 请求路径：roles

- 请求方法：get

- 响应数据说明
    - 第一层为角色信息
    
    - 第二层开始为权限说明，权限一共有 3 层权限
    
    - 最后一层权限，不包含 `children` 属性

- 响应数据

```
{
    "data": [
        {
            "id": 30,
            "roleName": "主管",
            "roleDesc": "技术负责人",
            "children": [
                {
                    "id": 101,
                    "authName": "商品管理",
                    "path": null,
                    "children": [
                        {
                            "id": 104,
                            "authName": "商品列表",
                            "path": null,
                            "children": [
                                {
                                    "id": 105,
                                    "authName": "添加商品",
                                    "path": null
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加角色

- 请求路径：roles

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 40,
        "roleName": "admin2",
        "roleDesc": "admin2Desc"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询角色

- 请求路径：roles/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| roleId | 角色 ID |  |
| roleName | 角色名称 |  |
| roleDesc | 角色描述 |  |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试负责人"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交角色

- 请求路径：roles/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |
| roleName | 角色名称 | 不能为空 |
| roleDesc | 角色描述 | 可以为空 |

- 响应数据

```
{
    "data": {
        "roleId": 31,
        "roleName": "测试角色",
        "roleDesc": "测试角色描述"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 5\. 删除角色

- 请求路径：roles/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 角色 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

### 6\. 角色授权

- 请求路径：roles/:roleId/rights

- 请求方法：post

- 请求参数：通过 `请求体` 发送给后端

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :roleId | 角色 ID | 不能为空`携带在url中` |
| rids | 权限 ID 列表（字符串） | 以 `,` 分割的权限 ID 列表（获取所有被选中、叶子节点的key和半选中节点的key, 包括 1，2，3级节点） |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 7\. 删除角色指定权限

- 请求路径：roles/:roleId/rights/:rightId

- 请求方法：delete

- 请求参数 参数名 参数说明 备注 :roleId 角色 ID 不能为空`携带在url中` :rightId 权限 ID 不能为空`携带在url中`

- 响应数据说明
    - 返回的data, 是当前角色下最新的权限数据

- 响应数据

```
  {
      "data": [
          {
              "id": 101,
              "authName": "商品管理",
              "path": null,
              "children": [
                  {
                      "id": 104,
                      "authName": "商品列表",
                      "path": null,
                      "children": [
                          {
                              "id": 105,
                              "authName": "添加商品",
                              "path": null
                          },
                          {
                              "id": 116,
                              "authName": "修改",
                              "path": null
                          }
                      ]
                  }
              ]
          }
      ],
      "meta": {
          "msg": "取消权限成功",
          "status": 200
      }
  }
```

## 六、商品分类管理

### 1\. 商品分类数据列表

- 请求路径：categories

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| type | \[1,2,3\] | 值：1，2，3 分别表示显示一层二层三层分类列表  
【可选参数】如果不传递，则默认获取所有级别的分类 |
| pagenum | 当前页码值 | 【可选参数】如果不传递，则默认获取所有分类 |
| pagesize | 每页显示多少条数据 | 【可选参数】如果不传递，则默认获取所有分类 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_id | 分类 ID |  |
| cat\_name | 分类名称 |  |
| cat\_pid | 分类父 ID |  |
| cat\_level | 分类当前层级 |  |

- 响应数据

```
{
    "data": [
        {
            "cat_id": 1,
            "cat_name": "大家电",
            "cat_pid": 0,
            "cat_level": 0,
            "cat_deleted": false,
            "children": [
                {
                    "cat_id": 3,
                    "cat_name": "电视",
                    "cat_pid": 1,
                    "cat_level": 1,
                    "cat_deleted": false,
                    "children": [
                        {
                            "cat_id": 6,
                            "cat_name": "曲面电视",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        },
                        {
                            "cat_id": 7,
                            "cat_name": "海信",
                            "cat_pid": 3,
                            "cat_level": 2,
                            "cat_deleted": false
                        }
                    ]
                }
            ]
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加分类

- 请求路径：categories

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| cat\_pid | 分类父 ID | 不能为空，如果要添加一级分类，则父分类Id应该设置为 `0` |
| cat\_name | 分类名称 | 不能为空 |
| cat\_level | 分类层级 | 不能为空，`0`表示一级分类；`1`表示二级分类；`2`表示三级分类 |

- 响应数据

```
{
    "data": {
        "cat_id": 62,
        "cat_name": "相框",
        "cat_pid": "1",
        "cat_level": "1"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 id 查询分类

- 请求路径：categories/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "cat_id": 3,
        "cat_name": "厨卫电器",
        "cat_pid": 0,
        "cat_level": 0
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交分类

- 请求路径：categories/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| cat\_name | 分类名称 | 不能为空【此参数，放到请求体中】 |

- 响应数据

```
{
    "data": {
        "cat_id": 22,
        "cat_name": "自拍杆",
        "cat_pid": 7,
        "cat_level": 2
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除分类

- 请求路径：categories/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 七、分类参数管理

### 1\. 参数列表

- 请求路径：categories/:id/attributes

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| sel | many或only | 不能为空，many表示动态参数，only表示静态参数（也称为静态属性） |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| attr\_id | 分类参数 ID |  |
| attr\_name | 分类参数名称 |  |
| cat\_id | 分类参数所属分类 |  |
| attr\_sel | 分类参数的类型，many表示动态参数，only表示静态属性 |  |
| attr\_write | list表示从列表选择（动态参数），manual表示手工录入（静态属性） |  |
| attr\_vals | 分类参数的明细，如果是动态参数，则该值是以`空格`分隔的字符串 |  |

- 响应数据

```
{
    "data": [
        {
            "attr_id": 1,
            "attr_name": "cpu",
            "cat_id": 22,
            "attr_sel": "many",
            "attr_write": "list",
            "attr_vals": "4K高清 5K高清 6K高清"
        }
    ],
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加动态参数或静态属性

- 请求路径：categories/:id/attributes

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空 |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 44,
        "attr_name": "测试参数",
        "cat_id": "1",
        "attr_sel": "many",
        "attr_write": "list",
        "attr_vals": "a,b,c"
    },
    "meta": {
        "msg": "创建成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询参数

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 属性 ID | 不能为空`携带在url中` |
| attr\_sel | many或only | 不能为空 |

- 响应数据

```
{
    "data": {
        "attr_id": 1,
        "attr_name": "cpu",
        "cat_id": 22,
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "ffff"
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 更新参数及明细

- 请求路径：categories/:id/attributes/:attrId

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrId | 参数 ID | 不能为空`携带在url中` |
| attr\_name | 参数名称 | 不能为空，携带在`请求体`中 |
| attr\_sel | many或only | 不能为空，携带在`请求体`中 |
| attr\_vals | 参数的明细 | 可选参数，携带在`请求体`中 |

- 响应数据

```
{
    "data": {
        "attr_id": 9,
        "attr_name": "测试更新",
        "cat_id": "43",
        "attr_sel": "only",
        "attr_write": "manual",
        "attr_vals": "abc"
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

### 5\. 删除参数

- 请求路径： categories/:id/attributes/:attrid

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| :id | 分类 ID | 不能为空`携带在url中` |
| :attrid | 参数 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

## 八、商品管理

### 1\. 商品列表数据

- 请求路径：goods

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 不能为空 |
| pagesize | 每页显示条数 | 不能为空 |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |

- 响应数据

```
{
    "data": {
        "total": 50,
        "pagenum": "1",
        "goods": [
            {
                "goods_id": 144,
                "goods_name": "iphone",
                "goods_price": 1,
                "goods_number": 1,
                "goods_weight": 1,
                "goods_state": null,
                "add_time": 1512954923,
                "upd_time": 1512954923,
                "hot_mumber": 0,
                "is_promote": false
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 添加商品

- 请求路径：goods

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以`,`逗号分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组），包含 `动态参数` 和 `静态属性` | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_cat": "1,2,3",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_cat | 以为','分割的分类列表 |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "cat_id": 1,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "创建商品成功",
        "status": 201
    }
}
```

### 3\. 根据 ID 查询商品

- 请求路径：goods/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| goods\_introduce | 介绍 |  |
| goods\_cat | 以为','分割的分类列表 |  |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选 |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 编辑提交商品

- 请求路径：goods/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| goods\_name | 商品名称 | 不能为空 |
| goods\_cat | 以为','分割的分类列表 | 不能为空 |
| goods\_price | 价格 | 不能为空 |
| goods\_number | 数量 | 不能为空 |
| goods\_weight | 重量 | 不能为空 |
| goods\_introduce | 介绍 | 可以为空 |
| pics | 上传的图片临时路径（对象） | 可以为空 |
| attrs | 商品的参数（数组） | 可以为空 |

- 请求数据

```
{
  "goods_name":"test_goods_name2",
  "goods_price":20,
  "goods_number":30,
  "goods_weight":40,
  "goods_introduce":"abc",
  "pics":[
    {"pic":"/tmp_uploads/30f08d52c551ecb447277eae232304b8"}
    ],
  "attrs":[
    {
      "attr_id":15,
      "attr_value":"ddd"
    },
    {
      "attr_id":15,
      "attr_value":"eee"
    }
    ]
}
```

- 响应参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| total | 总共商品条数 |  |
| pagenum | 当前商品页数 |  |
| goods\_id | 商品 ID |  |
| goods\_name | 商品名称 |  |
| goods\_price | 价格 |  |
| goods\_number | 数量 |  |
| goods\_weight | 重量 | 不能为空 |
| goods\_state | 商品状态 | 商品状态 0: 未通过 1: 审核中 2: 已审核 |
| add\_time | 添加时间 |  |
| upd\_time | 更新时间 |  |
| hot\_mumber | 热销品数量 |  |
| is\_promote | 是否是热销品 |  |
| pics | 上传的图片临时路径（对象） | pics\_id:图片 ID,goods\_id:商品 ID,pics\_big:大图,pics\_mid:中图,pics\_sma:小图 |
| attrs | 商品的参数（数组） | goods\_id:商品 ID,attr\_value:当前商品的参数值,add\_price:浮动价格,attr\_vals:预定义的参数值,attr\_sel:手动输入，还是单选, |

- 响应数据

```
{
    "data": {
        "goods_id": 145,
        "goods_name": "test_goods_name2",
        "goods_price": 20,
        "goods_number": 30,
        "goods_weight": 40,
        "goods_introduce": "abc",
        "goods_big_logo": "",
        "goods_small_logo": "",
        "goods_state": 1,
        "add_time": 1512962370,
        "upd_time": 1512962370,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 397,
                "goods_id": 145,
                "pics_big": "uploads/goodspics/big_30f08d52c551ecb447277eae232304b8",
                "pics_mid": "uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8",
                "pics_sma": "uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8"
            }
        ],
        "attrs": [
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 145,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新商品成功",
        "status": 200
    }
}
```

### 5\. 删除商品

- 请求路径：goods/:id

- 请求方法：delete

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

###同步商品图片

- 请求路径：goods/:id/pics

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |
| pics | 商品图片集合 | 如果有 pics\_id 字段会保留该图片，如果没有 pics\_id 但是有 pic 字段就会新生成图片数据 |

- 请求数据

```
;[
  { pic: 'tmp_uploads/db28f6316835836e97653b5c75e418be.png' },
  {
    pics_id: 397,
    goods_id: 145,
    pics_big: 'uploads/goodspics/big_30f08d52c551ecb447277eae232304b8',
    pics_mid: 'uploads/goodspics/mid_30f08d52c551ecb447277eae232304b8',
    pics_sma: 'uploads/goodspics/sma_30f08d52c551ecb447277eae232304b8'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###同步商品属性

- 请求路径：goods/:id/attributes

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 商品 ID | 不能为空`携带在url中` |

- 请求数据

```
;[
  {
    attr_id: 15,
    attr_value: 'ddd'
  },
  {
    attr_id: 15,
    attr_value: 'eee'
  }
]
```

- 响应数据

```
{
    "data": {
        "goods_id": 96,
        "goods_name": "iphoneXX",
        "goods_price": 2,
        "goods_number": 22,
        "goods_weight": 22,
        "goods_introduce": null,
        "goods_big_logo": "./uploads/goods/20201113/483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_small_logo": "./uploads/goods/20201113/small_483a3b8e99e534ec3e4312dbbaee7c9d.jpg",
        "goods_state": 0,
        "is_del": "1",
        "add_time": 1510045904,
        "upd_time": 1512635159,
        "delete_time": 1512635159,
        "hot_mumber": 0,
        "is_promote": false,
        "pics": [
            {
                "pics_id": 383,
                "goods_id": 96,
                "pics_big": "uploads/goodspics/big_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_mid": "uploads/goodspics/mid_6f5750132abd3f5b2b93dd722fcde653.jpg",
                "pics_sma": "uploads/goodspics/sma_6f5750132abd3f5b2b93dd722fcde653.jpg"
            }
        ],
        "attrs": [
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "eee",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            },
            {
                "goods_id": 96,
                "attr_id": 15,
                "attr_value": "ddd",
                "add_price": null,
                "attr_name": "fffffff",
                "attr_sel": "many",
                "attr_write": "list",
                "attr_vals": ""
            }
        ]
    },
    "meta": {
        "msg": "更新成功",
        "status": 200
    }
}
```

###商品图片处理必须安装 GraphicsMagick

- linux

```
apt-get install GraphicsMagick
```

- Mac OS X

```
brew install GraphicsMagick
```

- Windows [点击下载](https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick-binaries/1.3.27/GraphicsMagick-1.3.27-Q8-win64-dll.exe/download)

### 6\. 图片上传

- 请求路径：upload

- 请求方法：post

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| file | 上传文件 |  |

- 响应数据

```
{
    "data": {
        "tmp_path": "tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png",
        "url": "http://127.0.0.1:8888tmp_uploads/ccfc5179a914e94506bcbb7377e8985f.png"
    },
    "meta": {
        "msg": "上传成功",
        "status": 200
    }
}
```

## 九、订单管理

### 1\. 订单数据列表

- 请求路径：orders

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| query | 查询参数 | 可以为空 |
| pagenum | 当前页码 | 可以为空，默认为1 |
| pagesize | 每页显示条数 | 可以为空，省略时返回所有订单 |
| user\_id | 用户 ID | 可以为空 |
| pay\_status | 支付状态 | 可以为空 |
| is\_send | 是否发货 | 可以为空 |
| order\_fapiao\_title | \['个人','公司'\] | 可以为空 |
| order\_fapiao\_company | 公司名称 | 可以为空 |
| order\_fapiao\_content | 发票内容 | 可以为空 |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {
        "total": 1,
        "pagenum": "1",
        "goods": [
            {
                "order_id": 47,
                "user_id": 133,
                "order_number": "59e7502d7993d",
                "order_price": 322,
                "order_pay": "1",
                "is_send": "是",
                "trade_no": "",
                "order_fapiao_title": "个人",
                "order_fapiao_company": "",
                "order_fapiao_content": "办公用品",
                "consignee_addr": "江苏省南京市秦淮区龙蟠中路666号",
                "pay_status": "1",
                "create_time": 1508331565,
                "update_time": 1508331565
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 2\. 修改订单状态

- 请求路径：orders/:id

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| is\_send | 订单是否发货 | 1:已经发货，0:未发货 |
| order\_pay | 订单支付 | 支付方式 0 未支付 1 支付宝 2 微信 3 银行卡 |
| order\_price | 订单价格 |  |
| order\_number | 订单数量 |  |
| pay\_status | 支付状态 | 订单状态： 0 未付款、1 已付款 |

- 请求数据说明
    - 所有请求数据都是增量更新，如果参数不填写，就不会更新该字段

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 3\. 查看订单详情

- 请求路径：orders/:id

- 请求方法：get

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |

- 响应数据

```
{
    "data": {
        "order_id": 67,
        "user_id": 1,
        "order_number": "itany-g7kmck71vjaujfgoi",
        "order_price": 20,
        "order_pay": "0",
        "is_send": "否",
        "trade_no": "",
        "order_fapiao_title": "个人",
        "order_fapiao_company": "",
        "order_fapiao_content": "",
        "consignee_addr": "",
        "pay_status": "0",
        "create_time": 1512533560,
        "update_time": 1512533560,
        "goods": [
            {
                "id": 82,
                "order_id": 67,
                "goods_id": 96,
                "goods_price": 333,
                "goods_number": 2,
                "goods_total_price": 999
            },
            {
                "id": 83,
                "order_id": 67,
                "goods_id": 95,
                "goods_price": 666,
                "goods_number": 5,
                "goods_total_price": 999
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
}
```

### 4\. 修改地址

- 请求路径：orders/:id/address

- 请求方法：put

- 请求参数

| 参数名 | 参数说明 | 备注 |
| --- | --- | --- |
| id | 订单 ID | 不能为空`携带在url中` |
| consignee\_addr | 收货地址 | 可以为空 |

- 响应数据

```
{
    "data": {},
    "meta": {
        "msg": "修改地址成功",
        "status": 200
    }
}
```

### 5\. 查看物流信息

- 请求路径：/kuaidi/:id

- 请求方法：get

- 物流单号：815294206237577

- 响应数据：

```
  {
      "meta":{
          "status":200,
          "message":"获取物流信息成功！"
      },
      "data":[
          {
              "time":"2020-11-15 12:39:56",
              "ftime":"2020-11-15 12:39:56",
              "context":"已签收,签收人是 汤小洋 先生/女士，如有疑问请联系派件员阿奇(13805148888)，如您未收到此快递，请拨打投诉电话：15294207777，感谢使用申通快递，期待再次为您服务",
              "location":null
          },
          {
              "time":"2020-11-15 08:46:54",
              "ftime":"2020-11-15 08:46:54",
              "context":"上海浦东寒亭营业厅-寒亭阿奇(13805148888)-派件中",
              "location":null
          },
          {
              "time":"2020-11-15 08:38:57",
              "ftime":"2020-11-15 08:38:57",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-15 06:38:13",
              "ftime":"2020-11-15 06:38:13",
              "context":"已到达-上海浦东寒亭营业厅",
              "location":null
          },
          {
              "time":"2020-11-14 20:56:45",
              "ftime":"2020-11-14 20:56:45",
              "context":"上海浦东转运中心-已发往-上海浦东寒亭公司",
              "location":null
          },
          {
              "time":"2020-11-14 20:52:44",
              "ftime":"2020-11-14 20:52:44",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 17:43:48",
              "ftime":"2020-11-14 17:43:48",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:53:46",
              "ftime":"2020-11-14 10:53:46",
              "context":"上海浦东转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 10:43:31",
              "ftime":"2020-11-14 10:43:31",
              "context":"已到达-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:43:20",
              "ftime":"2020-11-14 02:43:20",
              "context":"江苏苏州转运中心-已发往-上海浦东转运中心",
              "location":null
          },
          {
              "time":"2020-11-14 02:41:40",
              "ftime":"2020-11-14 02:41:40",
              "context":"已到达-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 16:28:13",
              "ftime":"2020-11-13 16:28:13",
              "context":"江苏南京转运中心-已发往-江苏苏州转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 15:03:30",
              "ftime":"2020-11-13 15:03:30",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:47:56",
              "ftime":"2020-11-13 14:47:56",
              "context":"南京IT教育公司-已发往-江苏南京转运中心",
              "location":null
          },
          {
              "time":"2020-11-13 14:37:06",
              "ftime":"2020-11-13 14:37:06",
              "context":"南京IT教育公司-城东汪小主宠物店-已收件",
              "location":null
          }
      ]
  }
```
