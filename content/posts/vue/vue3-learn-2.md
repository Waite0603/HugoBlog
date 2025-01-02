---
title: "Vue3 基本指令(二)"
date: 2023-10-22T16:04:23+08:00
categories: ["Web"]
tags: ["Vue3"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

> 本文章来源于王红元老师（coderwhy）的 Vue3 + Ts 课程第二章节 - Vue3 基本指令附上链接：https//ke.qq.com/course/3453141
permalink: /archives/vue3-ji-ben-zhi-ling-er



# Vue3 基本指令

## VSCode 代码片段

+ 我们在前面练习Vue的过程中，有些代码片段是需要经常写的，我们再VSCode中我们可以生成一个代码片段，方 便我们快速生成。 
+ VSCode中的代码片段有固定的格式，所以我们一般会借助于一个在线工具来完成。 
+ 具体的步骤如下： 
  + 第一步，复制自己需要生成代码片段的代码； 
  + 第二步，https://snippet-generator.app/在该网站中生成代码片段； 
  + 第三步，在VSCode中配置代码片段；
+ 直接 Tab trigger 即可自动填充

![image-20231016233051010](https://qiniu.waite.wang/202310162330091.png)

## 模板语法

+ React的开发模式[了解]
  + React使用的jsx,所以对应的代码都是编写的类似于js的一种语法
  + 之后通过Babe将js编译成 React. create Element函数调用

```html
function () {
  return <div></div>
}
```

+ vue也支持 jsx 的开发模式:
  + 但是大多数情况下,使用基于HTML的模板语法
  + 在模板中,允许开发者以声明式的方式将 DOM 和底层组件实例的数据绑定在-起;口在底层的实现中,vue将模板编译成虚拟DOM渲染函数

```vue
<template>
  <div @click v-bind v-once> {{}} </div>
</template>
```

## Mustache 语法 双大括号语法

+ 如果我们希望把数据显示到模板（template）中，使用最多的语法是 “Mustache”语法 (双大括号) 的文本插值。 
  + 并且我们前端提到过，data返回的对象是有添加到Vue的响应式系统中； 
  + 当data中的数据发生改变时，对应的内容也会发生更新。 
  + 当然，Mustache中不仅仅可以是data中的属性，也可以是一个JavaScript的表达式。
+  mustache的使用
    	1. 基本使用 
      	2. 表达式
      	3. 函数
      	4. 三元运算符

```vue
<template id="my-app">
  <!-- 1.mustache的基本使用 -->
  <h2>{{message}} - {{message}}</h2>
  <!-- 2.是一个表达式 -->
  <h2>{{counter * 10}}</h2>
  <h2>{{ message.split(" ").reverse().join(" ") }}</h2>
  <!-- 3.也可以调用函数 -->
  <!-- 可以使用computed(计算属性) -->
  <h2>{{getReverseMessage()}}</h2>
  <!-- 4.三元运算符 -->
  <h2>{{ isShow ? "哈哈哈": "" }}</h2>
  <button @click="toggle">切换</button>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        message: "Hello World",
        counter: 100,
        isShow: true
      }
    },
    methods: {
      getReverseMessage() {
        return this.message.split(" ").reverse().join(" ");
      },
      toggle() {
        this.isShow = !this.isShow;
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

> 以下为错误写法

```html
<!-- 错误用法 -->
var name = "abc" ;

<h2>{{var name = "abc"}}</h2>
<h2>{{ if(isShow) {  return "哈哈哈" } }}</h2>
```

## 不常用指令

### v-once指令

+ v-once用于指定元素或者组件只渲染一次
  - 当数据发生变化时,元素或者组件以及其所有的子元素将视为静态内容并且跳过;
  
  - 该指令可以用于性能优化;
  
    ```vue
    <h2 v-once>{{counter}}</h2>
    <button @click="increment">+1</button>
    ```
  
+ 如果是子节点的化，也只能渲染一次

  ```vue
  <div v-once>
       <h2>{{counter}}</h2>
       <h2>{{message}}</h2>
   </div>
   <button @click="increment">+1</button>
  ```

> 完整代码

```vue
<div id="app"></div>

<template id="my-app">
  <h2>{{counter}}</h2>
  <div v-once>
    <h2>{{counter}}</h2>
    <h2>{{message}}</h2>
  </div>
  <button @click="increment">+1</button>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        counter: 100,
        message: "abc"
      }
    },
    methods: {
      increment() {
        this.counter++;
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

### v-html

+ 默认情况下，如果我们展示的内容本身是 html 的，那么vue并不会对其进行特殊的解析。
  - 如果我们希望这个内容被Vue可以解析出来，那么可以使用 v-html 来展示

```vue
<template id="my-app">
  <div>{{msg}}</div>
  <div v-html="msg"></div>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        msg: '<span style="color:red; background: blue;">哈哈哈</span>'
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

![image-20231018083558572](https://qiniu.waite.wang/202310180836759.png)

### v-text

+ 用于更新元素的 textContent

```html
 <h2 v-text="message"></h2>
 <!-- 等价于 -->
 <h2>{{message}}</h2>
```

### v-pre

+ v-pre用于跳过元素和它的子元素的编译过程，显示原始的Mustache标签：
  + 跳过不需要编译的节点，加快编译的速度

```html
 <template id="my-app">
   <h2 v-pre>{{message}}</h2>
 </template>
<!-- {{message}} -->
```

### v-cloak

+ 用于隐藏尚未完成编译的 DOM 模板。
  + **无需传入**
  + **详细信息**
+ **该指令只在没有构建步骤的环境下需要使用。**
  + 当使用直接在 DOM 中书写的模板时，可能会出现一种叫做“未编译模板闪现”的情况：用户可能先看到的是还没编译完成的双大括号标签，直到挂载的组件将它们替换为实际渲染的内容。
  + `v-cloak` 会保留在所绑定的元素上，直到相关组件实例被挂载后才移除。配合像 `[v-cloak] { display: none }` 这样的 CSS 规则，它可以在组件编译完毕前隐藏原始模板。

```css
[v-cloak] {
  display: none;
}
```

```html
<div v-cloak>
  {{ message }}
</div>
```

>直到编译完成前，`<div>` 将不可见。

## v-bind

动态的绑定一个或多个 attribute，也可以是组件的 prop。

- **缩写：**`:` 或者 `.` (当使用 `.prop` 修饰符)

- **期望：**`any (带参数) | Object (不带参数)`

- **参数：**`attrOrProp (可选的)`

- **修饰符**

  - `.camel` - 将短横线命名的 attribute 转变为驼峰式命名。
  - `.prop` - 强制绑定为 DOM property。`3.2+`
  - `.attr` - 强制绑定为 DOM attribute。`3.2+`

- **用途**

  当用于绑定 `class` 或 `style` attribute，`v-bind` 支持额外的值类型如数组或对象。详见下方的指南链接。

  在处理绑定时，Vue 默认会利用 `in` 操作符来检查该元素上是否定义了和绑定的 key 同名的 DOM property。如果存在同名的 property，则 Vue 会将它作为 DOM property 赋值，而不是作为 attribute 设置。这个行为在大多数情况都符合期望的绑定值类型，但是你也可以显式用 `.prop` 和 `.attr` 修饰符来强制绑定方式。有时这是必要的，特别是在和[自定义元素](https://cn.vuejs.org/guide/extras/web-components.html#passing-dom-properties)打交道时。

  当用于组件 props 绑定时，所绑定的 props 必须在子组件中已被正确声明。

  当不带参数使用时，可以用于绑定一个包含了多个 attribute 名称-绑定值对的对象。
  
- **用法** 

  - 动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

> 小知识: vue3 是允许template中有多个根元素

```html
<!-- vue2 template模板中只能有一个根元素 -->
<!-- vue3 是允许template中有多个根元素 -->
<template id="my-app">
    <!-- 1.v-bind的基本使用 -->
    <img v-bind:src="imgUrl" alt="">
    <a v-bind:href="link">百度一下</a>

    <!-- 2.v-bind提供一个语法糖 : -->
    <img :src="imgUrl" alt="">
    <img src="imgUrl" alt="">
</template>
```

### 基本使用

```vue
<template id="my-app">
    <!-- 1.v-bind的基本使用 -->
    <img v-bind:src="imgUrl" alt="">
    <a v-bind:href="link">百度一下</a>

    <!-- 2.v-bind提供一个语法糖 : -->
    <img :src="imgUrl" alt="">
    <!-- 以下报错 -->
    <img src="imgUrl" alt="">
</template>

<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                imgUrl: "https://avatars.githubusercontent.com/u/10335230?s=60&v=4",
                link: "https://www.baidu.com"
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

### 绑定 class

+ 在开发中,有时候我们的元素 class也是动态的,比如
  - 当数据为某个状态时,字体显示红色。
  - 当数据另一个状态时,字体显示黑色
+ 绑定class有两种方式：
  - 对象语法
  - 数组语法

#### 对象语法

+ 对象语法：我们可以传给 :class (v-bind:class 的简写) 一个对象，以动态地切换 class

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :class="className">哈哈哈哈</div>
    <!-- 对象语法: {'active': boolean} -->
    <div :class="{'active': isActive}">呵呵呵呵</div>
    <button @click="toggle">切换</button>

    <!-- 也可以有多个键值对 -->
    <div :class="{active: isActive, title: true}">呵呵呵呵</div>

    <!-- 默认的class和动态的class结合 -->
    <div class="abc cba" :class="{active: isActive, title: true}">
      呵呵呵呵
    </div>

    <!-- 将对象放到一个单独的属性中 -->
    <div class="abc cba" :class="classObj">呵呵呵呵</div>

    <!-- 将返回的对象放到一个methods(computed)方法中 -->
    <div class="abc cba" :class="getClassObj()">呵呵呵呵</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: "#my-app",
      data() {
        return { 
          className: "why",
          isActive: true,
          title: "abc",
          classObj: {
            active: true,
            title: true
          },
        };
      },
      methods: {
        toggle() {
          this.isActive = !this.isActive;
          this.classObj.active = !this.classObj.active;
        },
        getClassObj() {
          return this.classObj;
        }
      },
    };

    Vue.createApp(App).mount("#app");
  </script>

  <style>
    .active {
      color: red;
    }
  </style>
</body>
```

#### 数组语法

+ 绑定class – 数组语法
  - 数组语法：我们可以把一个数组传给 :class，以应用一个 class 列表

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :class="['abc', title]">哈哈哈哈</div>
  	<!-- class="abc cba active" -->
    <div :class="['abc', title, isActive ? 'active': '']">哈哈哈哈</div>
	<!-- 可以嵌套对象语法 -->
    <div :class="['abc', title, {active: isActive}]">哈哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          title: "cba",
          isActive: true
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### 绑定 style

+ 我们可以利用 `v-bind:style` 来绑定一些CSS内联样式
  + 这次因为某些样式我们需要根据数据动态来决定
  + 比如某段文字的颜色，大小等等
+ CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名
+ 绑定class有两种方式
  + 对象语法
  + 数组语法

> CSS property 名短横线分隔 (kebab-case，记得用引号括起来)

#### 对象语法

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- :style="{cssPropertyName: cssPropertyValue}" -->
    <div :style="{color: finalColor, 'font-size': '30px'}">哈哈哈哈</div>
    <div :style="{color: finalColor, fontSize: '30px'}">哈哈哈哈</div>
    <div :style="{color: finalColor, fontSize: finalFontSize + 'px'}">哈哈哈哈</div>

    <!-- 绑定一个data中的属性值, 并且是一个对象 -->
    <div :style="finalStyleObj">呵呵呵呵</div>
    <!-- 调用一个方法 -->
    <div :style="getFinalStyleObj()">呵呵呵呵</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          finalColor: 'red',
          finalFontSize: 50,
          finalStyleObj: {
            'font-size': '50px',
            fontWeight: 700,
            backgroundColor: 'red'
          }
        }
      },
      methods: {
        getFinalStyleObj() {
          return {
            'font-size': '50px',
            fontWeight: 700,
            backgroundColor: 'red'
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 数组语法

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :style="[style1Obj, style2Obj]">哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          style1Obj: {
            color: 'red',
            fontSize: '30px'
          },
          style2Obj: {
            textDecoration: "underline"
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body
```

### 动态绑定属性

+ 在某些情况下，我们属性的名称可能也不是固定的
  - 前端我们无论绑定src、href、class、style，属性名称都是固定的
  - 如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义
  - 这种绑定的方式，我们称之为 `动态绑定属性`；如下:

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- <div cba="kobe">哈哈哈</div> -->
    <div :[name]="value">哈哈哈</div>
    <img :src="" alt="">
    <a :href=""></a>
    <div :class></div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          name: "cba",
          value: "kobe"
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### 属性直接绑定一个对象

+ 如果我们希望将一个对象的所有属性，绑定到元素上的所有属性，应该怎么做呢？
  - 非常简单，我们可以直接使用 v-bind 绑定一个 对象
+ 案例：info对象会被拆解成div的各个属性

```html
<body>
  <div id="app"></div>

  <template id="my-app">
      <!-- <div name="why" age="18" height="1.88">哈哈哈哈</div> -->
    <div v-bind="info">哈哈哈哈</div>
    <div :="info">哈哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

## v-on

+ v-on绑定事件

  + 前面我们绑定了元素的内容和属性，在前端开发中另外一个非常重要的特性就是交互。
  + 在前端开发中，我们需要经常和用户进行各种各样的交互
    - 这个时候，我们就必须监听用户发生的事件，比如点击、拖拽、键盘事件等等
    - 在Vue中如何监听事件呢？使用v-on指令。

+ v-on的用法

  + 缩写：@
  + 预期：Function | Inline Statement | Object
  + 参数：event p 修饰符：
    + .stop - 调用 event.stopPropagation()。
    + .prevent - 调用 event.preventDefault()。
    + .capture - 添加事件侦听器时使用 capture 模式。
    + .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
    + .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
    + .once - 只触发一次回调。
    + .left - 只当点击鼠标左键时触发。
    + .right - 只当点击鼠标右键时触发。
    + .middle - 只当点击鼠标中键时触发。
    + .passive - { passive: true } 模式添加侦听器

  - 用法：绑定事件监听

> event 事件可以参考 https://developer.mozilla.org/en-US/docs/Web/Events

### 基本使用

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- 完整写法: v-on:监听的事件="methods中方法" -->
    <button v-on:click="btn1Click">按钮1</button>
    <div class="area" v-on:mousemove="mouseMove">div</div>
    <!-- 语法糖 -->
    <button @click="btn1Click">按钮1</button>
    <!-- 绑定一个表达式: inline statement -->
    <button @click="counter++">{{counter}}</button>
    <!-- 绑定一个对象 -->
    <div class="area" v-on="{click: btn1Click, mousemove: mouseMove}"></div>
    <div class="area" @="{click: btn1Click, mousemove: mouseMove}"></div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          counter: 100
        }
      },
      methods: {
        btn1Click() {
          console.log("按钮1发生了点击");
        },
        mouseMove() {
          console.log("鼠标移动");
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### 参数传递

+ 当通过methods中定义方法，以供 `@click`调用时，需要注意参数问题：
+ 情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。
  + 但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去
+ 情况二：如果需要同时传入某个参数，同时需要 `event` 时，可以通过 `$event` 传入事件。

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- 默认传入event对象, 可以在方法中获取 -->
    <button @click="btn1Click">按钮1</button>
    <!-- $event可以获取到事件发生时的事件对象 -->
    <button @click="btn2Click($event, 'coderwhy', 18)">按钮2</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World"
        }
      },
      methods: {
        btn1Click(event) {
          console.log(event);
        },
        btn2Click(event, name, age) {
          console.log(name, age, event);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

![image-20231019004604619](https://qiniu.waite.wang/202310190046946.png)

### v-on **的修饰符**

+ v-on支持修饰符，修饰符相当于对事件进行了一些特殊的处理：
  + .stop - 调用 event.stopPropagation()。
  + .prevent - 调用 event.preventDefault()。
  + .capture - 添加事件侦听器时使用 capture 模式。
  + .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  + .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
  + .once - 只触发一次回调。
  + .left - 只当点击鼠标左键时触发。
  + .right - 只当点击鼠标右键时触发。
  + .middle - 只当点击鼠标中键时触发。
  + .passive - { passive: true } 模式添加侦听器

> `stopPropagation` 是一个事件修饰符，用于阻止事件冒泡。在 Vue.js 中，当一个元素上的事件被触发时，它会先执行该元素上的事件处理函数，然后再冒泡到该元素的父元素，继续执行父元素的事件处理函数。使用 `stopPropagation` 可以阻止事件继续冒泡到父元素。在给元素绑定事件时，可以使用 `@click.stop` 来阻止 `click` 事件冒泡到父元素。

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div @click="divClick">
      <button @click="btnClick">按钮</button>
      <button @click.stop="btnClick">按钮</button>
    </div>
    <input type="text" @keyup.enter="enterKeyup">
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World"
        }
      },
      methods: {
        divClick() {
          console.log("divClick");
        },
        btnClick() {
          console.log('btnClick');
        },
        enterKeyup(event) {
          console.log("keyup", event.target.value);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

![image-20231019005251770](https://qiniu.waite.wang/202310190052768.png)

## 条件渲染

+ 在某些情况下，我们需要根据当前的条件决定某些元素或组件是否渲染，这个时候我们就需要进行条件判断了。
  - Vue提供了下面的指令来进行条件判断：
    - v-if
    - v-else
    - v-else-if
    - v-show

### 基本使用

+ v-if、v-else、v-else-if用于根据条件来渲染某一块的内容：
  - 这些内容只有在条件为true时，才会被渲染出来；
  - 这三个指令与JavaScript的条件语句if、else、else if类似；

```html
 <template id="my-app">
     <input type="text" v-model="score">
     <h2 v-if="score > 90">优秀</h2>
     <h2 v-else-if="score > 60">良好</h2>
     <h2 v-else>不及格</h2>
 </template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        score: 95
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

### template 和  v-if  结合使用

+ v-if的渲染原理：
  - v-if是惰性的；
  - 当条件为false时，其判断的内容完全不会被渲染或者会被销毁掉；
  - 当条件为true时，才会真正渲染条件块中的内容;
+ template元素
  - 因为v-if是一个指令，所以必须将其添加到一个元素上：
    - 但是如果我们希望切换的是多个元素呢？
    - 此时我们渲染div，但是我们并不希望div这种元素被渲染； 
    - 这个时候，我们可以选择使用template；
  - template元素可以当做不可见的包裹元素，并且在v-if上使用，但是最终template不会被渲染出来：
    - 有点类似于小程序中的block

```html
<body>  
  <div id="app"></div>

  <template id="my-app">
    <template v-if="isShowHa">
      <h2>哈哈哈哈</h2>
      <h2>哈哈哈哈</h2>
      <h2>哈哈哈哈</h2>
    </template>

    <template v-else>
      <h2>呵呵呵呵</h2>
      <h2>呵呵呵呵</h2>
      <h2>呵呵呵呵</h2>
    </template>

    <button @click="isShowHa = !isShowHa">切换</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          isShowHa: true
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### v-show

+ v-show和v-if的用法看起来是一致的，也是根据一个条件决定是否显示元素或者组件

```html
 <template id="my-app">
     <h2 v-show="isShow">哈哈哈哈</h2>
 </template>
```

### v-show和v-if的区别

+ 首先，在用法上的区别：
  - v-show是不支持template；
  - v-show不可以和v-else一起使用；
+ 其次，本质的区别：
  - v-show元素无论是否需要显示到浏览器上，它的DOM实际都是有渲染的，只是通过CSS的display属性来进行 切换；
  - v-if当条件为false时，其对应的元素压根不会被渲染到DOM中；
+ 开发中如何进行选择呢？
  - 如果我们的元素需要在显示和隐藏之间频繁的切换，那么使用v-show；
  - 如果不会频繁的发生切换，那么使用v-if；

```html
<template id="my-app">
  <h2 v-if="isShow">哈哈哈哈</h2>
  <h2 v-show="isShow">呵呵呵呵</h2>
</template>
```

![image-20231019222841008](https://qiniu.waite.wang/202310192228448.png)

## 列表渲染

+ 在真实开发中，我们往往会从服务器拿到一组数据，并且需要对其进行渲染。
  - 这个时候我们可以使用v-for来完成；
  - v-for类似于JavaScript的for循环，可以用于遍历一组数据；

### 基本使用

+ n v-for的基本格式是 "item in 数组"：
  - 数组通常是来自data或者prop，也可以是其他方式；
  - item是我们给每项元素起的一个别名，这个别名可以自定来定义；
+ 我们知道，在遍历一个数组的时候会经常需要拿到数组的索引：
  - 如果我们需要索引，可以使用格式： "(item, index) in 数组"；
  - 注意上面的顺序：数组元素项item是在前面的，索引项index是在后面的；

+ v-for支持的类型
  + v-for也支持遍历对象，并且支持有一二三个参数：
    - 一个参数： "value in object";
    - 二个参数： "(value, key) in object";
    - 三个参数： "(value, key, index) in object";
  + v-for同时也支持数字的遍历：
    - 每一个item都是一个数字；

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <h2>电影列表</h2>
    <ul>
      <!-- 遍历数组 -->
      <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <h2>个人信息</h2>
    <ul>
      <!-- 遍历对象 -->
      <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
    </ul>
    <h2>遍历数字</h2>
    <ul>
      <li v-for="(num, index) in 10">{{num}}-{{index}}</li>
    </ul>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          movies: [
            "星际穿越",
            "盗梦空间",
            "大话西游",
            "教父",
            "少年派"
          ],
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### template元素使用

+ 类似于v-if，你可以使用 template 元素来循环渲染一段包含多个元素的内容：
  - 我们使用template来对多个元素进行包裹，而不是使用div来完成；

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <ul>
      <template v-for="(value, key) in info">
        <li>{{key}}</li>
        <li>{{value}}</li>
        <li class="divider"></li>
      </template>
    </ul>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### 数组更新检测

+ Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：
  - push()
  - pop()
  - shift()
  - unshift()
  - splice()
  - sort()
  - reverse()
+ 替换数组的方法
  - 上面的方法会直接修改原来的数组，但是某些方法不会替换原来的数组，而是会生成新的数组，比如 filter()、 concat() 和 slice()。

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <h2>电影列表</h2>
    <ul>
      <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <input type="text" v-model="newMovie">
    <button @click="addMovie">添加电影</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          newMovie: "",
          movies: [
            "星际穿越",
            "盗梦空间",
            "大话西游",
            "教父",
            "少年派"
          ]
        }
      },
      methods: {
        addMovie() {
          this.movies.push(this.newMovie);
          this.newMovie = "";

          // filter 是过滤的意思, 下文中的代码的意思是: 过滤掉长度小于等于2的电影
          // this.movies = this.movies.filter(item => item.length > 2);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### v-for 中的 key 是什么作用？

> https://cn.vuejs.org/api/built-in-special-attributes.html#key

+ 在使用v-for进行列表渲染时，我们通常会给元素或者组件绑定一个key属性。

+ 这个key属性有什么作用呢？我们先来看一下官方的解释：

  + key属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes；

  + 如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法；

  + 而使用key时，它会基于key的变化重新排列元素顺序，并且会移除/销毁key不存在的元素；

+ 官方的解释对于初学者来说并不好理解，比如下面的问题：
  - 什么是新旧nodes，什么是VNode？
  - 没有key的时候，如何尝试修改和复用的？
  - 有key的时候，如何基于key重新排列的？

#### 认识 VNode

+ 我们先来解释一下VNode的概念：
  + 因为目前我们还没有比较完整的学习组件的概念，所以目前我们先理解HTML元素创建出来的VNode；
  + VNode的全称是Virtual Node，也就是虚拟节点；
  + 事实上，无论是组件还是元素，它们最终在Vue中表示出来的都是一个个VNode；
  + VNode的本质是一个JavaScript的对象；可以用于描述某一个标签/ 元素 的样子
  + 好处: 多平台的渲染, 跨平台(主要好处)

![image-20231020162812819](https://qiniu.waite.wang/202310201628396.png)

#### 虚拟 DOM

+ 如果我们不只是一个简单的div，而是有一大堆的元素，那么它们应该会形成一个 VNode Tree
+ 虚拟 DOM 与 真实 DOM 不一定一一对应

![image-20231020163154944](https://qiniu.waite.wang/202310201631261.png)

#### 插入 F 的案例

- 我们先来看一个案例：这个案例是当我点击按钮时会在中间插入一个f；

- 我们可以确定的是，这次更新对于ul和button是不需要进行更新，需要更新的是我们 li 的列表：

  + 在Vue中，对于相同父元素的子元素节点并不会重新渲染整个列 表；

  + 因为对于列表中 a、b、c、d它们都是没有变化的；

  + 在操作真实DOM的时候，我们只需要在中间插入一个 f 的 li 即可；

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <ul>
      <li v-for="item in letters" :key="item">{{item}}</li>
    </ul>
    <button @click="insertF">插入F元素</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          letters: ['a', 'b', 'c', 'd']
        }
      },
      methods: {
        insertF() {
          this.letters.splice(2, 0, 'f')
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

+ 那么Vue中对于列表的更新究竟是如何操作的呢？
  - Vue事实上会对于有key和没有key会调用两个不同的方法；

##### Vue源码对于key的判断

+ 有key，那么就使用 patchKeyedChildren方法；
+ 没有key，那么久使用 patchUnkeyedChildren方法；

![image-20231020164733530](https://qiniu.waite.wang/202310201647317.png)



> diff算法: diff 算法是指生成更新补丁的方式,主要应用于虚拟 DOM 树变化后,更新真实 DOM。所以 diff 算法一定存在这样一个过程:触发更新 → 生成补丁 → 应用补丁。

+ 没有key的操作过程

![image-20231022182341619](https://qiniu.waite.wang/202310221823690.png)

- 我们会发现上面的diff算法效率并不高：
  - c和d来说它们事实上并不需要有任何的改动；
  - 但是因为我们的c被f所使用了，所有后续所有的内容都要一次进行改动，并且最后进行新增；

![image-20231022182012027](https://qiniu.waite.wang/202310221820519.png)

+ 有key的操作过程-diff算法
  - 第一步的操作是从头开始进行遍历、比较：
    - a和b是一致的会继续进行比较；
    - c和f因为key不一致，所以就会break跳出循环
  - 第二步的操作是从尾部开始进行遍历、比较
  - 第三步是如果旧节点遍历完毕，但是依然有新的节点，那么就新增节点：
  - 第四步是如果新的节点遍历完毕，但是依然有旧的节点，那么就移除旧节点：
  - 第五步是最特色的情况，中间还有很多未知的或者乱序的节点：

![image-20231022182232738](https://qiniu.waite.wang/202310221822567.png)

![image-20231022182140133](https://qiniu.waite.wang/202310221821946.png)

![image-20231022182241976](https://qiniu.waite.wang/202310221822262.png)

![image-20231022182251535](https://qiniu.waite.wang/202310221822079.png)

![image-20231022182300781](https://qiniu.waite.wang/202310221823627.png)

##### 有无key的结论

+ 有无key的结论
  - 所以我们可以发现，Vue在进行diff算法的时候，会尽量利用我们的key来进行优化操作：
    - 在没有key的时候我们的效率是非常低效的；
    - 在进行插入或者重置顺序的时候，保持相同的key可以让diff算法更加的高效；