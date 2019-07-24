### 一、ES5新增内容
#### 1，严格模式
所谓严格模式，从字面上就很好理解，即更严格的模式 在这种模式下执行，浏览器会对JS的要求更苛刻，语法格式要求更细致，更符合逻辑。怪异模式：就是我们之前一直使用的开发模式，就叫怪异模式。因为很多时候出来的结果是非常怪异的，所以才称之为怪异模式。

使用严格（全局）模式

        "use strict";   //use strict写在代码最外层顶部
         n = 10;
         console.log(n);

使用严格（局部）模式

        function fn(){
            "use strict";   //use strict写在函数内部
            n = 10
            console.log(n);
        }
Javascript 为什么要设立严格模式：

    1.消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
    2.代码运行的一些不安全之处，保证代码运行的安全；
    3.提高编译器效率，增加运行速度；
 
严格模式下代码写法注意以下几点:

     1. 不可以省略var声明变量
    2. 禁止使用this关键字指向全局变量
    3. 禁止使用八进制方法
    4. 不允许在非函数的代码块内声明函数
#### 2，bind()方法
绑定一个新对象， 让函数中的this指向该对象， 一般在定时器中使用较多。

    setInterval(obj2.show.bind(obj), 2000);
#### 3，Array新增方法

##### 1）indexOf()
判断数组中是否包含某个元素， 和字符串的indexOf用法类似

##### 2）forEach()
用来遍历数组中的每一项；这个方法执行是没有返回值的，对原来数组也没有影响，传入一个回调函数，用回调函数对每一项进行处理

    var arr = [1,2,3,4,5];
    arr.forEach(function(value, index, array){
      console.log("值：" + value + ", 下标：" + index + ", 数组：" + array);
    });
    
##### 3）map()
和forEach非常相似，都是用来遍历数组中的每一项的，区别是map的回调函数中支持return返回

不管是forEach还是map都支持第二个参数值，第二个参数的意思是把匿名回调函数中的this进行修改。
```
var arr = [1,2,3,4,5];
var newArr = arr.map(function(value, index, array){
        return value * 2;
});
console.log(arr);  //[1,2,3,4,5]
console.log(newArr);  //[2,4,6,8,10]
```
==注意：forEach和map不支持IE8及以下==

##### 4）reduce()

用法： 接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终为一个值，是ES5中新增的一个数组逐项处理方法
```
   var result = arr.reduce(function(pre, cur, index, array){
	 console.log("前一个值：" + pre + "当前值：" + cur + ", 下标：" + index + ", 数组：" + array);
	 return cur;
   }, 10);
```
reduce接收两个参数， 第二个参数可选（为pre的初始值）

第一个参数是回调函数，该回调函数中又包含四个参数， 分别为：

    pre：上一次调用回调函数时的返回值，或者初始值
    cur：当前正在处理的数组元素
    index : 当前正在处理的数组元素下标
    array : 调用reduce()方法的数组
    result: 遍历完数组后的最终结果
使用reduce()求和

    var arr = [1,2,3,4,5];
    var sum = arr.reduce(function(pre, cur){
        return pre + cur;
    });
##### 5）forEach、map以及reduce的不同点：

    forEach 方法是将数组中的每一个值取出做一些程序员想让他们做的事情
    map 方法是将数组中的每一个值放入一个方法中做一些程序员想让他们做的事情后可以返回一个新的数组
    reduce 方法 将数组中的每一个值与前面的值处理后得到的最终值
##### 6）filter()
过滤出数组中你想要的元素， 不改变原数组,传入回调函数，用回调函数对数组中的元素进行过滤并返回一个新的数组
```
var arr = [1,2,3,4,5];
var newArr = arr.filter(function(value){
    return value>=3
});
console.log(arr);  //[1,2,3,4,5]
console.log(newArr); //[3,4,5]
```
### 二、ES6新增内容
#### 1，新增关键字let、const
let 是ES6中新增关键字。它的作用类似于var，用来声明变量，但是所声明的变量，只在let命令所在的代码块{}内有效。let关键字涉及到一个概念：==块级作用域==

ES6以前，只有全局作用域和函数局部作用域，ES6之后加入块级作用域，一个大括号{}我们称之为一个代码块，一个大括号{}的内部就是块级作用域

    利用块级作用域可以有效解决以下问题
    1.防止全局变量泄露
    2.防止变量提升带来的覆盖问题

const 声明的是常量，一旦声明，值将是不可变的, const 也具有块级作用域 ,  const 不可重复声明

#### 2，String新增方法

传统上，JavaScript只有 indexOf 方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法。

    includes()：返回布尔值，表示是否找到了参数字符串。
    startsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。
    endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。
    repeat()： 返回一个新字符串，表示将原字符串重复n次.

#### 3，Array新增
##### 1）Array.from()
用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）

    Array.from(arrayLike, mapFn, thisArg);
arrayLike：类似数组的对象（有length属性，有为索引的属性名）、字符串、Set、Map

mapFn：用来对转换中，每一个元素进行加工，并将加工后的结果作为结果数组的元素值（类似数组的map方法）。

thisArg：map函数中this指向的对象
```
let diObj = {
  handle: function(n){
    return n + 2
  }
}
let arr=Array.from([1, 2, 3, 4, 5], function (x){
    return this.handle(x)
  }, diObj)  
  console.log(arr)  //输出[3,4,5,6,7]
  ```
##### 2）Array.of()
用于将一组值，转换为数组

   var arr= Array.of(3, 11, 8) // [3,11,8]
##### 3）find()和findIndex()
find()函数用来查找目标元素，找到就返回该元素，找不到返回undefined。

findIndex()函数也是查找目标元素，找到就返回元素的位置，找不到就返回-1。

查找函数有三个参数。

    value：每一次迭代查找的数组元素。
    index：每一次迭代查找的数组元素索引。
    arr：被查找的数组。
例如
```
    const arr1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    var ret1 = arr1.find((value, index, arr) => {
        return value > 4
    })
    var ret2 = arr1.find((value, index, arr) => {
        return value > 14
    })
    var ret3 = arr1.findIndex((value, index, arr) => {
        return value > 4
    })
    var ret4 = arr1.findIndex((value, index, arr) => {
        return value > 14
    })
    console.log(ret1)   //5
    console.log(ret2)   //undefined
    console.log(ret3)   //4
    console.log(ret4)   //-1
```
##### 4）for-of遍历集合与for-in遍历集合
**for-in遍历** （遍历对象推荐使用）

    for(var index in objArr){
        console.log(objArr[index])
    }
以上代码会出现的问题：

1.index 值 会是字符串（String）类型，不能直接进行几何运算

2.循环不仅会遍历数组元素，还会遍历任意其他自定义添加的属性或方法，如，objArr上面包含自定义属性objArr.name，那这次循环中也会出现此name属性

3.某些情况下，上述代码会以随机顺序遍历数组

**for-of遍历**  （遍历数组推荐使用）

    for(let value of objArr){
        console.log(value)
    }
1.不会遍历自定义添加的属性和方法

2.不同于 forEach()，可以使用 break, continue 和 return

3.for-of 循环不仅仅支持数组的遍历。同样适用于很多类似数组的对象

4.for-in遍历的是键，for-of默认遍历的是值，按排列顺序进行遍历

for-of的扩展遍历
```
    //得到键值对
    for (let o of arr.entries()) {
        console.log(o[0], o[1]);    //o为单个键值对数组
    }

    //等到键
    for (let index of arr.keys()) {
        console.log(index);
    }

    //得到 值
    for (let val of arr.values()) {
        console.log(val);
    }
```
#### 4，Function新增
##### 1）函数默认参数
    function sayHello2(name='qianfeng'){
          document.write(`Hello ${name}`);  //注意引号
    }
##### 2）扩展运算符（...）
将一个数组转为用逗号分隔的参数序列。该运算符主要用于函数调用

     var arr=['张三','李四','王五'];
     function fn(a,b,c){
           console.log(`${a},${b},${c}`); //张三,李四,王五 
     }
     fn(...arr);  
     //还可用于合并数组
     [...arr1, ...arr2, ...arr3] 
##### 3）箭头函数
写法如下：
```

var fn=(param1, param2, …, paramN) => { statements }   //箭头左侧为参数，右侧为执行代码块并返回其值
var fn=(param1, param2, …, paramN) => expression   //右侧代码只有一句时可以省略大括号
var fn=singleParam => { statements }   // 如果只有一个参数，圆括号是可选的
var fn=() => { statements }        // 无参数的函数需要使用圆括号

//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。
var fn = id => ({ id: 11, name: "zhangsan" });

```
箭头函数的特点：
1，简化函数的书写
2,不绑定this

==**普通函数的this**==

普通函数的this指向==最终调用==该函数的==上层对象==,没有对象调用直接指向则this指向window
```
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
```
```
var o = {
    a:10,
    b:20,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12 函数是上层b对象.出来的
            console.log(this.b)  //undefined
        }
    }
}
o.b.fn();
```
```
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;     //赋值操作
j();                //最终执行时是window.j()进行的调用,
```
==**构造函数的this**==

new操作符会改变函数this的指向问题，首先new关键字会创建与构造函数同名的对象，然后将this指向这个对象。
```
function Fn(){
        this.user = "追梦子";
        console.dir(this);  //Fn对象
    }
    var a = new Fn();       //将Fn对象赋给a
    console.log(a.user); //追梦子
```
当构造函数this碰到return时,
```
function Fn(){
        this.user = "追梦子"; 
        console.dir(this);      //Fn对象
        return {}
    }
    var a = new Fn();       由于返回的是空对象,所以将空对象将Fn对象覆盖并赋给了a
    console.log(a.user); //undefined
```
当返回的数据类型为object或函数时,new产生的与构造函数同名的对象将被覆盖

==**箭头函数的this**==

由于箭头函数不绑定this， 它会捕获其所在（即定义的位置）上下文的this值， 作为自己的this值，所以 call() / apply() / bind() 方法对于箭头函数来说只是传入参数，对它的 this 毫无影响

箭头函数的 this 默认指向window

箭头函数的 this 具备继承性.根据父级function由谁最终调用来决定，没有父级function则指向window

如果调用父级function是某对象,那么this就是某对象
```
var objA = {
        name : "AAA",
        objB : {
            name : "BBB",
            show : function(){
                var fn=()=>{
                    console.log(this);  //this为function被执行时的this
                }
                fn();
            }
        }
    }
   objA.objB.show();            //  objB
   var CCC = objA.objB.show;
   window.CCC();                //window
```
作为对象的方法的箭头函数this指向全局window对象（没有父级function），而普通函数则指向调用它的对象
```
var objB={
        fn:{
            i: 10,
            b: () => console.log(this.i, this),
            c: function() {
                console.log( this.i, this)
            }
        }
    }
    objB.fn.b();  // undefined window{...}
    objB.fn.c();  // 10 Object {...}
```
#### 5，Object新增
##### 1）简写
属性的简写:

      var foo = 'bar';
      var baz = {foo};    // 等同于 var baz = {foo: foo};
方法的简写:

      var o = {
            method() {   
                 return "Hello!";
            }
      };
##### 2）表达式      
ES6 允许字面量定义对象时，用表达式作为对象的属性名，即把表达式放在方括号内

   属性名表达式:
   
      let obj = {};
      obj['a'+'bc'] = 123;
      console.log(obj);

方法名表达式:

      let obj = {
            ['h'+'ello']() {
                return 'hi';
            }
      }; 
##### 3）Object.is( , )
用来比较两个值是否严格相等。它与严格比较运算符（===）的行为基本一致， 不同之处只有两个：一是+0不等于-0，二是NaN等于自身
##### 4）Object.assign() 
用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。只要有一个参数不是对象，就会抛出TypeError错误。

    var target = { a: 1 };
    var source1 = { b: 2 };
    var source2 = { c: 3 };
    Object.assign(target, source1, source2);
    console.log(target) // {a:1, b:2, c:3}
#### 6，Set结构
数据结构Set类似于数组，但是成员的值都是唯一的，没有重复的值。

     var set = new Set([1,2,3,4,5,5,5,5]);
     console.log(set.size); // 5
Set的属性和方法:

size : 数量

add(value)：添加某个值，返回Set结构本身

delete(value)：删除某个值，返回一个布尔值，表示删除是否成功

has(value)：返回一个布尔值，表示该值是否为Set的成员

clear()：清除所有成员，没有返回值

**WeakSet**

WeakSet和Set一样都不存储重复的元素, 用法基本类似，但有一些不同点, WeakSet的成员只能是对象，而不能是其他类型的值。


==数组的去重问题==

    var arr = [1,2,1,2,3,4,3,4,6,6,2];		
    var set = new Set(arr);
    var newArr = new Array(...set);
    
#### 7，Map结构
Map 是一个“超对象”，其 key 除了可以是 String 类型之外，还可以为其他类型（如：对象）

    let map = new Map([[1, 'one'],[2, 'two'],[3, 'three']]);

他的方法和 Set 差不多:

size：返回成员总数。

set(key, value)：设置一个键值对。

get(key)：读取一个键。

has(key)：返回一个布尔值，表示某个键是否在Map数据结构中。

delete(key)：删除某个键。

clear()：清除所有成员。

keys()：返回键名的遍历器。

values()：返回键值的遍历器。

entries()：返回所有成员的遍历器。

#### 8，Symbol
Symbol 是ES6引入的一种新的数据类型，来表示独一无二的值

数据类型有6种：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object） 现在有7种了， 加上Symbol

Symbol值通过Symbol函数生成，Symbol不是一个构造函数，如果用new Symbol会报错（Symbol是一个原始类型的值，不是对象）。

    var symbol1 = Symbol();
    var symbol2 = Symbol("Alice");
    console.log(symbol1, symbol2) // 输出：Symbol() Symbol(Alice)
typeof运算符用于Symbol类型值，返回symbol。

    console.log(typeof Symbol("Alice")) // 输出：symbol
Symbol类型的值是一个独一无二的值，Symbol函数的参数只是表示对当前Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的。

    console.log(Symbol("Alice") === Symbol("Alice")); // 输出：false

在对象的内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中，如果不放在方括号中，该属性名就是字符串，而不是代表的Symbol值，Symbol值作为对象属性名时，不能用点运算符


    var name = Symbol();
    var obj1 = {
	[name]: "Alice"
    };
    var obj2 = {
	name: "Bruce"
    };
    console.log(obj1.name); // 输出：undefined
    console.log(obj1[name]); // 输出：Alice
    console.log(obj2.name); // 输出：Bruce
    console.log(obj2[name]); // 输出：undefined
    
    

#### 9，解构赋值
es6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称之为解构

**数组的解构赋值**

数组的解构一般有三种情况，完全解构，不完全解构，解构不成功

完全解构如下:

    var [a,b,c] = [1,2,3];
    console.log(a);//1
    console.log(b);//2
    console.log(c);//3
    //本质上这种写法属于‘模式匹配‘，只要等号两边的模式相同，左边的变量就会被赋予对应的值

不完全解构如下:

    let [x,y] = [1,2,3];
    console.log(x); //1
    console.log(y); //2
    let [a,[b],d] = [1,[2,3],4];
    console.log(a); //1
    console.log(b); //2
    console.log(d); //4
    //不完全解构：即等号左边的模式，只匹配一部分的等号右边的数组，这种情况下解构依然成功
    
解构不成功如下:

    let [x,y,z] = ['hah'];
    console.log(y); //undefined
    //如果解构不成功，变量的值就等于undefined
解构允许默认值:

    let [x,y='b'] = ['a'];
    console.log(y); //b

    let [x,y = 'b'] = ['a',undefined];
    console.log(y); //b ,数组成员为undefined时，默认值仍会生效(因为在ES6内部使用严格相等运算符‘===‘，判断一个位置是否有值，所以当一个数组成员严格等于undefined,默认值才会生效)

    let [x,y = 'b'] = ['a',null];
    console.log(y); //null,如果一个数组成员是null,默认值就不会生效，因为null不严格等于undefined
    
**对象的解构赋值**
    
对象的解构与数组有一个重要的不同，数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值
   
    //1. 变量名与属性名一致的情况下
    let {foo,bar} = {foo : "aaa",bar : "bbb"}
    console.log(foo); //aaa
    console.log(bar); //bbb
    //变量名与属性名不一致的情况下，必须这样写
    let {a : name, b : age} = {a : 'zhangsan', b : 33};
    console.log(name); //zhangsan
    console.log(age);  //33
    
**字符串的解构赋值**
    
    const [a,b,c,d,e] = 'hello';
    console.log(a); //h
    console.log(b); //e
    console.log(c); //l
    console.log(d); //l
    console.log(e); //o

    let { length : len} = 'yahooa';
    console.log(len); //5,类似数组的对象都有一个length属性，还可以对这个属性解构赋值
    
**函数的解构赋值**

    function func([x,y]){return x - y}；
    console.log( func([19,11]) );
    

