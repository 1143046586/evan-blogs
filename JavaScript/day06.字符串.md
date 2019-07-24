### 一、JS字符串的概念

    字符串就是一串字符，由双（单）引号括起来。
    字符串是 JavaScript 的一种数据类型。 
    
###  二、字符串的定义

    方式一(推荐):  var str = '亲'；    //基本类型 
    定义了一个字符串变量str，内容为'亲'

    方式二: var str = new String('hello');       //引用类型
    定义一个字符串变量str，内容为hello，
    注意: 此刻str为引用类型(object对象)
    用new产生的变量都是引用类型的变量，也叫对象

    方式三: var str = String("hello");//基本类型
      
值类型: string, number, boolean, undefined,null等

引用类型/对象: Array , Date, Object, String, Function等

##### new String()和String()的区别

    var s1 = new String(‘hello world’);
    var s2 = String(‘hello world’);
    console.log(typeof s1); //object
    console.log(typeof s2); //string


##### 字符串的属性
    
    length 字符串的长度
    
==注意: ECMAScript 中的字符串是不可变的; 也就是说，字符串一旦创建，它们的值就不能改变.==

    例如:  var str = “亲,包邮哦”;
           str[0] = “唉”;  //不会改变

如果要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量.

    var str =  “Hello”;
    str = str+” world!”;


### 三、字符串的方法(函数)

##### 1.charAt（）

通过指定的下标得到字符与[index]的作用是一致;
    
##### 2.charCodeAt(0)

通过指定的下标,返回该字符串的ASCII码
    
    
    ASCII（American Standard Code for Information Interchange，美国标准信息交换代码）
    
##### 3.String.fromCharCode(c1, c2, ...)   

含有指定编码的字符的新字符串。

    var s = String.fromCharCode(104, 101, 108, 108, 111);
    
    这个静态方法提供了一种创建字符串的方式，即字符串中的每个字符都由单独的数字 Unicode编码指定。注意，作为—种静态方法，fromcharCode()是构造函数 String()的属性，而不是字符串或String对象的方法。

    String.charCodeAt()是与String.fromCharCode()配套使用的实例方法，它提 供了获取字符串中单个宁符的编码的方法。

##### 4.str.concat();  连接字符串

    str.concat();  连接字符串
    例如: var str1 = “hello”;
    var str2 = str1.concat(“ world”);
    
    
#### 字符串的查找方法
##### 5.str.indexOf(“abc”);  
查找字符串第一次出现的位置, 如果没找到则返回-1
   
    例如:  var str = “abcdabcd”;
           var subStr = “bcd”;
           var index = str.indexOf(subStr);
              
##### str.lastIndexOf(“abc”); 

查找字符串最后一次出现的位置, 如果没找到则返回-1
    例如: var index = str.lastIndexOf(“abc”);

    
    
##### 6.str.search(); 正则匹配
    
    (返回第一次出现的位置)
    例如: var str = “Abcdabcd”;
          var index = str.search(/abc/gi);
          注: g表示进行全局匹配，i表示匹配的时候忽略大小写
          
#### 字符串的截取和替换         
##### 7.str.replace(); 替换字符串

    例如: var str = “how are Are are you!”;
          var newStr = str.replace(“are”, “old are”);
          
    这里的替换只能执行一次，不能够进行全局匹配，如果需要全局匹配，则应使用正则表达式： 
    str.replace(/are/gi, "old are")

##### 8.str.substring(start,end); 

截取字符串 范围是[start, end)
    
    例如: var str =  “Hello world!”;
        console.log(str.substring(2,5));
        注: 如果只有一个参数, 则表示到字符串最后
                
##### 9.关于字符串截取的3个函数slice,substring,substr对比

    slice，substring,substr三个函数都是截取字符串，但是对参数的处理有区别

    参数处理相似的两个函数式slice和substring

    slice(start,end)和substring(start,end)

    他们两个的end都是原字符串的索引，意思为截取到end（不包括end）位置的字符

    二者的区别是：slice中的start如果为负数，会从尾部算起，-1表示倒数第一个，-2表示倒数第2个，此时end必须为负数，并且是大于start的负数，否则返回空字符串,slice的end如果为负数，同样从尾部算起，如果其绝对值超过原字符串长度或者为0，返回空字符串

    substring会取start和end中较小的值为start,二者相等返回空字符串，任何一个参数为负数被替换为0(即该值会成为start参数)

    而substr比较特殊,substr的end参数表示，要截取的长度,若该参数为负数或0，都将返回空字符串
    
    当参数是负值情况下，slice将传入负值与字符串长度(string.length)相加，substr会将负的第一个参数加上字符串长度，第二个转换为0，substring会将所有负值都转换成0。
    IE的JavaScript实现在处理向substr()方法传递负值的情况时存在问题，它会返回原始的字符串。

##### 10.str.split(separator, howmany);

    根据分隔符、拆分成数组
          separator(字符串或正则表达式)
          howmany(可以指定返回的数组的最大长度, 可以省略)
          注:如果空字符串(“”)用作separator, 那么stringObject中的每个字符之间都会被分割。
          

##### 11.str.toLowerCase(); 把字符串转换成小写

##### 12.str.toUpperCase(); 把字符串转换成大写
### 四、字符串模板
ES6 中引进的一种新型的字符串字面量语法 - 模板字符串。书面上来解释，模板字符串是一种能在字符串文本中内嵌表示式的字符串字面量。简单来讲，就是增加了变量功能的字符串。
#### 用法：
    // 普通字符串
    `In JavaScript '\n' is a line-feed.`
 
    // 多行字符串
    `In JavaScript this is not legal.`
 
    // 字符串中嵌入变量
    var name = "Bob", time = "today";
    `Hello ${name}, how are you ${time}?`  // Hello Bob, how are you today?

模板字符串使用反引号 ` 来代替普通字符串中的用双引号和单引号。模板字符串可以包含特定语法(${expression})的占位符。占位符中的表达式和周围的文本会一起传递给一个默认函数，该函数负责将所有的部分连接起来。
而占位符${}，可以是任意的 js 表达式（函数或者运算），甚至是另一个模板字符串，会将其计算的结果作为字符串输出。如果模板中需要使用$,{等字符串，则需要进行转义。
不同于普通字符串，模板字符串还可以多行书写，模板字符串中所有的空格，新行，缩进都会原样的输出在生成的字符串中。

而单纯的模板字符串还存在着很多的局限性。如：

不能自动转义特殊的字符串。（这样很容易引起注入攻击）
不能很好的和国际化库配合（即不会格式化特定语言的数字，日期，文字等）
没有内建循环语法，（甚至连条件语句都不支持， 只可以使用模板套构的方法）



    
    


    

