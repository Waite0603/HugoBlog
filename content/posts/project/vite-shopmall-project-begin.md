---
title: "Vite B端后台管理起始项"
date: 2023-04-05T16:04:23+08:00
categories: ["Project"]
tags: ["Project"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---



## 创建vite项目并安装vscode插件

### Vite 

**Vite 是一个 web 开发构建工具，由于其原生 ES 模块导入方式，可以实现闪电般的冷服务器启动.通过在终端中运行以下命令，可以使用 Vite 快速构建 Vue 项目**

> 兼容性注意
>
> Vite 需要 Node.js 版本 14.18+，16+。然而，有些模板需要依赖更高的 Node 版本才能正常运行，当你的包管理器发出警告时，请注意升级你的 Node 版本。

```batchfile
使用 NPM:
$ npm create vite@latest

使用 Yarn:
$ yarn create vite

使用 PNPM:
$ pnpm create vite
```

然后按照提示操作即可！

你还可以通过附加的命令行选项直接指定项目名称和你想要使用的模板。例如，要构建一个 Vite + Vue 项目，运行:

```
# npm 6.x
npm create vite@latest my-vue-app --template vue

# npm 7+, extra double-dash is needed:
npm create vite@latest my-vue-app -- --template vue

# yarn
yarn create vite my-vue-app --template vue

# pnpm
pnpm create vite my-vue-app --template vue
```

查阅更多可以查看 [Vite](https://cn.vitejs.dev/guide/) and [Vue3](https://cn.vuejs.org/) 官网

![image.png](https://img.waite.wang/images/2023/02/11/image.png)

### 插件安装

1. Vue Language Features (Volar): VueLF 是一个专门为 Vue 3 构建的语言支持插件。它基于@vue/reactivity按需计算一切，实现原生 TypeScript 语言服务级别的性能。
2. Vue 3 Snippets: Vue 代码提示
3. WindiCSS IntelliSense
4. Stylelint: 然后禁用项目的 css 和 scss 验证。 (ctrl+shift+p) 并搜索“设置 json”
   ```json
   "scss.validate": false
   "css.validate": false
   ```
   ```
    在项目根目录stylelint.config.js中创建 stylelint 插件配置文件, 添加以下内容以忽略规则apply, tailwind,etc:
   ```

```javascript
module.exports = {
    rules: {
        'at-rule-no-unknown': [
            true,
            {
                ignoreAtRules: ['tailwind', 'apply', 'variants', 'responsive', 'screen']
            }
        ],
        'declaration-block-trailing-semicolon': null,
        'no-descending-specificity': null
    }
}
```

## [Element-plus](https://element-plus.org/zh-CN/)

https://element-plus.org/zh-CN/

我们建议您使用包管理器（如 NPM、Yarn 或 pnpm）安装 Element Plus，然后您就可以使用打包工具，例如 Vite 或 webpack。

```
# 选择一个你喜欢的包管理器

# NPM
$ npm install element-plus --save

# Yarn
$ yarn add element-plus

# pnpm
$ pnpm install element-plus
```

#### 用法

##### 完整引入

如果你对打包后的文件大小不是很在乎，那么使用完整导入会更方便。

```
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

## [Windi CSS](https://cn.windicss.org/guide/)

https://cn.windicss.org/guide/

- 安装相关包：

```
npm i -D vite-plugin-windicss windicss
```

- 然后，在你的 Vite 配置中添加插件：

```javascript
// vite.config.js
import WindiCSS from 'vite-plugin-windicss'

export default {
  plugins: [
    WindiCSS(),
  ],
}
```

- 最后，在你的 Vite 入口文件中导入 virtual:windi.css：

```javascript
import 'virtual:windi.css'
```

- 使用及 @apply 简化代码

```javascript
<script setup>

</script>

<template>
  <button class='btn'>
    按钮
  </button>
</template>

<style>
  .btn{
    @apply bg-purple-500 text-indigo-50 px-4 py-2 rounded-full transition-all duration-500 hover:( bg-purple-900) focus:(ring-8 ring-purple-900);
  }
</style>
```

## [Vue-router](https://router.vuejs.org/zh/index.html)

https://router.vuejs.org/zh/index.html

```
npm install vue-router@4
yarn add vue-router@4
```

- /router/index.js

```javascript
import {
    createRouter,
    // 函数用来创建一个路由实例
    createWebHashHistory
    // 用来创建一个基于哈希路由的历史对象
} from 'vue-router'

const routes = [{
    path: '/',
    name: 'Home',
    component: () => import('../views/Home.vue')
}]

// 这段代码定义了一个路由器，该路由器使用createWebHashHistory()创建的历史记录对象和给定的路由（routes）来定义路由。
// createWebHashHistory()创建的历史记录对象会把URL的片段存储在window.location.hash中，以支持前进和后退按钮和书签。
const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```

- main.js

```javascript
import router from './router'

const app = createApp(App)

app.use(router)

app.mount('#app')
```

### [捕获所有路由或 404 Not found 路由](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#%E6%8D%95%E8%8E%B7%E6%89%80%E6%9C%89%E8%B7%AF%E7%94%B1%E6%88%96-404-not-found-%E8%B7%AF%E7%94%B1)

常规参数只匹配 url 片段之间的字符，用 / 分隔。如果我们想匹配任意路径，我们可以使用自定义的 路径参数 正则表达式，在 路径参数 后面的括号中加入 正则表达式 :

```javascript
const routes = [
  // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.afterUser` 下
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

## 登陆页面开发

1. [ElementPlus_Layout 布局](https://element-plus.gitee.io/zh-CN/component/layout.html): 通过基础的 24 分栏，迅速简便地创建布局。
2. [Flexbox](https://cn.windicss.org/utilities/layout/flexbox.html#flex): 使用 flex 创建一个块级 flex 容器。

### Layout 响应式布局

参照了 Bootstrap 的 响应式设计，预设了五个响应尺寸：xs、sm、md、lg 和 xl。

```html
<template>
  <el-row :gutter="10">
    <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"
      ><div class="grid-content ep-bg-purple"
    /></el-col>
    <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"
      ><div class="grid-content ep-bg-purple-light"
    /></el-col>
    <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"
      ><div class="grid-content ep-bg-purple"
    /></el-col>
    <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"
      ><div class="grid-content ep-bg-purple-light"
    /></el-col>
  </el-row>
</template>

<style>
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
```

| xs   | <768px 响应式栅格数或者栅格属性对象  | number  /  object |
| ---- | ------------------------------------ | ----------------- |
| sm   | ≥768px 响应式栅格数或者栅格属性对象  | number  /  object |
| md   | ≥992px 响应式栅格数或者栅格属性对象  | number  /  object |
| lg   | ≥1200px 响应式栅格数或者栅格属性对象 | number  /  object |
| xl   | ≥1920px 响应式栅格数或者栅格属性对象 | number  /  object |

3. [icon 图标引入](https://element-plus.gitee.io/zh-CN/component/icon.html)

https://element-plus.gitee.io/zh-CN/component/icon.html

```
# 选择一个你喜欢的包管理器

# NPM
$ npm install @element-plus/icons-vue
# Yarn
$ yarn add @element-plus/icons-vue
# pnpm
$ pnpm install @element-plus/icons-vue
```

> 注册所有图标
>
> 您需要从 @element-plus/icons-vue 中导入所有图标并进行全局注册。

```typescript
// main.ts

// 如果您正在使用CDN引入，请删除下面一行。
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

const app = createApp(App)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

> 使用(插槽)

```javascript
<el-input v-model="form.username" placeholder="请输入用户名">
  <template #prefix>
    <el-icon class="el-input__icon">
      <search />
    </el-icon>
  </template>
</el-input>

import { Calendar, Search } from '@element-plus/icons-vue'
```

4. 外部 icon

[阿里巴巴矢量图库iconfont](https://www.iconfont.cn/)

选择图标加入购物车, 直接下载引用

```javascript
// mian.js
import './assets/icon/iconfont.css'


<el-link type="primary" class="text-white" @click="JumpToGithub">
  <i class="iconfont icon-icon_github mr-1"></i>
  Github
</el-link>
```

### [<script setup>](https://cn.vuejs.org/api/sfc-script-setup.html#script-setup)

setup 是在单文件组件 (SFC) 中使用组合式 API 的编译时语法糖。当同时使用 SFC 与组合式 API 时该语法是默认推荐。相比于普通的 <script> 语法，它具有更多优势：

- 更少的样板内容，更简洁的代码。
- 能够使用纯 TypeScript 声明 props 和自定义事件。
- 更好的运行时性能 (其模板会被编译成同一作用域内的渲染函数，避免了渲染上下文代理对象)。
- 更好的 IDE 类型推导性能 (减少了语言服务器从代码中抽取类型的工作)。

> 1. 普通的 <script> 只在组件被首次引入的时候执行一次不同，<script setup> 中的代码会在每次组件实例被创建的时候执行。
> 2. 当使用 <script setup> 的时候，任何在 <script setup> 声明的顶层的绑定 (包括变量，函数声明，以及 import 导入的内容) 都能在模板中直接使用
> 3. import 导入的内容也会以同样的方式暴露。这意味着我们可以在模板表达式中直接使用导入的 helper 函数，而不需要通过 methods 选项来暴露它

```javascript
<script setup>
// 变量
const msg = 'Hello!'

// 函数
function log() {
  console.log(msg)
}
</script>

<template>
  <button @click="log">{{ msg }}</button>
</template>

<script setup>
import { capitalize } from './helpers'
</script>

<template>
  <div>{{ capitalize('hello') }}</div>
</template>
```

> 以下为简单示例代码

```javascript
<template>
    <ul>
        <li>{{ myref0 }}</li>
        <li>{{ myref1 }}</li>
        <li>{{ myref2 }}</li>
        <li>{{ myref3 }}</li>
        <li>{{ myrea0 }}</li>
        <li>{{ myrea1 }}</li>
        <li>{{ myrea2.age }}</li>
        <li>{{ myrea3.user.name }}</li>
        <li>{{ myrea4.one.three.age.four }}</li>
    </ul>
    <el-button @click="change">change</el-button>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
    setup() {
        let myref0 = ref(1);
        let myref1 = ref(2);
        let myref2 = ref(true);
        let myref3 = ref('myref3')
        let myrea0 = reactive(0);
        let myrea1 = reactive('myrea1');
        let myrea2 = reactive({ age: 3 })
        let myrea3 = reactive({
            user: {
                name: "zs",
            }
        })
        let myrea4 = reactive({
            top: 'top', one: {
                two: "two",
                three: {
                    name: "LS",
                    age: {
                        four: "four"
                    }
                },
            }
        })
        function change() {
            myref0 = 2;
            myref1.value++;
            myref2.value = false;
            myref3.value = 'MyRef333'
            myrea0++;
            myrea1 = 'myrea1111';
            myrea2.age++;
            myrea3.user.name = "ww";
            myrea4.one.three.age.four = "five";
            console.log(myref0);
            console.log(myrea0);
            console.log(myrea1);
        }
        return { myref0, myrea0, myref1, myrea1, myref2, myrea2, myref3, myrea3, myrea4, change }
    }
}
</script>
```

```
1                 =>            1
2                 =>            3
false             =>            flase
MyRef3            =>            MyRef333
0                 =>            0
myrea1            =>            myrea1
3                 =>            4
zs                =>            ww
four              =>            five
```

1. ref包裹简单类型，可以响应数据变化。
2. reactive包裹简单类型，不可以响应数据变化。
3. 在更改ref包裹的值时，需要加 .value 来触发响应。
4. 在页面中使用，{{ }}进行文本插值时，即使使用ref包裹也不需要添加 .value 属性，得益于在检测到 __v_isRef:true 时，vue会帮我们添加。

### [登录表单校验](https://element-plus.gitee.io/zh-CN/component/form.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%A0%A1%E9%AA%8C%E8%A7%84%E5%88%99)

```javascript
<el-form ref="formRef" :model="form" :rules="rules" class="w-2/4">
  <el-form-item prop="username">
    <el-input v-model="form.username" placeholder="请输入用户名">
      <template #prefix>
        <el-icon>
          <User />
        </el-icon>
      </template>
    </el-input>
  </el-form-item>
  <el-form-item prop="password">
    <el-input v-model="form.password" type="password" placeholder="请输入密码" show-password @keyup.enter.native="onSubmit">
      <template #prefix>
        <el-icon>
          <Lock />
        </el-icon>
      </template>
    </el-input>
  </el-form-item>
  <el-form-item>
    <el-button round color="#2563eb " class="w-full" type="primary" @click="onSubmit">登 录</el-button>
  </el-form-item>
</el-form>
```

```javascript
import { reactive, ref } from 'vue'

// do not use same name with ref
const form = reactive({
  username: "",
  password: ""
})

const rules = {
  username: [
    {
      required: true,
      message: '用户名不能为空',
      trigger: 'blur'
    },
  ],
  password: [
    {
      required: true,
      message: '密码不能为空',
      trigger: 'blur'
    },
  ]
}


const formRef = ref(null)
const onSubmit = () => {
  formRef.value.validate((valid) => {
    if (!valid) {
      return false
    }
    console.log("验证通过");
  })
}
```

#### [Axios](http://axios-js.com/)

http://axios-js.com/

```
npm install axios
```

```javascript
// /src/axios.js

import axios from 'axios';


console.log(import.meta.env.VITE_APP_BASE_API);

const instance = axios.create({
    baseURL:import.meta.env.VITE_APP_BASE_API,
})

export default instance;
```

```javascript
// vue.config.js

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import WindiCSS from 'vite-plugin-windicss'

import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  // 它将别名“~”映射到当前目录的“src”文件夹，因此在进行路径引用时，可以使用“~”，而不是使用相对路径。
  resolve: {
    alias: {
      '~': path.resolve(__dirname, 'src'),
    },
  },
  plugins: [
    vue(),
    WindiCSS()
  ],
  server: {
    cors: true,
    proxy: {
      '/api': {
        target: 'https://ceshi13.dishait.cn',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },
  build: {
    sourcemap: false,
    // 消除打包大小超过500kb警告
    chunkSizeWarningLimit: 4000
  },
})
```

```javascript
import axios from '~/axios';

// 通过 export 导出
export const login = (username, password) => {
    return axios.post('/admin/login', {
        username,
        password
    });
};
```

### [Vueuse](https://vueuse.org/)

https://vueuse.org/

把一些原本不支持响应式的 api 等 支持响应式

#### [useCookies](https://vueuse.org/integrations/useCookies/#usage)

```
npm i @vueuse/integrations
npm i universal-cookie
```

```html
<template>
    <div class="container">
        <div class="row">
        <div class="col-12">
            <el-button type="primary" @click="set('test', 'test')">设置cookie</el-button>
            <el-button type="primary" @click="remove('test')">删除cookie</el-button>
            <el-button type="primary" @click="get('test')">读取cookie</el-button>
        </div>
        </div>
    </div>
</template>

<script setup>
    import { useCookies } from "@vueuse/integrations/useCookies";
    
    const TokenKey = "admin-token";
    const cookies = useCookies();
    
    // 获取token
    export function getToken() {
        return cookies.get(TokenKey);
    }
    
    // 设置token
    export function setToken(token) {
        // token 有效期为 60 分钟
        const expires = new Date(Date.now() + 60 * 60 * 1000);
        cookies.set(TokenKey, token, { expires });
    }
    
    // 移除token
    export function removeToken() {
        return cookies.remove(TokenKey);
    }
</script>
```

### [拦截器](http://axios-js.com/zh-cn/docs/#%E6%8B%A6%E6%88%AA%E5%99%A8)

```javascript
import { getToken} from '~/composables/auth'
import { toast } from '~/composables/util'
import axios from 'axios';

// 创建axios实例
const instance = axios.create({
    baseURL: import.meta.env.VITE_APP_BASE_API,
})

// 添加请求拦截器
instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    const token = getToken();

    if (token) {
        config.headers['token'] = token;
    }
    
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});

// 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response.data.data;
}, function (error) {
    // 对响应错误做点什么
    toast(error.response.data.msg, 'error');

    return Promise.reject(error);
});

export default instance;
```

### [Vuex](https://vuex.vuejs.org/zh/index.html)

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

```
npm install vuex@next --save
```

```javascript
// /store/index.js

import { createStore } from 'vuex'

// 创建一个新的 store 实例
const store = createStore({
    state() {
        return {
            // 用户信息
            user: {}
        }
    },
    mutations: {
        // 设置用户信息
        SET_USER_INFO(state, user) {
            state.user = user
        }
    }
});

export default store
```

```javascript
import { useStore } from 'vuex'

const store = useStore();
store.commit('SET_USER_INFO', res2);

<template>
    <div class="about">
        <h1> {{ $store.state.user }}</h1>
    </div>
</template>
```

> 具体使用看[官方文档](https://vuex.vuejs.org/zh/index.html)

### [全局路由守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%AF%BC%E8%88%AA%E5%AE%88%E5%8D%AB)

```javascript
// /src/permission.js

// 处理权限相关
import router from '~/router'
import { getToken } from '~/composables/auth'
import { toast } from '~/composables/util'

// 全局前置守卫
router.beforeEach((to, from, next) => {
    // 如果有token，就放行
    const token = getToken();

    // 如果没有token，就跳转到登录页
    if (!token && to.path !== '/login') {
        toast("请先登录", "warning");
        return next ({ path: "/login" })
    };

    // 防止重复登录
    if (token && to.path === '/login') {
        toast("请先退出登录", "warning");

        return next ({ path: from.path })
    };

    next()
})
```

```javascript
// mian.js

import "./permission"

app.use(store)
```

## [全局 loading](https://www.npmjs.com/package/nprogress)

```javascript
// npm i nprogress
// main.js

import "nprogress/nprogress.css"

// composable/util.js
// 显示全屏loading
export function showFullLoading() {
    nprogress.start()
}

// 隐藏全屏loading
export function hideFullLoading() {
    nprogress.done()
}

```

```javascript
// permission.js

import { toast, showFullLoading, hideFullLoading } from '~/composables/util'

// 全局前置守卫
router.beforeEach(async (to, from, next) => {
    // 显示 loading
    showFullLoading();
}

// 全局后置守卫
router.afterEach(() => {
    // 隐藏 loading
    hideFullLoading();
})
```

### 样式

```css
#nprogress .bar {
    background: wheat !important;
     height: 3px !important;
}
```

## 动态页面标题

```javascript
// router/index.js

{
    path: '/login',
    name: 'Login',
    component: Login,
    meta: {
        title: '登录'
    }
}
```

```javascript
// permission.js

// 全局前置守卫
router.beforeEach(async (to, from, next) => {
  // 设置页面标题
  document.title = to.meta.title;
}
```

## 后台 Layout 开发

### 组件引用

```javascript
// /layout/admin.vue

<template>
    <el-container>
        <el-header>
            <f-header></f-header>
        </el-header>
        <el-container>
            <el-aside width="200px">
                <f-menu></f-menu>
            </el-aside>
            <el-main>
                <f-tag-list></f-tag-list>
                <router-view></router-view>
            </el-main>
        </el-container>
        <el-footer>Footer</el-footer>
    </el-container>
</template>

import FHeader from './components/FHeader.vue';
import FMenu from './components/FMenu.vue';
import FTagList from './components/FTagList.vue';
```

## 头部代码

```javascript
<template>
    <div class="f-header">
        <span class="logo">
            <el-icon class="mr-1"><eleme-filled /></el-icon>
            帝莎编程
        </span>
        <el-icon class="icon-btn">
            <fold />
        </el-icon>
        <el-icon class="icon-btn">
            <refresh />
        </el-icon>
        <div class="ml-auto flex items-center">
            <el-icon class="icon-btn"><full-screen /></el-icon>
            <el-dropdown class="dropdown" @command="handleCommand">
                <span class="flex items-center text-light-50">
                    <el-avatar class="mr-2" :size="25" :src="$store.state.user.avatar" />
                    {{ $store.state.user.username }}
                    <el-icon class="el-icon--right">
                        <arrow-down />
                    </el-icon>
                </span>
                <template #dropdown>
                    <el-dropdown-menu>
                        <el-dropdown-item command="rePwd">修改密码</el-dropdown-item>
                        <el-dropdown-item command="logout">退出登录</el-dropdown-item>
                    </el-dropdown-menu>
                </template>
            </el-dropdown>
        </div>
    </div>
</template>

<style>
.f-header {
    @apply flex items-center bg-indigo-700 text-light-50 fixed top-0 left-0 right-0;
    height: 64px;
}

.logo {
    width: 250px;
    @apply flex justify-center items-center text-xl font-thin;
}

.icon-btn {
    @apply flex justify-center items-center;
    width: 42px;
    height: 64px;
    cursor: pointer;
}

.icon-btn:hover {
    @apply bg-indigo-600;
}

.f-header .dropdown {
    height: 64px;
    cursor: pointer;
    @apply flex justify-center items-center mx-5;
}
</style>
```

### [下拉菜单事件绑定](https://element-plus.gitee.io/zh-CN/component/dropdown.html#dropdown-events)

```javascript
<el-dropdown class="dropdown" @command="handleCommand">
    <span class="flex items-center text-light-50">
        <el-avatar class="mr-2" :size="25" :src="$store.state.user.avatar" />
        {{ $store.state.user.username }}
        <el-icon class="el-icon--right">
            <arrow-down />
        </el-icon>
    </span>
    <template #dropdown>
        <el-dropdown-menu>
            <el-dropdown-item command="rePwd">修改密码</el-dropdown-item>
            <el-dropdown-item command="logout">退出登录</el-dropdown-item>
        </el-dropdown-menu>
    </template>
</el-dropdown>

<script setup>
    const handleCommand = (command)=>{
        console.log(command);
    }
</script>
```

### 页面刷新和全屏

https://vueuse.org/core/useFullscreen/#usefullscreen

```
npm i @vueuse/core
```

```javascript
<el-tooltip effect="dark" content="刷新" placement="bottom">
    <el-icon class="icon-btn" @click="handleRefresh">
        <refresh />
    </el-icon>
</el-tooltip>
<el-tooltip effect="dark" content="全屏" placement="bottom">
    <el-icon class="icon-btn" @click="toggle">
        <full-screen v-if="!isFullscreen" />
        <aim v-else />
    </el-icon>
</el-tooltip>
// full-screen v-if="!isFullscreen"和aim v-else是两个自定义的组件，它们
// 分别代表全屏和非全屏状态下的图标。其中，v-if="!isFullscreen"表示当
// isFullscreen为false时，显示full-screen组件，否则显示aim组件。

<script setup>
    import { useFullscreen } from '@vueuse/core'
    const handleRefresh = ()=>{
        // router.go(0);
        location.reload();
    }

    // const toggle = ()=>{
    //     if (document.fullscreenElement) {
    //         document.exitFullscreen();
    //     } else {
    //         document.documentElement.requestFullscreen();
    //     }
    // }
    const { isFullscreen, toggle } = useFullscreen()
    // isFullscreen 是否全屏
    // toggle 切换全屏
</script>
```

## 通用抽屉组件封装 

### [defineExpose](https://cn.vuejs.org/api/sfc-script-setup.html#defineexpose)

使用 <script setup> 的组件是默认关闭的——即通过模板引用或者 $parent 链获取到的组件的公开实例，不会暴露任何在 <script setup> 中声明的绑定。

可以通过 defineExpose 编译器宏来显式指定在 <script setup> 组件中要暴露出去的属性：

```javascript
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```

当父组件通过模板引用的方式获取到当前组件的实例，获取到的实例会像这样 { a: number, b: number } (ref 会和在普通实例中一样被自动解包)

```javascript
<template>
    <el-drawer v-model="showDrawer" title="修改密码" size="45%" :close-on-click-modal="false">
        <div class="formDrawer">
            <div class="body">
                <slot></slot>
            </div>
            <div class="actions">
                <el-button type="primary">提交</el-button>
                <el-button type="default" @click="close">取消</el-button>
            </div>
        </div>
    </el-drawer>
</template>
<script setup>
    import { ref } from "vue"
    const showDrawer = ref(false)

    // 打开
    const open = ()=> showDrawer.value = true

    // 关闭
    const close = ()=>showDrawer.value = false

    // 向父组件暴露以下方法
    defineExpose({
        open,
        close
    })

</script>
<style>
    .formDrawer{
        width: 100%;
        height: 100%;
        position: relative;
        @apply flex flex-col;
    }

    .formDrawer .body{
        flex: 1;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 50px;
        overflow-y: auto;
    }

    .formDrawer .actions{
        height: 50px;
        @apply mt-auto flex items-center;
    }
</style>
```

### [defineProps() 和 defineEmits()](https://cn.vuejs.org/api/sfc-script-setup.html#using-custom-directives)

为了在声明 props 和 emits 选项时获得完整的类型推导支持，我们可以使用 defineProps 和 defineEmits API，它们将自动地在 <script setup> 中可用：

```javascript
<script setup>
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
// setup 代码
</script>
```

```javascript
<template>
    <el-drawer v-model="showDrawer" :title="title" :size="size" :close-on-click-modal="false"
        :destroy-on-close="destroyOnClose">
        <div class="formDrawer">
            <div class="body">
                <slot></slot>
            </div>
            <div class="actions">
                <el-button type="primary" @click="submit" :loading="loading">{{ submitText }}</el-button>
                <el-button type="default" @click="close">取消</el-button>
            </div>
        </div>
    </el-drawer>
</template>

<script setup>
  // defineProps() 接收父组件传递的参数
  const props = defineProps({
      title: String,
      size: {
          type: String,
          default: '45%'
      },
      // 控制是否在关闭 Drawer 之后将子元素全部销毁
      destroyOnClose: {
          type: Boolean,
          default: true
      },
      // 提交按钮的文字
      submitText: {
          type: String,
          default: '提交'
      },
  })
  
  // 这段代码使用了 Vue 3 中的 defineEmits 函数来声明了一个名为 submit 的事件。然后在 submit 函数中，调用了 emit 函数并传入了
  // 事件名称 'submit'，从而触发了名为 submit 的事件。这样做的好处是可以在组件的模板中使用 v-on:submit 或 @submit 来监听该事件
  // 从而实现组件与父组件之间的通信。
  const emit = defineEmits(['submit'])
  const submit = () => emit('submit')
<script>

<form-drawer ref="formDrawerRef" title="修改密码" size="30%" destroy-on-close @submit="onSubmit" />
```

## 菜单栏

```javascript
<template>
    <div class="f-menu" :style="{ width: $store.state.asideWidth }">
        <el-menu :default-active="defaultActive" unique-opened default-active="2" class="border-0" @select="handleSelect"
            :collapse="isCollapse" :collapse-transition="false">
            <template v-for="(item, index) in asideMenus" :key="index">
                <el-sub-menu v-if="item.child && item.child.length > 0" :index="item.name">
                    <template #title>
                        <el-icon>
                            <component :is="item.icon"></component>
                        </el-icon>
                        <span>{{ item.name }}</span>
                    </template>
                    <el-menu-item v-for="(item2, index2) in item.child" :key="index2" :index="item2.frontpath">
                        <el-icon>
                            <component :is="item2.icon"></component>
                        </el-icon>
                        <span>{{ item2.name }}</span>
                    </el-menu-item>
                </el-sub-menu>

                <el-menu-item v-else :index="item.frontpath">
                    <el-icon>
                        <component :is="item.icon"></component>
                    </el-icon>
                    <span>{{ item.name }}</span>
                </el-menu-item>
            </template>
        </el-menu>
    </div>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { computed, ref } from 'vue';
import { useStore } from 'vuex';

const store = useStore()
const isCollapse = computed(() => store.state.asideWidth != "250px")

// 默认选中当前路由路径
const route = useRoute()
const defaultActive = ref(route.path)


const router = useRouter()
const asideMenus = [
    {
        "name": "后台面板",
        "icon": "help",
        "child": [{
            "name": "主控台",
            "icon": "home-filled",
            "frontpath": "/",
        }]
    },
    {
        "name": "商城管理",
        "icon": "shopping-bag",
        "child": [{
            "name": "商品管理",
            "icon": "shopping-cart-full",
            "frontpath": "/goods/list",
        }],
    },
    {
        "name": "用户管理",
        "icon": "user",
        "frontpath": "/user/list"
    }
]

const handleSelect = (e) => {
    router.push(e)
}
</script>

<style>
.f-menu {
    transition: all 0.2s;
    top: 64px;
    bottom: 0;
    left: 0;
    overflow-y: auto;
    overflow-x: hidden;
    @apply shadow-md fixed bg-light-50;
}

.f-menu::-webkit-scrollbar {
    width: 0px;
}
</style>
```

```javascript
const store = createStore({
  state() {
      return {
          // 用户信息
          user: {},

          // 侧边栏宽度
          asideWidth: "250px",

      }
  },
  mutations: {
      // 设置用户信息
      SET_USER_INFO(state, user) {
          state.user = user
      },
      // 侧边栏展开
      ASIDE_OPEN(state) {
          state.asideWidth = state.asideWidth == "250px" ? "64px" : "250px"
      }
  }
});
```

### [Menu](https://element-plus.gitee.io/zh-CN/component/menu.html)

| 属性名         | 说明                           |
| -------------- | ------------------------------ |
| unique-opened  | 是否只保持一个子菜单的展开     |
| default-active | 页面加载时默认激活菜单的 index |

### 根据菜单添加路由

####  check 路由

https://router.vuejs.org/zh/guide/advanced/dynamic-routing.html#%E6%9F%A5%E7%9C%8B%E7%8E%B0%E6%9C%89%E8%B7%AF%E7%94%B1

Vue Router 提供了两个功能来查看现有的路由：

- router.hasRoute()：检查路由是否存在。
- router.getRoutes()：获取一个包含所有路由记录的数组。
- hasRoute(name): boolean
- Checks if a route with a given name exists

```javascript
import {
    createRouter,
    // 函数用来创建一个路由实例
    createWebHashHistory
    // 用来创建一个基于哈希路由的历史对象
} from 'vue-router'

import Index from '~/pages/index.vue'
import NotFound from '~/pages/404.vue'
import Admin from '~/layouts/admin.vue'
import GoodList from '~/pages/goods/list.vue'
import CategoryList from '~/pages/category/list.vue'

import Login from '~/pages/login/login.vue'

// 默认路由, 所有用户都可以访问
const routes = [
    {
        path: '/',
        name: 'Admin',
        component: Admin,
        meta: {
            title: '后台首页'
        }
    },
    {
        path: '/:pathMatch(.*)*',
        name: 'NotFound',
        component: NotFound,
        meta: {
            title: '404'
        }
    },
    {
        path: '/login',
        name: 'Login',
        component: Login,
        meta: {
            title: '登录'
        }
    }

]

// 动态路由, 用于匹配用户权限添加的路由
const asyncRoutes = [{
    path: "/",
    name: "/",
    component: Index,
    meta: {
        title: "后台首页"
    }
}, {
    path: "/goods/list",
    name: "/goods/list",
    component: GoodList,
    meta: {
        title: "商品管理"
    }
}, {
    path: "/category/list",
    name: "/category/list",
    component: CategoryList,
    meta: {
        title: "分类列表"
    }
}]

// 这段代码定义了一个路由器，该路由器使用createWebHashHistory()创建的历史记录对象和给定的路由（routes）来定义路由。
// createWebHashHistory()创建的历史记录对象会把URL的片段存储在window.location.hash中，以支持前进和后退按钮和书签。
export const router = createRouter({
    history: createWebHashHistory(),
    routes
})

// 用于添加动态路由
export function addRoutes(menus) {
    // 是否有新的路由
    let hasNewRoutes = false
    const findAndAddRoutesByMenu = (menuList) => {
        menuList.forEach(menu => {
            let item = asyncRoutes.find(item => item.path == menu.frontpath)
            // 判断是否已经添加过路由
            if (item && !router.hasRoute(item.path)) {
                router.addRoute('Admin', item)
                hasNewRoutes = true
            }
            if (menu.child && menu.child.length > 0) {
                findAndAddRoutesByMenu(menu.child)
            }
        })
    }

    findAndAddRoutesByMenu(menus)

    return hasNewRoutes
}
```

> 动态路由主要通过两个函数实现。router.addRoute() 和 router.removeRoute()。它们只注册一个新的路由，也就是说，如果新增加的路由与当前位置相匹配，就需要你用 router.push() 或 router.replace() 来手动导航

```javascript
// 全局前置守卫
router.beforeEach(async (to, from, next) => {
    // 如果用户登录了，自动获取用户信息，并存储在vuex当中
    let hasNewRoutes = false
    if (token) {
        let { menus } = await store.dispatch("getinfo")
        // 动态添加路由
        hasNewRoutes = addRoutes(menus);
    }

    // 设置页面标题
    document.title = to.meta.title;

    hasNewRoutes ? next(to.fullPath) : next();
})
```

#### 标签导航方法

```javascript
const tabList = ref([
    {
        title: '后台首页',
        path: "/"
    }
])

// 添加标签导航
const addTab = (tab) => {
    // 判断是否存在
    let notab = tabList.value.findIndex(item => item.path == tab.path) == -1;
    if (notab) {
        tabList.value.push(tab);
    }

    cookies.set("tabList", tabList.value);

}


// 初始化标签导航列表
function initTabList() {
    let tbs = cookies.get("tabList");
    if (tbs) {
        tabList.value = tbs;
    }
}

initTabList()

// onBeforeRouteUpdate 监听路由变化
onBeforeRouteUpdate((to, from) => {
    activeTab.value = to.path;
    addTab({
        title: to.meta.title,
        path: to.path
    });
})

// 页面切换 
const changeTab = (t) => {
    activeTab.value = t;
    router.push(t);
}

// 移除标签导航
const removeTab = (t) => {
    let tabs = tabList.value
    let a = activeTab.value
    if (a == t) {
        tabs.forEach((tab, index) => {
            if (tab.path == t) {
                const nextTab = tabs[index + 1] || tabs[index - 1]
                if (nextTab) {
                    a = nextTab.path
                }
            }
        })
    }

    activeTab.value = a;
    tabList.value = tabList.value.filter(tab => tab.path != t);

    cookies.set("tabList", tabList.value);
}
```

### Keep-alive 页面缓存

https://cn.vuejs.org/guide/built-ins/keep-alive.html#keepalive

> 默认情况下，一个组件实例在被替换掉后会被销毁。这会导致它丢失其中所有已变化的状态——当这个组件再一次被显示时，会创建一个只带有初始状态的新实例。
>
> 在切换时创建新的组件实例通常是有意义的，但在这个例子中，我们的确想要组件能在被“切走”的时候保留它们的状态。要解决这个问题，我们可以用 <KeepAlive> 内置组件将这些动态组件包装起来：

```javascript
<!-- 非活跃的组件将会被缓存！ -->
<KeepAlive>
  <component :is="activeComponent" />
</KeepAlive>
```

### transition全局过渡动画

https://cn.vuejs.org/guide/built-ins/transition.html

> 每个子页面只能有一个 <div> 根结点

https://animate.style/ 动画效果！

https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.css
    

## 权限控制 V-permission

### [自定义指令](https://cn.vuejs.org/guide/reusability/custom-directives.html#custom-directives)

```javascript
// directives\permission.js
import store from "~/store"

function hasPermission(value, el = false) {
    if (!Array.isArray(value)) {
        throw new Error(`需要配置权限，例如 v-permission="['getStatistics3,GET']"`)
    }
    const hasAuth = value.findIndex(v => store.state.ruleNames.includes(v)) != -1
    if (el && !hasAuth) {
        // 拿到当前元素的父元素，然后删除当前元素
        el.parentNode && el.parentNode.removeChild(el)
    }
    return hasAuth
}

export default {
    install(app) {
        app.directive("permission", {
            // el: 指令所绑定的元素，可以用来直接操作 DOM 。
            // binding: 传递给指令的值，例如：v-my-directive="1 + 1" 中，参数为 2。
            mounted(el, binding) {
                hasPermission(binding.value, el)
            }
        })
    }
}
```

```javascript
// main.js
import permission from '~/directives/permission'

const app = createApp(App)
app.use(permission)

app.mount('#app')
```

**运用**

```vue
// 直接调用 directives\permission.js 中方法判断 不存在删除元素
<el-col v-permission="['getStatistics3,GET']" :span="12" :offset="0">               
    <IndexChart />
</el-col>
```

## 统计面板

- Element Plus 提示插件： Element Plus Snippets

- [Layout 布局](https://element-plus.org/zh-CN/component/layout.html#%E5%88%86%E6%A0%8F%E9%97%B4%E9%9A%94)
- [卡片](https://element-plus.org/zh-CN/component/card.html#%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95)
- [tag 标签](https://element-plus.org/zh-CN/component/tag.html)

### [骨架屏组件](https://element-plus.org/zh-CN/component/skeleton.html)

> 在需要等待加载内容的位置设置一个骨架屏，某些场景下比 Loading 的视觉效果更好。

### 数字滚动动画

> [gsap](https://www.npmjs.com/package/gsap)

1. 封装组件 /components/CountTo.vue

```vue
<template>
    <!-- toFix 保留两位 限制加载小数点过多 -->
    {{ d.num.toFixed(2) }}
</template>

<script setup>
import { reactive, watch } from 'vue';
import gsap from 'gsap'

const props = defineProps({
    value: {
        type: Number,
        default: 0
    }
})

const d = reactive({
    num: 0
})

function AnimateToValue() {
    gsap.to(d, {
        duration: 0.5,
        num: props.value
    })
}

AnimateToValue()

// watch 是 Vue.js 3 中的一个 API，用于监听变量或者对象的变化，并在变化时执行回调函数。
watch(() => props.value, () =>
    AnimateToValue()
)
</script>
```

2. 应用以及使用 

```javascript
import CountTo from "~/components/CountTo.vue";

<span class="text-3xl font-bold text-gray-500">
    <CountTo :value="item.value" />
</span>
```

### Echarts

#### 初步实现

```vue
<template>
    <el-card shadow="never" class="mt-5">
        <template #header>
            <div class="flex justify-between">
                <span class="text-sm">订单统计</span>
                <div>
                    <el-check-tag v-for="(item, index) in options" :key="index" :checked="current == item.value"
                        style="margin-right: 8px" @click="handleChoose(item.value)">{{ item.text }}</el-check-tag>
                </div>
            </div>
        </template>
        <div id="chart" style="width: 100%;height: 300px;"></div>
    </el-card>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';

import {
    getStatistics3
} from "~/api/index.js"


const current = ref("week")
const options = [{
    text: "近1个月",
    value: "month"
}, {
    text: "近1周",
    value: "week"
}, {
    text: "近24小时",
    value: "hour"
}]

const handleChoose = (type) => {
    current.value = type;
    getData();
}

var myChart = null
onMounted(() => {
    var chartDom = document.getElementById('chart');
    myChart = echarts.init(chartDom);

    getData()
})

function getData() {
    var option;

    option = {
        xAxis: {
            type: 'category',
            data: []
        },
        yAxis: {
            type: 'value'
        },
        series: [
            {
                data: [],
                type: 'bar',
                showBackground: true,
                backgroundStyle: {
                    color: 'rgba(180, 180, 180, 0.2)'
                }
            }
        ]
    };

    // option && myChart.setOption(option);
    myChart.showLoading()
    getStatistics3(current.value).then(res=>{
        option.xAxis.data = res.x
        option.series[0].data = res.y

        myChart.setOption(option)
    }).finally(()=>{
        myChart.hideLoading()
    })
}

</script>
```

> [loading 动画](https://echarts.apache.org/handbook/zh/how-to/data/dynamic-data/#loading-%E5%8A%A8%E7%94%BB) 

```vue
myChart.showLoading()
myChart.hideLoading()
```

> 在页面销毁之前释放图表 不然可能出现白屏现象
>
> https://echarts.apache.org/handbook/zh/concepts/chart-size/#%E5%AE%B9%E5%99%A8%E8%8A%82%E7%82%B9%E8%A2%AB%E9%94%80%E6%AF%81%E4%BB%A5%E5%8F%8A%E8%A2%AB%E9%87%8D%E5%BB%BA%E6%97%B6

+ 假设页面中存在多个标签页，每个标签页都包含一些图表。当选中一个标签页的时候，其他标签页的内容在 DOM 中被移除了。这样，当用户再选中这些标签页的时候，就会发现图表“不见”了。
+ 本质上，这是由于图表的容器节点被移除导致的。即使之后该节点被重新添加，图表所在的节点也已经不存在了。
+ 正确的做法是，在图表容器被销毁之后，调用 [`echartsInstance.dispose`](https://echarts.apache.org/api.html#echartsInstance.dispose) 销毁实例，在图表容器重新被添加后再次调用 [echarts.init](https://echarts.apache.org//api.html#echarts.init) 初始化。

```javascript
import { ref, onMounted, onBeforeUnmount } from 'vue';

onBeforeUnmount(()=>{
    if(myChart) echarts.dispose(myChart)
})
```

#### 图表跟随画面变化

+ https://vueuse.org/core/useResizeObserver/#useresizeobserver
+ https://echarts.apache.org/handbook/zh/concepts/chart-size/#%E4%B8%BA%E5%9B%BE%E8%A1%A8%E8%AE%BE%E7%BD%AE%E7%89%B9%E5%AE%9A%E7%9A%84%E5%A4%A7%E5%B0%8F

```vue
<div ref="el" id="chart" style="width: 100%;height: 300px;"></div>

<script setup>
import { useResizeObserver } from '@vueuse/core';
    
const el = ref(null);

useResizeObserver(el, (entries) => {
    const entry = entries[0];
    const { width, height } = entry.contentRect;
    myChart.resize({ width, height });
});
</script>

```

#### 页面变化监听

```javascript
//使用防抖 (setTimeout定时器)
const getWindowInfo = () => {
	const windowInfo = {
		width: window.innerWidth,
		hight: window.innerHeight
	}
};
const debounce = (fn, delay) => {
	let timer;
	return function() {
		if (timer) {
			clearTimeout(timer);
		}
		timer = setTimeout(() => {
			fn();
		}, delay);
	}
};
//触发事件    触发时间（指定时间内执行事件）
const cancalDebounce = debounce(getWindowInfo, 500);
window.addEventListener('resize', cancalDebounce);

// vue3 使用生命周期销毁钩子
onUnmounted(() => {
    console.log('onUnmounted','打印是否生效');
    //移除监听事件
    window.removeEventListener('resize', cancalDebounce);
})

```

> 样例

```vue
<template>
    <div ref="el">
        <el-container class="bg-white rounded" :style="{ height: h + 'px' }">
            <el-header>Header</el-header>
            <el-container>
                <el-aside width="200px">Aside</el-aside>
                <el-main>Main</el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script setup>
import { watch, ref, onUnmounted } from 'vue';
import { useResizeObserver } from '@vueuse/core';

// 获取屏幕高度
let windowHeight = window.innerHeight || document.documentElement.clientHeight
// 计算表格高度: 屏幕高度 - 头部高度 - 页签高度 - padding
const h = ref(windowHeight - 60 - 44 - 40);


//使用防抖 (setTimeout定时器)
const getWindowInfo = () => {
    const windowInfo = {
        width: window.innerWidth,
        hight: window.innerHeight
    }
    // 改变 h 的值
    h.value = windowInfo.hight - 60 - 44 - 40;
};
const debounce = (fn, delay) => {
    let timer;
    return function () {
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            fn();
        }, delay);
    }
};
// 触发事件    触发时间（指定时间内执行事件）
const cancalDebounce = debounce(getWindowInfo, 500);
window.addEventListener('resize', cancalDebounce);

// 销毁监听
onUnmounted(() => {
    //移除监听事件
    window.removeEventListener('resize', cancalDebounce);
});

</script>
```