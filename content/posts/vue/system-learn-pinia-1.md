---
title: "系统学习 Pinia"
date: 2023-11-11T21:05:54+08:00
lastmod: 2023-11-22T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue", "Pinia"]
author: "Waite Wang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
description: "Vue 学习笔记-系统学习 Pinia"
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

## 为什么使用 Pinia?

> https://pinia.vuejs.org/zh/

Pinia 是 Vue 的专属状态管理库，它允许你跨组件或页面共享状态。如果你熟悉组合式 API 的话，你可能会认为可以通过一行简单的 `export const state = reactive({})` 来共享一个全局状态。对于单页应用来说确实可以，但如果应用在服务器端渲染，这可能会使你的应用暴露出一些安全漏洞。 而如果使用 Pinia，即使在小型单页应用中，你也可以获得如下功能：

- Devtools 支持
  - 追踪 actions、mutations 的时间线
  - 在组件中展示它们所用到的 Store
  - 让调试更容易的 Time travel
- 热更新
  - 不必重载页面即可修改 Store
  - 开发时可保持当前的 State
- 插件：可通过插件扩展 Pinia 功能
- 为 JS 开发者提供适当的 TypeScript 支持以及**自动补全**功能。
- 支持服务端渲染

## 简单使用

1. 安装

```javascript
yarn add pinia
npm install pinia
```

在mian.js中，创建根存储

```javascript
import { createPinia } from 'pinia'

app.use(createPinia()
```

2. 创建store

`store`实例相当于一个容器，里面存放的有类似于`data`，计算属性，方法之类的东西。通过`defineStore()`方法定义

在`src`下面创建一个`store`文件夹，再创建与之对应的`js`文件，比如``counter.js`

```javascript
// option store
import { defineStore } from "pinia"

export const useCouterStore = defineStore('counter',
  {
    state: () => ({
      count: 0,
    }),
    getters: {
      doubleCount: (state) => state.count * 2,
    },
    actions: {
      increment() {
        this.count++
      }
    }
  }
)
```

```javascript
import { defineStore } from "pinia"
import { ref, computed } from 'vue'

export const useCouterStore = defineStore('counter', () => {
  // 定义数据
  const count = ref(0)

  // 定义修改数据的方法 (actions 同步加异步)
  const increment = () => count.value++

  // getters 计算属性
  const double = computed(() => count.value * 2)

  // 以对象的方式返回以供组件使用
  return {
    count,
    increment
  }
})
```

3. 使用store

```vue
<template>
  <button @click="counterStore.increment">{{ counterStore.count }}</button>
</template>

<script setup>
import { useCouterStore } from './store/counter';

const counterStore = useCouterStore();
console.log(counterStore);
</script>
```

## State

1. 读取state

```javascript
import { storeToRefs } from 'pinia'
const userStore = useUser()
// 如果直接解构出数据，这个数据不是响应式的。如果想要变成响应式的，需要调用storeToRefs方法
const { userName, avatar } = storeToRefs(userStore)
```

2. 修改state

```javascript
userStore.userName = '张三'
```

3. 重置 state

使用[选项式 API](https://pinia.vuejs.org/zh/core-concepts/#option-stores) 时，你可以通过调用 store 的 `$reset()` 方法将 state 重置为初始值。

在 `$reset()` 内部，会调用 `state()` 函数来创建一个新的状态对象，并用它替换当前状态。

```javascript
const store = useStore()
store.$reset()
```

4. 变更 state

除了用 `store.count++` 直接改变 store，你还可以调用 `$patch` 方法。它允许你用一个 `state` 的补丁对象在同一时间更改多个属性：

```javascript
store.$patch({
  count: store.count + 1,
  age: 120,
  name: 'DIO',
})
```

不过，用这种语法的话，有些变更真的很难实现或者很耗时：任何集合的修改（例如，向数组中添加、移除一个元素或是做 `splice` 操作）都需要你创建一个新的集合。因此，`$patch` 方法也接受一个函数来组合这种难以用补丁对象实现的变更。

```javascript
store.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```

两种变更 store 方法的主要区别是，`$patch()` 允许你将多个变更归入 `devtools` 的同一个条目中

> 你可以在 `pinia` 实例上使用 `watch()` 函数侦听整个 state。
>
> ```javascript
> watch(
>   pinia.state,
>   (state) => {
>     // 每当状态发生变化时，将整个 state 持久化到本地存储。
>     localStorage.setItem('piniaState', JSON.stringify(state))
>   },
>   { deep: true }
> )
> ```

## getters

1. 基础使用

类似计算属性。推荐里面传递一个形参的写法，不容易出错。

```javascript
  getters: {
    isAdult: (state) => {
      return state.age >= 18 ? '成年人' : '未成年'
    }
  }
```

```html
// 直接获取
<div>{{userStore.isAdult}}</div>
```

2. 调用本模块其他 getters

```javascript
  getters: {
    isAdult: (state) => {
      return state.age >= 18 ? '成年人' : '未成年'
    },
    msg: (state) => {
      // msg这个getter访问了自身的getter（isAdult）
      return state.userName + state.isAdult
    }
  }
```

3. getters传参

```javascript
  getters: {
    isAdmin: (state) => {
      // 如果getter里面是返回的函数，那么它就可以传参数了
      return (name) => name === 'admin' ? '是管理员' : '不是管理员'
    }
  }
```

```html
{{userStore.isAdmin}}
```

4. 调用其他store里面的getter

```javascript
// goods.js
import { defineStore } from 'pinia'

// 第一个参数是应用程序中 store 的唯一 id
// 第二个参数是配置对象
export const useGoods = defineStore('goods', {
  // state是一个函数，返回一个对象
  state: () => {
    return {
      goodsName: 'iphone'
    }
  },
  getters: {
    newIphone: (state) => {
      return state.goodsName + ' 14 pro'
    }
  }
})
```

```javascript
import { useGoods } from './goods.js'
// ..........
getters: {
    info: (state) => {
      // 获取goods模块的store
      const goodsStore = useGoods()
      return state.userName + '买了' + goodsStore.newIphone
    }
}
```

## actions

Action 相当于组件中的 [method](https://v3.vuejs.org/guide/data-methods.html#methods)。它们可以通过 `defineStore()` 中的 `actions` 属性来定义，**并且它们也是定义业务逻辑的完美选择。**

```javascript
export const useCounterStore = defineStore('main', {
  state: () => ({
    count: 0,
  }),
  actions: {
    increment() {
      this.count++
    },
    randomizeCounter() {
      this.count = Math.round(100 * Math.random())
    },
  },
})
```

类似 [getter](https://pinia.vuejs.org/zh/core-concepts/getters.html)，action 也可通过 `this` 访问**整个 store 实例**，并支持**完整的类型标注(以及自动补全✨)**。**不同的是，`action` 可以是异步的**，你可以在它们里面 `await` 调用任何 API，以及其他 action！下面是一个使用 [Mande](https://github.com/posva/mande) 的例子。请注意，你使用什么库并不重要，只要你得到的是一个`Promise`，你甚至可以 (在浏览器中) 使用原生 `fetch` 函数：

```javascript
import { mande } from 'mande'

const api = mande('/api/users')

export const useUsers = defineStore('users', {
  state: () => ({
    userData: null,
    // ...
  }),

  actions: {
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        // 让表单组件显示错误
        return error
      }
    },
  },
})
```

你也完全可以自由地设置任何你想要的参数以及返回任何结果。当调用 action 时，一切类型也都是可以被自动推断出来的。

Action 可以像函数或者通常意义上的方法一样被调用：

```vue
<script setup>
const store = useCounterStore()
// 将 action 作为 store 的方法进行调用
store.randomizeCounter()
</script>
<template>
  <!-- 即使在模板中也可以 -->
  <button @click="store.randomizeCounter()">Randomize</button>
</template>
```

想要使用另一个 store 的话，那你直接在 *action* 中调用就好了：

```javascript
import { useAuthStore } from './auth-store'

export const useSettingsStore = defineStore('settings', {
  state: () => ({
    preferences: null,
    // ...
  }),
  actions: {
    async fetchUserPreferences() {
      const auth = useAuthStore()
      if (auth.isAuthenticated) {
        this.preferences = await fetchPreferences()
      } else {
        throw new Error('User must be authenticated')
      }
    },
  },
})
```

## 模块化

在实际开发中，不可能把多个模块的数据都定义到一个store中，而是一个模块对应一个store，最后通过一个根store进行整合

1.随便建立两个store，并导出

```javascript
// 模块一
import { defineStore } from 'pinia'
 
const useUserStore = defineStore('user', {
  state: () => {
    return {
      name: 'haha',
      age: 18,
    }
  },
})
 
export default useUserStore
 
// 模块二
import { defineStore } from 'pinia'
 
const useCounterStore = defineStore('user', {
  state: () => {
    return {
      count: 1
    }
  },
})
 
export default useUserStore
```

2. 新建store/index.js

```javascript
import useUserStore from './user'
import useCounterStore from './counter'
 
// 统一导出useStore方法
export default function useStore() {
  return {
    user: useUserStore(),
    counter: useCounterStore(),
  }
}
```

3. 组件中使用

```vue
<script setup>
import { storeToRefs } from 'pinia'
import useStore from './store'
const { counter } = useStore()
 
// 使用storeToRefs可以保证解构出来的数据也是响应式的
const { count } = storeToRefs(counter)
</script>
```

