
### 一.什么是JavaScript?

     1.与页面进行交互的脚本语言,具有较强的逻辑性.
     
### 二.JavaScript语言的特点

    1.脚本语言。JavaScript是一种解释型的脚本语言
    2.基于对象
    3.简单
    4.动态性
    5.跨平台性
    
### 三.JavaScript的组成部分

    1.  核心(ECMAscript:欧洲计算机制造商系会) (语法规范:ECMA-262标准)
    2.  BOM 浏览器对象模型(browser object model)
    3.  DOM 文档对象模型(document object model)

### 四.<script>标签的属性: 

    - src 表示要引入的外部文件
    - type 表示脚本语言的类型  text/javascript,默认值就是它.
    - language已废弃。原来用于代码使用的脚本语言。由于大多数浏览器忽略它，所以不要用了。
    - defer：可选。(等页面加载完成后,才执行js)表示脚本可以延迟到文档完全被解析和显示之后再执行。由于大多数浏览器不支持，故很少用。
    - charset：可选。表示通过 src 属性指定的字符集。由于大多数浏览器忽略它，所以很少有人用它。
    - async:可选,能简单实现js的异步加载.
    
### 五.声明变量

```
// 1.使用var关键字(variable:可变的量)
    var uname; // undefined 
    uname="周杰伦"; //赋值
    
// 2.声明变量即赋值(推荐)
     var unam="周杰伦";
     
// 3.声明多个变量
     var unam="周杰伦",age=20;
```

     
#####  ==注意: 我们在定义变量的时候， 尽可能的不要只声明，不赋值, 而是声明的同时初始化一个值==
    
### 六.数据类型

1.基本的数据类有6种
    
    string 字符串类(单引号或双引号都是字符串)、
    number 数值 类型(包含整型,浮点型)、
    boolean 布尔类型(true/false)、
    null (顶级)  Object、 
    undefined 未定义类型、 
    object  对象类型 (array,function ....)
    
    null派生了undefined
    
    //分2种类型
    1. 值类型 string,number,boolean,undefined
    
    2. 引用类型 object ,array,function
    
    
    
2.查看基本的数据类型的方式


```
    typeof uname
    typeof(uname)
```
### 七 数据类型转换
#### 1.显示转换
##### a. 转数字

**1）Number(a):**

代码：
```    
var a = “123”;
 
a = Number(a);
```
注意：

a) 如果转换的内容本身就是一个数值类型的字符串，那么将来在转换的时候会返回自己。

b) 如果转换的内容本身不是一个数值类型的字符串，那么在转换的时候结果是NaN.

c) 如果要转换的内容是空的字符串，那以转换的结果是0.

d) 如果是其它的字符，那么将来在转换的时候结果是NaN.

==ECMAScript 提供了 isNaN()函数，用来判断是不是 NaN。isNaN()函数在接收到一个值之后，会尝试将这个值转换为数值==

**2）parseInt():**

代码：
```
var a = “123”; a = parseInt(a);
```

a) 忽略字符串前面的空格，直至找到第一个非空字符,还会将数字后面的非数字的字符串去掉。

b) 如果第一个字符不是数字符号或者负号，返回NaN

c) 会将小数取整。（向下取整）

**3）parseFloat();**//浮点数（小数）

与parseInt一样，唯一区别是parseFloat可以保留小数。
##### b.转字符串
**1）String():**
```
var a = 123;
 
a = String(a);

```
**2）toString()**
```
var a = 123; a = a.toString();
```
undefined，null不能用toString。
##### c.转boolean类型：
**Boolean():**
```
var a =”true”; a = Boolean(a);
```
注意：在进行boolean转换的时候所有的内容在转换以后结果都是true，除了：false、""（空字符串）、0、NaN、undefined
#### 2.隐式转换
##### a) 转number:
```
var a = “123”;

a = +a;
```
加减乘除以及最余都可以让字符串隐式转换成number.
##### b) 转string:
```
var a = 123;

a = a + “”;
```
##### c) 转boolean:
```
var a = 123;

a = !!a;
```
     
### 八 关键字 : 已经被JS内部使用了的 

        
  ![image](F7C09A1F293C4734B2999A949EB37CAE)

### 九 保留字: 虽然暂时还未被使用, 但将来可能会被JS内部使用

![image](57145C3F15C9406483A143615D8CEF39)



### 十 变量的命名规范 
    只能是数字,字母,$,下划线_,但是不能以数值开头,不能使用关键字或保留字.
    驼峰命名法.
    1.大驼峰
        var YourName=new Person();//用于表示对象
    2.小驼峰
        var yourName="周杰伦";//表示普通的变量
     


#### 特殊的面试题
    []==[]; //false 2个[]不同对象的地址
    []!=[]; //true 
    []==false; //ture []和false不同类型先转化为0，然后0==0
#### 关于0.1+0.2=0.30000000000000004

我们知道，计算机中存储的都是二进制的0和1，而我们现实中的数存入计算机中转换为二进制时有可能不能整除，也就是不能正好整除，所以用二进制表示现实中的数并计算就产生了误差。

[0.1+0.2!=0.3 参考文献](https://www.cnblogs.com/mooncher/p/5145571.html/)