---
title: "系统学习 Vue -- 2-深入组件"
date: 2023-02-07T21:05:54+08:00
lastmod: 2023-03-02T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue"]
author: "Waite Wang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
description: "Vue 学习笔记-Vue组件化"
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

## 认识组件化开发

### 认识组件化开发

+ 人面对复杂问题的处理方式：
  + 任何一个人处理信息的逻辑能力都是有限的
  + 所以，当面对一个非常复杂的问题时，我们不太可能一次性搞定一大堆的内容。
  + 但是，我们人有一种天生的能力，就是将问题进行拆解。
  + 如果将一个复杂的问题，拆分成很多个可以处理的小问题，再将其放在整体当中，你会发现大的问题也会迎刃而解。
+ 组件化也是类似的思想：
  + 如果我们将一个页面中所有的处理逻辑 全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展；
  + 但如果，我们讲一个页面拆分成一个个 小的功能块，每个功能块完成属于自己 这部分独立的功能，那么之后整个页面 的管理和维护就变得非常容易了；
  + 如果我们将一个个功能块拆分后，就可 以像搭建积木一下来搭建我们的项目；
+ 现在可以说整个的大前端开发都是组件化的天下，无论从三大框架（Vue、React、Angular），还是跨平台方案 的Flutter，甚至是移动端都在转向组件化开发，包括小程序的开发也是采用组件化开发的思想
+ 所以，学习组件化最重要的是它的思想，每个框架或者平台可能实现方法不同，但是思想都是一样的。
+ 我们需要通过组件化的思想来思考整个应用程序：
  + 我们将一个完整的页面分成很多个组件；
  + 每个组件都用于实现页面的一个功能块；
  + 而每一个组件又可以进行细分；
  + 而组件本身又可以在多个地方进行复用；

### Vue的组件化

+ vue 项目起始文件 `createApp` 函数传入了一个对象App，这个对象其实本质上就是一个组件，也是我们应用程序的根 组件；
+ 组件化提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用；
+ 任何的应用都会被抽象成一颗组件树；

![image-20231031204213497](https://qiniu.waite.wang/202310312042389.png)

### 组件名称

+ 在通过 `app.componen` t注册一个组件的时候，第一个参数是组件的名称，定义组件名的方式有两种：
+ 方式一：使用 kebab-case（短横线分割符）
  + 当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case， 例如 `<my-component-name>`;
+ 方式二：使用 PascalCase（驼峰标识符）
  + 当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也 就是说  `<my-component-name>`和 `MyComponentName`  都是可接受的；
+ 在单文件组件和内联字符串模板中，我们都推荐这样做。但是，PascalCase 的标签名在 DOM 模板中是不可用的，详情参见 [DOM 内模板解析注意事项](https://cn.vuejs.org/guide/essentials/component-basics.html#in-dom-template-parsing-caveats)。
+ 为了方便，Vue 支持将模板中使用 kebab-case 的标签解析为使用 PascalCase 注册的组件。这意味着一个以 `MyComponent` 为名注册的组件，在模板中可以通过 `<MyComponent>` 或 `<my-component>` 引用。这让我们能够使用同样的 JavaScript 组件注册代码来配合不同来源的模板。

### 注册组件的方式

> <https://cn.vuejs.org/guide/components/registration.html#component-registration>

+ 如果我们现在有一部分内容（模板、逻辑等），我们希望将这部分内容抽取到一个独立的组件中去维护，这个时候 如何注册一个组件呢？
+ 我们先从简单的开始谈起，比如下面的模板希望抽离到一个单独的组件：

```html
<h2>{{title}}</h2>
<h2>{{message}}</h2>
```

+ 注册组件分成两种：
  + 全局组件：在任何其他的组件中都可以使用的组件；
  + 局部组件：只有在注册的组件中才能使用的组件；

#### 注册全局组件

+ 全局组件需要使用我们全局创建的app来注册组件；
+ 通过component方法传入组件名称、组件对象即可注册一个全局组件了；
+ 之后，我们可以在App组件的template中直接使用这个全局组件：

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <component-a></component-a>
  </template>
  
  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
    }

    // 使用 app.component() 注册一个全局组件, app.component() 是 Vue.createApp() 的一个方法
    const app = Vue.createApp(App);

    // app.component() 的第一个参数是组件的名称, 第二个参数是组件的配置对象
    app.component('component-a', {
      template: '<h2>{{ title }}</h2>',
      data() {
        return {
          title: '我是标题',
          desc: '我是内容, 哈哈哈哈哈'
        }
      },
      methods: {
        btnClick() {
          console.log('按钮的点击');
        }
      }
    });

    app.mount('#app');
  </script>
</body>
```

> 也可以

```html
<template id="component-a">
    <h2>{{ title }}</h2>
    <p>{{ desc }}</p>
</template>

app.component('component-a', {
 template: '#component-a',
 ...
}
```

#### 注册局部组件

全局注册虽然很方便，但有以下几个问题：

1. 全局注册，但并没有被使用的组件无法在生产打包时被自动移除 (也叫“tree-shaking”)。如果你全局注册了一个组件，即使它并没有被实际使用，它仍然会出现在打包后的 JS 文件中。
2. 全局注册在大型项目中使项目的依赖关系变得不那么明确。在父组件中使用子组件时，不太容易定位子组件的实现。和使用过多的全局变量一样，这可能会影响应用长期的可维护性。

相比之下，局部注册的组件需要在使用它的父组件中显式导入，并且只能在该父组件中使用。它的优点是使组件之间的依赖关系更加明确，并且对 tree-shaking 更加友好。

局部注册需要使用 `components` 选项：

```vue
<script>
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  }
}
</script>

<template>
  <ComponentA />
</template>
```

对于每个 `components` 对象里的属性，它们的 key 名就是注册的组件名，而值就是相应组件的实现。上面的例子中使用的是 ES2015 的缩写语法，等价于：

```javascript
export default {
  components: {
    ComponentA: ComponentA
  }
  // ...
}
```

> 请注意：**局部注册的组件在后代组件中并不可用**。在这个例子中，`ComponentA` 注册后仅在当前组件可用，而在任何的子组件或更深层的子组件中都不可用。

+ 全局组件往往是在应用程序一开始就会全局组件完成，那么就意味着如果某些组件我们并没有用到，也会一起被注 册：
  + 比如我们注册了三个全局组件：ComponentA、ComponentB、ComponentC；
  + 在开发中我们只使用了ComponentA、ComponentB，如果ComponentC没有用到但是我们依然在全局进行 了注册，那么就意味着类似于webpack这种打包工具在打包我们的项目时，我们依然会对其进行打包；
  + 这样最终打包出的JavaScript包就会有关于ComponentC的内容，用户在下载对应的JavaScript时也会增加包 的大小；
+ 所以在开发中我们通常使用组件的时候采用的都是局部注册：
  + 局部注册是在我们需要使用到的组件中，通过components属性选项来进行注册；
  + 比如之前的App组件中，我们有data、computed、methods等选项了，事实上还可以有一个components选项；
  + 该components选项对应的是一个对象，对象中的键值对是 组件的名称: 组件对象；

### Vue的开发模式

+ 目前我们使用vue的过程都是在html文件中，通过template编写自己的模板、脚本逻辑、样式等。
+ 但是随着项目越来越复杂，我们会采用组件化的方式来进行开发：
  + 这就意味着每个组件都会有自己的模板、脚本逻辑、样式等；
  + 当然我们依然可以把它们抽离到单独的js、css文件中，但是它们还是会分离开来；
  + 也包括我们的script是在一个全局的作用域下，很容易出现命名冲突的问题；
  + 并且我们的代码为了适配一些浏览器，必须使用ES5的语法；
  + 在我们编写代码完成之后，依然需要通过工具对代码进行构建、代码；
+ 所以在真实开发中，我们可以通过一个后缀名为 .vue 的single-file components (单文件组件) 来解决，并且可以使用 webpack 或者 vite 或者 rollup 等构建工具来对其进行处理。

> 比如: 我们可以单独抽离组件 componentA

```vue
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>{{ desc }}</p>
    <button @click="btnClick">按钮点击</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "我是标题",
      desc: "我是内容, 哈哈哈哈哈",
    };
  },
  methods: {
    btnClick() {
      console.log("按钮的点击");
    },
  },
};
</script>

<style scoped></style>
```

> 在这个组件中我们可以获得非常多的特性：
>
> + 代码的高亮；
> + ES6、CommonJS的模块化能力；
> + 组件作用域的CSS；
> + 可以使用预处理器来构建更加丰富的组件，比如TypeScript、Babel、Less、Sass等；

### 如何支持SFC

+ 如果我们想要使用这一 componentA.vue 文件，比较常见的是两种方式：
  + 方式一：使用Vue CLI来创建项目，项目会默认帮助我们配置好所有的配置选项，可以在其中直接使用.vue文件；
  + 方式二：自己使用webpack或rollup或 vite 这类打包工具，对其进行打包处理；

### 组件实例

#### `$refs`

> <https://cn.vuejs.org/api/component-instance.html#refs>

+ 某些情况下，我们在组件中想要直接获取到元素对象或者子组件实例：
  + 在Vue开发中我们是不推荐进行DOM操作的；
  + 这个时候，我们可以给元素或者组件绑定一个ref的attribute属性；
  + 在Vue 3中，$refs属性用于访问父组件中的子组件或DOM元素。它允许您以编程方式直接引用和操作这些组件或元素。

```vue
<template>
  <div>
    <child-component ref="childRef"></child-component>
    <button @click="logChildRef">Log Child Ref</button>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  methods: {
    logChildRef() {
      console.log(this.$refs.childRef);
    }
  }
}
</script>
```

在上面的示例中，我们通过使用ref属性给子组件命名为`childRef`，然后可以通过`this.$refs.childRef`来访问和操作子组件。在点击"Log Child Ref"按钮时，会将子组件实例打印到控制台。

### `$parent, $root`

> <https://cn.vuejs.org/api/component-instance.html#parent>

在Vue中，`$parent`和`$root`都是用于访问组件层级关系的特殊属性。

+ `$parent`属性用于访问当前组件的父组件实例。通过`this.$parent`可以访问父组件的属性和方法。
+ `$root`属性用于访问根组件实例。根组件是Vue应用的最顶层组件，通过`this.$root`可以访问根组件的属性和方法。

这些属性在处理组件之间的通信或访问全局状态时非常有用。

```vue
<template>
  <div>
    <child-component></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  mounted() {
    console.log(this.$parent); // 访问父组件实例
    console.log(this.$root); // 访问根组件实例
  }
}
</script>
```

在上面的示例中，父组件中通过使用`$parent`属性访问了父组件实例，使用​`$root`属性访问了根组件实例，并将它们打印到控制台。

### 组件的 v-model

> <https://cn.vuejs.org/guide/components/v-model.html#component-v-model>

`v-model` 可以在组件上使用以实现双向绑定。

首先让我们回忆一下 `v-model` 在原生元素上的用法：

```vue
<input v-model="searchText" />
```

在代码背后，模板编译器会对 `v-model` 进行更冗长的等价展开。因此上面的代码其实等价于下面这段：

```vue
<input
  :value="searchText"
  @input="searchText = $event.target.value"
/>
```

而当使用在一个组件上时，`v-model` 会被展开为如下的形式：

```vue
<CustomInput
  :model-value="searchText"
  @update:model-value="newValue => searchText = newValue"
/>
```

要让这个例子实际工作起来，`<CustomInput>` 组件内部需要做两件事：

1. 将内部原生 `<input>` 元素的 `value` attribute 绑定到 `modelValue` prop
2. 当原生的 `input` 事件触发时，触发一个携带了新值的 `update:modelValue` 自定义事件

```vue
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue']
}
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

现在 `v-model` 可以在这个组件上正常工作了：

```vue
<CustomInput v-model="searchText" />
```

另一种在组件内实现 `v-model` 的方式是使用一个可写的，同时具有 getter 和 setter 的 `computed` 属性。`get` 方法需返回 `modelValue` prop，而 `set` 方法需触发相应的事件：

```vue
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
}
</script>

<template>
  <input v-model="value" />
</template>
```

#### 多个 `v-model` 绑定

利用刚才在 [`v-model` 参数](https://cn.vuejs.org/guide/components/v-model.html#v-model-arguments)小节中学到的指定参数与事件名的技巧，我们可以在单个组件实例上创建多个 `v-model` 双向绑定。

组件上的每一个 `v-model` 都会同步不同的 prop，而无需额外的选项：

```vue
<UserName
  v-model:first-name="first"
  v-model:last-name="last"
/>
```

```vue
<script>
export default {
  props: {
    firstName: String,
    lastName: String
  },
  emits: ['update:firstName', 'update:lastName']
}
</script>

<template>
  <input
    type="text"
    :value="firstName"
    @input="$emit('update:firstName', $event.target.value)"
  />
  <input
    type="text"
    :value="lastName"
    @input="$emit('update:lastName', $event.target.value)"
  />
</template>
```

```vue
<script>
  export default {
    props: {
      modelValue: String,
      title: String 
    },
    emits: ["update:modelValue", "update:title"],
    computed: {
      value: {
        set(value) {
          this.$emit("update:modelValue", value);
        },
        get() {
          return this.modelValue;
        }
      },
      why: {
        set(why) {
          this.$emit("update:title", why);
        },
        get() {
          return this.title;
        }
      }
    }
  }
</script>
```

## 组件之间的通信

### 认识组件的嵌套

+ 在之前的案例中，我们只是创建了一个组件App；
+ 如果我们一个应用程序将所有的逻辑都放在一个组件中，那么这个组件就会变成非常的臃肿和难以维护；
+ 所以组件化的核心思想应该是对组件进行拆分，拆分成一个个小的组件；
+ 再将这些组件组合嵌套在一起，最终形成我们的应用程序；

> 我们来分析一下下面代码的嵌套逻辑，假如我们将所有的代码逻辑都放到一个App.vue组件中：

```vue
<template>
  <div>
    <h2>Header</h2>
    <h2>NavBar</h2>
  </div>
  <div>
    <h2>Banner</h2>
    <ul>
      <li>商品列表1</li>
      <li>商品列表2</li>
      <li>商品列表3</li>
      <li>商品列表4</li>
      <li>商品列表5</li>
    </ul>
  </div>
  <div>
    <h2>Footer</h2>
    <h2>免责声明</h2>
  </div>
</template>

<script>
export default {

};
</script>

<style scoped></style>
```

我们会发现，将所有的代码逻辑全部放到一个组件中，代码是非常的臃肿和难以维护的。并且在真实开发中，我们会有更多的内容和代码逻辑，对于扩展性和可维护性来说都是非常差的。

所有，在真实的开发中，我们会对组件进行拆分，拆分成一个个功能的小组件。

#### 组件的拆分

如上代码, 我们可以按照如下的方式进行拆分：

+ App.vue

  ```vue
  <template>
    <div id="app">
      <VueHeader></VueHeader>
      <VueMain></VueMain>
      <VueFooter></VueFooter>
    </div>
  </template>
  
  <script>
  import VueHeader from './VueHeader.vue';
  import VueMain from './VueMain.vue';
  import VueFooter from './VueFooter.vue';
  
  export default {
    name: 'App',
    components: {
      VueHeader,
      VueMain,
      VueFooter
    }
  };
  </script>
  
  <style scoped></style>
  ```

+ Header.vue组件

  ```vue
  <template>
    <div>
      <h2>Header</h2>
      <h2>NavBar</h2>
    </div>
  </template>
  ```

+ Main.vue组件：

  ```vue
  <template>
    <div>
      <vue-banner></vue-banner>
      <vue-product-list></vue-product-list>
    </div>
  </template>
  
  <script>
  import VueBanner from './VueBanner.vue';
  import VueProductList from './VueProductList.vue';
  
  export default {
    name: 'VueMain',
    components: {
      VueBanner,
      VueProductList
    }
  };
  </script>
  ```

+ Banner.vue组件：

  ```vue
  <template>
    <h2>Banner</h2>
  </template>
  ```

+ ProductList组件：

  ```vue
  <template>
    <ul>
      <li>商品列表1</li>
      <li>商品列表2</li>
      <li>商品列表3</li>
      <li>商品列表4</li>
      <li>商品列表5</li>
    </ul>
  </template>
  ```

+ Footer.vue组件：

  ```vue
  <template>
    <div>
      <h2>Footer</h2>
      <h2>免责声明</h2>
    </div>
  </template>
  ```

+ 按照如上的拆分方式后，我们开发对应的逻辑只需要去对应的组件编写就可。

### 组件的通信

上面的嵌套逻辑如下，它们存在如下关系：

+ App组件是Header、Main、Footer组件的父组件；
+ Main组件是Banner、ProductList组件的父组件；

在开发过程中，我们会经常遇到需要组件之间相互进行通信：

+ 比如App可能使用了多个Header，每个地方的Header展示的内容不同，那么我们就需要使用者传递给Header一些数据，让其进行展示；
+ 又比如我们在Main中一次性请求了Banner数据和ProductList数据，那么就需要传递给他们来进行展示；
+ 也可能是子组件中发生了事件，需要有父组件来完成某些操作，那就需要子组件向父组件传递事件；

> 父子组件之间如何进行通信呢？
>
> + 父组件传递给子组件：通过props属性；
> + 子组件传递给父组件：通过$emit触发事件；

![image-20231124000011715](https://qiniu.waite.wang/202311240000049.png)

#### 父传子

在开发中很常见的就是父子组件之间通信，比如父组件有一些数据，需要子组件来进行展示：

+ 这个时候我们可以通过props来完成组件之间的通信；

> 什么是 props?
>
> 在Vue3中，props是一种用于向组件传递数据的机制。它允许父组件向子组件传递数据，并在子组件中使用这些数据。
>
> 在Vue3中，每个组件都可以定义自己的props，并指定每个prop的类型、默认值和其他验证规则。当父组件向子组件传递数据时，子组件可以使用这些props来访问传递过来的数据。

##### props 的定义

在Vue3中，props可以使用两种方式来定义：

1. 字符串数组，数组中的字符串就是attribute的名称；
2. 对象类型，对象类型我们可以在指定attribute名称的同时，指定它需要传递的类型、是否是必须的、默认值等等；

###### 字符串数组

使用字符串数组的方式，可以简单地指定需要接收的属性名称。在这种情况下，属性类型默认为`any`。

在下面的示例中，父组件使用`message="Hello from parent"将message`属性作为字符串传递给子组件。在子组件中，使用`props`选项并传递一个字符串数组来定义`message`属性。这样子组件就可以使用`message`属性来访问父组件传递过来的数据了。

```vue
<!-- App.vue -->
<template>
  <div>
    <child-component message="Hello from parent"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: {
    ChildComponent
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    {{ message }}
  </div>
</template>

<script>
export default {
  props: ['message']
}
</script>
```

###### 对象类型

使用对象类型的方式，可以更详细地指定需要接收的属性名称、类型、是否必须、默认值等等。

在下面的示例中，父组件使用`:message="parentMessage"将parentMessage`属性作为字符串传递给子组件。在子组件中，使用`props`选项并传递一个对象来定义`message`属性。在这个对象中，我们指定了`type`为字符串、`required`为true、`default`为'Hello from child'、以及一个自定义的验证函数。

这样子组件就可以使用`message`属性来访问父组件传递过来的数据了。

```vue
<!-- App.vue -->
<template>
  <div>
    <child-component :message="parentMessage"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentMessage: 'Hello from parent'
    }
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    {{ message }}
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      // 必须传输?
      required: true,
      // 默认值
      default: 'Hello from child',
      // 传递的数据是否符合要求?
      validator: (value) => {
        return value.length > 0
      },
      info: String
    }
  }
}
</script>
```

###### 其他

1. **Type的类型都可以是哪些？**
    + String：用于指定字符串类型的属性。
    + Number：用于指定数字类型的属性。
    + Boolean：用于指定布尔类型的属性。
    + Array：用于指定数组类型的属性。
    + Object：用于指定对象类型的属性。
    + Date：用于指定日期类型的属性。
    + Function：用于指定函数类型的属性。
    + Symbol：用于指定符号类型的属性。
2. **对象类型的其他写法**

```javascript
props: {
  // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
  propA: Number,
  // 多个可能的类型
  propB: [String, Number],
  // 必填的字符串
  propC: {
    type: String,
    required: true
  },
  // 带有默认值的数字
  propD: {
    type: Number,
    default: 100
  },
  // 带有默认值的对象
  propE: {
    type: Object,
    // 对象或数组默认值必须从一个工厂函数获取, 因为每个实例需要维护一份被返回对象的独立的副本
    default: function () {
      return { message: 'hello' }
    }
  },
  // 自定义验证函数
  propF: {
    validator: function (value) {
      // 这个值必须匹配下列字符串中的一个
      return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
  },
  // 具有默认值的函数
  propG: {
    type: Function,
    // 对象或数组默认值必须从一个工厂函数获取
    default: function () {
      return { message: 'hello' }
    }
  }
} 
```

3. **Prop 的大小写命名(camelCase vs kebab-case)**

在Vue.js中，你可以使用驼峰式(camelCase)或短横线分隔(kebab-case)来命名你的props。然而，由于HTML属性不区分大小写，所以在模板中使用驼峰式命名的props时，需要转换为短横线分隔的形式。

例如，如果你在JavaScript中定义了一个名为`myProp`的prop，你需要在模板中使用`my-prop`来引用它。

这是一个例子：

```vue
<template>
  <div>
    <!-- 在模板中使用短横线分隔的形式 -->
    <child-component :my-prop="parentValue"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentValue: 'Hello from Parent Component'
    }
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    <!-- 使用prop的值 -->
    <h2>{{ myProp }}</h2>
  </div>
</template>

<script>
export default {
  props: {
    // 在JavaScript中使用驼峰式命名
    myProp: String
  }
}
</script>
```

在这个例子中，父组件将其数据`parentValue`传递给子组件的`myProp` prop。注意在父组件模板中，我们使用短横线分隔的形式`:my-prop`，而在子组件的JavaScript代码中，我们使用驼峰式命名`myProp`, 这也是官方推荐的写法。

##### 非 Prop 的Attribute

在Vue.js中，非prop的attribute是指那些被绑定到组件，但没有对应的prop定义的attribute。这些attribute会被添加到组件的根元素上。

例如，如果你有一个组件，它的模板是一个`<div>`元素，然后你在使用这个组件时添加了一个`class`或`style`属性，那么这个`class`或`style`属性就会被添加到`<div>`元素上，即使你没有在组件的props中定义它们。

这是一个例子：

```vue
<template>
  <div>
    <my-component id="abc" class="my-class" style="color: red;"></my-component>
  </div>
</template>

<script>
import MyComponent from './MyComponent.vue';

export default {
  components: {
    MyComponent
  }
}
</script>
```

在这个例子中，`id`, `class`和`style`就是非prop的attribute。它们会被添加到`MyComponent`的根元素上。

##### 禁用 Attribute 继承

如果你不希望非prop的attribute被添加到根元素上，你可以在组件中定义一个`inheritAttrs: false`选项。这样，非prop的attribute将只能通过`$attrs`变量来访问，而不会被添加到根元素上。

```javascript
export default {
  inheritAttrs: false
}
```

+ 禁用attribute继承的常见情况是需要将attribute应用于根元素之外的其他元素；
+ 我们可以通过 `$attrs`来访问所有的 `非props的attribute`；

```vue
<template>
  <div>
    <h2 v-bind="$attrs">{{title}}</h2>
    <p>{{content}}</p>
  </div>
</template>
```

+ 如上, `<h2>` 仍然会继承非prop的attribute

##### 多个根节点的attribute

> 多个根节点的attribute如果没有显示的绑定，那么会报警告，我们必须手动的指定要绑定到哪一个属性上：

```vue
<template>  
 <div :class="$attrs.class">
        我是NotPropAttribue组件1
    </div>  
 <div>
     我是NotPropAttribue组件2
    </div>  
 <div>
        我是NotPropAttribue组件3
    </div>
</template>
```

#### 子传父

什么情况下子组件需要传递内容到父组件呢？

+ 当子组件有一些事件发生的时候，比如在组件中发生了点击，父组件需要切换内容；
+ 子组件有一些内容想要传递给父组件的时候；

我们如何完成上面的操作呢？

+ 首先，我们需要在子组件中定义好在某些情况下触发的事件名称；
+ 其次，在父组件中以v-on的方式传入要监听的事件名称，并且绑定到对应的方法中；
+ 最后，在子组件中发生某个事件的时候，根据事件名称触发对应的事件；

> 以下是一个简单的示例

```vue
<!-- 子组件 -->
<template>
  <button @click="sendDataToParent">传递数据给父组件</button>
</template>

<script>
export default {
  methods: {
    sendDataToParent() {
      const data = 'Hello, parent!';
      // 传递参数给父组件
      this.$emit('data-to-parent', data);
    }
  }
};
</script>
```

```vue
<!-- 父组件 -->
<template>
  <div>
    <child-component @data-to-parent="handleDataFromChild"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  methods: {
    handleDataFromChild(data) {
      console.log(data); 
      // 在控制台打印子组件传递的数据
      // 在这里处理从子组件接收到的数据
    }
  }
};
</script>
```

##### `emits`

> 当使用Vue 3时，你可以使用emits选项来对子组件触发的自定义事件进行校验: 使用`emits`选项可以提供类型检查和错误提示，确保子组件只触发被允许的自定义事件。这有助于提高代码的可维护性和可靠性。以下是一个示例：

```javascript
export default {
  // 一般写法
  emits: ["add", "sub", "addN"],
  // 对象写法的目的是为了进行参数的验证
  emits: {
    add: null,
    sub: null,
    addN: (num, name, age) => {
      console.log(num, name, age);
      if (num > 10) {
        return true
      }
      return false;
    }
  }
}
```

### 非父子组件之间的通信

在开发中，我们构建了组件树之后，除了父子组件之间的通信之外，还会有非父子组件之间的通信。

这里我们主要讲两种方式：

+ Provide/Inject
+ Mitt全局事件总线；

#### Provide/Inject

Provide/Inject用于非父子组件之间共享数据：

+ 比如有一些深度嵌套的组件，子组件想要获取父组件的部分内容；
+ 在这种情况下，如果我们仍然将props沿着组件链逐级传递下去，就会非常的麻烦；

对于这种情况下，我们可以使用 `Provide` 和 `Inject` ：

+ 无论层级结构有多深，父组件都可以作为其所有子组件的依赖提供者；
+ 父组件有一个 `provide` 选项来提供数据；
+ 子组件有一个 `inject` 选项来开始使用这些数据；

![image-20231130195805276](https://qiniu.waite.wang/202311301958291.png)

实际上，你可以将依赖注入看作是“long range props”，除了：

+ 父组件不需要知道哪些子组件使用它 `provide`的 `property`
+ 子组件不需要知道 `inject`的 `property`来自哪里

```vue
<template>
  <div>
    <child-component></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  provide() {
    return {
      message: 'Hello from the parent component'
    };
  }
};
</script>
```

```vue
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  inject: ['message']
};
</script>
```

+ 当然, 我们也可以通过 this 获取到当前组件定义的 data

```javascript
import VueHome from './VueHome.vue';
import { computed } from 'vue';

export default {
  components: {
    VueHome
  },
  provide() {
    return {
      name: "why",
      age: 18,
      length: computed(() => this.names.length) // ref对象 .value
    }
  },
  data() {
    return {
      names: ["abc", "cba", "nba"]
    }
  },
  methods: {
    addName() {
      this.names.push("why");
      console.log(this.names);
    }
  }
}
```

#### 全局事件总线 mitt 库

在 Vue 3 中，全局事件总线是一种用于在不同组件之间进行通信的机制。它允许你在任何组件中触发事件并在其他组件中监听和响应这些事件。

> <https://cn.vuejs.org/api/application.html#app-config-globalproperties>

在 Vue 3 中，可以使用 `app.config.globalProperties` 来创建一个全局事件总线。通过将事件总线实例添加到全局属性中，你可以在任何组件中访问它，从而实现跨组件的事件通信。

以下是一个示例：

```javascript
// 在 main.js 中创建全局事件总线
import { createApp } from 'vue';

const app = createApp(App);

app.config.globalProperties.$bus = createEventBus();

app.mount('#app');
```

创建一个名为 `createEventBus` 的函数来创建事件总线实例：

```javascript
function createEventBus() {
  const listeners = {};

  function on(event, callback) {
    if (!listeners[event]) {
      listeners[event] = [];
    }
    listeners[event].push(callback);
  }

  function emit(event, ...args) {
    if (listeners[event]) {
      listeners[event].forEach(callback => {
        callback(...args);
      });
    }
  }

  return {
    on,
    emit
  };
}
```

现在，你可以在任何组件中使用 `$bus`来触发事件和监听事件：

```vue
<template>
  <div>
    <button @click="sendMessage">发送消息</button>
  </div>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$bus.emit('message', 'Hello from component A');
    }
  }
};
</script>
```

```vue
<template>
  <div>
    <button @click="sendMessage">发送消息</button>
  </div>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$bus.emit('message', 'Hello from component A');
    }
  }
};
</script>
```

在上述示例中，当点击按钮时，组件 A 使用`$bus.emit` 发送了一个名为 `'message'` 的事件，并传递了消息 `'Hello from component A'`。组件 B 使用 ​`$bus.on` 监听了 `'message'`事件，并将接收到的消息显示在页面上。

通过全局事件总线，你可以在不同组件之间进行简单而方便的通信，而无需显式地通过 props 或其他方式传递数据。
[TOC]

### 认识组件的嵌套

+ 在之前的案例中，我们只是创建了一个组件App；
+ 如果我们一个应用程序将所有的逻辑都放在一个组件中，那么这个组件就会变成非常的臃肿和难以维护；
+ 所以组件化的核心思想应该是对组件进行拆分，拆分成一个个小的组件；
+ 再将这些组件组合嵌套在一起，最终形成我们的应用程序；

> 我们来分析一下下面代码的嵌套逻辑，假如我们将所有的代码逻辑都放到一个App.vue组件中：

```vue
<template>
  <div>
    <h2>Header</h2>
    <h2>NavBar</h2>
  </div>
  <div>
    <h2>Banner</h2>
    <ul>
      <li>商品列表1</li>
      <li>商品列表2</li>
      <li>商品列表3</li>
      <li>商品列表4</li>
      <li>商品列表5</li>
    </ul>
  </div>
  <div>
    <h2>Footer</h2>
    <h2>免责声明</h2>
  </div>
</template>

<script>
export default {

};
</script>

<style scoped></style>
```

我们会发现，将所有的代码逻辑全部放到一个组件中，代码是非常的臃肿和难以维护的。并且在真实开发中，我们会有更多的内容和代码逻辑，对于扩展性和可维护性来说都是非常差的。

所有，在真实的开发中，我们会对组件进行拆分，拆分成一个个功能的小组件。

#### 组件的拆分

如上代码, 我们可以按照如下的方式进行拆分：

+ App.vue

  ```vue
  <template>
    <div id="app">
      <VueHeader></VueHeader>
      <VueMain></VueMain>
      <VueFooter></VueFooter>
    </div>
  </template>
  
  <script>
  import VueHeader from './VueHeader.vue';
  import VueMain from './VueMain.vue';
  import VueFooter from './VueFooter.vue';
  
  export default {
    name: 'App',
    components: {
      VueHeader,
      VueMain,
      VueFooter
    }
  };
  </script>
  
  <style scoped></style>
  ```

+ Header.vue组件

  ```vue
  <template>
    <div>
      <h2>Header</h2>
      <h2>NavBar</h2>
    </div>
  </template>
  ```

+ Main.vue组件：

  ```vue
  <template>
    <div>
      <vue-banner></vue-banner>
      <vue-product-list></vue-product-list>
    </div>
  </template>
  
  <script>
  import VueBanner from './VueBanner.vue';
  import VueProductList from './VueProductList.vue';
  
  export default {
    name: 'VueMain',
    components: {
      VueBanner,
      VueProductList
    }
  };
  </script>
  ```

+ Banner.vue组件：

  ```vue
  <template>
    <h2>Banner</h2>
  </template>
  ```

+ ProductList组件：

  ```vue
  <template>
    <ul>
      <li>商品列表1</li>
      <li>商品列表2</li>
      <li>商品列表3</li>
      <li>商品列表4</li>
      <li>商品列表5</li>
    </ul>
  </template>
  ```

+ Footer.vue组件：

  ```vue
  <template>
    <div>
      <h2>Footer</h2>
      <h2>免责声明</h2>
    </div>
  </template>
  ```

+ 按照如上的拆分方式后，我们开发对应的逻辑只需要去对应的组件编写就可。

### 组件的通信

上面的嵌套逻辑如下，它们存在如下关系：

+ App组件是Header、Main、Footer组件的父组件；
+ Main组件是Banner、ProductList组件的父组件；

在开发过程中，我们会经常遇到需要组件之间相互进行通信：

+ 比如App可能使用了多个Header，每个地方的Header展示的内容不同，那么我们就需要使用者传递给Header一些数据，让其进行展示；
+ 又比如我们在Main中一次性请求了Banner数据和ProductList数据，那么就需要传递给他们来进行展示；
+ 也可能是子组件中发生了事件，需要有父组件来完成某些操作，那就需要子组件向父组件传递事件；

> 父子组件之间如何进行通信呢？
>
> + 父组件传递给子组件：通过props属性；
> + 子组件传递给父组件：通过$emit触发事件；

![image-20231124000011715](https://qiniu.waite.wang/202311240000049.png)

#### 父传子

在开发中很常见的就是父子组件之间通信，比如父组件有一些数据，需要子组件来进行展示：

+ 这个时候我们可以通过props来完成组件之间的通信；

> 什么是 props?
>
> 在Vue3中，props是一种用于向组件传递数据的机制。它允许父组件向子组件传递数据，并在子组件中使用这些数据。
>
> 在Vue3中，每个组件都可以定义自己的props，并指定每个prop的类型、默认值和其他验证规则。当父组件向子组件传递数据时，子组件可以使用这些props来访问传递过来的数据。

##### props 的定义

在Vue3中，props可以使用两种方式来定义：

1. 字符串数组，数组中的字符串就是attribute的名称；
2. 对象类型，对象类型我们可以在指定attribute名称的同时，指定它需要传递的类型、是否是必须的、默认值等等；

###### 字符串数组

使用字符串数组的方式，可以简单地指定需要接收的属性名称。在这种情况下，属性类型默认为`any`。

在下面的示例中，父组件使用`message="Hello from parent"将message`属性作为字符串传递给子组件。在子组件中，使用`props`选项并传递一个字符串数组来定义`message`属性。这样子组件就可以使用`message`属性来访问父组件传递过来的数据了。

```vue
<!-- App.vue -->
<template>
  <div>
    <child-component message="Hello from parent"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: {
    ChildComponent
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    {{ message }}
  </div>
</template>

<script>
export default {
  props: ['message']
}
</script>
```

###### 对象类型

使用对象类型的方式，可以更详细地指定需要接收的属性名称、类型、是否必须、默认值等等。

在下面的示例中，父组件使用`:message="parentMessage"将parentMessage`属性作为字符串传递给子组件。在子组件中，使用`props`选项并传递一个对象来定义`message`属性。在这个对象中，我们指定了`type`为字符串、`required`为true、`default`为'Hello from child'、以及一个自定义的验证函数。

这样子组件就可以使用`message`属性来访问父组件传递过来的数据了。

```vue
<!-- App.vue -->
<template>
  <div>
    <child-component :message="parentMessage"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentMessage: 'Hello from parent'
    }
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    {{ message }}
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      // 必须传输?
      required: true,
      // 默认值
      default: 'Hello from child',
      // 传递的数据是否符合要求?
      validator: (value) => {
        return value.length > 0
      },
      info: String
    }
  }
}
</script>
```

###### 其他

1. **Type的类型都可以是哪些？**
    + String：用于指定字符串类型的属性。
    + Number：用于指定数字类型的属性。
    + Boolean：用于指定布尔类型的属性。
    + Array：用于指定数组类型的属性。
    + Object：用于指定对象类型的属性。
    + Date：用于指定日期类型的属性。
    + Function：用于指定函数类型的属性。
    + Symbol：用于指定符号类型的属性。
2. **对象类型的其他写法**

```javascript
props: {
  // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
  propA: Number,
  // 多个可能的类型
  propB: [String, Number],
  // 必填的字符串
  propC: {
    type: String,
    required: true
  },
  // 带有默认值的数字
  propD: {
    type: Number,
    default: 100
  },
  // 带有默认值的对象
  propE: {
    type: Object,
    // 对象或数组默认值必须从一个工厂函数获取, 因为每个实例需要维护一份被返回对象的独立的副本
    default: function () {
      return { message: 'hello' }
    }
  },
  // 自定义验证函数
  propF: {
    validator: function (value) {
      // 这个值必须匹配下列字符串中的一个
      return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
  },
  // 具有默认值的函数
  propG: {
    type: Function,
    // 对象或数组默认值必须从一个工厂函数获取
    default: function () {
      return { message: 'hello' }
    }
  }
} 
```

3. **Prop 的大小写命名(camelCase vs kebab-case)**

在Vue.js中，你可以使用驼峰式(camelCase)或短横线分隔(kebab-case)来命名你的props。然而，由于HTML属性不区分大小写，所以在模板中使用驼峰式命名的props时，需要转换为短横线分隔的形式。

例如，如果你在JavaScript中定义了一个名为`myProp`的prop，你需要在模板中使用`my-prop`来引用它。

这是一个例子：

```vue
<template>
  <div>
    <!-- 在模板中使用短横线分隔的形式 -->
    <child-component :my-prop="parentValue"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentValue: 'Hello from Parent Component'
    }
  }
}
</script>
```

```vue
<!--ChildComponent.vue -->
<template>
  <div>
    <!-- 使用prop的值 -->
    <h2>{{ myProp }}</h2>
  </div>
</template>

<script>
export default {
  props: {
    // 在JavaScript中使用驼峰式命名
    myProp: String
  }
}
</script>
```

在这个例子中，父组件将其数据`parentValue`传递给子组件的`myProp` prop。注意在父组件模板中，我们使用短横线分隔的形式`:my-prop`，而在子组件的JavaScript代码中，我们使用驼峰式命名`myProp`, 这也是官方推荐的写法。

##### 非 Prop 的Attribute

在Vue.js中，非prop的attribute是指那些被绑定到组件，但没有对应的prop定义的attribute。这些attribute会被添加到组件的根元素上。

例如，如果你有一个组件，它的模板是一个`<div>`元素，然后你在使用这个组件时添加了一个`class`或`style`属性，那么这个`class`或`style`属性就会被添加到`<div>`元素上，即使你没有在组件的props中定义它们。

这是一个例子：

```vue
<template>
  <div>
    <my-component id="abc" class="my-class" style="color: red;"></my-component>
  </div>
</template>

<script>
import MyComponent from './MyComponent.vue';

export default {
  components: {
    MyComponent
  }
}
</script>
```

在这个例子中，`id`, `class`和`style`就是非prop的attribute。它们会被添加到`MyComponent`的根元素上。

##### 禁用 Attribute 继承

如果你不希望非prop的attribute被添加到根元素上，你可以在组件中定义一个`inheritAttrs: false`选项。这样，非prop的attribute将只能通过`$attrs`变量来访问，而不会被添加到根元素上。

```javascript
export default {
  inheritAttrs: false
}
```

+ 禁用attribute继承的常见情况是需要将attribute应用于根元素之外的其他元素；
+ 我们可以通过 `$attrs`来访问所有的 `非props的attribute`；

```vue
<template>
  <div>
    <h2 v-bind="$attrs">{{title}}</h2>
    <p>{{content}}</p>
  </div>
</template>
```

+ 如上, `<h2>` 仍然会继承非prop的attribute

##### 多个根节点的attribute

> 多个根节点的attribute如果没有显示的绑定，那么会报警告，我们必须手动的指定要绑定到哪一个属性上：

```vue
<template>  
 <div :class="$attrs.class">
        我是NotPropAttribue组件1
    </div>  
 <div>
     我是NotPropAttribue组件2
    </div>  
 <div>
        我是NotPropAttribue组件3
    </div>
</template>
```

#### 子传父

什么情况下子组件需要传递内容到父组件呢？

+ 当子组件有一些事件发生的时候，比如在组件中发生了点击，父组件需要切换内容；
+ 子组件有一些内容想要传递给父组件的时候；

我们如何完成上面的操作呢？

+ 首先，我们需要在子组件中定义好在某些情况下触发的事件名称；
+ 其次，在父组件中以v-on的方式传入要监听的事件名称，并且绑定到对应的方法中；
+ 最后，在子组件中发生某个事件的时候，根据事件名称触发对应的事件；

> 以下是一个简单的示例

```vue
<!-- 子组件 -->
<template>
  <button @click="sendDataToParent">传递数据给父组件</button>
</template>

<script>
export default {
  methods: {
    sendDataToParent() {
      const data = 'Hello, parent!';
      // 传递参数给父组件
      this.$emit('data-to-parent', data);
    }
  }
};
</script>
```

```vue
<!-- 父组件 -->
<template>
  <div>
    <child-component @data-to-parent="handleDataFromChild"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  methods: {
    handleDataFromChild(data) {
      console.log(data); 
      // 在控制台打印子组件传递的数据
      // 在这里处理从子组件接收到的数据
    }
  }
};
</script>
```

##### `emits`

> 当使用Vue 3时，你可以使用emits选项来对子组件触发的自定义事件进行校验: 使用`emits`选项可以提供类型检查和错误提示，确保子组件只触发被允许的自定义事件。这有助于提高代码的可维护性和可靠性。以下是一个示例：

```javascript
export default {
  // 一般写法
  emits: ["add", "sub", "addN"],
  // 对象写法的目的是为了进行参数的验证
  emits: {
    add: null,
    sub: null,
    addN: (num, name, age) => {
      console.log(num, name, age);
      if (num > 10) {
        return true
      }
      return false;
    }
  }
}
```

### 动态组件

Vue.js 的动态组件是指可以根据不同的数据渲染不同的组件的功能。你可以使用 Vue.js 的 `<component>`元素来实现动态组件。

例如，你可以在父组件中定义一个数据属性，根据这个属性的值来决定渲染哪个子组件。然后，在模板中使用 `<component>`元素，并将该数据属性绑定到 is 属性上，这样就可以动态地渲染不同的子组件了。

```vue
<script>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
  
export default {
  components: {
    Home,
    Posts,
    Archive
  },
  data() {
    return {
      currentTab: 'Home',
      tabs: ['Home', 'Posts', 'Archive']
    }
  }
}
</script>

<template>
  <div class="demo">
    <button
       v-for="tab in tabs"
       :key="tab"
       :class="['tab-button', { active: currentTab === tab }]"
       @click="currentTab = tab"
     >
      {{ tab }}
    </button>
   <component :is="currentTab" class="tab"></component>
  </div>
</template>
```

### 非父子组件之间的通信

在开发中，我们构建了组件树之后，除了父子组件之间的通信之外，还会有非父子组件之间的通信。

这里我们主要讲两种方式：

+ Provide/Inject
+ Mitt全局事件总线；

#### Provide/Inject

Provide/Inject用于非父子组件之间共享数据：

+ 比如有一些深度嵌套的组件，子组件想要获取父组件的部分内容；
+ 在这种情况下，如果我们仍然将props沿着组件链逐级传递下去，就会非常的麻烦；

对于这种情况下，我们可以使用 `Provide` 和 `Inject` ：

+ 无论层级结构有多深，父组件都可以作为其所有子组件的依赖提供者；
+ 父组件有一个 `provide` 选项来提供数据；
+ 子组件有一个 `inject` 选项来开始使用这些数据；

![image-20231130195805276](https://qiniu.waite.wang/202311301958291.png)

实际上，你可以将依赖注入看作是“long range props”，除了：

+ 父组件不需要知道哪些子组件使用它 `provide`的 `property`
+ 子组件不需要知道 `inject`的 `property`来自哪里

```vue
<template>
  <div>
    <child-component></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  provide() {
    return {
      message: 'Hello from the parent component'
    };
  }
};
</script>
```

```vue
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  inject: ['message']
};
</script>
```

+ 当然, 我们也可以通过 this 获取到当前组件定义的 data

```javascript
import VueHome from './VueHome.vue';
import { computed } from 'vue';

export default {
  components: {
    VueHome
  },
  provide() {
    return {
      name: "why",
      age: 18,
      length: computed(() => this.names.length) // ref对象 .value
    }
  },
  data() {
    return {
      names: ["abc", "cba", "nba"]
    }
  },
  methods: {
    addName() {
      this.names.push("why");
      console.log(this.names);
    }
  }
}
```

#### 全局事件总线 mitt 库

在 Vue 3 中，全局事件总线是一种用于在不同组件之间进行通信的机制。它允许你在任何组件中触发事件并在其他组件中监听和响应这些事件。

> <https://cn.vuejs.org/api/application.html#app-config-globalproperties>

在 Vue 3 中，可以使用 `app.config.globalProperties` 来创建一个全局事件总线。通过将事件总线实例添加到全局属性中，你可以在任何组件中访问它，从而实现跨组件的事件通信。

以下是一个示例：

```javascript
// 在 main.js 中创建全局事件总线
import { createApp } from 'vue';

const app = createApp(App);

app.config.globalProperties.$bus = createEventBus();

app.mount('#app');
```

创建一个名为 `createEventBus` 的函数来创建事件总线实例：

```javascript
function createEventBus() {
  const listeners = {};

  function on(event, callback) {
    if (!listeners[event]) {
      listeners[event] = [];
    }
    listeners[event].push(callback);
  }

  function emit(event, ...args) {
    if (listeners[event]) {
      listeners[event].forEach(callback => {
        callback(...args);
      });
    }
  }

  return {
    on,
    emit
  };
}
```

现在，你可以在任何组件中使用 `$bus`来触发事件和监听事件：

```vue
<template>
  <div>
    <button @click="sendMessage">发送消息</button>
  </div>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$bus.emit('message', 'Hello from component A');
    }
  }
};
</script>
```

```vue
<template>
  <div>
    <button @click="sendMessage">发送消息</button>
  </div>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$bus.emit('message', 'Hello from component A');
    }
  }
};
</script>
```

在上述示例中，当点击按钮时，组件 A 使用`$bus.emit` 发送了一个名为 `'message'` 的事件，并传递了消息 `'Hello from component A'`。组件 B 使用 ​`$bus.on` 监听了 `'message'`事件，并将接收到的消息显示在页面上。

通过全局事件总线，你可以在不同组件之间进行简单而方便的通信，而无需显式地通过 props 或其他方式传递数据。

## 插槽

> <https://cn.vuejs.org/guide/components/slots.html#slots>

### 认识组件  Slot

+ 在开发中，我们会经常封装一个个可复用的组件：
  + 前面我们会通过props传递给组件一些数据，让组件来进行展示；
  + 但是为了让这个组件**具备更强的通用性**，我们**不能将组件中的内容限制为固定的div、span等等这些元素**；
  + 比如某种情况下我们使用组件，希望组件显示的是一个按钮，某种情况下我们使用组件希望显示的是一张图片；
  + 我们应该让使用者可以**决定某一块区域到底存放什么内容和元素**；
+ 举个栗子：假如我们定制一个通用的导航组件 - `NavBar`
  + 这个组件分成三块区域：左边-中间-右边，每块区域的内容是不固定；
  + 左边区域可能显示一个菜单图标，也可能显示一个返回按钮，可能什么都不显示；
  + 中间区域可能显示一个搜索框，也可能是一个列表，也可能是一个标题，等等；
  + 右边可能是一个文字，也可能是一个图标，也可能什么都不显示；

![在这里插入图片描述](https://qiniu.waite.wang/202312050058752.png)

### 如何使用插槽slot？

+ 这个时候我们就可以来定义插槽slot：
  + 插槽的使用过程其实是抽取共性、预留不同；
  + 我们会将共同的元素、内容依然在组件内进行封装；
  + 同时会将不同的元素使用 slot 作为占位，让外部决定到底显示什么样的元素；
+ 如何使用slot呢？
  + Vue中将 元素作为承载分发内容的出口；
  + 在封装组件中，使用特殊的元素就可以为封装组件开启一个插槽；
  + 该插槽插入什么内容取决于父组件如何使用；

### 插槽的基本使用

```vue
<script>
import FancyButton from './FancyButton.vue'
  
export default {
  components: { FancyButton }
}
</script>

<template>
  <FancyButton>
    Click me <!-- slot content -->
  </FancyButton>

  <FancyButton>
  </FancyButton>
</template>
```

```vue
<template>
  <button class="fancy-btn">
   <slot> Hello </slot>
  </button>
</template>

<style>
.fancy-btn {
  color: #fff;
  background: linear-gradient(315deg, #42d392 25%, #647eff);
  border: none;
  padding: 5px 10px;
  margin: 5px;
  border-radius: 8px;
  cursor: pointer;
}
</style>
```

`<slot>` 元素是一个**插槽出口** (slot outlet)，标示了父元素提供的**插槽内容** (slot content) 将在哪里被渲染。

![image-20231205010910410](https://qiniu.waite.wang/202312050109950.png)

通过使用插槽，`<FancyButton>` 仅负责渲染外层的 `<button>` (以及相应的样式)，而其内部的内容由父组件提供。

理解插槽的另一种方式是和下面的 JavaScript 函数作类比，其概念是类似的：

```javascript
// 父元素传入插槽内容
FancyButton('Click me!')

// FancyButton 在自己的模板中渲染插槽内容
function FancyButton(slotContent) {
  return `<button class="fancy-btn">
      ${slotContent}
    </button>`
}
```

插槽内容可以是任意合法的模板内容，不局限于文本。例如我们可以传入多个元素，甚至是组件：

```vue
<FancyButton>
  <span style="color:red">Click me!</span>
  <AwesomeIcon name="plus" />
</FancyButton>
```

#### 插槽的默认内容

在外部没有提供任何内容的情况下，可以为插槽指定默认内容。比如有这样一个 `<SubmitButton>` 组件：

```html
<button type="submit">
  <slot></slot>
</button>
```

如果我们想在父组件没有提供任何插槽内容时在 `<button>` 内渲染“Submit”，只需要将“Submit”写在 `<slot>` 标签之间来作为默认内容：

```html
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>
```

现在，当我们在父组件中使用 `<SubmitButton>` 且没有提供任何插槽内容时：

```html
<SubmitButton />
```

“Submit”将会被作为默认内容渲染：

```html
<button type="submit">Submit</button>
```

但如果我们提供了插槽内容：

```html
<SubmitButton>Save</SubmitButton>
```

那么被显式提供的内容会取代默认内容：

```html
<button type="submit">Save</button>
```

#### 多个插槽的效果

```vue
<template>
  <div>
    <my-slot-cpn>
      <button>我是按钮</button>
    </my-slot-cpn>

    <my-slot-cpn>
      我是普通的文本
    </my-slot-cpn>

    <my-slot-cpn></my-slot-cpn>

    <my-slot-cpn>
      <h2>哈哈哈</h2>
      <button>我是按钮</button>
      <strong>我是strong</strong>
    </my-slot-cpn>
  </div>
</template>

<script>
import MySlotCpn from './MySlotCpn.vue';

export default {
  components: {
    MySlotCpn,
  }
}
</script>
```

```vue
<template>
  <div>
    <h2>组件开始</h2>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <h2>组件结束</h2>
  </div>
</template>
```

![image-20231205011717002](https://qiniu.waite.wang/202312050117020.png)

### 具名插槽的使用

有时在一个组件中包含多个插槽出口是很有用的。举例来说，在一个 `<BaseLayout>` 组件中，有如下模板：

```html
<div class="container">
  <header>
    <!-- 标题内容放这里 -->
  </header>
  <main>
    <!-- 主要内容放这里 -->
  </main>
  <footer>
    <!-- 底部内容放这里 -->
  </footer>
</div>
```

对于这种场景，`<slot>` 元素可以有一个特殊的 attribute `name`，用来给各个插槽分配唯一的 ID，以确定每一处要渲染的内容：

```vue
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

这类带 `name` 的插槽被称为具名插槽 (named slots)。没有提供 `name` 的 `<slot>` 出口会隐式地命名为“default”。

在父组件中使用 `<BaseLayout>` 时，我们需要一种方式将多个插槽内容传入到各自目标插槽的出口。此时就需要用到**具名插槽**了：

要为具名插槽传入内容，我们需要使用一个含 `v-slot` 指令的 `<template>` 元素，并将目标插槽的名字传给该指令：

```vue
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```

`v-slot` 有对应的简写 `#`，因此 `<template v-slot:header>` 可以简写为 `<template #header>`。其意思就是“将这部分模板片段传入子组件的 header 插槽中”。

![image-20231205012048085](https://qiniu.waite.wang/202312050120758.png)

```vue
<script>
import BaseLayout from './BaseLayout.vue'
  
export default {
  components: {
    BaseLayout
  }
}
</script>

<template>
  <BaseLayout>
    <template #header>
      <h1>Here might be a page title</h1>
    </template>

    <template #default>
      <p>A paragraph for the main content.</p>
      <p>And another one.</p>
    </template>

    <template #footer>
      <p>Here's some contact info</p>
    </template>
  </BaseLayout>
</template>
```

```vue
<template>
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<style>
  footer {
    border-top: 1px solid #ccc;
    color: #666;
    font-size: 0.8em;
  }
</style>
```

使用 JavaScript 函数来类比可能更有助于你来理解具名插槽：

```javascript
// 传入不同的内容给不同名字的插槽
BaseLayout({
  header: `...`,
  default: `...`,
  footer: `...`
})

// <BaseLayout> 渲染插槽内容到对应位置
function BaseLayout(slots) {
  return `<div class="container">
      <header>${slots.header}</header>
      <main>${slots.default}</main>
      <footer>${slots.footer}</footer>
    </div>`
}
```

#### 动态插槽名

在Vue 3中，动态插槽名可以使用`v-slot`指令来实现。你可以将插槽名作为一个变量来传递给`v-slot`指令，以实现动态插槽名的效果。

例如，如果你有一个动态的插槽名变量`slotName`，你可以这样使用动态插槽名：

```vue
<template v-slot:[slotName]>
  <!-- 插槽内容 -->
</template>
```

这样，`slotName`变量的值将作为插槽名来动态指定插槽的位置。

![image-20231205012902405](https://qiniu.waite.wang/202312050129225.png)

### 渲染作用域

插槽内容可以访问到父组件的数据作用域，因为插槽内容本身是在父组件模板中定义的。举例来说：

```vue
<span>{{ message }}</span>
<FancyButton>{{ message }}</FancyButton>
```

这里的两个 `{{ message }}` 插值表达式渲染的内容都是一样的。

插槽内容**无法访问**子组件的数据。Vue 模板中的表达式只能访问其定义时所处的作用域，这和 JavaScript 的词法作用域规则是一致的。换言之：

> 父组件模板中的表达式只能访问父组件的作用域；子组件模板中的表达式只能访问子组件的作用域。

#### 作用域插槽

然而在某些场景下插槽的内容可能想要同时使用父组件域内和子组件域内的数据。要做到这一点，我们需要一种方法来让子组件在渲染时将一部分数据提供给插槽。

我们也确实有办法这么做！可以像对组件传递 props 那样，向一个插槽的出口上传递 attributes：

```vue
<script>
export default {
  data() {
    return {
      greetingMessage: 'hello'
    }
  }
}
</script>

<template>
  <div>
   <slot :text="greetingMessage" :count="1"></slot>
 </div>
</template>
```

当需要接收插槽 props 时，默认插槽和具名插槽的使用方式有一些小区别。下面我们将先展示默认插槽如何接受 props，通过子组件标签上的 `v-slot` 指令，直接接收到了一个插槽 props 对象：

```vue
<script>
import MyComponent from './MyComponent.vue'
  
export default {
  components: {
    MyComponent
  }
}
</script>

<template>
 <MyComponent v-slot="slotProps">
   {{ slotProps.text }} {{ slotProps.count }}
  </MyComponent>
</template>
```

![image-20231205013406501](https://qiniu.waite.wang/202312050134291.png)

子组件传入插槽的 props 作为了 `v-slot` 指令的值，可以在插槽内的表达式中访问。

你可以将作用域插槽类比为一个传入子组件的函数。子组件会将相应的 props 作为参数传给它：

```javascript
MyComponent({
  // 类比默认插槽，将其想成一个函数
  default: (slotProps) => {
    return `${slotProps.text} ${slotProps.count}`
  }
})

function MyComponent(slots) {
  const greetingMessage = 'hello'
  return `<div>${
    // 在插槽函数调用时传入 props
    slots.default({ text: greetingMessage, count: 1 })
  }</div>`
}
```

实际上，这已经和作用域插槽的最终代码编译结果、以及手动编写[渲染函数](https://cn.vuejs.org/guide/extras/render-function.html)时使用作用域插槽的方式非常类似了。

`v-slot="slotProps"` 可以类比这里的函数签名，和函数的参数类似，我们也可以在 `v-slot` 中使用解构：

```vue
<MyComponent v-slot="{ text, count }">
  {{ text }} {{ count }}
</MyComponent>
```

#### 具名作用域插槽

具名作用域插槽的工作方式也是类似的，插槽 props 可以作为 `v-slot` 指令的值被访问到：`v-slot:name="slotProps"`。

```vue
<script>
import MyComponent from './MyComponent.vue'
  
export default {
  components: {
    MyComponent
  }
}
</script>

<template>
  <div>
    <MyComponent>
      <template #header="slotProps">
        <h2>{{ slotProps.title }}</h2>
      </template>
      <template #content="slotProps">
        <p>{{ slotProps.text }}</p>
      </template>
    </MyComponent>
  </div>
</template>
```

```vue
<template>
  <div>
    <slot name="header" :title="title"></slot>
    <slot name="content" :text="content"></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'Hello',
      content: 'This is the content'
    };
  }
};
</script>
```

##### 独占默认插槽的缩写

+ 如果我们的插槽是默认插槽`default`，那么在使用的时候 `v-slot:default="slotProps"`可以简写为`v-slot=“slotProps”`

![在这里插入图片描述](https://qiniu.waite.wang/202312050153014.png)

+ 并且如果我们的插槽只有默认插槽时，组件的标签可以被当做插槽的模板来使用，这样，我们就可以将 `v-slot` 直接用在组件上

![在这里插入图片描述](https://qiniu.waite.wang/202312050153237.png)

#### 默认插槽和具名插槽混合

+ 但是，如果我们有默认插槽和具名插槽，那么按照完整的template来编写。

![在这里插入图片描述](https://qiniu.waite.wang/202312050153775.png)

+ 只要出现多个插槽，请始终为所有的插槽使用完整的基于

![在这里插入图片描述](https://qiniu.waite.wang/202312050154033.png)

## 动态组件

### 切换组件案例

+ 比如我们现在想要实现了一个功能：

  + 点击一个tab-bar，切换不同的组件显示；

  ![](https://qiniu.waite.wang/202312091618269.png)

+ 这个案例我们可以通过两种不同的实现思路来实现：

  + 方式一：通过v-if来判断，显示不同的组件；
  + 方式二：动态组件的方式；

#### v-if显示不同的组件

```vue
<template>
  <div>
    <button v-for="item in tabs" :key="item" @click="itemClick(item)" :class="{ active: currentTab === item }">
      {{ item }}
    </button>

    <!-- 1.v-if的判断实现 -->
    <template v-if="currentTab === 'home'">
      <home></home>
    </template>
    <template v-else-if="currentTab === 'about'">
      <about></about>
    </template>
    <template v-else>
      <category></category>
    </template>
  </div>
</template>
```

### 动态组件

Vue.js 的动态组件是指可以根据不同的数据渲染不同的组件的功能。你可以使用 Vue.js 的 `<component>`元素来实现动态组件。

例如，你可以在父组件中定义一个数据属性，根据这个属性的值来决定渲染哪个子组件。然后，在模板中使用 `<component>`元素，并将该数据属性绑定到 is 属性上，这样就可以动态地渲染不同的子组件了。

```vue
<script>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
  
export default {
  components: {
    Home,
    Posts,
    Archive
  },
  data() {
    return {
      currentTab: 'Home',
      tabs: ['Home', 'Posts', 'Archive']
    }
  }
}
</script>

<template>
  <div class="demo">
    <button
       v-for="tab in tabs"
       :key="tab"
       :class="['tab-button', { active: currentTab === tab }]"
       @click="currentTab = tab"
     >
      {{ tab }}
    </button>
   <component :is="currentTab" class="tab"></component>
  </div>
</template>
```

#### 动态组件的传值

```vue
<component :is="currentTab"
            name="coderwhy"
            :age="18"
            @pageClick="pageClick">
</component>
```

```javascript
pageClick() {
  console.log("page内部发生了点击");
}
```

```javascript
export default {
  name: "home",  
  props: {
    name: {
      type: String,
      default: ""
    },
    age: {
      type: Number,
      default: 0
    }
  }
}
```

### keep-alive

> <https://cn.vuejs.org/guide/built-ins/keep-alive.html#keepalive>

`<KeepAlive>` 是一个内置组件，它的功能是在多个组件间动态切换时缓存被移除的组件实例。

默认情况下，一个组件实例在被替换掉后会被销毁。这会导致它丢失其中所有已变化的状态——当这个组件再一次被显示时，会创建一个只带有初始状态的新实例。

在切换时创建新的组件实例通常是有意义的，但在这个例子中，我们的确想要组件能在被“切走”的时候保留它们的状态。要解决这个问题，我们可以用 `<KeepAlive>` 内置组件将这些动态组件包装起来：

```vue
<!-- 非活跃的组件将会被缓存！ -->
<KeepAlive>
  <component :is="activeComponent" />
</KeepAlive>
```

> 在 [DOM 内模板](https://cn.vuejs.org/guide/essentials/component-basics.html#in-dom-template-parsing-caveats)中使用时，它应该被写为 `<keep-alive>`。

#### 包含/排除

`<KeepAlive>` 默认会缓存内部的所有组件实例，但我们可以通过 `include` 和 `exclude` prop 来定制该行为。这两个 prop 的值都可以是一个以英文逗号分隔的字符串、一个正则表达式，或是包含这两种类型的一个数组：

```vue
<!-- 以英文逗号分隔的字符串 -->
<KeepAlive include="a,b">
  <component :is="view" />
</KeepAlive>

<!-- 正则表达式 (需使用 `v-bind`) -->
<KeepAlive :include="/a|b/">
  <component :is="view" />
</KeepAlive>

<!-- 数组 (需使用 `v-bind`) -->
<KeepAlive :include="['a', 'b']">
  <component :is="view" />
</KeepAlive>
```

> 它会根据组件的 [`name`](https://cn.vuejs.org/api/options-misc.html#name) 选项进行匹配，所以组件如果想要条件性地被 `KeepAlive` 缓存，就必须显式声明一个 `name` 选项。

#### 最大缓存实例数

我们可以通过传入 `max` prop 来限制可被缓存的最大组件实例数。`<KeepAlive>` 的行为在指定了 `max` 后类似一个 [LRU 缓存](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU))：如果缓存的实例数量即将超过指定的那个最大数量，则最久没有被访问的缓存实例将被销毁，以便为新的实例腾出空间。

```vue
<KeepAlive :max="10">
  <component :is="activeComponent" />
</KeepAlive>
```

## 异步组件

> <https://cn.vuejs.org/guide/components/async.html#async-components>

### 基本用法

在大型项目中，我们可能需要拆分应用为更小的块，并仅在需要时再从服务器加载相关组件。Vue 提供了 [`defineAsyncComponent`](https://cn.vuejs.org/api/general.html#defineasynccomponent) 方法来实现此功能：

```javascript
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...从服务器获取组件
    resolve(/* 获取到的组件 */)
  })
})
// ... 像使用其他一般组件一样使用 `AsyncComp`
```

如你所见，`defineAsyncComponent` 方法接收一个返回 Promise 的加载函数。这个 Promise 的 `resolve` 回调方法应该在从服务器获得组件定义时调用。你也可以调用 `reject(reason)` 表明加载失败。

[ES 模块动态导入](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)也会返回一个 Promise，所以多数情况下我们会将它和 `defineAsyncComponent` 搭配使用。类似 Vite 和 Webpack 这样的构建工具也支持此语法 (并且会将它们作为打包时的代码分割点)，因此我们也可以用它来导入 Vue 单文件组件：

```javascript
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
```

最后得到的 `AsyncComp` 是一个外层包装过的组件，仅在页面需要它渲染时才会调用加载内部实际组件的函数。它会将接收到的 props 和插槽传给内部组件，所以你可以使用这个异步的包装组件无缝地替换原始组件，同时实现延迟加载。

+ `defineAsyncComponent`接受两种类型的参数：

  + 类型一：工厂函数，该工厂函数需要返回一个Promise对象；

  ```javascript
  import { defineAsyncComponent } from 'vue';
  
  const AsyncCategory = defineAsyncComponent(() => import("./AsyncCategory.vue"))
  
  export default {
    components: {
      Home,
      AsyncCategory,
      Loading
    }
  }
  ```

  + 类型二：接受一个对象类型，对异步函数进行配置；
  + 异步操作不可避免地会涉及到加载和错误状态，因此 `defineAsyncComponent()` 也支持在高级选项中处理这些状态：

  ```javascript
  const AsyncCategory = defineAsyncComponent({
    loader: () => import("./AsyncCategory.vue"),
    loadingComponent: Loading,
    // errorComponent,
    // 在显示loadingComponent组件之前, 等待多长时间
    delay: 2000,
    // 加载失败后展示的组件
    errorComponent: ErrorComponent,
    // 如果提供了一个 timeout 时间限制，并超时了
    // 也会显示这里配置的报错组件，默认值是：Infinity
    timeout: 3000
  })
  ```

  ```typescript
  function defineAsyncComponent(
    source: AsyncComponentLoader | AsyncComponentOptions
  ): Component
  
  type AsyncComponentLoader = () => Promise<Component>
  
  interface AsyncComponentOptions {
    loader: AsyncComponentLoader
    loadingComponent?: Component
    errorComponent?: Component
    delay?: number
    timeout?: number
    suspensible?: boolean
    onError?: (
      error: Error,
      retry: () => void,
      fail: () => void,
      attempts: number
    ) => any
  } 
  ```

  + 如果提供了一个加载组件，它将在内部组件加载时先行显示。在加载组件显示之前有一个默认的 200ms 延迟——这是因为在网络状况较好时，加载完成得很快，加载组件和最终组件之间的替换太快可能产生闪烁，反而影响用户感受。
  + 如果提供了一个报错组件，则它会在加载器函数返回的 Promise 抛错时被渲染。你还可以指定一个超时时间，在请求耗时超过指定时间时也会渲染报错组件。

### 搭配 Suspense 使用

> <https://cn.vuejs.org/guide/built-ins/suspense.html>
>
> `<Suspense>` 是一项实验性功能。它不一定会最终成为稳定功能，并且在稳定之前相关 API 也可能会发生变化。

`<Suspense>` 是一个内置组件，用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态。

`<Suspense>` 组件有两个插槽：`#default` 和 `#fallback`。两个插槽都只允许**一个**直接子节点。在可能的时候都将显示默认槽中的节点。否则将显示后备槽中的节点。

```vue
<suspense>
  <!-- 具有深层异步依赖的组件 -->
  <template #default>
    <async-category></async-category>
  </template>
    
  <!-- 在 #fallback 插槽中显示 “正在加载中” -->
  <template #fallback>
    <loading></loading>
  </template>
</suspense>
```

在初始渲染时，`<Suspense>` 将在内存中渲染其默认的插槽内容。如果在这个过程中遇到任何异步依赖，则会进入**挂起**状态。在挂起状态期间，展示的是后备内容。当所有遇到的异步依赖都完成后，`<Suspense>` 会进入**完成**状态，并将展示出默认插槽的内容。

如果在初次渲染时没有遇到异步依赖，`<Suspense>` 会直接进入完成状态。

进入完成状态后，只有当默认插槽的根节点被替换时，`<Suspense>` 才会回到挂起状态。组件树中新的更深层次的异步依赖**不会**造成 `<Suspense>` 回退到挂起状态。

发生回退时，后备内容不会立即展示出来。相反，`<Suspense>` 在等待新内容和异步依赖完成时，会展示之前 `#default` 插槽的内容。这个行为可以通过一个 `timeout` prop 进行配置：在等待渲染新内容耗时超过 `timeout` 之后，`<Suspense>` 将会切换为展示后备内容。若 `timeout` 值为 `0` 将导致在替换默认内容时立即显示后备内容。

#### 和其他组件结合

我们常常会将 `<Suspense>` 和 [``](https://cn.vuejs.org/guide/built-ins/transition.html)、[``](https://cn.vuejs.org/guide/built-ins/keep-alive.html) 等组件结合。要保证这些组件都能正常工作，嵌套的顺序非常重要。

另外，这些组件都通常与 [Vue Router](https://router.vuejs.org/zh/) 中的 `<RouterView>` 组件结合使用。

下面的示例展示了如何嵌套这些组件，使它们都能按照预期的方式运行。若想组合得更简单，你也可以删除一些你不需要的组件：

```vue
<RouterView v-slot="{ Component }">
  <template v-if="Component">
    <Transition mode="out-in">
      <KeepAlive>
        <Suspense>
          <!-- 主要内容 -->
          <component :is="Component"></component>

          <!-- 加载中状态 -->
          <template #fallback>
            正在加载...
          </template>
        </Suspense>
      </KeepAlive>
    </Transition>
  </template>
</RouterView>
```

Vue Router 使用动态导入对[懒加载组件](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html)进行了内置支持。这些与异步组件不同，目前他们不会触发 `<Suspense>`。但是，它们仍然可以有异步组件作为后代，这些组件可以照常触发 `<Suspense>`。
