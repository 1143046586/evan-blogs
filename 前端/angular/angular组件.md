### 一、生命周期函数
##### 1，初始化阶段
```
    构造函数 constructor()
    初始化生命周期函数 ngOnInit()
```   
##### 2，属性变更阶段
```
    ngOnChanges()   //输入属性的指针发生变更时执行(输入属性为对象不触发)，组件内的值变化不会触发
    ngDoCheck() //执行变更检测，任何可能导致视图或组件（不一定是该组件）发生变化时都会触发该函数，（值改变、输入框切换焦点。。。）
                //因此该函数必须高效和轻量级，
```
##### 3，视图更新阶段
```
ngAfterViewInit()   //视图初始化渲染完成，先执行子组件再执行父组件的生命周期函数
ngAfterViewChecked()   //视图变更检测，非常敏感，任何对组件的属性的改变和方法的调用都可以全部触发（不一定是该组件定义的） 
```
##### 4，投影阶段
```
ngAfterContengInit()  //当投影的内容初始化或改变时进行触发，顺序是先执行父组件的在执行子组件的
ngAfterContentChecked() //当投影的内容可能发生改变时就会触发，非常敏感
```
##### 5，卸载阶段
```
ngOnDestory()       //组件被卸载的时候触发
```


### 二、获取子组件实例
在父组件模板中通过#号，为子组件取个名字

    <app-child #c1></app-child>
    
在父组件中，通过viewChild装饰器将组件的实例赋给childC1，通过childC1可直接获取组件的属性和方法

    @viewChild("c1")
    childC1:LifeChildComponent
    
    
### 三、投影
类似于vue的slot内容插槽，不同的是，angular使用的是`<ng-content></ng-content>`标签，

具名投影：

给`<ng-content>`标签添加select属性对插入子组件的内容进行选择, 父组件引用子组件时可通过不同的标签或类名进行选择出口
```
//子组件
<div>
    <ng-content select=“div”></ng-content>
    <ng-content select=“.active”></ng-content>
</div>

//父组件
<app-child>
    <div>第一个插槽的内容</div>
    <p class="active">第二个插槽的内容</p>
</app-child>

```
### 四、管道（过滤器）
在组件控制器中定义数据, 在模板中使用管道对数据进行从处理，通过“ ：”进行传参，多个参数是使用多个：进行连接，例如处理时间
```
//js文件
time:Date=new Date()

//HTML文件
<p>{{time | date:yyyy年MM月dd日 HH:mm:ss}}</p>  //2018年12月3日 12:39:00
```
angular内置了一些管道，例如：
```
日期管道 ： date
数字管道 ： number : xx.yy-zz  //xx代表整数位，不够前面补零，yy代表小数位最少位数，zz代表小数最多位数
字母大写 :  uppercase
字母小写 ： lowercase

```
管道可以进行套接，执行顺序从左到右，左边的结果为右边的输入值

    <p>{{"UpdateTime" | uppercase | lowercase}}</p>      //updatetime

通过“ng g p pipedir/pipename”命令行命令新建自定义管道