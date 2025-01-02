---
title: "鸿蒙学习笔记1"
date: 2024-03-23T16:04:23+08:00
categories: ["Harmony"]
tags: ["Harmony"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

## 编辑器安装

### 安装

1. [下载安装包](https://developer.huawei.com/consumer/cn/deveco-studio/#download)
2. 正常安装包, 下一步安装

![image-20240417121723826](https://qiniu.waite.wang/202404171217774.png)

3. 配置开发环境

![image-20240417121810196](https://qiniu.waite.wang/202404171218425.png)

选择 Agree, 进入配置选择页面, 选择不导入配置

![image-20240417121841096](https://qiniu.waite.wang/202404171218109.png)

选择要安装的Node.is和hpm位置，其中ohpm是Open Harmony Package Management的缩写，也就是类似npm的包管理工具。这里有几点注意事项:

+ 如果电脑上已经有Node.js，但是版本不一致，建议选择让工具重新安装
+ 如果电脑上已经有Node.js，并且版本一致，可以选择Local，指定node目录即可
+ 如果电脑上对Node.is做了一些特殊的options配置，建议先移除
+ 配置目录中不要出现中文、特殊字符，建议用默认路径

![image-20240417121951184](https://qiniu.waite.wang/202404171219038.png)

选择Next后，进入HarmonyOs的SDK安装目录选择页面以及同意协议, 配置目录/ 同意协议后下一步即可

### 环境错误处理

在安装的过程中，如果出现类似下面的错误

![image-20240417122147373](https://qiniu.waite.wang/202404171221142.png)

一般就是因为你本地原本的Node.is配置异常导致的，建议清理环境变量中对于Node.is的配置之后再重试
重试时无需重新安装，而是再次打开DevEco Studio，点击界面左下方的 ? 按钮: 选择第一个 Diagnose Development Environment 进入诊断页面, 这里会提示安装出现问题的选项，点击异常项后面的set it up now即可重新安装
如果所有问题都已经解决，最终重试后等待所有项都是 √即可

![image-20240417122238577](https://qiniu.waite.wang/202404171222747.png)

![image-20240417122305876](https://qiniu.waite.wang/202404171223032.png)

### 中文设置

设置->插件->已安装->其他工具->chinese->启用即可

![image-20240417122439395](https://qiniu.waite.wang/202404171224670.png)

### 创建项目

Create Project -> Empty Ability -> 按要求填写目录即可

此时项目内已有 Hello World 基础代码, 点右侧预览器即可预览效果

![image-20240417122904349](https://qiniu.waite.wang/202404171229268.png)

### 模拟器安装

我们也可以利用设备模拟器来查看更真实的效果。不过需要先配置模拟器

首先，选择主菜单中的Tools，找到其中的Device Manager，即设备管理

![image-20240417122932610](https://qiniu.waite.wang/202404171229476.png)

设备可以是 远端设备 ，也可以是 本地设备 ，我们以本地设备为例。

默认本地没有任何设备，选择install来安装一个

![image-20240417122953229](https://qiniu.waite.wang/202404171229123.png)

首次点击时，会弹出一个窗口，下载必要的SDK依赖, 安装完下一步即可

进入创建模拟器页面，选择New Emulator:

![image-20240417123032281](https://qiniu.waite.wang/202404171230423.png)

![image-20240417123043858](https://qiniu.waite.wang/202404171230731.png)

选择api9版本，不过需要注意，首次进入此页面，需要下载手机设备需要的系统，大概2.2G，需要耐心等待:

![image-20240417123103057](https://qiniu.waite.wang/202404171231835.png)

![image-20240417123115419](https://qiniu.waite.wang/202404171231885.png)

创建完成后，在设备列表中会出现一个本地设备，点击后面的运行按钮即可启动设备模拟器

![image-20240417123136573](https://qiniu.waite.wang/202404171231949.png)

启动后如下

![image-20240417123155182](https://qiniu.waite.wang/202404171231029.png)

然后，在应用启动位置选择刚刚添加的模拟器, 然后点击启动即可

![image-20240417123220059](https://qiniu.waite.wang/202404171232015.png)

![image-20240417123227197](https://qiniu.waite.wang/202404171232136.png)

效果如下

![image-20240417123240912](https://qiniu.waite.wang/202404171232051.png)

### Stage 与 FA 模型的区别

#### FA模型：早期的探索

FA模型是HarmonyOS早期版本开始支持的应用模型。它通过PageAbility、ServiceAbility和DataAbility三种组件，为开发者提供了构建应用的基础。FA模型的特点是每个组件运行在自己的进程中，拥有独立的JS VM引擎实例，这使得组件之间相互隔离，但也带来了一定的内存占用。

随着HarmonyOS的演进，特别是1+8+N的战略被提出，多设备和多窗口形态成为主流，此时FA模型在处理复杂应用时存在一定的局限性， FA模型逐渐不再被主推。

#### Stage模型：未来的主流

为了更好地适应复杂应用的开发需求，HarmonyOS 3.1 Developer Preview版本引入了Stage模型。Stage模型通过AbilityStage、WindowStage等类，将应用组件和Window窗口作为“舞台”进行管理，从而提供了更加灵活和高效的开发方式。

Stage模型的设计出发点是为了复杂应用而设计，它通过以下几个方面实现了对复杂应用的优化：

1. 共享ArkTS引擎实例：在Stage模型中，多个应用组件共享同一个ArkTS引擎实例，这使得组件之间可以方便地共享对象和状态，同时减少了内存占用。

2. 面向对象的开发方式：Stage模型采用面向对象的开发方式，提高了代码的可读性、易维护性和可扩展性。

3. 支持多设备和多窗口形态：应用组件管理和窗口管理在架构层面解耦，使得应用组件可以在不同设备上使用同一套生命周期，便于系统扩展窗口形态。

4. 平衡应用能力和系统管控成本：Stage模型重新定义了应用能力的边界，提供了特定场景的应用组件，规范化了后台进程管理，防止了恶意应用行为。

### （Stage模型）目录结构

![](https://qiniu.waite.wang/202404181340550.png)

+ AppScope > app.json5：应用的全局配置信息。
+ entry：HarmonyOS工程模块，编译构建生成一个HAP包。
  + src > main > ets：用于存放ArkTS源码。
src > main > ets > entryability：应用/服务的入口。
  + src > main > ets > pages：应用/服务包含的页面
  + src > main > resources：用于存放应用/服务所用到的资源文件，如图形、多媒体、字符串、布局文件等。关于资源文件，详见资源分类与访问。
  + src > main > module.json5：Stage模型模块配置文件。主要包含HAP包的配置信息、应用/服务在具体设备上的配置信息以及应用/服务的全局配置信息。具体的配置文件说明，详见[module.json5配置文件](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/module-configuration-file-0000001427744540-V2)。
  + build-profile.json5：当前的模块信息、编译信息配置项，包括buildOption、targets配置等。其中targets中可配置当前运行环境，默认为HarmonyOS。
  + hvigorfile.ts：模块级编译构建任务脚本，开发者可以自定义相关任务和代码实现。
+ oh_modules：用于存放三方库依赖信息。关于原npm工程适配ohpm操作，请参考历史工程迁移。
+ build-profile.json5：应用级配置信息，包括签名、产品配置等。
+ hvigorfile.ts：应用级编译构建任务脚本。

## ArkUI 组件

### 基础组件

#### Text

![](https://qiniu.waite.wang/202404181859349.png)

```ts
Text($r("app.string.module_desc"))
```

> 文本会先在对应国家的资源文件中查找，如果没有找到，会在 base 全局资源文件中查找。

![](https://qiniu.waite.wang/202404181902495.png)

![](https://qiniu.waite.wang/202404181903070.png)

#### TextInput

![](https://qiniu.waite.wang/202404182154723.png)

#### Button

![](https://qiniu.waite.wang/202404182156605.png)

方法1： Button(options?: {type?: ButtonType, stateEffect?: boolean})

方法2： Button(label?: ResourceStr, options?: { type?: ButtonType, stateEffect?: boolean })

使用文本内容创建相应的按钮组件，此时Button无法包含子组件。

#### Image

图片组件，支持本地图片和网络图片的渲染展示。

> Image(src: string | PixelMap | Resource)
> 图片的数据源，支持本地图片和网络图片。当使用相对路径引用图片资源时，例如Image("common/test.jpg")，不支持该Image组件被跨包/跨模块调用，建议使用$r方式来管理需全局使用的图片资源。
>
> + 支持的图片格式包括png、jpg、bmp、svg和gif。
> + 支持Base64字符串。格式data:image/[png|jpeg|bmp|webp];base64,[base64 data], 其中[base64 data]为Base64字符串数据。
> + 支持dataability://路径前缀的字符串，用于访问通过data ability提供的图片路径。

![](https://qiniu.waite.wang/202404181838112.png)

##### 从网络加载图片

使用网络图片时，需要申请权限ohos.permission.INTERNET。具体申请方式请参考[权限申请声明](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V3/accesstoken-guidelines-0000001333800609-V3#ZH-CN_TOPIC_0000001333800609__%E6%9D%83%E9%99%90%E7%94%B3%E8%AF%B7%E5%A3%B0%E6%98%8E)。

```json
// entry/src/main/module.json5

{
  "module": {
    "reqPermissions": [
      {
        "name": "ohos.permission.INTERNET",
      }
    ]
  }
}
```

通过以下代码，可以加载网络图片：

```ts
Image("https://example.com/image.jpg")
```

##### 从本地加载

```ts
Image($r("app.media.icon"))

Image($rawfile("abstract.png"))
```

#### Slider

```ts
Slider({
  min: 0, // 最小值
  max: 100, // 最大值
  value: 40, // 当前值
  step: 10, // 步长
  style: SliderStyle.InSet, // Outer Slider
  direction: Axis.Horizontal, // 方向
  reverse: false // 方向滑动
})
  .showTips(true) // 展示 value 百分比
  .margin({
    top: 20
  })
```

### 容器组件

<https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V3/arkui-ts-container-components-0000001334734185-V3>

#### Column

沿垂直方向布局的容器。

```ts
@Entry
@Component
struct ColumnExample {
  build() {
    Column() {
        Text('space').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Column({ space: 5 }) {
          Column().width('100%').height(30).backgroundColor(0xAFEEEE)
          Column().width('100%').height(30).backgroundColor(0x00FFFF)
        }.width('90%').height(100).border({ width: 1 })

        Text('alignItems(Start)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Column() {
          Column().width('50%').height(30).backgroundColor(0xAFEEEE)
          Column().width('50%').height(30).backgroundColor(0x00FFFF)
        }.alignItems(HorizontalAlign.Start).width('90%').border({ width: 1 })

        Text('alignItems(End)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Column() {
          Column().width('50%').height(30).backgroundColor(0xAFEEEE)
          Column().width('50%').height(30).backgroundColor(0x00FFFF)
        }.alignItems(HorizontalAlign.End).width('90%').border({ width: 1 })

        Text('justifyContent(Center)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Column() {
          Column().width('30%').height(30).backgroundColor(0xAFEEEE)
          Column().width('30%').height(30).backgroundColor(0x00FFFF)
        }.height('15%').border({ width: 1 }).justifyContent(FlexAlign.Center)

        Text('justifyContent(End)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Column() {
          Column().width('30%').height(30).backgroundColor(0xAFEEEE)
          Column().width('30%').height(30).backgroundColor(0x00FFFF)
        }.height('15%').border({ width: 1 }).justifyContent(FlexAlign.End)
    }.width('100%').padding({ top: 5 })
  }
}
```

![](https://qiniu.waite.wang/202404182315467.png)

#### Row

沿水平方向布局容器。

```ts
// xxx.ets
@Entry
@Component
struct RowExample {
  build() {
    Column({ space: 5 }) {
      Text('space').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Row({ space: 5 }) {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').height(107).border({ width: 1 })

        Text('alignItems(Top)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.alignItems(VerticalAlign.Top).height('15%').border({ width: 1 })

        Text('alignItems(Center)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.alignItems(VerticalAlign.Center).height('15%').border({ width: 1 })

        Text('justifyContent(End)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').border({ width: 1 }).justifyContent(FlexAlign.End)

        Text('justifyContent(Center)').fontSize(9).fontColor(0xCCCCCC).width('90%')
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').border({ width: 1 }).justifyContent(FlexAlign.Center)
    }.width('100%')
  }
}
```

![](https://qiniu.waite.wang/202404182315827.png)

### 案例

```ts
// xxx.ets
@Entry
@Component
struct Index {
  @State imgWidth: number = 30

  build() {
    Row() {
      Column() {
        Image($r("app.media.icon"))
          .width(this.imgWidth)
          .interpolation(ImageInterpolation.High)

        Text(`图片宽度: ${this.imgWidth}`)
          .margin(20)

        TextInput({
          placeholder: "请输入图片宽度",
          text: this.imgWidth.toString()
        })
          .width(200)
          .type(InputType.Number)
          .onChange(value => {
            this.imgWidth = parseInt(value)
          })
        Row() {
          Button("缩小")
            .width(80)
            .onClick(() => {
              if (this.imgWidth >= 10) {
                this.imgWidth -= 10
              }
            })

          Button("放大")
            .width(80)
            .onClick(() => {
              if (this.imgWidth <= 300) {
                this.imgWidth += 10
              }
            })
        }
        .margin({
          top: 20
        })

        Slider({
          min: 0, // 最小值
          max: 100, // 最大值
          value: 40, // 当前值
          step: 10, // 步长
          style: SliderStyle.InSet, // Outer Slider
          direction: Axis.Horizontal, // 方向
          reverse: false // 方向滑动
        })
          .showTips(true) // 展示 value 百分比
          .margin({
            top: 20
          })
          .width("80%")
      }
      .width("100%")
    }
    .height("100%")
  }
}
```

![](https://qiniu.waite.wang/202404182317729.png)

### 循环控制

<https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/arkts-rendering-control-foreach-0000001524537153-V2>

```ts
ForEach(
  arr: Array,
  itemGenerator: (item: any, index: number) => void,
  keyGenerator?: (item: any, index: number) => string
)
```

在ForEach循环渲染过程中，系统会为每个数组元素生成一个唯一且持久的键值，用于标识对应的组件。当这个键值变化时，ArkUI框架将视为该数组元素已被替换或修改，并会基于新的键值创建一个新的组件。

ForEach提供了一个名为keyGenerator的参数，这是一个函数，开发者可以通过它自定义键值的生成规则。如果开发者没有定义keyGenerator函数，则ArkUI框架会使用默认的键值生成函数，即(item: any, index: number) => { return index + '__' + JSON.stringify(item); }。

ArkUI框架对于ForEach的键值生成有一套特定的判断规则，这主要与itemGenerator函数的第二个参数index以及keyGenerator函数的第二个参数index有关，具体的键值生成规则判断逻辑如下图所示。

![](https://qiniu.waite.wang/202404191757732.png)

> 以下是一个简单例子, 具体可以到官方文档查看

```ts
class Item {
  name: string
  image: string
  price: number
  discount: number

  constructor(name: string, image: string, price: number, discount: number = 0) {
    this.name = name
    this.image = image
    this.price = price
    this.discount = discount
  }
}

@Entry
@Component
struct Second {
  private items: Array<Item> = [
    new Item("华为Mate1", "https://qiniu.waite.wang/202404182317729.png", 1666, 1000),
    new Item("华为Mate2", "https://qiniu.waite.wang/202404182317729.png", 1999),
    new Item("华为Mate3", "https://qiniu.waite.wang/202404182317729.png", 2666),
    new Item("华为Mate4", "https://qiniu.waite.wang/202404182317729.png", 2999),
    new Item("华为Mate5", "https://qiniu.waite.wang/202404182317729.png", 3666),
    new Item("华为Mate6", "https://qiniu.waite.wang/202404182317729.png", 3999),

  ]
  @State message: string = 'Hi there'

  build() {
    Column({ space: 10 }) {
      Row() {
        Text("商品列表")
          .fontSize(30)
          .fontWeight(FontWeight.Bold)
      }
      .width("100%")

      ForEach(
        this.items,
        (item: Item) => {
          Row({ space: 10 }) {
            Image(item.image)
              .width(100)

            Column() {
              Text(item.name)
                .fontSize(20)
                .fontWeight(FontWeight.Bold)

              if (item.discount) {
                Text(`原价$ ${item.price}`)
                  .fontColor("#ccc")
                  .fontSize(18)
                  .decoration({
                    type: TextDecorationType.LineThrough
                  })

                Text(`折扣价$ ${item.discount}`)
                  .fontColor("red")
                  .fontSize(18)
              }
              else {
                Text(`$ ${item.price}`)
                  .fontColor("red")
                  .fontSize(18)
              }
            }
            .height("100%")
            .alignItems(HorizontalAlign.Start)
          }
          .width("100%")
          .backgroundColor("#f8f8f8")
          .borderRadius(20)
          .height(120)
          .padding(10)
        }
      )


    }
    .padding(20)
  }
}
```

![](https://qiniu.waite.wang/202404191800166.png)

> 注意 当不同数组项按照键值生成规则生成的键值相同时，框架的行为是未定义的。例如，在以下代码中，ForEach渲染相同的数据项two时，只创建了一个ChildItem组件，而没有创建多个具有相同键值的组件。

```ts
@Entry
@Component
struct Parent {
  @State simpleList: Array<string> = ['one', 'two', 'two', 'three'];

  build() {
    Row() {
      Column() {
        ForEach(this.simpleList, (item: string) => {
          ChildItem({ 'item': item } as Record<string, string>)
        }, (item: string) => item)
      }
      .width('100%')
      .height('100%')
    }
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}

@Component
struct ChildItem {
  @Prop item: string;

  build() {
    Text(this.item)
      .fontSize(50)
  }
}
```

![](https://qiniu.waite.wang/202404191801029.png)

#### 补充: List/ ListItem

> 在以上案例中, 超出屏幕的内容无法滚动查看(会隐藏), 可以使用 List/ ListItem 组件来实现

```ts
// xxx.ets
@Entry
@Component
struct ListItemExample {
  private arr: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  @State editFlag: boolean = false

  build() {
    Column() {
      List({ space: 20, initialIndex: 0 }) {
        ForEach(this.arr, (item) => {
          ListItem() {
            Text('' + item)
              .width('100%').height(100).fontSize(16)
              .textAlign(TextAlign.Center).borderRadius(10).backgroundColor(0xFFFFFF)
          }
        }, item => item)
      }
    }.width('100%').height('100%').backgroundColor(0xDCDCDC).padding({ top: 5 })
  }
}
```

![](https://qiniu.waite.wang/202404201343460.png)

## ArkUI 自定义组件

### 自定义组件的基本结构

struct：自定义组件基于struct实现，struct + 自定义组件名 + {...}的组合构成自定义组件，不能有继承关系。对于struct的实例化，可以省略new。

> 自定义组件名、类名、函数名不能和系统组件名相同。

@Component：@Component装饰器仅能装饰struct关键字声明的数据结构。struct被@Component装饰后具备组件化的能力，需要实现build方法描述UI，一个struct只能被一个@Component装饰。

> 从API version 9开始，该装饰器支持在ArkTS卡片中使用。

```ts
@Component
struct MyComponent {
}
```

build()函数：build()函数用于定义自定义组件的声明式UI描述，自定义组件必须定义build()函数。

```ts
@Component
struct MyComponent {
  build() {
  }
}
```

@Entry：@Entry装饰的自定义组件将作为UI页面的入口。在单个UI页面中，最多可以使用@Entry装饰一个自定义组件。@Entry可以接受一个可选的LocalStorage的参数。

> 从API version 9开始，该装饰器支持在ArkTS卡片中使用。

```ts
@Entry
@Component
struct MyComponent {
}
```

### 自定义组件的基本用法

```ts
@Entry
@Component
struct ParentComponent {
  build() {
    Column() {
      Text('ArkUI message')
      HelloComponent({ message: 'Hello, World!' });
      HelloComponent({ message: '你好!' });
    }
  }
}
@Component
struct HelloComponent {
  @State message: string = 'Hello, World!';

  build() {
    // HelloComponent自定义组件组合系统组件Row和Text
    Row() {
      Text(this.message)
        .onClick(() => {
          // 状态变量message的改变驱动UI刷新，UI从'Hello, World!'刷新为'Hello, ArkUI!'
          this.message = 'Hello, ArkUI!';
        })
    }
  }
}
```

> 如果在另外的文件中引用该自定义组件，需要使用export关键字导出，并在使用的页面import该自定义组件。

```ts
@Component
export struct HelloComponent {
  @State message: string = 'Hello, World!';

  build() {
    // HelloComponent自定义组件组合系统组件Row和Text
    Row() {
      Text(this.message)
        .onClick(() => {
          // 状态变量message的改变驱动UI刷新，UI从'Hello, World!'刷新为'Hello, ArkUI!'
          this.message = 'Hello, ArkUI!';
        })
    }
  }
}
```

```ts
import { HelloComponent } from './HelloComponent'

@Entry
@Component
struct ParentComponent {
  build() {
    Column() {
      Text('ArkUI message')
      HelloComponent({ message: 'Hello, World!' });
      HelloComponent({ message: '你好!' });
    }
  }
}
```

### 自定义组件的参数规定

从上文的示例中，我们已经了解到，可以在build方法里创建自定义组件，在创建自定义组件的过程中，根据装饰器的规则来初始化自定义组件的参数。

```ts
@Component
struct MyComponent {
  private countDownFrom: number = 0;
  private color: Color = Color.Blue;

  build() {
  }
}

@Entry
@Component
struct ParentComponent {
  private someColor: Color = Color.Pink;

  build() {
    Column() {
      // 创建MyComponent实例，并将创建MyComponent成员变量countDownFrom初始化为10，将成员变量color初始化为this.someColor
      MyComponent({ countDownFrom: 10, color: this.someColor })
    }
  }
}
```

### build()函数

所有声明在build()函数的语言，我们统称为UI描述，UI描述需要遵循以下规则：

+ @Entry装饰的自定义组件，其build()函数下的根节点唯一且必要，且必须为容器组件，其中ForEach禁止作为根节点。
+ @Component装饰的自定义组件，其build()函数下的根节点唯一且必要，可以为非容器组件，其中ForEach禁止作为根节点。

```ts
@Entry
@Component
struct MyComponent {
  build() {
    // 根节点唯一且必要，必须为容器组件
    Row() {
      ChildComponent() 
    }
  }
}

@Component
struct ChildComponent {
  build() {
    // 根节点唯一且必要，可为非容器组件
    Image('test.jpg')
  }
}
```

+ 不允许声明本地变量，反例如下。

```ts
build() {
  // 反例：不允许声明本地变量
  let a: number = 1;
}
```

+ 不允许在UI描述里直接使用console.info，但允许在方法或者函数里使用，反例如下。

```ts
build() {
  // 反例：不允许console.info
  console.info('print debug log');
}
```

+ 不允许创建本地的作用域，反例如下。

```ts
build() {
  // 反例：不允许本地作用域
  {
    ...
  }
}
```

+ 不允许调用没有用@Builder装饰的方法，允许系统组件的参数是TS方法的返回值。

```ts
@Component
struct ParentComponent {
  doSomeCalculations() {
  }

  calcTextValue(): string {
    return 'Hello World';
  }

  @Builder doSomeRender() {
    Text(`Hello World`)
  }

  build() {
    Column() {
      // 反例：不能调用没有用@Builder装饰的方法
      this.doSomeCalculations();
      // 正例：可以调用
      this.doSomeRender();
      // 正例：参数可以为调用TS方法的返回值
      Text(this.calcTextValue())
    }
  }
}
```

+ 不允许switch语法，如果需要使用条件判断，请使用if。反例如下。

```ts
build() {
  Column() {
    // 反例：不允许使用switch语法
    switch (expression) {
      case 1:
        Text('...')
        break;
      case 2:
        Image('...')
        break;
      default:
        Text('...')
        break;
    }
  }
}
```

+ 不允许使用表达式，反例如下。

```ts
build() {
  Column() {
    // 反例：不允许使用表达式
    (this.aVar > 10) ? Text('...') : Image('...')
  }
}
```

### 自定义组件通用样式

自定义组件通过“.”链式调用的形式设置通用样式。

```ts
@Component
struct MyComponent2 {
  build() {
    Button(`Hello World`)
  }
}

@Entry
@Component
struct MyComponent {
  build() {
    Row() {
      MyComponent2()
        .width(200)
        .height(300)
        .backgroundColor(Color.Red)
    }
  }
}
```

## ArkUI 入门

### ArkTS语言

ArkTS是HarmonyOS优选的主力应用开发语言。ArkTS围绕应用开发在TypeScript（简称TS）生态基础上做了进一步扩展，继承了TS的所有特性，是TS的超集。因此，在学习ArkTS语言之前，建议开发者具备TS语言开发能力。

当前，ArkTS在TS的基础上主要扩展了如下能力：

+ 基本语法：ArkTS定义了声明式UI描述、自定义组件和动态扩展UI元素的能力，再配合ArkUI开发框架中的系统组件及其相关的事件方法、属性方法等共同构成了UI开发的主体。
+ 状态管理：ArkTS提供了多维度的状态管理机制。在UI开发框架中，与UI相关联的数据可以在组件内使用，也可以在不同组件层级间传递，比如父子组件之间、爷孙组件之间，还可以在应用全局范围内传递或跨设备传递。另外，从数据的传递形式来看，可分为只读的单向传递和可变更的双向传递。开发者可以灵活地利用这些能力来实现数据和UI的联动。
+ 渲染控制：ArkTS提供了渲染控制的能力。条件渲染可根据应用的不同状态，渲染对应状态下的UI内容。循环渲染可从数据源中迭代获取数据，并在每次迭代过程中创建相应的组件。数据懒加载从数据源中按需迭代数据，并在每次迭代过程中创建相应的组件。

未来，ArkTS会结合应用开发/运行的需求持续演进，逐步提供并行和并发能力增强、系统类型增强、分布式开发范式等更多特性。

### 基本语法

#### ArkTS 基本组成

![](https://qiniu.waite.wang/202404181345776.png)

![](https://qiniu.waite.wang/202404181347607.png)

> 自定义变量不能与基础通用属性/事件名重复。

+ 装饰器： 用于装饰类、结构、方法以及变量，并赋予其特殊的含义。如上述示例中@Entry、@Component和@State都是装饰器，@Component表示自定义组件，@Entry表示该自定义组件为入口组件，@State表示组件中的状态变量，状态变量变化会触发UI刷新。
+ UI描述：以声明式的方式来描述UI的结构，例如build()方法中的代码块。
+ 自定义组件：可复用的UI单元，可组合其他组件，如上述被@Component装饰的struct Hello。
+ 系统组件：ArkUI框架中默认内置的基础和容器组件，可直接被开发者调用，比如示例中的Column、Text、Divider、Button。
+ 属性方法：组件可以通过链式调用配置多项属性，如fontSize()、width()、height()、backgroundColor()等。
+ 事件方法：组件可以通过链式调用设置多个事件的响应逻辑，如跟随在Button后面的onClick()。
+ 系统组件、属性方法、事件方法具体使用可参考基于ArkTS的声明式开发范式。
