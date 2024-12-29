---
title: "TypeScript 入门"
date: 2023-08-22T16:04:23+08:00
categories: ["Web"]
tags: ["Vue3"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

## 什么是 TypeScript

1. TypeScript既是一种语言又是一组工具。TypeScript是JavaScript的一个超集。换句话说，TypeScript是JavaScript加上一些额外的功能。

3. TypeScript 扩展了 JavaScript 的语法，所以任何现有的 JavaScript 程序可以不加改变的在 TypeScript 下工作。TypeScript 是为大型应用之开发而设计，而编译时它产生 JavaScript 以确保兼容性。

5. TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的 JavaScript 引擎中。

### TypeScript的组成部分

1. 语言 - 它包括语法，关键字和类型注释。

3. 编译器 - TypeScript编译器（TSC）将使用TypeScript编写的指令转换为其等效的JavaScript。

5. 语言服务 - “TypeScript语言服务”在核心编译管道周围公开了一个额外的层，它是类似编辑器的应用程序。语言服务支持常见的编辑器操作集，如语句完成，签名帮助，代码格式化和大纲，着色等。

### ![](https://qiniu.waite.wang/ts1_0.png)

**Typescript 官网地址**: https://www.typescriptlang.org/zh/

使用 nvm 来管理 node 版本: https://github.com/nvm-sh/nvm

## 安装 Typescript:

```bash
npm install -g typescript
```

使用 tsc 全局命令：

```bash
// 查看 tsc 版本
tsc -v
// 编译 ts 文件
tsc fileName.ts

// 使用 tsc -v 查看是否安装成功, 若成功返回 typescript 版本号
// 链接失败使用淘宝镜像
npm config set registry https://registry.npm.taobao.org
```

### TypeScript 转 JavaScript

1. cmd 到文件目录

```
tsc 文件名.ts
```

2. WebStorm 中自动转换, 勾选 Recompile on change, Webstorm 中勾选如下

![](https://qiniu.waite.wang/ts1_1.png)

![](https://qiniu.waite.wang/ts1_2.png)

## 类型声明

### Boolean、Number

```typescript
let val2:boolean;
val2 = true;
// val2 = 1; // 会报错
console.log(val2);

let val1:number; // 定义了一个名称叫做val1的变量, 这个变量中将来只能存储数值类型的数据
val1 = 123;
// val1 = "123"; // 会报错
console.log(val1);
```

### String

#### 多行字符串

```typescript
let hello: string = `Welcome to W3cschool`;
// 类似于 "Welcome to \nW3cschool";
```

#### 内嵌表达式

```typescript
let name: string = `Loen`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.
I'll be ${ age + 1 } years old next month.`;
// 类似于 "Hello, my name is " + name + ".\nI'll be " + (age + 1) + " years old next month.";
// 和 format 性质差不多
```

#### 自动拆分字符串

```typescript
function userinfo(params,name,age){
    console.log(params);
    console.log(name);
    console.log(age);
}


let myname = "Loen Wang";
let getAge = function(){
    return 18;
}
// 调用
userinfo`hello my name is ${myname}, i'm ${getAge()}`
```

![](images/ts2_0.png)

### 数组和元祖

#### 数组数据类型一致

1. Array < number >

```typescript
// 需求: 要求定义一个数组, 这个数组中将来只能存储数值类型的数据
let arr1: Array<number>; // 表示定义了一个名称叫做arr1的数组, 这个数组中将来只能够存储数值类型的数据
arr1 = [1, 3, 5];
// arr1 = ['a', 3, 5]; // 报错
console.log(arr1);
```

2. string\[ \]

```typescript
// 需求: 要求定义一个数组, 这个数组中将来只能存储字符串类型的数据
let arr2:string[]; // 表示定义了一个名称叫做arr2的数组, 这个数组中将来只能够存储字符串类型的数据
arr2 = ['a', 'b', 'c'];
// arr2 = [1, 'b', 'c']; // 报错
console.log(arr2);
```

#### 数组数据类型不一致

联合类型声明数组 (number | string)\[ \]

```typescript
let arr3:(number | string)[];
// 表示定义了一个名称叫做arr3的数组, 这个数组中将来既可以存储数值类型的数据, 也可以存储字符串类型的数据
arr3 = [1, 'b', 2, 'c'];
// arr3 = [1, 'b', 2, 'c', false]; // 报错
console.log(arr3);
```

#### 自由任意类型元素的数组

1. 如果不希望类型检查器对值进行检查,直接通过编译阶段的检查。 那么我们可以使用 any 类型来标记这些变量

```typescript
let notSure: any = 4;
notSure = "这是一个字符串";
notSure = false; // 现在我们又可以将其改成布尔类型
```

2. 当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```typescript
let arr4:any[]; // 表示定义了一个名称叫做arr4的数组, 这个数组中将来可以存储任意类型的数据
arr4 = [1, 'b', false];
console.log(arr4);
```

#### 严格限制类型和长度的元祖数组

{% note blue '' flat %}  
TS中的元祖类型其实就是数组类型的扩展,元祖用于保存定长定数据类型的数据  
{% endnote %}

```typescript
let arr5:[string, number, boolean]; 

// 表示定义了一个名称叫做arr5的元祖, 这个元祖中将来可以存储3个元素, 第一个元素必须是字符串类型, 第二个元素必须是数字类型, 第三个元素必须是布尔类型

arr5 = ['a', 1, true];
// arr5 = ['a', 1, true, false]; // 超过指定的长度会报错
arr5 = ['a', 1, true];
console.log(arr5);
```

### enum枚举

枚举用于表示固定的几个取值,例如: 一年只有四季、人的性别只能是男或者女。 枚举类型是TS为JS扩展的一种类型, 在原生的JS中是没有枚举类型的。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
// 默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值
// tips:
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
// 或者
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
/* 
  枚举类型提供的一个便利是你可以由枚举的值得到它的名字。
   例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，
   我们可以查找相应的名字：
*/
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

alert(colorName);  // 显示'Green'因为上面代码里它的值是2
```

### Any、Void

any表示任意类型, 当我们不清楚某个值的具体类型的时候我们就可以使用any，任何数据类型的值都可以赋值给any类型, 一般用于定义一些通用性比较强的变量, 或者用于保存从其它框架中获取的不确定类型的值

```typescript
let value:any; // 定义了一个可以保存任意类型数据的变量
value = 123;
value = "abc";
value = true;
value = [1, 3, 5];
```

void与any正好相反, 表示没有任何类型, 一般用于函数返回值。在TS中只有null和undefined可以赋值给void类型

```typescript
function test():void {
    console.log("hello world");
}
test();

let value:void; // 定义了一个不可以保存任意类型数据的变量, 只能保存null和undefined
// value = 123; // 报错
// value = "abc";// 报错
// value = true;// 报错
// 注意点: null和undefined是所有类型的子类型, 所以我们可以将null和undefined赋值给任意类型
// value = null; // 不会报错
value = undefined;// 不会报错
```

### Never

- 表示的是那些永不存在的值的类型,一般用于抛出异常或根本不可能有返回值的函数。

- Never 可以赋值给任意类型, 但其他类型不可以赋值给 Never

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

### Object 对象

```typescript
let obj:object; // 定义了一个只能保存对象的变量
// obj = 1;
// obj = "123";
// obj = true;
obj = {name:'lnj', age:33};
console.log(obj);
```

### interface 接口

#### 基本用法

```typescript
interface Person {
    firstName: string;
    lastName: string;
}
function hello(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { 
  firstName: "Wang", 
  lastName: "Loen" 
  // else: "error"  会报错
};
document.body.innerHTML = hello(user);
```

#### 属性数量不确定时的定义方法

##### 少用可选属性

属性名字后面加一个 ？表示可选属性  

```typescript
interface FullName{
    firstName:string
    lastName:string
    middleName?:string
}

function say({firstName, lastName, middleName}:FullName):void {
    // console.log(`我的姓名是:${firstName}_${lastName}`);
    if(middleName){
        console.log(`我的姓名是:${firstName}_${middleName}_${lastName}`);
    }else{
        console.log(`我的姓名是:${firstName}_${lastName}`);
    }
}

say({firstName:'Jonathan', lastName:'Lee', middleName:"666"});
say({firstName:'Jonathan', lastName:'Lee'});
```

##### 多用索引签名

在定义对象中key（propName）和value的数据结构，后续对象中的属性，只要key和value满足索引签名的限定即可, 无论有多少个都无所谓。

```typescript
interface FullName {
    [propName:string]:string
}
let obj:FullName = {
    // 注意点: 只要key和value满足索引签名的限定即可, 无论有多少个都无所谓
    firstName:'Jonathan',
    lastName:'Lee',
    // middleName:false // 报错
    // 无论key是什么类型最终都会自动转换成字符串类型, 所以没有报错
    // false: '666' 
}



interface stringArray {
    [propName:number]:string
}

let arr:stringArray = {
    0:'a',
    1:'b',
    2:'c'
};

// let arr:stringArray = ['a', 'b', 'c'];
console.log(arr[0]);
console.log(arr[1]);
console.log(arr[2]);
```

#### 接口的继承

```typescript
interface LengthInterface {
  length:number
}

interface WidthInterface {
  width:number
}

interface HeightInterface {
  height:number
}

interface RectInterface extends LengthInterface,WidthInterface,HeightInterface {
  // length:number
  // width:number
  // height:number

  color:string
}

let rect:RectInterface = {
  length:10,
  width:20,
  height:30,
  color:'red'
}
```

#### 函数接口

```typescript
interface SumInterface {
  (a:number, b:number):number
}

// 建议使用这种写法
let sum:SumInterface= function(x,y) {
  return x + y;
}

let res = sum(10, 20);

console.log(res);
```

### 只读属性

可以在属性名前用 readonly来指定只读属性:

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

可以通过赋值一个对象字面量来构造一个Point。 赋值后， x 和 y 再也不能被改变了。

```typescript
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

TypeScript 具有 ReadonlyArray 类型，它与 Array 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

## 函数声明

### 定义函数

```typescript
// typescript定义函数的方法
// 命名函数

function say1(name:string):void {
  console.log(name);
}

// 匿名函数

let say2 = function (name:string):void {
  console.log(name);
}

// 箭头函数

let say3 = (name:string):void =>{
  console.log(name);
}
```

### 函数声明和分离实现

#### 利用 type 声明函数

```typescript
// 先利用type声明一个函数
type AddFun = (a:number, b:number)=>number;

// 再根据声明去实现这个函数
// 此时函数的参数和返回值可以不需要写类型声明了，因为ts可以通过这个函数声明推断出来类型了
let add:AddFun = function (x, y) {
    return x + y;
};
let res = add(30, 20);
console.log(res);
```

#### 利用 interface 声明函数

```typescript
// 先利用interface声明一个函数
interface AddFun {
      (a:number, b:number):number   
}

let add:AddFun = function (x, y) {
    return x + y;
};
let res = add(30, 20);
console.log(res);
```

### 参数

#### 可选参数

```typescript
// 需求: 要求定义一个函数可以实现2个数或者3个数的加法

function add(x:number, y:number, z?:number):number {
  return x + y + (z ? z : 0);
}

let res = add(10, 20);
let res = add(10, 20, 30);
// 可选参数后面只能跟可选参数
// 可选参数可以是一个或多个
```

#### 默认参数

```typescript
function add(x:number, y:number=10):number {
  return x + y;
}

let res = add(10);
let res = add(10, 30);
```

#### 剩余参数

```typescript
function add(x:number, ...ags:number[]) {
  console.log(x);
  console.log(ags);
}

add(10, 20, 30, 40, 50)
```

## 类型断言

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。TypeScript 会假设你，程序员，已经进行了必须的检查。

- 类型断言有两种形式。 其一是尖括号语法：

```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

- 另一个为as语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

例如: 我们拿到了一个any类型的变量, 但是我们明确的知道这个变量中保存的是字符串类型，此时我们就可以通过类型断言将any类型转换成string类型, 使用字符串类型中相关的方法了。

```typescript
let str:any = 'it666';
// 当还是any的时候是没有.length的提示的
let len = (str as string).length;
console.log(len);
```

## 泛型

### 什么是泛型

- 用来弥补any没有语法提示和报错的缺点。

- 最开始不指定类型，后面根据我们传入的类型确定类型。

- 软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。在像C#和Java这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

### 使用方法

我们给identity添加了类型变量T。 T帮助我们捕获用户传入的类型（比如：number），之后我们就可以使用这个类型。 之后我们再次使用了 T当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。

我们把这个版本的identity函数叫做泛型，因为它可以适用于多个类型。 不同于使用 any，它不会丢失信息，像第一个例子那像保持准确性，传入数值类型并返回数值类型。

### 泛型类

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}


let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };\

// GenericNumber类的使用是十分直观的，并且你可能已经注意到了，没有什么去限制它只能使用number类型。 也可以使用字符串或其它更复杂的类型。

let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function(x, y) { return x + y; };


console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));

//与接口一样，直接把泛型类型放在类后面，可以帮助我们确认类的所有属性都在使用相同的类型。
```

### 泛型约束

默认情况下我们可以指定泛型为任意类型，但是有些情况下我们需要指定的类型满足某些条件后才能指定

那么这个时候我们就可以使用泛型约束。

```typescript
interface IWithLength {
  length: number
}

function echoWithLength<T extends IWithLength>(arg: T): T {
  console.log(arg.length)
  return arg
}

const len01 = echoWithLength('abc') // 3
const len02 = echoWithLength({ length: 12 }) // 12
const len03 = echoWithLength([1, 2, 3]) // 3
```