---
title: "系统学习 Vue -- 4. 动画和路由"
date: 2023-03-11T21:05:54+08:00
lastmod: 2023-03-22T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue"]
author: "Waite Wang"
showToc: false
TocOpen: true
draft: false
hidemeta: false
description: "Vue 学习笔记-Vue 动画和路由"
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

## Vue 过渡与动画初体验

### 认识动画

- 在开发中，我们想要给一个组件的显示和消失添加某种过渡动画，可以很好的增加用户体验：
  - React框架本身并没有提供任何动画相关的API，所以在React中使用过渡动画我们需要使用一个第三方库 react-transition-group；
  - Vue中为我们提供一些内置组件和对应的API来完成动画，利用它们我们可以方便的实现过渡动画效果；
- 我们来看一个案例：
  - Hello World的显示和隐藏；
  - 通过下面的代码实现，是不会有任何动画效果的；

```vue
<template>
  <div>
    <button @click="toggle">显示/隐藏</button>
    <h2 v-if="show">App组件</h2>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        show: true
      }
    },
    methods: {
      toggle() {
        this.show = !this.show;
      }
    }
  }
</script>
```

- 没有动画的情况下，整个内容的显示和隐藏会非常的生硬：
  - 如果我们希望给单元素或者组件实现过渡动画，可以使用 `transition`内置组件来完成动画；

### Vue的 transition 动画

> <https://cn.vuejs.org/guide/built-ins/transition.html#transition>

`<Transition>` 会在一个元素或组件进入和离开 DOM 时应用动画。

`<Transition>` 是一个内置组件，这意味着它在任意别的组件中都可以被使用，无需注册。它可以将进入和离开动画应用到通过默认插槽传递给它的元素或组件上。进入或离开可以由以下的条件之一触发：

- 由 `v-if` 所触发的切换
- 由 `v-show` 所触发的切换
- 由特殊元素 `<component>` 切换的动态组件
- 改变特殊的 `key` 属性

以下是最基本用法的示例：

```vue
<button @click="show = !show">Toggle</button>
<Transition>
  <p v-if="show">hello</p>
</Transition>
```

```css
/* 下面我们会解释这些 class 是做什么的 */
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
```

> `<Transition>` 仅支持单个元素或组件作为其插槽内容。如果内容是一个组件，这个组件必须仅有一个根元素。

当一个 `<Transition>` 组件中的元素被插入或移除时，会发生下面这些事情：

1. Vue 会自动检测目标元素是否应用了 CSS 过渡或动画。如果是，则一些 [CSS 过渡 class](https://cn.vuejs.org/guide/built-ins/transition.html#transition-classes) 会在适当的时机被添加和移除。
2. 如果有作为监听器的 [JavaScript 钩子](https://cn.vuejs.org/guide/built-ins/transition.html#javascript-hooks)，这些钩子函数会在适当时机被调用。
3. 如果没有探测到 CSS 过渡或动画、也没有提供 JavaScript 钩子，那么 DOM 的插入、删除操作将在浏览器的下一个动画帧后执行。

#### 基于 CSS 的过渡效果

##### [CSS 过渡 class](https://cn.vuejs.org/guide/built-ins/transition.html#transition-classes)

一共有 6 个应用于进入与离开过渡效果的 CSS class。

![过渡图示](https://qiniu.waite.wang/202312111414504.png)

1. `v-enter-from`：进入动画的起始状态。在元素插入之前添加，在元素插入完成后的下一帧移除。
2. `v-enter-active`：进入动画的生效状态。应用于整个进入动画阶段。在元素被插入之前添加，在过渡或动画完成之后移除。这个 class 可以被用来定义进入动画的持续时间、延迟与速度曲线类型。
3. `v-enter-to`：进入动画的结束状态。在元素插入完成后的下一帧被添加 (也就是 `v-enter-from` 被移除的同时)，在过渡或动画完成之后移除。
4. `v-leave-from`：离开动画的起始状态。在离开过渡效果被触发时立即添加，在一帧后被移除。
5. `v-leave-active`：离开动画的生效状态。应用于整个离开动画阶段。在离开过渡效果被触发时立即添加，在过渡或动画完成之后移除。这个 class 可以被用来定义离开动画的持续时间、延迟与速度曲线类型。
6. `v-leave-to`：离开动画的结束状态。在一个离开动画被触发后的下一帧被添加 (也就是 `v-leave-from` 被移除的同时)，在过渡或动画完成之后移除。

##### 为过渡效果命名

我们可以给 `<Transition>` 组件传一个 `name` prop 来声明一个过渡效果名：

```vue
<Transition name="fade">
  ...
</Transition>
```

对于一个有名字的过渡效果，对它起作用的过渡 class 会以其名字而不是 `v` 作为前缀。比如，上方例子中被应用的 class 将会是 `fade-enter-active` 而不是 `v-enter-active`。这个“fade”过渡的 class 应该是这样：

```css
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
```

##### CSS 的 transition

`<Transition>` 一般都会搭配[原生 CSS 过渡](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)一起使用，正如你在上面的例子中所看到的那样。这个 `transition` CSS 属性是一个简写形式，使我们可以一次定义一个过渡的各个方面，包括需要执行动画的属性、持续时间和[速度曲线](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function)。

下面是一个更高级的例子，它使用了不同的持续时间和速度曲线来过渡多个属性：

```vue
<Transition name="slide-fade">
  <p v-if="show">hello</p>
</Transition>
```

```css
/*
  进入和离开动画可以使用不同
  持续时间和速度曲线。
*/
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
```

### CSS 的 animation

> 帧动画, 可以指定在什么时间是什么状态!

[原生 CSS 动画](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)和 CSS transition 的应用方式基本上是相同的，只有一点不同，那就是 `*-enter-from` 不是在元素插入后立即移除，而是在一个 `animationend` 事件触发时被移除。

对于大多数的 CSS 动画，我们可以简单地在 `*-enter-active` 和 `*-leave-active` class 下声明它们。下面是一个示例：

```vue
<Transition name="bounce">
  <p v-if="show" style="text-align: center;">
    Hello here is some bouncy text!
  </p>
</Transition>
```

```css
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}
```

### [自定义过渡 class](https://cn.vuejs.org/guide/built-ins/transition.html#custom-transition-classes)

你也可以向 `<Transition>` 传递以下的 props 来指定自定义的过渡 class：

- `enter-from-class`
- `enter-active-class`
- `enter-to-class`
- `leave-from-class`
- `leave-active-class`
- `leave-to-class`

他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如  Animate.css.

你传入的这些 class 会覆盖相应阶段的默认 class 名。这个功能在你想要在 Vue 的动画机制下集成其他的第三方 CSS 动画库时非常有用，比如 [Animate.css](https://daneden.github.io/animate.css/)：

```vue
<script>
export default {
  data() {
    return {
      show: true
    }
  }
}
</script>

<template>
 <button @click="show = !show">Toggle</button>
  <Transition
    name="custom-classes"
    enter-active-class="animate__animated animate__tada"
    leave-active-class="animate__animated animate__bounceOutRight"
  >
    <p v-if="show">hello</p>
  </Transition>
</template>

<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css";
</style>
```

### 同时设置过渡和动画

- Vue为了知道过渡的完成，内部是在监听 `transitionend`或 `animationend`，到底使用哪一个取决于元素应用的 CSS规则：
  - 如果我们只是使用了其中的一个，那么Vue能自动识别类型并设置监听；
- 但是如果我们同时使用了过渡和动画呢？
  - 并且在这个情况下可能某一个动画执行结束时，另外一个动画还没有结束；
  - 在这种情况下，我们可以设置 type 属性为 animation 或者 transition 来明确的告知Vue监听的类型；

```vue
<Transition type="animation">...</Transition>
```

### [深层级过渡与显式过渡时长](https://cn.vuejs.org/guide/built-ins/transition.html#nested-transitions-and-explicit-transition-durations)

尽管过渡 class 仅能应用在 `<Transition>` 的直接子元素上，我们还是可以使用深层级的 CSS 选择器，在深层级的元素上触发过渡效果。

```vue
<Transition name="nested">
  <div v-if="show" class="outer">
    <div class="inner">
      Hello
    </div>
  </div>
</Transition>
```

```css
/* 应用于嵌套元素的规则 */
.nested-enter-active .inner,
.nested-leave-active .inner {
  transition: all 0.3s ease-in-out;
}

.nested-enter-from .inner,
.nested-leave-to .inner {
  transform: translateX(30px);
  opacity: 0;
}

/* ... 省略了其他必要的 CSS */
```

我们甚至可以在深层元素上添加一个过渡延迟，从而创建一个带渐进延迟的动画序列：

```css
/* 延迟嵌套元素的进入以获得交错效果 */
.nested-enter-active .inner {
  transition-delay: 0.25s;
}
```

然而，这会带来一个小问题。默认情况下，`<Transition>` 组件会通过监听过渡根元素上的**第一个** `transitionend` 或者 `animationend` 事件来尝试自动判断过渡何时结束。而在嵌套的过渡中，期望的行为应该是等待所有内部元素的过渡完成。

在这种情况下，你可以通过向 `<Transition>` 组件传入 `duration` prop 来显式指定过渡的持续时间 (以毫秒为单位)。总持续时间应该匹配延迟加上内部元素的过渡持续时间：

```css
<Transition :duration="550">...</Transition>
```

如果有必要的话，你也可以用对象的形式传入，分开指定进入和离开所需的时间：

```vue
<Transition :duration="{ enter: 500, leave: 800 }">...</Transition>
```

### [过渡模式](https://cn.vuejs.org/guide/built-ins/transition.html#transition-modes)

在之前的例子中，进入和离开的元素都是在同时开始动画的，因此我们不得不将它们设为 `position: absolute` 以避免二者同时存在时出现的布局问题。

然而，很多情况下这可能并不符合需求。我们可能想要先执行离开动画，然后在其完成**之后**再执行元素的进入动画。手动编排这样的动画是非常复杂的，好在我们可以通过向 `<Transition>` 传入一个 `mode` prop 来实现这个行为：

```vue
<Transition mode="out-in">
  ...
</Transition>
```

`<Transition>` 也支持 `mode="in-out"`，虽然这并不常用。

`<Transition>` 也可以作用于[动态组件](https://cn.vuejs.org/guide/essentials/component-basics.html#dynamic-components)之间的切换：

```vue
<Transition name="fade" mode="out-in">
  <component :is="activeComponent"></component>
</Transition>
```

#### [出现时过渡](https://cn.vuejs.org/guide/built-ins/transition.html#transition-on-appear)

如果你想在某个节点初次渲染时应用一个过渡效果，你可以添加 `appear` prop：

```vue
<Transition appear>
  ...
</Transition>
```

#### [元素间过渡](https://cn.vuejs.org/guide/built-ins/transition.html#transition-between-elements)

除了通过 `v-if` / `v-show` 切换一个元素，我们也可以通过 `v-if` / `v-else` / `v-else-if` 在几个组件间进行切换，只要确保任一时刻只会有一个元素被渲染即可：

```vue
<Transition>
  <button v-if="docState === 'saved'">Edit</button>
  <button v-else-if="docState === 'edited'">Save</button>
  <button v-else-if="docState === 'editing'">Cancel</button>
</Transition>
```

#### 示例

```vue
<template>
  <div class="app">
    <div><button @click="isShow = !isShow">显示/隐藏</button></div>

    <transition name="why" mode="out-in" appear>
      <component :is="isShow ? 'home': 'about'"></component>
    </transition>
  </div>
</template>

<script>
  import Home from './pages/Home.vue';
  import About from './pages/About.vue';

  export default {
    components: {
      Home,
      About
    },
    data() {
      return {
        isShow: true
      }
    }
  }
</script>

<style scoped>
  .app {
    width: 200px;
    margin: 0 auto;
  }

  .title {
    display: inline-block;
  }

  .why-enter-from,
  .why-leave-to {
    opacity: 0;
  }

  .why-enter-active,
  .why-leave-active {
    transition: opacity 1s ease;
  }

  .why-enter-active {
    animation: bounce 1s ease;
  }

  .why-leave-active {
    animation: bounce 1s ease reverse;
  }

  @keyframes bounce {
    0% {
      transform: scale(0)
    }

    50% {
      transform: scale(1.2);
    }

    100% {
      transform: scale(1);
    }
  }
</style>
```

### JavaScript 钩子

你可以通过监听 `<Transition>` 组件事件的方式在过渡过程中挂上钩子函数：

```html
<Transition
  @before-enter="onBeforeEnter"
  @enter="onEnter"
  @after-enter="onAfterEnter"
  @enter-cancelled="onEnterCancelled"
  @before-leave="onBeforeLeave"
  @leave="onLeave"
  @after-leave="onAfterLeave"
  @leave-cancelled="onLeaveCancelled"
>
  <!-- ... -->
</Transition>
```

```javascript
export default {
  // ...
  methods: {
    // 在元素被插入到 DOM 之前被调用
    // 用这个来设置元素的 "enter-from" 状态
    onBeforeEnter(el) {},

    // 在元素被插入到 DOM 之后的下一帧被调用
    // 用这个来开始进入动画
    onEnter(el, done) {
      // 调用回调函数 done 表示过渡结束
      // 如果与 CSS 结合使用，则这个回调是可选参数
      done()
    },

    // 当进入过渡完成时调用。
    onAfterEnter(el) {},
    onEnterCancelled(el) {},

    // 在 leave 钩子之前调用
    // 大多数时候，你应该只会用到 leave 钩子
    onBeforeLeave(el) {},

    // 在离开过渡开始时调用
    // 用这个来开始离开动画
    onLeave(el, done) {
      // 调用回调函数 done 表示过渡结束
      // 如果与 CSS 结合使用，则这个回调是可选参数
      done()
    },

    // 在离开过渡完成、
    // 且元素已从 DOM 中移除时调用
    onAfterLeave(el) {},

    // 仅在 v-show 过渡中可用
    onLeaveCancelled(el) {}
  }
}
```

```vue
<template>
  <div class="app">
    <div><button @click="isShow = !isShow">显示/隐藏</button></div>

    <transition @before-enter="beforeEnter"
                @enter="enter"
                @after-enter="afterEnter"
                @before-leave="beforeLeave"
                @leave="leave"
                @afterLeave="afterLeave">
      <h2 class="title" v-if="isShow">Hello World</h2>
    </transition>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        isShow: true
      }
    },
    methods: {
      beforeEnter() {
        console.log("beforeEnter");
      },
      enter() {
        console.log("enter");
      },
      afterEnter() {
        console.log("afterEnter");
      },
      beforeLeave() {
        console.log("beforeLeave");
      },
      leave() {
        console.log("leave");
      },
      afterLeave() {
        console.log("afterLeave");
      }
    }
  }
</script>

<style scoped>
  .title {
    display: inline-block;
  }
</style>
```

这些钩子可以与 CSS 过渡或动画结合使用，也可以单独使用。

在使用仅由 JavaScript 执行的动画时，最好是添加一个 `:css="false"` prop。这显式地向 Vue 表明可以跳过对 CSS 过渡的自动探测。除了性能稍好一些之外，还可以防止 CSS 规则意外地干扰过渡效果。

```vue
<Transition
  ...
  :css="false"
>
  ...
</Transition>
```

在有了 `:css="false"` 后，我们就自己全权负责控制什么时候过渡结束了。这种情况下对于 `@enter` 和 `@leave` 钩子来说，回调函数 `done` 就是必须的。否则，钩子将被同步调用，过渡将立即完成。

这里是使用 [GreenSock 库](https://greensock.com/)执行动画的一个示例，你也可以使用任何你想要的库，比如 [Anime.js](https://animejs.com/) 或者 [Motion One](https://motion.dev/)。

### 第三方库的使用

#### animate.css

> <https://animate.style/>

- 如果我们手动一个个来编写这些动画，那么效率是比较低的，所以在开发中我们可能会引用一些第三方库的动画库， 比如animate.css。
- Animate.css是一个已经**准备好的、跨平台的**动画库为我们的web项目，对于强调、主页、滑动、注意力引导 非常有用；

**如何使用?**

- 安装animate.css：

  ```less
  npm install animate.css 
  ```

- 在main.js中导入animate.css：

  ```javascript
  import { createApp } from 'vue'
  import App from './test/App.vue'
  import 'animate.css'
  
  createApp(App).mount('#app')
  ```

- 接下来在使用的时候我们有两种用法：

  - 用法一：直接使用animate库中定义的 keyframes 动画；
  - 用法二：直接使用animate库提供给我们的类；

- 使用参考: [自定义过渡 class](https://cn.vuejs.org/guide/built-ins/transition.html#custom-transition-classes)

```vue
<transition enter-active-class="animate__animated animate__backInDown"
            leave-active-class="animate__animated animate__bounceOutDown">
  <h2 class="title" v-if="isShow">Hello World</h2>
</transition>
```

#### gsap

> <https://gsap.com/docs/v3/Installation/>
>
> <https://gsap.framer.wiki/stated> -- 中文文档

- 某些情况下我们希望通过JavaScript来实现一些动画的效果，这个时候我们可以选择使用gsap库来完成。
- 什么是`gsap`呢？
  - `GSAP`是`The GreenSock Animation`
  - `latform`（`GreenSock`动画平台）的缩写；
- 它可以通过JavaScript为CSS属性、SVG、Canvas等设置动画，并且是浏览器兼容的；

**如何使用**

- 安装

```less
npm install gsap
```

- 调用 api

```vue
<template>
  <div class="app">
    <div><button @click="isShow = !isShow">显示/隐藏</button></div>

    <transition @enter="enter"
                @leave="leave"
                :css="false">
      <h2 class="title" v-if="isShow">Hello World</h2>
    </transition>
  </div>
</template>

<script>
  import gsap from 'gsap';

  export default {
    data() {
      return {
        isShow: true,
      }
    },
    methods: {
      enter(el, done) {
        console.log("enter");
        gsap.from(el, {
          scale: 0,
          x: 200,
          onComplete: done
        })
      },
      leave(el, done) {
        console.log("leave");
        gsap.to(el, {
          scale: 0,
          x: 200,
          onComplete: done
        })
      }
    }
  }
</script>

<style scoped>
  .title {
    display: inline-block;
  }
</style>
```

- `enter`函数接收两个参数：`el`和`done`。`el`是正在进入的元素，`done`是一个在进入过渡完成时应该被调用的函数。
- `onComplete: done`部分在动画完成时调用`done`函数。这是必要的，以便让Vue知道过渡已经完成。如果不调用`done`，Vue可能无法正确地确定过渡的结束，这可能会导致意外的行为。
- 在使用仅由 JavaScript 执行的动画时，最好是添加一个 `:css="false"` prop。这显式地向 Vue 表明可以跳过对 CSS 过渡的自动探测。除了性能稍好一些之外，还可以防止 CSS 规则意外地干扰过渡效果。

#### gsap 数字递增效果

```vue
<template>
  <div class="app">
    <input type="number" step="100" v-model="counter">
    <h2>当前计数: {{showNumber.toFixed(0)}}</h2>
  </div>
</template>

<script>
import { gsap } from 'gsap';

export default {
  data() {
    return {
      counter: 0,
      showNumber: 0
    };
  },
  watch: {
    counter(newValue) {
      gsap.to(this, { duration: 1, showNumber: newValue });
    }
  }
};
</script>
```

### 认识列表的过渡

- 目前为止，过渡动画我们只要是针对单个元素或者组件的：
  - 要么是单个节点；
  - 要么是同一时间渲染多个节点中的一个；
- 那么如果希望渲染的是一个列表，并且该列表中添加删除数据也希望有动画执行呢？
  - 这个时候我们要使用 组件来完成；
- 使用 有如下的特点：
  - 默认情况下，它不会渲染一个元素的包裹器，但是你可以指定一个元素并以 tag attribute 进行渲染；
  - 过渡模式不可用，因为我们不再相互切换特有的元素；
  - 内部元素总是需要提供唯一的 key attribute 值；
  - CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身；

#### 基本使用

```vue
<template>
  <div>
    <button @click="addNum">添加数字</button>
    <button @click="removeNum">删除数字</button>
    <button @click="shuffleNum">数字洗牌</button>

    <transition-group tag="p" name="why">
      <span v-for="item in numbers" :key="item" class="item">
        {{item}}
      </span>
    </transition-group>
  </div>
</template>

<script>
  import _ from 'lodash';

  export default {
    data() {
      return {
        numbers: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
        numCounter: 10
      }
    },
    methods: {
      addNum() {
        // this.numbers.push(this.numCounter++)
        this.numbers.splice(this.randomIndex(), 0, this.numCounter++)
      },
      removeNum() {
        this.numbers.splice(this.randomIndex(), 1)
      },
      shuffleNum() {
        this.numbers = _.shuffle(this.numbers);
      },
      randomIndex() {
        return Math.floor(Math.random() * this.numbers.length)
      }
    },
  }
</script>

<style scoped>
  .item {
    margin-right: 10px;
    display: inline-block;
    /* 设置了元素为内联块级元素。 */ 
  }

  .why-enter-from,
  .why-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }

  .why-enter-active,
  .why-leave-active {
    transition: all 1s ease;
  }

  .why-leave-active {
    position: absolute;
  }

  .why-move {
    transition: transform 1s ease;
  }
</style>
```

`methods`对象定义了四个方法：

- `addNum`方法在`numbers`数组的随机位置插入一个新的数字，然后`numCounter`加1。
- `removeNum`方法从`numbers`数组的随机位置删除一个数字。
- `shuffleNum`方法使用lodash的`shuffle`函数打乱`numbers`数组的顺序。
- `randomIndex`方法返回一个`numbers`数组的随机索引

#### 列表过渡的移动动画

在上面的案例中虽然新增的或者删除的节点是有动画的，但是对于哪些其他需要移动的节点是没有动画的：

- 我们可以通过使用一个新增的 v-move 的class来完成动画；
- 它会在元素改变位置的过程中应用；
- 像之前的名字一样，我们可以通过name来自定义前缀;

> `<TransitionGroup>` 支持通过 CSS transform 控制移动效果。当一个子节点在屏幕上的位置在更新之后发生变化时，它会被添加一个使其位移的 CSS class (基于 `name` attribute 推导，或使用 `move-class` prop 显式配置)。如果使其位移的 class 被添加时 CSS 的 `transform` 属性是“可过渡的”，那么该元素会基于 [FLIP 技巧](https://aerotwist.com/blog/flip-your-animations/)平滑地到达动画终点。
>
> <https://cn.vuejs.org/api/built-in-components.html#transitiongroup>

```css
.why-move {
    transition: transform 1s ease;
}
```

#### 演练 => 列表的交替动画

```vue
<template>
  <div>
    <input v-model="keyword">
    <transition-group tag="ul" name="why" :css="false"
                      @before-enter="beforeEnter"
                      @enter="enter"
                      @leave="leave">
      <li v-for="(item, index) in showNames" :key="item" :data-index="index">
        {{item}}
      </li>
    </transition-group>
  </div>
</template>

<script>
  import gsap from 'gsap';

  export default {
    data() {
      return {
        names: ["abc", "cba", "nba", "why", "lilei", "hmm", "kobe", "james"],
        keyword: ""
      }
    },
    computed: {
      showNames() {
        return this.names.filter(item => item.indexOf(this.keyword) !== -1)
      }
    },
    methods: {
      beforeEnter(el) {
        el.style.opacity = 0;
        el.style.height = 0;
      },
      enter(el, done) {
        gsap.to(el, {
          opacity: 1,
          height: "1.5em",
          delay: el.dataset.index * 0.5,
          onComplete: done
        })
      },
      leave(el, done) {
        gsap.to(el, {
          opacity: 0,
          height: 0,
          delay: el.dataset.index * 0.5,
          onComplete: done
        })
      }
    }
  }
</script>

<style scoped>
  /* .why-enter-from,
  .why-leave-to {
    opacity: 0;
  }

  .why-enter-active,
  .why-leave-active {
    transition: opacity 1s ease;
  } */
</style>
```

## 路由使用

### 关于路由

#### 认识前端路由

- 路由其实是网络工程中的一个术语：
  - 在架构一个网络时，非常重要的两个设备就是路由器和交换机。
  - 当然，目前在我们生活中路由器也是越来越被大家所熟知，因为我们生活中都会用到路由器：
  - 路由器的主要功能是维护一个映射表，这个映射表决定了数据的流向。在网络中，路由器通过这个映射表来确定数据包的传输路径，使得数据能够按照设定的规则正确地传输到目的地。
- 路由的概念在软件工程中出现，最早是在后端路由中实现的，原因是web的发展主要经历了这样一些阶段：
  - 后端路由阶段；
  - 前后端分离阶段；
  - 单页面富应用（SPA）；

#### 后端路由阶段

- 早期的网站开发整个HTML页面是由服务器来渲染的.
  - 服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示.
- 一个页面有自己对应的网址, 也就是URL；
- URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller(控制器)进行处理；
- Controller进行各种处理, 最终生成HTML或者数据, 返回给前端.
- 上面的这种操作, 就是后端路由：
  - 当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端.
  - 这种情况下渲染好的页面, 不需要单独加载任何的`js`和`css`, 可以直接交给浏览器展示, 这样也有利于SEO的优化.
- 后端路由的缺点:
  - 一种情况是整个页面的模块由后端人员来编写和维护的；
  - 另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码；
  - 而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情；

#### 前后端分离阶段

- 前端渲染的理解：
  - 每次请求涉及到的静态资源都会从静态资源服务器获取，这些资源包括HTML+CSS+JS，然后在前端对这些请求回来的资源进行渲染；
  - 需要注意的是，客户端的每一次请求，都会从静态资源服务器请求文件；
  - 同时可以看到，和之前的后端路由不同，这时后端只是负责提供API了；
- 前后端分离阶段：
  - 随着Ajax的出现, 有了前后端分离的开发模式；
  - 后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JavaScript将数据渲染到页面中；
  - 这样做最大的优点就是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上；
  - 并且当移动端(iOS/Android)出现后，后端不需要进行任何处理，依然使用之前的一套API即可；
  - 目前比较少的网站采用这种模式开发（jQuery开发模式）；

#### 单页面富应用（SPA）

随着前端框架（如AngularJS、React、Vue等）的兴起，单页面富应用成为主流。单页面富应用（SPA）是一种Web应用程序的架构模式，它通过动态加载页面内容，实现在单个HTML页面上切换视图和更新内容，而无需每次都从服务器请求新的页面。这种方式提高了用户体验和应用性能，因为页面只在初始化时加载一次，之后的页面切换和内容更新都是通过异步加载数据和更新页面内容来实现的。常见的前端框架如AngularJS、React和Vue等都支持SPA的开发模式。

### Vue-router 简介

> 官网: <https://router.vuejs.org/zh/>

Vue Router 是 [Vue.js](https://vuejs.org/) 的官方路由。它与 Vue.js 核心深度集成，让用 Vue.js 构建单页应用变得轻而易举。功能包括：

- 嵌套路由映射
- 动态路由选择
- 模块化、基于组件的路由配置
- 路由参数、查询、通配符
- 展示由 Vue.js 的过渡系统提供的过渡效果
- 细致的导航控制
- 自动激活 CSS 类的链接
- HTML5 history 模式或 hash 模式
- 可定制的滚动行为
- URL 的正确编码

**具体使用示例：**

网易云音乐 <https://music.163.com/>

单页面应用(SPA): 所有功能在一个 html 页面上实现

前端路由作用: 实现业务场景切换

- 优点：
  - 简单易用
  - 支持嵌套路由
  - 支持路由参数、查询、动态路由等
- 缺点：
  - 对于大型单页应用可能不够灵活
  - 在处理复杂路由时可能需要额外的插件或工具

### 路由初体验

Vue Router 支持两种路由模式：

- Hash 模式：
  - 使用 URL 中的 `##`来管理路由，适用于不需要服务端支持的单页应用。`createWebHashHistory`是 Vue Router 提供的一种路由模式，它基于 URL 中的 hash（#）来管理路由。这种模式在不需要服务器端支持的情况下可以工作
- History 模式：
  - 使用 HTML5 History API 来管理路由，可以去掉 URL 中的 `#`，需要服务器端支持来处理路由`createWebHistory` 是 Vue Router 提供的基于 HTML5 History API 的路由模式。这种模式需要服务器端支持来处理路由，但可以去掉 URL 中的 #，看起来更加干净。

用 Vue + Vue Router 创建单页应用非常简单：通过 Vue.js，我们已经用组件组成了我们的应用。当加入 Vue Router 时，我们需要做的就是将我们的组件映射到路由上，让 Vue Router 知道在哪里渲染它们。下面是一个基本的例子：

```vue
<script src="https://unpkg.com/vue@3"></script>
<script src="https://unpkg.com/vue-router@4"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>

<script>
  // 定义 (路由) 组件。
  // 可以从其他文件 import 进来
  const Home = { template: '<div>Home</div>' }
  const About = { template: '<div>About</div>' }

  // 定义路由
  // 每个路由应该映射一个组件。 其中"component" 可以是
  // 通过 Vue.extend() 创建的组件构造器，
  // 或者，只是一个组件配置对象。

  const routes = [
    { path: '/', component: Home },
    { path: '/about', component: About }
  ]

  // 创建 router 实例
  // 你可以在这里传入配置参数
  // 我们在这里使用 `routes` 配置参数
  const router = VueRouter.createRouter({
    history: VueRouter.createWebHashHistory(),
    routes // `routes: routes` 的缩写
  })

  // 创建和挂载根实例
  // 记得要通过 router 配置参数注入路由，
  // 从而让整个应用都有路由功能
  const app = Vue.createApp({})
  app.use(router)
  app.mount('#app')
</script>
```

#### router-link

请注意，我们没有使用常规的 `a` 标签，而是使用一个自定义组件 `router-link` 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

#### router-view

`router-view` 将显示与 URL 对应的组件。你可以把它放在任何地方，以适应你的布局。

### 安装以及使用

- 安装

  ```bash
  npm install vue-router
  ```

- 创建 `router/index.js` 并在其中编辑基本配置(默认你已经注册了 `components` 中的两个文件)

```javascript
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../components/login.vue')
  },
  {
    path: '/req',
    component: () => import('../components/req.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

- 在 `src/App.vue` 中引入

```javascript
import router from '../router'

createApp(App).use(router).mount('#app')
```

- 当然, 我们需要一个 `router-view` 显示与 URL 对应的组件, 一般会在 `src/App.vue` 中做如下配置, 当然你可以把它放在任何地方，以适应你的布局。

```vue
<template>
    <router-view></router-view>
</template>
```

- 我们也可以在其中使用 `router-link`

```vue
<template>
  <div>
    <h1>小满最骚</h1>
    <div>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
      <router-link tag="div" to="/">跳转a</router-link>
      <router-link tag="div" style="margin-left:200px" to="/register">跳转b</router-link>
    </div>
    <hr />
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>
```

### 带参数的动态路由匹配

很多时候，我们需要将给定匹配模式的路由映射到同一个组件。例如，我们可能有一个 `User` 组件，它应该对所有用户进行渲染，但用户 ID 不同。在 Vue Router 中，我们可以在路径中使用一个动态字段来实现，我们称之为 *路径参数* ：

```javascript
const User = {
  template: '<div>User</div>',
}

// 这些都会传递给 `createRouter`
const routes = [
  // 动态字段以冒号开始
  { path: '/users/:id', component: User },
]
```

现在像 `/users/johnny` 和 `/users/jolyne` 这样的 URL 都会映射到同一个路由。

*路径参数* 用冒号 `:` 表示。当一个路由被匹配时，它的 *params* 的值将在每个组件中以 `this.$route.params` 的形式暴露出来。因此，我们可以通过更新 `User` 的模板来呈现当前的用户 ID：

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
}
```

你可以在同一个路由中设置有多个 *路径参数*，它们会映射到 `$route.params` 上的相应字段。例如：

| 匹配模式                       | 匹配路径                 | $route.params                            |
| :----------------------------- | :----------------------- | :--------------------------------------- |
| /users/:username               | /users/eduardo           | `{ username: 'eduardo' }`                |
| /users/:username/posts/:postId | /users/eduardo/posts/123 | `{ username: 'eduardo', postId: '123' }` |

除了 `$route.params` 之外，`$route` 对象还公开了其他有用的信息，如 `$route.query`（如果 URL 中存在参数）、`$route.hash` 等。你可以在 [API 参考](https://router.vuejs.org/zh/api/#routelocationnormalized)中查看完整的细节。

> 以下是一个小 Demo

```javascript
// router.js
import { createRouter, createWebHistory } from 'vue-router'
import UserPost from './views/UserPost.vue'

export const router = createRouter({
  history: createWebHistory(),
  routes: [{ path: '/users/:username/posts/:postId', component: UserPost }],
})
```

```vue
<!-- App.vue -->
<template>
  <ul>
    <li>
      <router-link to="/users/eduardo/posts/1"
        >/users/eduardo/posts/1</router-link
      >
    </li>
    <li>
      <router-link to="/users/eduardo/posts/20"
        >/users/eduardo/posts/20</router-link
      >
    </li>
  </ul>
  <router-view></router-view>
</template>

<script>
export default {
  name: "App",
};
</script>
```

```vue
<!-- ./views/UserPost.vue -->
<template>
  <div>
    User {{ $route.params.username }} with post {{ $route.params.postId }}
  </div>
</template>
```

![image-20231223000208274](https://qiniu.waite.wang/202312230002394.png)

### 声明式/ 编程式导航

#### 声明式导航

##### 基础使用/ 命名路由

除了 `path` 之外，你还可以为任何路由提供 `name`。这有以下优点：

- <https://router.vuejs.org/zh/guide/essentials/named-routes.html>

- 没有硬编码的 URL
- `params` 的自动编码/解码。
- 防止你在 `url`中出现打字错误。
- 绕过路径排序（如显示一个）
- 这跟代码调用 `router.push()` 是一回事：

```javascript
router.push({name: 'user', params: {username: 'erina'}})
// 在这两种情况下，路由将导航到路径 /user/erina。
```

```typescript
const routes:Array<RouteRecordRaw> = [
    {
        path:"/",
        name:"Login",
        component:()=> import('../components/login.vue')
    },
    {
        path:"/reg",
        name:"Reg",
        component:()=> import('../components/reg.vue')
    }
]
```

> 跳转方式需要改变 变为对象并且有对应name
>
> 两种跳转方式有区别, a 标签有新的网络请求, 会刷新整个页面

```vue
<template>
  <div>
    <div class="">
      <router-link :to="{name: 'Login'}">Home</router-link>
      <router-link :to="{name: 'Req'}">Req</router-link>

      <a href="/">Login</a>
      <a href="/req">Req</a>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>
```

##### 跳转传参

目标: 在跳转路由时, 可以给路由对应的组件内传值

在 `router-link` 上的 `to` 属性传值, 语法格式如下

- `/path?参数名=值`
- `/path/值` – 需要路由对象提前配置 `path: "/path/参数名"`

对应页面组件接收传递过来的值

- `route.query.参数名`
- `route.params.参数名`

1、新建 `views/Part2.vue` - 接收路由上传递的参数和值

```vue
<template>
  <div>
    <p>我的好友</p>
    <!-- query 查询 ？ 号后面的。 params 是获取 url : 中的参数-->
    <p>人名(path --> query): {{ route.query?.name }}</p>
    <p>人名(?后参数 --> params): {{ route.params?.name }}</p>
  </div>
</template>

<script setup>
// 目标: 声明式导航 - 基础使用
// 本质: vue-router 提供的全局组件 "router-link" 替代a标签
// 1. router-link  替代 a 标签
// 2. to 属性      替代 href 属性
// 好处: router-link 自带高亮的类名(激活时类名)
// 3. 对激活的类名做出样式的编写
import {useRoute} from 'vue-router'

const route = useRoute()
</script>
```

2、修改路由定义

```javascript
const routes = [
    {'path': '/find', component: () => import('../views/Find.vue')},
    {'path': '/my', component: () => import('../views/My.vue')},
    {'path': '/part', name: 'Part', component: () => import('../views/Part.vue')},
    {
        path: "/part/:name", // 有:的路径代表要接收具体的值
        component: () => import('../views/Part2.vue')
    },
]
```

3、修改 `App.vue` 进行跳转

```vue
<template>
  <div>
    <div class="footer_wrap">
      <!--      <a href="#/find">发现音乐</a>-->
      <!--      <a href="#/my">我的音乐</a>-->
      <!--      <a href="#/part">朋友</a>-->
      <router-link to="/find">发现音乐</router-link>
      <router-link to="/my">我的音乐</router-link>
      <router-link to="/part">朋友</router-link>
      <router-link to="/part?name=小传">朋友-小传</router-link>
      <router-link to="/part/小智?name=小智2">朋友-小智</router-link>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>
```

总结:

- `?key=value` 用 `$route.query.key` 取值
- `/值` 提前在路由规则 `/path/:key` 用 `$route.params.key` 取值
- `query` 是查询参数, `params` 是 `path` 路径
- 有:的路径代表要接收具体的值, 不然会报警告

![image-20231223002327799](https://qiniu.waite.wang/202312230023125.png)

在这个特定的场景中，我们在括号之间使用了[自定义正则表达式](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#在参数中自定义正则)，并将`pathMatch` 参数标记为[可选可重复](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#可选参数)。这样做是为了让我们在需要的时候，可以通过将 `path` 拆分成一个数组，直接导航到路由

##### 捕获所有路由或 404 Not found 路由

常规参数只匹配 url 片段之间的字符，用 `/` 分隔。如果我们想匹配**任意路径**，我们可以使用自定义的 *路径参数* 正则表达式，在 *路径参数* 后面的括号中加入 正则表达式 :

```javascript
const routes = [
  // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.afterUser` 下
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

在这个特定的场景中，我们在括号之间使用了[自定义正则表达式](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#在参数中自定义正则)，并将`pathMatch` 参数标记为[可选可重复](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#可选参数)。这样做是为了让我们在需要的时候，可以通过将 `path` 拆分成一个数组，直接导航到路由：

```javascript
this.$router.push({
  name: 'NotFound',
  // 保留当前路径并删除第一个字符，以避免目标 URL 以 `//` 开头。
  params: { pathMatch: this.$route.path.substring(1).split('/') },
  // 保留现有的查询和 hash 值，如果有的话
  query: this.$route.query,
  hash: this.$route.hash,
})
```

更多内容请参见[重复参数](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#可重复的参数)部分。

> 一般使用如下

```javascript
const routes = [
    // ...省略了其他配置  
    // 404在最后(规则是从前往后逐个比较path)  
    {
        path: "/:pathMatch(.*)*",
        component: () => import('../views/NotFound.vue')
    }
]
```

#### 编程式导航

除了使用 `<router-link>` 创建 `a` 标签来定义导航链接，我们还可以借助 `router` 的实例方法，通过编写代码来实现。

##### 导航到不同的位置

注意：在 Vue 实例中，你可以通过 `$router` 访问路由实例。因此你可以调用 `$router.push`。

想要导航到不同的 URL，可以使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，会回到之前的 URL。

当你点击 `<router-link>` 时，内部会调用这个方法，所以点击 `<router-link :to="...">` 相当于调用 `router.push(...)` ：

| 声明式                    | 编程式             |
| :------------------------ | :----------------- |
| `<router-link :to="...">` | `router.push(...)` |

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：

```javascript
// 字符串路径
router.push('/users/eduardo');

// 带有路径的对象
router.push({path: '/users/eduardo'});

// 命名的路由，并加上参数，让路由建立 url
router.push({name: 'user', params: {username: 'eduardo'}});

// 带查询参数，结果是 /register?plan=private
router.push({path: '/register', query: {plan: 'private'}});

// 带 hash，结果是 /about.md#team
router.push({path: '/about.md', hash: '#team'});
```

**注意**：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path` ：

```javascript
const username = 'eduardo'
// 我们可以手动建立 url，但我们必须自己处理编码
router.push(`/user/${username}`) // -> /user/eduardo
// 同样
router.push({path: `/user/${username}`}) // -> /user/eduardo
// 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
router.push({name: 'user', params: {username}}) // -> /user/eduardo
// `params` 不能与 `path` 一起使用
router.push({path: '/user', params: {username}}) // -> /user
```

当指定 `params` 时，可提供 `string` 或 `number` 参数（或者对于[可重复的参数](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#repeatable-params) 可提供一个数组）。**任何其他类型（如 `undefined`、`false` 等）都将被自动字符串化** 。对于[可选参数](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#repeatable-params) ，你可以提供一个空字符串（`""`）来跳过它。

由于属性 `to` 与 `router.push` 接受的对象种类相同，所以两者的规则完全相同。

##### 基础使用

语法:

```javascript
router.push({
    path: "路由路径", // 都去 router/index.js 定义
    name: "路由名"
})
```

1. `src/router/index.js` - 路由数组里, 给路由起名字

```javascript
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: "/part/:name", // 有:的路径代表要接收具体的值
    name: 'Part2',
    component: () => import('../components/HelloWorld.vue')
  },
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

2. App.vue - 换成 span 配合js的编程式导航跳转

```vue

<template>
  <div>
    <div class="footer_wrap">
      <a @click="change_router('/part', 'Part')">朋友</a> <br />
      <a @click="change1">朋友-小传</a> <br />
      <a @click="change2">朋友-小智</a>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
import {useRouter} from 'vue-router';

const router = useRouter();

const change_router = (path, name) => {
  router.push({name: name});
};

const change1 = () => {
  router.push({
    name: 'Part2',
    params: {
      name: '小传',
    },
  });
};

const change2 = () => {
  router.push(
    {
      name: 'Part2',
      params: {  
        name: '小智'  
      },
      query: {
        name: '小智',
      },
    },
  );
};
</script>
```

### 横跨历史

该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步，类似于 `window.history.go(n)`。

```javascript
// 向前移动一条记录，与 router.forward() 相同
router.go(1)

// 返回一条记录，与 router.back() 相同
router.go(-1)

// 前进 3 条记录
router.go(3)

// 如果没有那么多记录，静默失败
router.go(-100)
router.go(100)
```

### 替换当前位置

它的作用类似于 `router.push`，唯一不同的是，它在导航时不会向 history 添加新记录，正如它的名字所暗示的那样——它取代了当前的条目。

| 声明式                            | 编程式                |
| :-------------------------------- | :-------------------- |
| `<router-link :to="..." replace>` | `router.replace(...)` |

也可以直接在传递给 `router.push` 的 `routeLocation` 中增加一个属性 `replace: true` ：

```javascript
router.push({path: '/home', replace: true})
// 相当于
router.replace({path: '/home'})
```

### 重定向

重定向也是通过 `routes` 配置来完成，下面例子是从 `/home` 重定向到 `/`：

```javascript
const routes = [{path: '/home', redirect: '/'}]
```

重定向的目标也可以是一个命名的路由：

````javascript
const routes = [{path: '/home', redirect: {name: 'homepage'}}]
````

甚至是一个方法，动态返回重定向目标：

```javascript
const routes = [
    {
        // /search/screens -> /search?q=screens
        path: '/search/:searchText',
        redirect: to => {
            // 方法接收目标路由作为参数
            // return 重定向的字符串路径/路径对象
            return {path: '/search', query: {q: to.params.searchText}}
        },
    },
    {
        path: '/search',
        // ...
    },
]
```

例如: 网页默认打开, 匹配路由 `"/"`, 强制切换到 `"/find"` 上

```javascript
const routes = [
    {
        path: "/", // 默认hash值路径  
        redirect: "/find" // 重定向到 /find  
        // 浏览器 url 中 ## 后的路径被改变成 /find -重新匹配数组规则
    },
]
```

总结: 强制重定向后, 还会重新来数组里匹配一次规则

### 别名

有时候，同一个路径可能需要多个路由，此时可以使用 `alias` 创建别名。

```javascript
const routes = [
  { path: '/foo', component: Foo, alias: '/bar' },
]
```

> 有个通用的场景是，你可能要把 `src` 目录下的 `@` 指向 `src` 目录，这时候就可以使用别名：

在 `vue.config.js` 中配置别名

```javascript
module.exports = {
  configureWebpack: {
    resolve: {
      alias: {
        '@': path.resolve(__dirname, 'src')
      }
    }
  }
}
```

## 路由进阶

### 路由嵌套

一些应用程序的 UI 由多层嵌套的组件组成。在这种情况下，URL 的片段通常对应于特定的嵌套组件结构，例如：

```
/user/johnny/profile                     /user/johnny/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

通过 Vue Router，你可以使用嵌套路由配置来表达这种关系。

接着上节创建的 app ：

```html
<div id="app">
  <router-view></router-view>
</div>
```

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
}

// 这些都会传递给 `createRouter`
const routes = [{ path: '/user/:id', component: User }]
```

这里的 `<router-view>` 是一个顶层的 `router-view`。它渲染顶层路由匹配的组件。同样地，一个被渲染的组件也可以包含自己嵌套的 `<router-view>`。例如，如果我们在 `User` 组件的模板内添加一个 `<router-view>`：

```javascript
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `,
}
```

要将组件渲染到这个嵌套的 `router-view` 中，我们需要在路由中配置 `children`：

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // 当 /user/:id/profile 匹配成功
        // UserProfile 将被渲染到 User 的 <router-view> 内部
        path: 'profile',
        component: UserProfile,
      },
      {
        // 当 /user/:id/posts 匹配成功
        // UserPosts 将被渲染到 User 的 <router-view> 内部
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
```

**注意，以 `/` 开头的嵌套路径将被视为根路径。这允许你利用组件嵌套，而不必使用嵌套的 URL。**

如你所见，`children` 配置只是另一个路由数组，就像 `routes` 本身一样。因此，你可以根据自己的需要，不断地嵌套视图。

此时，按照上面的配置，当你访问 `/user/eduardo` 时，在 `User` 的 `router-view` 里面什么都不会呈现，因为没有匹配到嵌套路由。也许你确实想在那里渲染一些东西。在这种情况下，你可以提供一个空的嵌套路径：

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      // 当 /user/:id 匹配成功
      // UserHome 将被渲染到 User 的 <router-view> 内部
      { path: '', component: UserHome },

      // ...其他子路由
    ],
  },
]
```

### 导航守卫

> <https://router.vuejs.org/zh/guide/advanced/navigation-guards.html>

正如其名，vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。这里有很多方式植入路由导航中：全局的，单个路由独享的，或者组件级的。

#### 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```javascript
const router = createRouter({ ... })

router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  return false
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于**等待中**。

每个守卫方法接收两个参数：

- **`to`**: 即将要进入的目标 [用一种标准化的方式](https://router.vuejs.org/zh/api/#routelocationnormalized)
- **`from`**: 当前导航正要离开的路由 [用一种标准化的方式](https://router.vuejs.org/zh/api/#routelocationnormalized)

可以返回的值如下:

- `false`: 取消当前的导航。如果浏览器的 URL 改变了(可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
- 一个[路由地址](https://router.vuejs.org/zh/api/#routelocationraw): 通过一个路由地址重定向到一个不同的地址，如同调用 `router.push()`，且可以传入诸如 `replace: true` 或 `name: 'home'` 之类的选项。它会中断当前的导航，同时用相同的 `from` 创建一个新导航。

```javascript
 router.beforeEach(async (to, from) => {
   if (
     // 检查用户是否已登录
     !isAuthenticated &&
     // ❗️ 避免无限重定向
     to.name !== 'Login'
   ) {
     // 将用户重定向到登录页面
     return { name: 'Login' }
   }
 })
```

如果遇到了意料之外的情况，可能会抛出一个 `Error`。这会取消导航并且调用 [`router.onError()`](https://router.vuejs.org/zh/api/interfaces/router#onError) 注册过的回调。

如果什么都没有，`undefined` 或返回 `true`，**则导航是有效的**，并调用下一个导航守卫

以上所有都同 **`async` 函数** 和 Promise 工作方式一样：

```javascript
router.beforeEach(async (to, from) => {
  // canUserAccess() 返回 `true` 或 `false`
  const canAccess = await canUserAccess(to)
  if (!canAccess) return '/login'
})
```

在之前的 Vue Router 版本中，还可以使用 *第三个参数* `next` 。这是一个常见的错误来源，我们经过 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0037-router-return-guards.md#motivation) 讨论将其移除。然而，它仍然是被支持的，这意味着你可以向任何导航守卫传递第三个参数。在这种情况下，**确保 `next`** 在任何给定的导航守卫中都被**严格调用一次**。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错。这里有一个在用户未能验证身份时重定向到`/login`的**错误用例**：

```javascript
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})
```

下面是正确的版本:

```javascript
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

#### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

```javascript
router.afterEach((to, from) => {
    sendToAnalytics(to.fullPath)
})
```

它们对于分析、更改页面标题、声明页面等辅助功能以及许多其他事情都很有用。

它们也反映了 [navigation failures](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html) 作为第三个参数：

```javascript
router.afterEach((to, from, failure) => {
    if (!failure) sendToAnalytics(to.fullPath)
})
```

更多关于 navigation failures 的信息在[它的指南](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html) 中。

#### 路由独享的守卫

你可以直接在路由配置上定义 `beforeEnter` 守卫：

```javascript
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    },
  },
]
```

`beforeEnter` 守卫 **只在进入路由时触发**，不会在 `params`、`query` 或 `hash` 改变时触发。例如，从 `/users/2` 进入到 `/users/3` 或者从 `/users/2#info` 进入到 `/users/2#projects`。它们只有在 **从一个不同的** 路由导航时，才会被触发。

你也可以将一个函数数组传递给 `beforeEnter`，这在为不同的路由重用守卫时很有用：

```javascript
function removeQueryParams(to) {
  if (Object.keys(to.query).length)
    return { path: to.path, query: {}, hash: to.hash }
}

function removeHash(to) {
  if (to.hash) return { path: to.path, query: to.query, hash: '' }
}

const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: [removeQueryParams, removeHash],
  },
  {
    path: '/about',
    component: UserDetails,
    beforeEnter: [removeQueryParams],
  },
]
```

请注意，你也可以通过使用[路径 meta 字段](https://router.vuejs.org/zh/guide/advanced/meta)和全局导航守卫来实现类似的行为。

#### 组件内的守卫

最后，你可以在路由组件内直接定义路由导航守卫(传递给路由配置的)

#### 可用的配置 API

你可以为路由组件添加以下配置：

- `beforeRouteEnter`
- `beforeRouteUpdate`
- `beforeRouteLeave`

```javascript
const UserDetails = {
  template: `...`,
  beforeRouteEnter(to, from) {
    // 在渲染该组件的对应路由被验证前调用
    // 不能获取组件实例 `this` ！
    // 因为当守卫执行时，组件实例还没被创建！
  },
  beforeRouteUpdate(to, from) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 `/users/:id`，在 `/users/1` 和 `/users/2` 之间跳转的时候，
    // 由于会渲染同样的 `UserDetails` 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 因为在这种情况发生的时候，组件已经挂载好了，导航守卫可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from) {
    // 在导航离开渲染该组件的对应路由时调用
    // 与 `beforeRouteUpdate` 一样，它可以访问组件实例 `this`
  },
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next` 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数：

```javascript
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以*不支持* 传递回调，因为没有必要了：

```javascript
beforeRouteUpdate (to, from) {
  // just use `this`
  this.name = to.params.name
}
```

这个 **离开守卫** 通常用来预防用户在还未保存修改前突然离开。该导航可以通过返回 `false` 来取消。

```javascript
beforeRouteLeave (to, from) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (!answer) return false
}
```

##### 使用组合 AP

如果你正在使用[组合 API 和 `setup` 函数](https://cn.vuejs.org/api/composition-api-setup.html)来编写组件，你可以通过 `onBeforeRouteUpdate` 和 `onBeforeRouteLeave` 分别添加 update 和 leave 守卫。 请参考[组合 API 部分](https://router.vuejs.org/zh/guide/advanced/composition-api.html#导航守卫)以获得更多细节。

#### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫(2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫(2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 路由案例

目标: 路由跳转之前, 先执行一次前置守卫函数, 判断是否可以正常跳转

```javascript
// `router.beforeEach`
router.beforeEach((to, form, next) => {
    console.log(to, form);
    next()
})
```

每个守卫方法接收三个参数：

```javascript
to: Route， 即将要进入的目标 路由对象；
from: Route，当前导航正要离开的路由；
next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。
```

#### 登录拦截

使用例子: 在跳转路由前, 判断用户登陆了才能去 `<我的音乐>` 页面, 未登录弹窗提示回到发现音乐页面

在路由对象上使用固定方法 beforeEach

```javascript
// 目标: 路由守卫
// 场景: 当你要对路由权限判断时
// 语法: router.beforeEach((to, from, next)=>{//路由跳转"之前"先执行这里, 决定是否跳转})
// 参数1: 要跳转到的路由 (路由对象信息)    目标
// 参数2: 从哪里跳转的路由 (路由对象信息)  来源
// 参数3: 函数体 - next()才会让路由正常的跳转切换, next(false)在原地停留, next("强制修改到另一个路由路径上")
// 注意: 如果不调用 next, 页面留在原地
const isLogin = ref(true)// 登录状态(未登录)
router.beforeEach((to, from, next) => {
    if (to.path === '/my' && isLogin.value === false) {
        alert('请登录')
        next(false) // 阻止路由跳转
    } else {
        next() // 正常放行
    }
})
```

> 总结: next()放行, next(false)留在原地不跳转路由, next(path路径)强制换成对应path路径跳转

#### 权限判断

```javascript
const whileList = ['/']

router.beforeEach((to, from, next) => {
    let token = localStorage.getItem('token')
    //白名单 有值 或者登陆过存储了token信息可以跳转 否则就去登录页面
    if (whileList.includes(to.path) || token) {
        next()
    } else {
        next({
            path: '/'
        })
    }
})
```

### 路由原信息

有时，你可能希望将任意信息附加到路由上，如过渡名称、谁可以访问路由等。这些事情可以通过接收属性对象的`meta`属性来实现，并且它可以在路由地址和导航守卫上都被访问到。定义路由的时候你可以这样配置 `meta` 字段：

```javascript
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // 只有经过身份验证的用户才能创建帖子
        meta: { requiresAuth: true },
      },
      {
        path: ':id',
        component: PostsDetail
        // 任何人都可以阅读文章
        meta: { requiresAuth: false },
      }
    ]
  }
]
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，它可能匹配多个路由记录。

例如，根据上面的路由配置，`/posts/new` 这个 URL 将会匹配父路由记录 (`path: '/posts'`) 以及子路由记录 (`path: 'new'`)。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象(还有在导航守卫中的路由对象)的`$route.matched` 数组。我们需要遍历这个数组来检查路由记录中的 `meta` 字段，但是 Vue Router 还为你提供了一个 `$route.meta` 方法，它是一个非递归合并**所有 `meta`** 字段（从父字段到子字段）的方法。这意味着你可以简单地写

```javascript
router.beforeEach((to, from) => {
  // 而不是去检查每条路由记录
  // to.matched.some(record => record.meta.requiresAuth)
  if (to.meta.requiresAuth && !auth.isLoggedIn()) {
    // 此路由需要授权，请检查是否已登录
    // 如果没有，则重定向到登录页面
    return {
      path: '/login',
      // 保存我们所在的位置，以便以后再来
      query: { redirect: to.fullPath },
    }
  }
})
```

### 过渡动效

想要在你的路径组件上使用转场，并对导航进行动画处理，你需要使用 [v-slot API](https://router.vuejs.org/zh/api/#router-view-s-v-slot)：

```vue
<router-view #default="{route,Component}">
    <transition  :enter-active-class="`animate__animated ${route.meta.transition}`">
        <component :is="Component"></component>
    </transition>
</router-view>
```

上面的用法会对所有的路由使用相同的过渡。如果你想让每个路由的组件有不同的过渡，你可以将[元信息](https://router.vuejs.org/zh/guide/advanced/meta.html)和动态的 `name` 结合在一起，放在`<transition>` 上：

```javascript
const routes = [
  {
    path: '/custom-transition',
    component: PanelLeft,
    meta: { transition: 'slide-left' },
  },
  {
    path: '/other-transition',
    component: PanelRight,
    meta: { transition: 'slide-right' },
  },
]
```

```html
<router-view v-slot="{ Component, route }">
  <!-- 使用任何自定义过渡和回退到 `fade` -->
  <transition :name="route.meta.transition || 'fade'">
    <component :is="Component" />
  </transition>
</router-view>
```

也可以根据目标路由和当前路由之间的关系，动态地确定使用的过渡。使用和刚才非常相似的片段：

```vue
<!-- 使用动态过渡名称 -->
<router-view v-slot="{ Component, route }">
  <transition :name="route.meta.transition">
    <component :is="Component" />
  </transition>
</router-view>
```

我们可以添加一个 [after navigation hook](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#全局后置钩子)，根据路径的深度动态添加信息到 `meta` 字段。

```javascript
router.afterEach((to, from) => {
  const toDepth = to.path.split('/').length
  const fromDepth = from.path.split('/').length
  to.meta.transition = toDepth < fromDepth ? 'slide-right' : 'slide-left'
})
```

Vue 可能会自动复用看起来相似的组件，从而忽略了任何过渡。幸运的是，可以[添加一个 `key` 属性](https://cn.vuejs.org/api/built-in-special-attributes.html#key)来强制过渡。这也允许你在相同路由上使用不同的参数触发过渡：

```vue
<router-view v-slot="{ Component, route }">
  <transition name="fade">
    <component :is="Component" :key="route.path" />
  </transition>
</router-view>
```

### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 vue-router 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 history.pushState 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```javascript
const router = createRouter({
  history: createWebHashHistory(),
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 函数接收 `to`和`from` 路由对象，如 [Navigation Guards](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)。第三个参数 `savedPosition`，只有当这是一个 `popstate` 导航时才可用（由浏览器的后退/前进按钮触发）。

该函数可以返回一个 [`ScrollToOptions`](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions) 位置对象:

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始终滚动到顶部
    return { top: 0 }
  },
})
```

你也可以通过 `el` 传递一个 CSS 选择器或一个 DOM 元素。在这种情况下，`top` 和 `left` 将被视为该元素的相对偏移量。

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始终在元素 #main 上方滚动 10px
    return {
      // 也可以这么写
      // el: document.getElementById('main'),
      el: '#main',
      // 在元素上 10 像素
      top: 10,
    }
  },
})
```

如果返回一个 falsy 的值，或者是一个空对象，那么不会发生滚动。

返回 `savedPosition`，在按下 后退/前进 按钮时，就会像浏览器的原生表现那样：

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  },
})
```

如果你要模拟 “滚动到锚点” 的行为：

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (to.hash) {
      return {
        el: to.hash,
      }
    }
  },
})
```

如果你的浏览器支持[滚动行为](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions/behavior)，你可以让它变得更流畅：

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (to.hash) {
      return {
        el: to.hash,
        behavior: 'smooth',
      }
    }
  }
})
```

有时候，我们需要在页面中滚动之前稍作等待。例如，当处理过渡时，我们希望等待过渡结束后再滚动。要做到这一点，你可以返回一个 Promise，它可以返回所需的位置描述符。下面是一个例子，我们在滚动前等待 500ms：

```javascript
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ left: 0, top: 0 })
      }, 500)
    })
  },
})
```

我们可以将其与页面级过渡组件的事件挂钩，以使滚动行为与你的页面过渡很好地结合起来，但由于使用场景可能存在的差异和复杂性，我们只是提供了这个基础来实现特定的用户场景。

### 动态路由

对路由的添加通常是通过 `routes` 选项来完成的，但是在某些情况下，你可能想在应用程序已经运行的时候添加或删除路由。具有可扩展接口(如 [Vue CLI UI](https://cli.vuejs.org/dev-guide/ui-api.html) )这样的应用程序可以使用它来扩展应用程序。

#### 添加路由

动态路由主要通过两个函数实现。`router.addRoute()` 和 `router.removeRoute()`。它们**只**注册一个新的路由，也就是说，如果新增加的路由与当前位置相匹配，就需要你用 `router.push()` 或 `router.replace()` 来**手动导航**，才能显示该新路由。我们来看一个例子：

想象一下，只有一个路由的以下路由：

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [{ path: '/:articleName', component: Article }],
})
```

进入任何页面，`/about`，`/store`，或者 `/3-tricks-to-improve-your-routing-code` 最终都会呈现 `Article` 组件。如果我们在 `/about` 上添加一个新的路由：

```javascript
router.addRoute({ path: '/about', component: About })
```

页面仍然会显示 `Article` 组件，我们需要手动调用 `router.replace()` 来改变当前的位置，并覆盖我们原来的位置（而不是添加一个新的路由，最后在我们的历史中两次出现在同一个位置）：

```javascript
router.addRoute({ path: '/about', component: About })
// 我们也可以使用 this.$route 或 route = useRoute() （在 setup 中）
router.replace(router.currentRoute.value.fullPath)
```

记住，如果你需要等待新的路由显示，可以使用 `await router.replace()`。

#### 删除路由

有几个不同的方法来删除现有的路由：

- 通过添加一个名称冲突的路由。如果添加与现有途径名称相同的途径，会先删除路由，再添加路由：

  ```javascript
  router.addRoute({ path: '/about', name: 'about', component: About })
  // 这将会删除之前已经添加的路由，因为他们具有相同的名字且名字必须是唯一的
  router.addRoute({ path: '/other', name: 'about', component: Other })
  ```

- 通过调用 `router.addRoute()` 返回的回调：

  ```javascript
  const removeRoute = router.addRoute(routeRecord)
  removeRoute() // 删除路由如果存在的话
  ```

  当路由没有名称时，这很有用。

- 通过使用 `router.removeRoute()` 按名称删除路由：

  ```javascript
  router.addRoute({ path: '/about', name: 'about', component: About })
  // 删除路由
  router.removeRoute('about')
  ```

  需要注意的是，如果你想使用这个功能，但又想避免名字的冲突，可以在路由中使用 `Symbol` 作为名字。

当路由被删除时，**所有的别名和子路由也会被同时删除**

#### 查看现有路由

Vue Router 提供了两个功能来查看现有的路由：

- [`router.hasRoute()`](https://router.vuejs.org/zh/api/interfaces/Router.html#Methods-hasRoute)：检查路由是否存在。
- [`router.getRoutes()`](https://router.vuejs.org/zh/api/interfaces/Router.html#Methods-getRoutes)：获取一个包含所有路由记录的数组。

### 动态路由案例

#### 后端代码 -> Python Flask

```python
from flask import Flask, request
from flask_cors import CORS

app = Flask(__name__)
CORS().init_app(app)


@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    password = request.json.get('password')
    if username == 'admin' and password == '123456':
        return {
            'routes': [
                {
                    'path': "/articles",
                    'name': "articles",
                    'parent': "subviews",
                    'component': 'Articles.vue'
                },
                {
                    'path': "/hot",
                    'name': "hot",
                    'parent': "subviews",
                    'component': 'Hot.vue'
                },
                {
                    'path': "/author",
                    'name': "author",
                    'parent': "subviews",
                    'component': 'Author.vue'
                }
            ]
        }
    else:
        return {
            'code': 400,
            'message': "账号密码错误"
        }


@app.route('/menus', methods=['GET'])
def menus():
    return {
        'routes': [
            {
                'path': '/login',
                'name': 'login',
                'component': 'Login.vue'
            },
            {
                'path': '/main',
                'name': 'main',
                'component': 'Main.vue'
            }
        ]
    }


@app.route('/', methods=['GET'])
def index():
    return {
        'routes': [
            {
                'path': '/login',
                'name': 'login',
                'component': './views/Login.vue'
            }
        ]
    }


if __name__ == '__main__':
    app.run(debug=True, host='127.0.0.1', port=5000)
```

#### 前端代码

- App.vue

```vue
<script setup>
import {onMounted} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";

const router = useRouter()

// 启动项目之后才加载登录路由
onMounted(async () => {
  const response = await axios.get('http://127.0.0.1:5000/menus')
  const data = response.data
  data.routes.forEach((v) => {
    router.addRoute({
      path: v.path,
      name: v.name,
      component: () => import(/* @vite-ignore */ `./components/${v.component}`)
    })
  })
  await router.push('/login')
})
</script>

<template>
  <router-view></router-view>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body, #app {
  width: 100%;
  height: 100%;
}
</style>
```

- router.js

```javascript
import { createRouter, createWebHistory } from 'vue-router'


// @ 路径
console.log(import.meta.url)

const routes = [
  {
    path: '/login',
    name: 'login',
    component: () => import('@/components/login.vue')
  },
  {
    path: '/main',
    name: 'main',
    component: () => import('@/components/main.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

- login.vue

```vue
<template>
  <div class="login">
    <el-form :model="formInline" class="demo-form-inline">
      <el-form-item label="用户名：">
        <el-input v-model="formInline.username" placeholder="请输入用户名"/>
      </el-form-item>

      <el-form-item label="密&nbsp;&nbsp;&nbsp;码：">
        <el-input v-model="formInline.password" placeholder="请输入密码">
        </el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">提交登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import axios from 'axios';
import {reactive} from 'vue'
import {useRouter} from "vue-router";

const router = useRouter()

const formInline = reactive({
  username: '',
  password: '',
})

const onSubmit = async () => {  
  const response = await axios.post('http://127.0.0.1:5000/login', formInline)  
  const data = response.data  
  console.log(data)  
  
  // 登录成功之后, 解析后端返回的路由数据  
  if (data?.routes) {  
    data.routes.forEach((v) => {  
      console.log(v)  
      const path = v?.parent ? `./${v.parent}/${v.component}` : `./${v.component}`  
  
      router.addRoute('main', {  
        path: '/main' + v.path,  
        name: v.name,  
        component: () => import(/* @vite-ignore */ path)
      })  
    })  
    console.log(router.getRoutes())  
    await router.push('/main')  
  }  
}
</script>

<style scoped>
.login {
  width: 30%;
  margin: 100px auto;
}
</style>
```

- main.js

```vue
<template>
  <h1>Main</h1>
  <div class="nav">
    <router-link to="/main/articles">阅读文章</router-link>
    <router-link to="/main/hot">排行热榜</router-link>
    <router-link to="/main/author">作者排行</router-link>
  </div>
  <div class="subview">
    <router-view></router-view>
  </div>
</template>

<script setup>

</script>

<style scoped>
.nav a {
  margin: 20px;
}
</style>
```
