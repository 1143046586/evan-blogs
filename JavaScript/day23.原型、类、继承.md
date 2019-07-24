### 一、原型
#### 1, 原型是什么
[原型链参考资料](https://www.cnblogs.com/shuiyi/p/5305435.html)

原型, 英文名prototype是函数中一个自带的属性, 我们创建的每个函数都有一个prototype(原型)属性, 这个属性是一个对象. 

#### 2, 原型的作用

原型的作用是: 可以让同一个构造函数创建的所有对象共享属性和方法. 也就是说, 你可以不在构造函数中定义对象的属性和方法,  而是可以直接将这些信息添加到原型对象中。

例如: 
```
function Person() {}    //声明一个构造函数
Person.prototype.name = “zhang”   //在原型里添加属性
Person.prototype.age = 100;
Person.prototype.show = function () {     //在原型里添加方法
      return this.name + this.age;
};
var person = new Person();

比较一下原型内的方法地址是否一致
var person1 = new Person();
var person2 = new Person();
console.log(person1.show == person2.show);  //true，方法为共有的一个方法,所以引用地址一致
```
#### 3, 原型的属性
constructor: 是原型的属性, 指向原型对象所属的构造函数;

__proto__: 是对象的属性，指向构造函数的原型。

    console.log(box1.__proto__);   //[object Object]
    console.log(box1.__proto__ === Box.prototype);

isPrototypeOf(): 判断一个对象是否指向了该构造函数的原型对象

    console.log(Box.prototype.isPrototypeOf(box));  //true, 实例对象都会指向

**原型模式的执行流程：**

1.先查找实例对象里的属性或方法，如果有，立刻返回；

2.如果实例对象里没有，则去它的原型对象里找，如果有就返回；

例如: 

    function Person() {}
    Person.prototype.name = "张三";
    var person = new Person();
    person.name = "李四";
    console.log(person.name); //李四, 找到了实例对象的值
#### 4, 原型使用字面量的写法
```
function Person() {};
Person.prototype = {   
      name: '张三',
      show: function () {
            return this.name;
      }
};
```
如果想让字面量方式的constructor指向实例对象，那么可以这么做：

    Person.prototype = {
         constructor: Person  //直接强制指向即可
    };
**仅使用原型的缺点**

单独使用原型来给对象添加属性和方法, 是有缺点的, 具体有以下两点:

   1, 它省略了构造函数传参初始化这一过程, 带来的缺点就是初始化的值都是一致的
   
   2, 原型对象共享的属性或者方法是公用的, 在一个对象修改后,会影响其他对象对该属性或方法的使用.

**构造函数+原型模式**

使用构造函数添加私有属性和方法, 
使用原型添加共享的属性和方法

优点: 

1, 实例对象都有自己的独有属性

2, 同时共享了原型中的方法,最大限度的节省了内存

3, 支持向构造函数传递参数

### 二、类
ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。
```
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
         return '('+this.x+', '+this.y+')';
    }
}
```
 注意： constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。

Class的继承

Class之间可以通过extends关键字，实现继承
```
class Point {
	constructor(x, y) {
	       this.x = x;
	       this.y = y;
	 }
}
class ColorPoint extends Point {
	 constructor(x, y, color) {
	       super(x, y);   
	       this.color = color; 
	 }           
	 getData(){
	       console.log(this.x+','+this.y+','+this.color);
	       return this.x+','+this.y+','+this.color;
    }
} 			
```
### 三、继承
#### 1, 继承的概念  

继承是面向对象的一个非常重要的特征. 继承指的是: 子类继承父类的属性和方法.

我们可以通过继承的方式, 在父类的属性和方法基础上, 在无需修改父类的情况下, 让子类也有拥有这些属性和方法, 并可以扩展其他属性和方法.

继承的特点: 

1, 子类拥有父类的属性和方法；

2, 子类可以有自己新的属性和方法；

3, 子类可以重写(override)父类的方法   (js里面没有重载 overload,同名不同参)

继承的优点

1, 代码复用：子类可以继承父类的属性和方法

2, 更灵活：子类可以追加或修改属性和方法

#### 2, 子类和父类
子类: 通过继承创建的新类, 也称“派生类”

父类: 被继承的类, 也称“基类”.  base 

继承的过程，就是从一般到特殊的过程.

例如:

有一个Person类, Employee是员工类，Manager是管理者类，Employee员工类可以继承Person类,因为员工也是人, Manager管理者类可以继承Employee员工类, 因为管理者也是员工。

有一个Animal动物类, Monkey是猴子类, Dog是狗类, 那么Monkey和Dog都是动物,都可以继承Animal动物类的属性和方法, 
#### 3, 原型链继承
因为JS在ES6之前没有类, 所以和其他语言的类继承不一样, 我们需要借助原型来实现继承, 称为原型链继承, 即通过原型进行继承.

例如: 

    function Person() {
          this.name = 'Zhang';
    }
Employee构造函数(Employee类)

    function Employee() {
          this.age = 100;
    }
Employee继承了 Person，通过原型，形成链条

    Employee.prototype = new Person();  

    var employee= new Employee();     //创建Employee对象, 得到被继承的属性
    console.log(employee.age);
    console.log(employee.name);	

#### 4，对象冒充继承

创建子类的实例对象时，无法向父类的构造函数中传递参数 

为了解决这个问题，我们采用一种叫借用构造函数的技术，或者称为对象冒充的技术来解决

对象冒充: 使用构造函数调用call()或者apply()

父类

    function Person(name, age){
          this.name = name;
          this.age = age;
    }		
子类

    function Employee(name, age){
          Person.call(this, name, age); //对象冒充
          //Person.apply(this, [name, age]);
    }
对象冒充的实质是在子类实例化时通过call()和apply()函数调用了父类函数，在父类函数中将this换成子类的实例化对象并对子类实例化对象进行添加属性并赋值的操作

#### 5, 组合继承
组合继承: 原型链+借用构造函数(对象冒充)的模式
父类

    function Person(name, age){
         this.name = name;
         this.age = age;
    };
    Person.prototype.run = function(){ 
         return this.name + this.age;
    };
子类

    function Employee(name, age){				
         Person.call(this, name, age);
    };
    Employee.prototype = new Person(); //原型链继承Person原型中的方法
    var employee = new Employee("zhangsan", 100);
    var employee2 = new Employee("lisi", 200);
    console.log(employee.run());
    console.log(employee2.run());
    
其过程可以看做先使用原型链继承来自父类的公有属性和方法，然后使用对象冒充对子类赋予和父类公有属性相同名字的属性 ,将使用原型链继承自父类的公有属性进行屏蔽,从而实现了公有属性的私有化！　


#### 请说说 bind和call,apply的区别?
 都用来更改this的指向
 
bind() 作用改变函数的this,但是函数不会立即执行,需要再接()才能执行,参数写在后接的()内

    fn.bind(this)(param1,param2...)

call() 作用也是改变函数的this,但是立即执行.

call函数里的参数,第一个就是要改变的this对象,后面就是对应参数

    fn.call(this,param1,param2..);
    
apply 作用也是改变函数的this,立即执行.与call一样

apply 只有2个参数,第一个参数修改this的指向，第二参数是一个数组;

    fn,apply(this,[param1,param2...]);



#### 其他

instanceof 检测引用类型 检测某一个实例对象 是否来之与 某一个 构造函数

typeof     检测值类型
