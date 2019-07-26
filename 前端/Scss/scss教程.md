### 一、什么是Sass

Sass (Syntactically Awesome StyleSheets)是css的一个扩展开发工具，它允许你使用变量、条件语句等，使开发更简单可维护。这里是官方文档。

 

### 二、安装是Sass

因为Sass依赖于Ruby环境，所以安装Sass前，需要安装Ruby环境，官网下载。

安装时请勾选Add Ruby executables to your PATH这个选项，添加环境变量，不然以后使用编译软件的时候会提示找不到ruby环境

（安装Ruby时，路径中请勿出现中文，避免后续安装Sass失败）

![image](https://images0.cnblogs.com/blog2015/660113/201503/111421489028833.png)

安装完Ruby环境后，在开始菜单中，找到刚才安装的ruby，打开Start Command Prompt with Ruby

![https://images0.cnblogs.com/blog2015/660113/201503/111423536679582.png](https://images0.cnblogs.com/blog2015/660113/201503/111423536679582.png)

然后直接命令行输入

    gem install sass

按回车确认，等待一段时间就会提示Sass安装成功，现在因为墙的比较厉害，如果没有安装成功，请参考淘宝的RubyGems镜像安装sass，如果成功则忽略。

复制代码

    $ gem sources --remove https://rubygems.org/
    $ gem sources -a https://ruby.taobao.org/
    $ gem sources -l
    *** CURRENT SOURCES ***

    https://ruby.taobao.org
    # 请确保只有 ruby.taobao.org

    $ gem install sass

 

### 三、编译Sass工具

Sass文件后缀为 .scss，因此要编译成 .css 文件。

1）命令行编译

    sass style.scss style.css
Sass 提供4种编译风格

    # nested：嵌套缩进的css代码，它是默认值。
    # expanded：没有缩进的、扩展的css代码。
    # compact：简洁格式的css代码。
    # compressed：压缩后的css代码。
生产环境当中，一般选择最后一项。

    sass --style compressed style.sass style.css
也可以监听某个文件或目录，一旦文件发生改变，就自动生成编译后文件

    # 单文件监听命令
    sass --watch input.scss:output.css
    # 文件夹监听命令
    sass --watch app/sass:public/stylesheets
css文件转成sass/scss文件

    sass-convert style.css style.sass   
    sass-convert style.css style.scss

### 四、基本语法

####  1）变量

 sass的变量名必须是一个$符号开头，后面紧跟变量名

```
//sass 样式
$red: #f00;
div {
   color: $red;   
}
// 编译为css后
div {
    color:#f00;   
}
```
特殊变量：如果变量嵌套在字符串中，则需要写在 #{} 符号里面，如：
```
$top: top;
div {
    margin-#{$top}: 10px;       /* margin-top: 10px; */
}
```
默认变量：仅需在值后面加入 !default即可， 默认变量一般用来设置默认值，当该变量出现另外一个值时，无论定义先后，都会使用另外一个值，覆盖默认值
```
$color: red;
$color: blue !default;
div {
    color: $color;    /* color:red; */
}
```
多值变量：多值变量分为list类型和map类型，list有点像js对象中的数组，map类型像js中的对象

list : 可通过空格，逗号或小括号分割多个值，使用 nth($变量名, $索引)取值。 list数据操作有其他函数，具体参考sass Functions

```
//一维数据
$px: 5px 10px 20px 30px;

//二维数据，相当于js中的二维数组
$px: 5px 10px, 20px 30px;
$px: (5px 10px) (20px 30px);

// 例子
$px: 10px 20px;
div {
    margin:nth($px, 1) 0 0 nth($px, 2);    /* margin:10px 0 0 20px; */
}
```
map: 数据以key和value组成，格式：$map: (key1: value1, key2: value2); 通过map-get($map, $key); 
```
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}
```
#### 2）计算功能

 sass允许使用算式。
```
div {
    padding: 2px * 4px;
    margin: (10px / 2);
    font-size: 12px + 4px;    
}
```
#### 3）嵌套

标签嵌套

```
// sass 样式
div {
    color: #333;
    a {
       font-size:14px; 
       &:hover {
          text-decoration:underline;
       }
    }
} 

// 编译后css
div {
    color: #333;
}
div a {
    font-size:14px; 
}
div a:hover {
    text-decoration:underline;
}
```
属性嵌套：

```
//sass 样式
.fakeshadow {
  border: {
    style: solid;
    left: {
      width: 4px;
      color: #888;
    }
    right: {
      width: 2px;
      color: #ccc;
    }
  }
}

//css 编译后样式
.fakeshadow {
  border-style: solid;
  border-left-width: 4px;
  border-left-color: #888;
  border-right-width: 2px;
  border-right-color: #ccc; 
}
```
####  4）注释

 sass有两种注释风格

标准css注释： /* 注释 */， 会保留到编译后的文件中，压缩则删除

单行注释： // 注释

在标准注释 /*后面加入一个感叹号，表示重要注释，压缩模式也会保留注释，用于版权声明等。

/*! 重要注释 */
#### 5）继承

sass 中，选择器继承可以让选择器继承另一个选择器的所有样式

```
// sass样式
h1 {
    font-size:20px;
}
div {
    @extend h1;
    color:red;
}
// css编译后样式
h1 {
    font-size:20px;
}
div {
    font-size:20px;
    color:red;
}
```
使用占位符选择器 % 

从sass3.2.0后，就可以定义占位选择器%，这个的优势在于，不调用不会有多余的css文件

```
// sass样式
%h1 {
    font-size:20px;
}
div {
    @extend %h1;
    color:red;
}
// css编译后样式
div {
    font-size:20px;
    color:red;
}
```
#### 6）混合(mixin)

 sass中使用@mixin声明混合，可以传递参数，参数名义$符号开始，多个参数以逗号分开，如果参数有多组值，那么在变量后面加三个点表示，如： $var...

```
//sass 样式
@mixin opacity($opacity:50) {
  opacity: $opacity / 100;
  filter: alpha(opacity=$opacity);
}

.opacity{
  @include opacity;      //参数使用默认值  50/100 = 0.5
}
.opacity-80{
  @include opacity(80); //传递参数  80/100 = 0.8
}

//  css编译后样式
.opacity{
  opacity: 0.5;
  filter: alpha(opacity=50);
}

// ---------------------

// 多参数
@mixin center($width, $height) {
    position: absolute;
    left:50%;
    top:50%;
    width:$width;
    height:$height;
    margin:(-$height / 2) 0 0 (-$width / 2);
}
div {
    @include center(200px, 100px);
}
// css编译后样式
div {
    position: absolute;
    left:50%;
    top:50%;
    width:200px;
    height:100px;
    margin:-50px 0 0 -100px;
}

// -------------------

//多组值
@mixin box-shadow($shadow...) {
    -webkit-box-shadow: $shadow;
    box-shadow: $shadow;
}
div {
    @include box-shadow(0 1px 0 rgba(0,0,0,.4), 0 -1px 1px rgba(0,0,0,.4));
}
// css编译后样式
div {
    -webkit-box-shadow: 0 1px 0 rgba(0,0,0,.4), 0 -1px 1px rgba(0,0,0,.4);
    box-shadow: 0 1px 0 rgba(0,0,0,.4), 0 -1px 1px rgba(0,0,0,.4);
}
```
@content：在sass3.2.0中引入， 可以用来解决css3中 @meidia 或者 @keyframes 带来的问题。它可以使@mixin接受一整块样式，接收的样式从@content开始

```
//sass 样式               
@mixin max-screen($res){
  @media only screen and ( max-width: $res )
  {
    @content;
  }
}

@include max-screen(480px) {
  body { color: red }
}

//css 编译后样式
@media only screen and (max-width: 480px) {
  body { color: red }
}  
```
 使用@content解决@keyframes关键帧的浏览器前缀问题

```
// 初始化变量
$browser: null;
// 设置关键帧
@mixin keyframes($name) {
    @-webkit-keyframes #{$name} {
        $browser: '-webkit-'; @content;
    }
    @-moz-keyframes #{$name} {
        $browser: '-moz-'; @content;
    }
    @-o-keyframes #{$name} {
        $browser: '-o-'; @content;
    }
    @keyframes #{$name} {
        $browser: ''; @content;
    }
}

// 引入
@include keyframes(scale) {
    100% {
        #{$browser}transform: scale(0.8);
    }
}

// css编译后
@-webkit-keyframes scale {
    -webkit-transform: scale(0.8);
}
@-moz-keyframes scale  {
   -moz-transform: scale(0.8);
}
@-o-keyframes scale  {
    -o-transform: scale(0.8);
}
@keyframes scale  {
    transform: scale(0.8);
}
```
#### 7）颜色函数

 sass提供了一些内置的颜色函数
```
lighten(#cc3, 10%)　　  // #d6d65c
darken(#cc3, 10%) 　　　// #a3a329
grayscale(#cc3) 　　　　// #808080
complement(#cc3) 　　　// #33c
```
#### 8）引入外部文件

使用 @import 命令引入外部文件， 引入后，可使用外部文件中的变量等。

    @import "_base.scss";
### 五、高级用法

#### 1）函数 function

 sass允许用户编写自己的函数，以@function开始

```
$fontSize: 10px;
@function pxTorem($px) {
    @return $px / $fontSize * 1rem;
}
div {
    font-size: pxTorem(16px);
}
// css编译后样式
div {
    font-size: 1.6rem;
}
```
####  2）if条件语句

  @if语句可以用来判断

```
// sass样式
$type: monster;
div {
    @if $type == ocean {
        color: blue;
    } @else if $type == matador {
        color: red;
    } @else if $type == monster {
        color: green;
    } @else {
        color: black;
    }
}
// css编译后样式
div {
    color: green;
}
```
三目判断：语法为 if($condition, $if_true, $if_false)。 三个参数分别表示： 条件，条件为真的值，条件为假的值
```
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```
#### 3）循环语句

for循环有两种形式，分别为：

@for $var from <start> through <end> 和 @for $var from <start> to <end>。 

$var 表示变量，start表示开始值，end表示结束值，两种形式的区别在于 through 包括 end 的值，to 不包括 end 值。

```
// sass样式
@for $i from 1 to 4 {
    .item-#{$i} {width: 2em * $i;}
}
// css编译后样式
.item-1 {
    width: 2em;
}
.item-2 {
    width: 4em;
}
.item-3 {
    width: 6em;
}
```
 while循环 

```
// sass样式
$i: 2;
@while $i > 0 {
    .item-#{$i} {width: 2em * $i;}
    $i: $i - 1;
}
// css编译后样式
.item-2 {
  width: 4em;
}
.item-1 {
  width: 2em;
}
```
@each循环：语法为@each $var in <list or map>。 其中$var表示变量，而list和map表示数据类型，sass3.3.0新加入多字段循环和map数据循环

单字段list数据循环

```
//sass 样式
$animal-list: puma, sea-slug, egret;
@each $animal in $animal-list {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
//css 编译后样式
.puma-icon {
  background-image: url('/images/puma.png'); 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); 
}
.egret-icon {
  background-image: url('/images/egret.png'); 
}
```
 多字段list数据循环

```
//sass 样式
$animal-data: (puma, black, default),(sea-slug, blue, pointer);
@each $animal, $color, $cursor in $animal-data {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
    border: 2px solid $color;
    cursor: $cursor;
  }
}
//css 编译后样式
.puma-icon {
  background-image: url('/images/puma.png');
  border: 2px solid black;
  cursor: default; 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
  border: 2px solid blue;
  cursor: pointer; 
}
```
 多字段map数据循环

```
//sass 样式
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}
//css 编译后样式
h1 {
  font-size: 2em; 
}
h2 {
  font-size: 1.5em; 
}
h3 {
  font-size: 1.2em; 
}
```