### 一、对象
##### 1.对象的概念
对象Object 是一种引用数据类型 (在后期还会继续延伸对象的详细讲解)。

在 ECMAScript 中对象可以存储变量和函数(数据和功能)
##### 2.创建对象
    方式一:  使用new
      var obj = new Object();        //new方式  
      obj.name = ‘张三’;   	//创建属性字段    
      obj.age = 18;		//创建属性字段
      new关键字可以省略 
      var obj = Object();       //省略了new关键字,不建议
      
    方式二: 字面量方式 
      var obj = {               
           name :‘张三’,    //创建属性字段,最后加逗号 
           age : 18 
      };
##### 3.对象的属性与方法
1. 属性字段也可以使用字符串形式 
```
var box={ 
           “name” : “张三”,     //也可以用字符串形式 
           “age" : 28
      };
```
 
2.  使用字面量及传统赋值方式 
```
    var box={};       //字面量方式声明空的对象
    box.name=‘张三’;     //点符号给属性赋值
    box.age= 18;
```
3.  两种属性输出方式 
```
      alert(box.age);       //点表示法输出 
      alert(box[“age”]);        //中括号表示法输出，注意引号
```
4. 给对象创建方法 
```
    var obj={ 
           run : function() {   //对象中添加方法(函数)run
	    retrun “正在跑步..”; 
           }
    } 
    obj.run();    //调用对象中的方法
```
      
5.  使用 delete 删除对象属性 或 方法
```
     delete obj.name;     //删除属性
     delete obj.run;     //删除方法
```
### 二、Date对象
##### 1、Date对象的定义
Date类型使用自UTC(Coordinated Universal Time,国际协调时间) 1970年1月1日午夜(零时)开始经过的毫秒数来保存日期。Date类型保存的日期能够精确到1970年1月1日之前或之后的 285616年.
##### 2、Date对象的创建
创建一个日期对象，使用new运算符和 Date构造函数即可.

      var d = new Date(); 
//在调用Date构造方法而不传递参数的情况下，新建的对象自动获取当前的时间和日期
      
     创建日期对象并指定时间
      var d = new Date("2015/08/22");//字符串的形式指定时间
      var d = new Date(2016, 8, 13, 14, 34);//传参的形式指定时间
     注: 日期的格式可以是“2015/08/22”，“2015-08-22”，或1970年当前日期的毫秒数1443453475234
##### 3、Date对象的方法
```
setDate() / getDate();   从Date对象中返回一个月中的某一天(1~31)
getDay();   从Date对象返回一周中的某一天(0~6)
set / getMonth();  从Date对象中返回月份(0~11),使用是要+1
set / getFullYear();   从Date对象以四位数返回年份
set / getHours();	  返回Date对象的小时(0~23)
set / getMinutes();   返回Date对象的分钟(0~59)
set / getSeconds();   返回Date对象的秒数(0~59)
set / getMilliseconds();   返回Date对象的毫秒
set / getTime();   返回1970年1月1日至今的毫秒数
Date.parse(“2015-08-24”);   转换格式默认支持2015-08-24或2015/08/24, 返回距离1970年1月1日0时的毫秒数
date.toString();   把Date对象转换为字符串
date.valueOf();   返回Date对象的原始值

//下面的方法不常用
date.toDateString();  以特定的格式显示星期几、月、日和年
date.toTimeString();  以特定的格式显示时、分、秒和时区
date.toLocaleDateString();  以特定地区格式显示年、月、日
date.toLocaleTimeString();  以特定地区格式显示时、分、秒
date.toUTCString();  以特定的格式显示完整的 UTC 日期: 年,月,日,时,分,秒。

```
### 三、定时器与延时器
#### 1、定时器setInterval
      setInterval(): 定时器方法, 可按照指定的周期（以毫秒计）来调用函数或计算表达式

##### 1）创建定时器

    setInterval(code,millisec)
    code: 要调用的代码块或者函数
    millisec: 是周期性执行代码块或函数的间隔，以毫秒计
    
例如: 创建定时器timer, 每隔1秒调用一次函数function

    var timer = setInterval( function(){},1000);
    
##### 2）关闭定时器
setInterval()方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。 

由setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

例如: 关闭上面创建的定时器timer

    clearInterval(timer); 
    
##### 3）定时器的三种写法
写法一:  (直接使用字符串,不建议)

    setInterval(“alert(‘hello’)”, 1000);   

写法二: (直接传入函数名即可)
```
function func(){
     alert(“hello”);
}
setInterval(func, 1000); 
```

写法三: (推荐写法, 以后最常用)
```
setInterval(function(){   	 
      alert(“hello”);
}, 1000);
```
#### 2、延时器setTimeout

      setTimeout():  指定的时间过后执行一次代码.

setTimeout和setInterval的用法类似
##### 1）创建延时器

    var timer = setTimeout(function(){ }, 1000);

##### 2）取消延时器

    clearTimeout(timer);
