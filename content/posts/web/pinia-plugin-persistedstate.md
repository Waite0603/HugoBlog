---
title: "Pinia 数据持久化储存（pinia-plugin-persistedstate）"
date: 2023-11-22T16:04:23+08:00
categories: ["Web"]
tags: ["Vue3", "Pinia"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---


pinia需要使用pinia-plugin-persistedstate插件来进行数据的存储
插件官网地址：
https://prazdevs.github.io/pinia-plugin-persistedstate/guide/config.html

## 安装以及使用

```
pnpm i pinia-plugin-persistedstate

npm i pinia-plugin-persistedstate

yarn add pinia-plugin-persistedstate
```

``` typescript
import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)
```

## 关于全部缓存及部分缓存的说明
（1）将store的state中的全部数据进行缓存，直接在state同级下面添加persist对象

![](https://qiniu.waite.wang/202401070057203.png)

此时，默认将数据存放在浏览器的SessionStorage中，key为store的名称，value为该store中所有的数据

（2）将store的state中的数据进行部分缓存
此时需要在persist中添加strategies数组，

![](https://qiniu.waite.wang/202401070057633.png)

每个元素的key是想要储存的数据变量名（在state中定义的），storage可以写sessionStorage或者localStorage，此时，sessionStorage中的key就是变量名，value就是该变量的值

