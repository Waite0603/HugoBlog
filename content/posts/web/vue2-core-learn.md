---
title: "Vue2 核心语法"
date: 2022-01-22T16:04:23+08:00
categories: ["Web"]
tags: ["Vue3"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---



> https://v2.cn.vuejs.org/

## 简单使用

> https://v2.cn.vuejs.org/v2/guide/installation.html

[**直接用 script 引入**](https://v2.cn.vuejs.org/v2/guide/installation.html#直接用-lt-script-gt-引入)

1. 直接下载并用 <script> 标签引入，`Vue` 会被注册为一个全局变量。(在开发环境下不要使用压缩版本，不然你就失去了所有常见错误相关的警告!)

2. ### [CDN](https://v2.cn.vuejs.org/v2/guide/installation.html#CDN)

对于制作原型或学习，你可以这样使用最新版本：

```

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

```

对于生产环境，我们推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏：

```

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14"></script>

```

如果你使用原生 ES Modules，这里也有一个兼容 ES Module 的构建文件：

```

<script type="module">

import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.esm.browser.js'

</script>

```

3. 你可以在 [cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue/) 浏览 NPM 包的源代码。

Vue 也可以在 [unpkg](https://unpkg.com/vue@2.7.14/dist/vue.js) 和 [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.7.14/vue.js) 上获取 (cdnjs 的版本更新可能略滞后)。

请确认了解[不同构建版本](https://v2.cn.vuejs.org/v2/guide/installation.html#对不同构建版本的解释)并在你发布的站点中使用**生产环境版本**，把 vue.js 换成 vue.min.js。这是一个更小的构建，可以带来比开发环境下更快的速度体验。

4. 在用 Vue 构建大型应用时推荐使用 NPM 安装[[1\]](https://v2.cn.vuejs.org/v2/guide/installation.html#footnote-1)。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件](https://v2.cn.vuejs.org/v2/guide/single-file-components.html)。

```

# 最新稳定版

$ npm install vue@^2

```

## 响应式与插值

### 响应式数据与插值表达式

```javascript

let value = 'hello world';

document.getElementById('box').innerHTML = value;

// 更改数据, 还要在进行一次 DOM 操作

value = 'hello vue';

document.getElementById('box').innerHTML = value;

```

> 在内部对数据做操作, 会自动更新到页面视图上, 省去了手动操作 DOM 的步骤

```html

<div id="box">

<!-- 1.2 插值表达式 -->

<p>{{ title }}</p>

<p>{{ content }}</p>

<p>{{ title + content }}</p>

<p>{{ title ? content : 'hello' }}</p>

</div>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

<script>

const vm = new Vue({

// 1. 响应式数据与插值表达式

// 在内部对数据做操作, 会自动更新到页面视图上, 省去了手动操作 DOM 的步骤

el: '#box',

// el 选项用于指定一个页面中已存在的 DOM 元素来挂载 Vue 实例

data() {

return {

title: 'hello world',

content: 'hello vue'

}

}

})

// 1.3 通过 vm 实例来访问数据

vm.title = 'hello vue'

</script>

```

## 方法以及计算属性

### Methods 属性

```html

<div id="box">

<p>{{ output() }}</p>

<p>{{ output() }}</p>

<p>{{ output() }}</p>

<p>{{ outputContent }}</p>

<p>{{ outputContent }}</p>

<p>{{ outputContent }}</p>

</div>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

<script>

const vm = new Vue({

el: '#box',

// methods 选项用于定义方法, 该方法可以在插值表达式中使用

methods: {

output() {

console.log('output');

}

},

// 1.3 计算属性, 具有缓存性

computed: {

outputContent() {

console.log('outputContent');

}

}

})

</script>

```

> 以上代码 console 结果

```

output

output

output

outputContent

```

### 计算属性

> 计算属性, Methods 具有缓存性质, 当我们进行第一次计算的时候, 它会将计算结果在内部做一个缓存, 当我们再次调用的时候, 它会直接返回缓存的结果, 不会再次执行函数(**插值不能加括号**)

```html

<div id="box">

<p>{{ outputContent }}</p>

<p>{{ output() }}</p>

<p>{{ outputContent }}</p>

<p>{{ outputContent }}</p>

</div>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

<script>

const vm = new Vue({

el: '#box',

// methods 选项用于定义方法, 该方法可以在插值表达式中使用

data() {

return {

message: 'Hello World'

}

},

methods: {

output() {

this.message = 'Hello Vue';

}

},

computed: {

outputContent() {

console.log(`computed: ${this.message}`);

}

}

})

</script>

```

```

// console 结果

Hello World

Hello Vue

```

## 侦听器

> 监听响应式数据的变化, 自动 Vue 内部实现, 可以参与内部函数变化

```javascript

watch: {

message: function (newVal, oldVal) {

console.log(newVal, oldVal)

}

}

```

## 指令

### 内容指令

```vue

<!-- 内容指令 -->

<p v-text="message"></p>

<p v-html="htmlContent"></p>

<script>

data() {

return {

message: 'Hello World',

htmlContent: '<span style="color: red">Hello World</span>',

}

}

</script>

// Hello World

// Hello World(红色)

```

### 渲染指令

> 区别：v-if 会在判定条件为假时，将元素从 DOM 中移除，v-show 会将元素的 display 属性设置为 none, 一般使用 v-show

```vue

<!-- 渲染指令 -->

<!-- v-if 判定条件为真时，渲染元素 -->

<p v-if="bool">message 存在</p>

<p v-show="bool">message 存在</p>

<p v-for="(item, key, index) in obj">{{ item }} {{ key }} {{ index }}</p>

<script>

data() {

return {

message: 'Hello World',

htmlContent: '<span style="color: red">Hello World</span>',

obj: {

name: '张三',

age: 18

},

bool: true

}

}

</script>

// message 存在

// message 存在

// 张三 name 0

// 18 age 1

```

### 属性指令

> 具体可以看 [https://v2.cn.vuejs.org/v2/guide/class-and-style.html](https://v2.cn.vuejs.org/v2/guide/class-and-style.html), 可以修改标签属性

```vue

<!-- 属性指令 -->

<p v-bind:title="title">message</p>

<!-- 简写 -->

<p :title="title">message</p>

<!-- https://v2.cn.vuejs.org/v2/guide/class-and-style.html -->

```

### 事件指令

```vue

<!-- 事件指令 -->

<button v-on:click="alterMsg">修改 message</button>

<!-- 简写 -->

<button @click="alterMsg">修改 message</button>

```

### 表单指令

> v-model 可以实现双向数据绑定, 即数据的变化会影响视图，视图的变化也会影响数据

```vue

<!-- v-model 可以实现双向数据绑定 -->

<input type="text" v-model="message">

<p>{{ message }}</p>

```

## 修饰符

> 在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

>

> 具体可以看 [https://v2.cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6](https://v2.cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

> 以下为示例

```vue

<!-- 修饰符 -->

<!-- 实现原始内容的替换 -->

<input type="text" v-model="message">

<p v-text="message" style="white-space: pre; "></p>

<!-- 在视图中，HTML中的连续空格会被默认合并为一个空格，并不会完全保留。

所以在代码中的 <p> {{message}}</p> 中，连续的空格会被合并为一个空格，并且在渲染时不会显示多个空格。

可以使用CSS中的 white-space 属性来控制文本的处理方式 -->

<input type="text" v-model.trim="message">

<script>

data() {

return {

message: 'Hello World'

}

}

</script>

```

> 一些基本[修饰符](https://v2.cn.vuejs.org/v2/guide/forms.html#修饰符)

+ [`.lazy`](https://v2.cn.vuejs.org/v2/guide/forms.html#lazy)

在默认情况下，`v-model` 在每次 input 事件触发后将输入框的值与数据进行同步 (除了[上述](https://v2.cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时)。你可以添加 lazy 修饰符，从而转为在 change 事件*之后*进行同步：

```

<!-- 在“change”时而非“input”时更新 -->

<input v-model.lazy="msg">

```

+ [`.number`](https://v2.cn.vuejs.org/v2/guide/forms.html#number)

如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：

```

<input v-model.number="age" type="number">

```

这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。

+ [`.trim`](https://v2.cn.vuejs.org/v2/guide/forms.html#trim)

如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：

```

<input v-model.trim="msg">

```