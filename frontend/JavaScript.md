# JavaScript

JavaScript是一门弱类型的脚本语言，其源代码在发往客户端运行之前不需要经过编译，而是将文本格式的字符代码发送给浏览器由浏览器解释运行

**ECMAScript** 原生开发标准，简称ES
- ES3
- ES4 （内部，未正式发布）
- ES5 （全浏览器支持）
- ES6 （常用，当前主流版本：webpack打包成为ES5支持）
- ES7
- ES8
- ES9

**TypeScript **微软的标准，JavaScript的一个超集

除了具备ES的特性之外，还纳入了许多不在标准范围内的新特性，所以会导致很多浏览器不能支持TypeScript语法，需要编译后（编译成JS）才能被浏览器正确执行
## 前端相关技术

### JavaScript框架

- JQuery
- **Angular**：由Java程序员开发，增加了**模块发开发**的理念
- **React**：提出了**虚拟DOM**
- **Vue**:综合了**模块化**和**虚拟DOM**的优点
- Axios：前端通信框架
### UI框架

- Ant-Design：阿里巴巴出品，基于React的UI框架
- ElementUI、iview、ice：饿了么出品，基于Vue的UI框架
- BootStrap：Twitter推出的用于前端开发的开源工具包
- AmazeUI：HTML5跨屏前端框架
### JavaScript构建工具

- Babel：JS编译工具，主要用于浏览器不支持的ES新特性
- WebPack：模块打包器，主要用于打包、压缩、合并及按序加载
## 快速入门

```html
<!--html script标签内，写JS代码
        可以放在head或body的最后面-->
    <script>
        // 注释
        alert('hello,world');
    </script>

<!--html 外部引入 不用显示定义type，默认就是js-->
<script src="js/hello.js"></script>
```
### 基本语法

```javascript
// 严格区分大小写
// 1、定义变量： 变量类型 变量名 = 变量值
// ES6不再建议使用var类型，
// 局部变量用 let 代替
// 常量用 const 代替
let num = 1;
// 2、条件控制
if (num > 0){
    alert('正');
}else {
    alert('负');
}
// 浏览器控制台打印信息
console.log(num)
```
### 数据类型

- **number**
	- js不区分小数和整数
- **字符串**：'add'，"adv"
- **Boolean**：true，false
- **逻辑运算符**：&&、||、！
- **比较运算符**：
	- =
	- ==：等于（类型不一样，值一样，结果也为true），不建议使用
	- ===：绝对等于（类型一样，值一样，结果为true）
	- NaN与所有的数值都不相等，包括自己，使用 `isNaN()` 来判断是否为NaN
- null和undefined
	- null：空
	- undefined：未定义
- **数组**：js中数组元素类型可以不同
	- 取数组下标时如果越界，不会报错，会返回 `undefined`
- **对象**：大括号
	- 每个属性之间使用 `，` 隔开

```javascript
123      // 整数
123.33   // 小数
1.2345e4 // 科学计数法
-99      // 负数
NaN      // not a number
Infinity // 无限大
// 尽量避免使用浮点数进行运算，存在精度问题

let arr = [1,2,3,null,'j',true];

let person = {
    name = "liu",
    age = 11
}
```
### 严格检查模式

```javascript
// 预防JavaScript的随意性导致产生的一些问题
// 必须写在JavaScript的第一行
'use strict'
let i = 1；
```
## 数据类型

### 字符串

- 正常字符使用`''`、`" "`包裹
- 注意转义字符 `\`

```
\'
\n
\t
\u4e2d \u#### Unicode字符
\x41          Ascii字符
```

- 多行字符串

```javascript
// 使用 `` 包裹
let msg = `
    hello
    world
    你好
    世界！
`
```

- 模板字符串

```javascript
let name = 'liu';
// ${}:EL表达式
let msg = `
    hello
    ${name}
`
```

- 字符串长度
- 字符串不可变
- 大小写转换
	- toUpperCase
	- toLowerCase
- ...
### 数组

**Array可以包含任意的数据类型**
- **数组长度可变**：
	- 给arr.length赋值，数组大小就会发生变化，如果赋值过小，元素就会丢失!
- 通过元素获得下标索引：indexOf()
	- 字符串和数字是不同的
- 截取Array的一部分：**slice()**（类似与String中的substring）
- push() 、pop() —— 尾部，压入和弹出
- unshift()、 shift() —— 头部，压入和弹出
- 排序：sort()
- 元素反转：reverse()
- 拼接：concat() （不会修改原数组，只是会返回一个新的数组）
- 连接符：join() （打印时拼接数组，使用特定的字符串连接）
- 多维数组：\[\[1,2],\[3,4],\[5,6]]
- ...
### 对象

若干个键值对，键都是字符串，值是任意类型

```javascript
// {...}表示对象，最后一个属性不用加 ，
let person = {
    name: 'liu',
    age: 12
}
// 对象赋值
person.age = 21
// 使用一个不存在的对象属性不会报错，只会返回undefined

// 动态删减属性
delete person.age
// 动态的添加属性
person.id = 123
// 判断属性值是否在这对象中
'age' in person
// 会继承方法
'toString' in person 
true
// 判断一个属性是否是这个对象自身拥有的
person.hasOwnProperty('toString')
false
```
### 流程控制

- if  判断 
- while  循环
- for  循环
- forEach  循环
- for in  循环获取下标
- for of  循环获取值（类似Java的forEach，可以作用于map、set）
### Map、Set

map：键值对
- set() —— 新增或修改
- delete() —— 删除

set：无序不重复的集合
- add() —— 新增
- delete() —— 删除
- has() —— 是否包含某个元素
## 函数

### 定义函数

```javascript
// 执行到return 代表函数结束，返回结果
// 方法一
function abs(x){
    if (x>0){
        return x;
    }else {
        return -x;
    }
}
// 方法二
// function (x){...} 是一个匿名函数，但是可以把结果赋值给abs，通过abs就可以调用函数！
let abs = function (x){
    return x>0?x:-x;
}
abs(-10) // 10
// 方法一和方法二等效
```

**JavaScript可以传递任意个参数，也可以不传递参数，不会报错**

```javascript
function abs(x){
    // 手动抛出异常
    if (typeof x !== 'number'){
        throw 'Not a number!';
    }
    if (x>0){
        return x;
    }else {
        return -x;
    }
}
```

> arguments

**JS免费赠送的关键字**

代表传递进来的所有参数，是一个数组

arguments包含所有的参数，如果需要使用多余的参数来进行附加参数，需要排除默认已有的参数

> rest

```javascript
// ...rest 写在参数的最后。获取已经定义的参数之外的所有参数
function a(a,b,...rest){
    console.log('a >' + a);
    console.log('b >' + b);
    console.log(rest);
}
```

### 变量的作用域

在javaScript中，var定义变量实际是有作用域的
- 假设在函数体内声明，则在函数体外不可以使用
- 两个函数中使用了相同的变量名，也不会冲突

