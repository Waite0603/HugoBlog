---
title: '使用Cursor+DevBox 从零创建一个 TodeList 网页应用'
description: '使用Cursor+DevBox 从零创建一个 TodeList 网页应用'
date: '2024-12-27T22:02:05+08:00'

categories: ["AI"]
tags: ["Cursor", "DevBox"]

draft: false
---


> Devbox 地址：https://cloud.sealos.run/?uid=Kt1gH3_BTa
>
> Cursor 地址: https://www.cursor.com/settings

## 创建数据库和初始项目

我们这里创建一个 MongoDB 数据库

![image-20241227205105497](https://qiniu.waite.wang/202412272051422.png)

创建完之后在 DevBox 创建一个 NodeJS + Express 的项目,注意创建在 3000 端口

![image-20241227205403921](https://qiniu.waite.wang/202412272054172.png)

选择用 Cursor 打开，这里会提示安装插件，安装即可

打开后 Ctrl + L 打开 AI 创作窗口，选择 Composer 输入提示词创建项目。

```
请为我开发一个基于 Node.js 和Express 框架的 Todo List 后端项目。项目需要实现以下四个 RESTful API 接口：

1. 查询所有待办事项
    - 接口名: GET /api/get-todo
    - 功能: 从数据库的'list'集合中查询并返回所有待办事项
    - 参数: 无
    - 返回: 包含所有待办事项的数组
2. 添加新的待办事项
    - 接口名: POST /api/add-todo
    - 功能: 向'list'集合中添加新的待办事项
    - 参数:
    {
    "value": string, // 待办事项的具体内容
    "isCompleted": boolean // 是否完成，默认为 false
    }
    - 返回: 新添加的待办事项对象，包含自动生成的唯一 id
3. 更新待办事项状态
    - 接口名: POST /api/update-todo/
    - 功能: 根据 id 更新指定待办事项的完成状态（将 isCompleted 值取反）
    - 参数: id
    - 返回: 更新后的待办事项对象
4. 删除待办事项
    - 接口名: POST /api/del-todo/
    - 功能: 根据 id 删除指定的待办事项
    - 参数: id
    - 返回: 删除操作的结果状态

技术要求：

1. 使用 Express 框架构建 API
2. 使用 MongoDB 作为数据库，通过 Mongoose 进行数据操作
3. 实现适当的错误处理和输入验证
4. 使用异步/等待（async/await）语法处理异步操作
5. 遵循 RESTful API 设计原则
6. 添加基本的日志记录功能

### 这里数据库连接方式要填写！！！

以下是数据库连接方式：
```

这里 AI 会自动帮我们创建项目结构以及生成相应代码

![image-20241227210448660](https://qiniu.waite.wang/202412272104031.png)

在终端运行 `node app.js` 这里可能会报错，如果看不懂报错直接复制丢给 AI 即可， 这里提示我们安装依赖包，按要求安装

![image-20241227210817014](https://qiniu.waite.wang/202412272108019.png)

![image-20241227210843024](https://qiniu.waite.wang/202412272108203.png)

这里我们可以看到运行成功了

![image-20241227212452631](https://qiniu.waite.wang/202412272124267.png)

在 DevBox 打开后端项目详情，看到公网地址，我们可以根据这个域名访问到我们的后端项目

![image-20241227212545099](https://qiniu.waite.wang/202412272125447.png)

为了测试接口 我们可以让 Cursor 帮我们生成测试用例

![image-20241227212625877](https://qiniu.waite.wang/202412272126706.png)

![image-20241227212709400](https://qiniu.waite.wang/202412272127258.png)

可以看到接口正常运行了

## 创建 Web 端

+ 创建一个 Vue 项目，并且更改端口为 5173（不要为3000或者其他常用端口即可）
+ 用 Cursor 打开项目，输入提示词

```
请为我开发一个基于 Vue 3 的Todo List 应用。要求如下：

1. 功能需求：
    - 添加新的待办事项
    - 标记待办事项为完成/未完成
    - 删除待办事项
    - 统计待办事项完成度
    - 过滤显示（全部/已完成/未完成）
2. UI/UX 设计要求：
    - 全屏响应式设计，适配不同设备
    - 拥有亮色模式和夜间模式
    - 现代化、简洁的界面风格
    - 丰富的色彩运用，但保持整体和谐
    - 流畅的交互动画，提升用户体验
    - 在按钮和需要的地方添加上图标
    - 参考灵感：结合苹果官网的设计美学

要求：

1. 直接以当前目录作为项目根目。注意 此目录已经初始化完了vue3项目结构 直接修改即可
2. 如果需要执行命令，请暂停创建文件，让我先执行命令
3. 请你根据我的需要，一步一步思考，给我开发这个项目。特别是UI部分 一定要足够美观和现代化

后端接口如下： https://wpifxnmfmpef.sealoshzh.site/api/

API 接口：

1. 查询所有待办事项
    - 接口名: GET /api/get-todo
    - 功能: 从数据库的'list'集合中查询并返回所有待办事项
    - 参数: 无
    - 返回: 包含所有待办事项的数组
2. 添加新的待办事项
    - 接口名: POST /api/add-todo
    - 功能: 向'list'集合中添加新的待办事项
    - 参数:
    {
    "value": string, // 待办事项的具体内容
    "isCompleted": boolean // 是否完成，默认为 false
    }
    - 返回: 新添加的待办事项对象，包含自动生成的唯一 id
3. 更新待办事项状态
    - 接口名: POST /api/update-todo/
    - 功能: 根据 id 更新指定待办事项的完成状态（将 isCompleted 值取反）
    - 参数: id
    - 返回: 更新后的待办事项对象
4. 删除待办事项
    - 接口名: POST /api/del-todo/
    - 功能: 根据 id 删除指定的待办事项
    - 参数: id
    - 返回: 删除操作的结果状态
```

按提示安装

![image-20241227213221302](https://qiniu.waite.wang/202412272132296.png)

安装后输入继续，将会继续帮我们创建项目，创建成功使用 `npm run dev` 启动项目

![image-20241227213558899](https://qiniu.waite.wang/202412272136259.png)

可以看到項目正常启动，并且功能正常，如果想要更改可以继续精华提示词

![image-20241227214258303](https://qiniu.waite.wang/202412272142474.png)

![image-20241227214306110](https://qiniu.waite.wang/202412272143317.png)

## 生成的代码

> https://github.com/Waite0603/todolist-cursor-nodeApi
>
> https://github.com/Waite0603/todolist-cursor-web