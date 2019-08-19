### 一、函数的两种形式
```js
//第一种
function checkName(){
  ...
}
//第二种
var checkName=function(){
  ...
}

```
函数同样是变量，全局函数同样会污染全局变量空间
### 二、对象收编函数
#### 1、对象收编函数的两种方法
```js
//第一种
var CheckObject={
  checkName:function(){
    ...
  },
  ...
}
//第二种
var CheckObject={}
CheckObject.checkName=function(){
    ...
}
...
```
#### 2、可复制对象收编函数
通过函数返回一个对象来实现方法的复制
```js
var CheckObject=function(){
  return {
    checkName:function(){
      ...
    },
    ...
  }
}
```
使用时：
```js
var a=CheckObject()
a.checkName()
```
#### 3、使用类（构造函数）来完成方法的复制
```
var CheckObject=function(){
  this.checkName:function(){
    ...
  },
  ... 
}
```
使用时：
```js
var a=new CheckObject()
a.checkName()
```
#### 4、使用原型实现公用方法的两种方式
上面的方法每次实例化一次，都会新创建一个对象的方法，会造成额外的消耗，使用原型链实现公用方法
```js
//第一种
var CheckObject=function(){};
CheckObject.prototype.checkName=function(){
  ...
};
...
//第二种
var CheckObject=function(){};
CheckObject.prototype={
  checkName:function(){
    ...
  },
  ...
}
```
**注意：使用第一种方式后不能使用第二种，否则使用第一种添加的属性会被覆盖掉**
#### 5、链式调用方法
通过在方法的最后返回对象本身来实现方法的链式调用
```js
var CheckObject=function(){};
CheckObject.prototype={
  checkName:function(){
    ...
    return this;
  },
  checkEmail:function(){
    ...
    return this;
  },
  ...
}
```
链式调用：
```js
var a=new CheckObject();
a.checkName().checkEmail();
```
