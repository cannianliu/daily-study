# CSS

> HTML-CSS-JS  
> 结构-美化-交互
## 简介

 **Cascading Style Sheet——层叠样式表**

层叠样式表 (英文全称：Cascading Style Sheets) 是一种用来表现 HTML（[标准通用标记语言]的一个应用）或 XML（标准通用标记语言的一个子集）等文件样式的计算机语言。CSS不仅可以静态地修饰网页，还可以配合各种脚本语言动态地对网页各元素进行格式化。

CSS 能够对网页中元素位置的排版进行像素级精确控制，支持几乎所有的字体字号样式，拥有对网页对象和模型样式编辑的能力。

**发展史**
- CSS 1.0
- CSS 2.0	DIV（块）+CSS，HTML与CSS结构分离的思想，网页变的更简单
- CSS 2.1	浮动，定位
- CSS 3.0	圆角，阴影，动画... 浏览器兼容问题

```html
<!--规范：每一个声明以；结尾
    <style>可以编写css代码
    css文件中 语法：
        选择器{
            声明1；
            声明2；
            ...
        }-->
    <style>
        h1{
            color: brown;
        }
    </style>

<!--导入样式-->
<link rel="stylesheet" href="css/style.css">
```

**css的优势：**

1、内容和表现分离  
2、网页结构表现统一，可以实现复用  
3、样式十分的丰富  
4、建议使用独立与html的css文件  
5、利用SEO，容易被搜索引擎收录
## CSS导入方式

1、行内样式：在标签元素中，编写一个style属性，编写样式  
2、内部样式  
3、外部样式

**优先级：遵循就近原则**

**外部样式拓展：**

- 链接式：建议使用link

```html
<!--外部样式-->
<link rel="stylesheet" href="css/style.css">
```

- 导入式：

CSS2.1 特有的

```html
<style>
    @import url("css/style.css");
</style>
```

## 选择器

> 选择页面上的某一个或者某一类元素
### 基本选择器

- 标签选择器：`标签{}`
  - 会选择到页面上的所有的这个标签的元素
- 类选择器：`.class的名称{}`
  - 可以多个标签归类为同一个class，实现复用
- id选择器：`#id名称{}`
  - 必须保证全局唯一

> **优先级：id选择器 > 类选择器 > 标签选择器**
### 层次选择器

- **后代选择器**：`标签 后代标签{}`
	 - 在某个元素的后面
- **子选择器**：`标签>子标签{}`
- **相邻兄弟选择器**：`.class名+同层标签{}`
	- 只会选择向下相邻的一个
- **通用兄弟选择器**：`.class名~同层标签{}`
	- 选中当前元素的向下的所有兄弟元素
### 结构伪类选择器

```html
<style>
    /*选择当前p元素的父级元素，选中父级元素的第一个，并且是当前元素才能生效*/
    p:nth-child(5){
        background: #1c7272;
    }
    p:nth-of-type(2){
        background: brown;
    }
    a:hover{
        background: aquamarine;
    }
</style>
```
### 属性选择器（常用）

> **标签名[属性名=属性值（支持正则表达式）]{}**

**=**	：绝对等于  
***=**	：包含值  
**^=**	：以值开头  
**$=**	：以值结尾
## 美化网页元素

- span：重点要突出的字
### 字体样式

- font-family：字体类型
- font-size：字体大小
- font-weight：字体粗细
- color：字体颜色
- 简写属性，将字体样式写在一行中：
	- `font：字体风格（斜体/粗体） 字体粗细 字体大小/行高 字体类型`
### 文本样式

- **颜色**——`color：`
	- 颜色单词
	- RGB #FFFFFF
	- RGBA A:0~1（表示颜色深浅）
- **文本对齐方式**——`text-align:`
	- 文本居中： center
- **首行缩进**——`text-indent`
	- 首行缩进两个字：2em
- **上下居中**
	- 行高和块的高度一致，就可以实现上下居中
	- 行高—— `line-height:`
- **划线**——`text-decoration`
	- 下划线：underline
	- 中划线：line-through
	- 上划线：overline
- **两个标签水平对齐**——`vertical-align: center`
### 超链接伪类和文本阴影

鼠标悬浮 —— :hover  
鼠标点击 —— :active  
文本阴影 —— \#price
### 列表

**list-style:**
- none——去掉原点
- circle——空心圆
- decimal——数字
- square——正方形
### 背景

backgroud-image ——图片默认平铺

backgroud-repeat：—— 平铺类型
- repeat-x：水平平铺
- repeat-y：竖直平铺
- no-repeat：不平铺

**渐变**

> https://www.grabient.com/

调整角度，复制css
## 盒子模型

![[image-20211028162603931.png]]
margin：外边距
- margin: 0 auto; 元素居中（需要是块元素，且块元素有固定的宽度） 

border：边框——粗细 样式 颜色
- border-radius——圆角边框

padding：内边距  

box-shadow——盒子阴影

> body总有一个默认的外边距

元素的大小 = margin + border + padding + 内容
## 浮动

**display:**
- block ——变为块元素
- inline ——行内元素
- inline-block ——既是块元素，但是可以内联，可以在一行中

**float：**
- right——靠右
- left——靠左

**clear：**不允许有浮动元素
- right
- left
- both
- none

**父级边框塌陷问题**

**overflow：**有元素超出大小
- 在父级元素中增加`overflow: hidden;`
- 在父级元素中增加一个伪类

```css
#father:after{
    content: '';
    display: block;
    clear: both;
}
```

**总结：**

1、浮动元素后面增加空div —— 简单，但代码中应避免空div

2、设置父元素的高度 —— 简单，元素假如有了固定的高度，就会被限制

3、overflow —— 简单，下拉的一些场景避免使用

4、父类添加一个伪类：after（推荐）—— 写法稍微复杂，但没有副作用

**对比：**
- display：方向不可控制
- float：浮动起来的话会脱离标准文档流，所以要解决父级边框塌陷的问题
## 定位

### 相对定位

`position: relative`

相对自己原来的位置进行偏移，仍然在标准文档流中，原来的位置会被保留
### 绝对定位

`position: absolute`

1、没有父级元素定位的前提下，相对于浏览器定位

2、假设父级元素存在定位，通常会相对于父级元素进行偏移

3、绝对定位的元素会脱离标准文档流，原来的位置不会被保留
### 固定定位

`position: fixed`

固定在页面的位置不会发生移动，如页面中的”回到首页“图标
### z-index

图层层级设定
## 动画

webkit
## CSS预处理

常见预处理器：

SASS：基于Ruby，通过服务端处理，功能强大，解析效率高。需要学习Ruby语言，上手难度高于LESS

LESS：基于NodeJS，通过客户端处理，使用简单，功能比SASS简单，解析效率也低于SASS，但在实际开发中足够了。对于后台人员，建议使用LESS
