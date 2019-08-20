### 一、正则表达式介绍      
正则表达式(Regular Expression)是一个描述字符模式的对象，用于对字符串进行匹配, 一般用在有规律的字符串匹配中； 如： 匹配用户名是否正确， 邮箱是否正确等，正则表达式常用于表单验证, 如在HTML表单中填写的用户名、地址、出生日期, 邮箱等信息， 在表单提交到服务器做进一步处理之前, 我们需要先检查表单中的信息是否符合要求, 做表单验证,，以确认用户确实输入了信息并且这些信息是符合要求的.
### 二、正则表达式的创建
创建正则表达式有两种方式 : 

1, 使用new

    var box = new RegExp("box"); //传入非空字符串 
    console.log(box);   // /box/
    console.log(typeof box); //object

    var box = new RegExp("box", "gi");  //第二个参数是模式修饰符
    console.log(box); // /box/gi

2, 采用字面量方式

    var box = /box/;
    console.log(box);   // /box/
    console.log(typeof box); //object
			
    var box = /box/gi;
    console.log(box); // /box/gi

注意: 其中g表示全局匹配,  i表示忽略大小写
### 三、正则的使用
##### 1, 使用正则表达式匹配字符串有两种方式: 

    1, test() : 返回true则符合, false则不符合
    2, exec() : 返回数组则符合, null则不符合
这两种方法使用正则表达式对象去调用, 参数为要匹配的字符串

    var box = /box/gi;
    var str = "This is a Box bOX box";
    console.log(box.test(str));
    console.log(box.exec(str));
##### 2, 字符串的正则表达式方法

除了 test()和 exec()方法，String 对象也提供了 4 个使用正则表达式的方法。

**返回匹配结果的数组**

    var str = "This is a Box box BoX";
    var matchArr = str.match(/box/gi);
    console.log(matchArr); //返回数组或null
			
**查找并替换, 返回替换后的新字符串**

    var replaceStr = str.replace(/box/gi, "xxx");
    console.log(replaceStr);
			
**查找并返回匹配的字符串的起始位置,找不到匹配的则返回-1**

    var searchIndex = str.search(/box/i);
    console.log(searchIndex);
			
**根据指定字符串拆分, 返回拆分后的数组, 否则返回原字符串**

    var splitArr = str.split(/b/i);
    console.log(splitArr.length);
### 四、正则表达式的书写
```
```
[正则表达式手册](http://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
``` 
```