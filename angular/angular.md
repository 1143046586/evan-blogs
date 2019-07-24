#### 安装脚手架:
```
npm i angular-cli-g//全局安装angular脚手架工具
ng -v  //查看angular-cli版本号
ng new 项目名 //创建angular项目
npm i//进入项目目录安装项目需要的的模块
ng g component components/组件名 //创建组件
ng serve ||npm run start //启动服务
goodsList:Array<object>=[{name:"shoes",price:998,stock:98}]//typescript是一种强类型语言定义数据模型的时候需要声明数据的类型  
ng g class classBox/Goods //创建类
<p>
  goods works!
</p>
<ul *ngIf="true">
  <li *ngFor="let item of goodsList;let i = index">
  <img [src]="item.imgSrc" />
  <h5>商品名{{i}}:{{item.name}}</h5>
  <p>价格:{{item.price}}</p>
</li>
</ul>
<!-- 
  常用指令:
    *ngIf="" 决定元素是否存在
    *ngFor="" 循环列表类型的数据
 
  标签属性的动态值使用方式
    1  <img src="{{imgSrc}}">
    2  <img [src]="imgSrc">
 -->
 export class GoodsComponent implements OnInit {
  //输入属性
  @Input('list') //参数用于定义输入属性名  
  //在调用时 应该采取如下格式  <app-goods [list]="xxx"></app-goods>
  goodsList : Array<Goods>=[]; 
  constructor() { }

  ngOnInit() {
  }

}
 import { Component } from '@angular/core';
import { Goods } from './classBox/goods';
/** 组件元数据装饰器 */
@Component({
  selector: 'app-root',//定义组件名
  templateUrl: './app.component.html',//声名模板文件(html结构代码存放文件)
  styleUrls: ['./app.component.css']//声名组件的样式文件
})
export class AppComponent {//控制器
  title = 'app works!';
  list:Array<Goods>=[
    new Goods("父级内的商品1",998,"1","http://iph.href.lu/200x100"),
    new Goods("父级内的商品2",98,"2","http://iph.href.lu/200x100"),
    new Goods("父级内的商品3",198,"3","http://iph.href.lu/200x100")
  ]
}
<buttom (click)="addItem()">addItem</buttom>//(click)绑定事件
```   
#### angular路由
`ng new 项目名 --routing//创建带路由的angular项目`  
###### 路由文件配置
```
const routes: Routes = [ //创建路由
  {
   { path: '', redirectTo:"/home",pathMatch:"full" },//重定向
    path: 'home',//路径 此处不加'/'
    component:HomeComponent,//引入组件
    children: [//引入子路由
      {
        path:'a',
        component:AComponent
      },
      {
        path:'b',
        component:BComponent
      }
    ]
  },
  {
    path:'goods',
    component: GoodsComponent
  }
];
```
#### 路由跳转
###### 完全匹配

`<router-outlet></router-outlet> //父路由标签,当某个路径拥有子路由时添加到该路由的html文件`  

`routerLink //可以使用任意标签,用法如下:`
```
<button routerLink="/goods"></button>
//必须加'/'
```
###### 编程式导航
```
 constructor(private router : Router) {  }//依赖注入  
  goGoods(){  
    this.router.navigate(['/goods'])
    this.router.navigateByUrl('/goods')//两种路由路径引入方式
  }
  //编程式导航
  <button (click)="goGoods()">去到Goods页</button>
  
```
##### 路由传参  
###### 问号传参
```
<!-- 
传递通过queryParams属性进行传递 例如:   <a routerLink=""  [queryParams]="{id:9}"></a>
接收时  需要通过ActivatedRoute 服务来进行接收 通过依赖注入的形式来获取该服务的实例
例如:    constructor(private abc:ActivatedRoute){}
然后在组件类的内部  通过  this.abc.snapshot.queryParams来获取
-->
<a  href="javascript:;" routerLink=""  [queryParams]="{id:9}">商品列表</a>
export class GoodsComponent implements OnInit {
  constructor(private ar:ActivatedRoute) { }
  ngOnInit() {
    let {goodsID} = this.ar.snapshot.queryParams
    console.log(goodsID)
  }
 ``` 

###### 路径传参
```
<!-- 
首先需要在访问路径上进行配置  例如:   path:"goods/:goodsID"
传递参数通过 routerLink属性中数组的第二个值进行传递 例如:   [routerLink]="['/goods',123]"
接收参数一样通过 ActivateRoute 服务进行接收   同上
this.abc.snapshot.params来获取 --> 

const routes: Routes = [
  {
    path:'goods/:goodsID',
    component: GoodsComponent
  }
];
<button [routerLink]="['/goods',goodsID]" >商品列表</button>
export class AppComponent {
  title = 'app works!';
  goodsID = 123
}
export class GoodsComponent implements OnInit {
  constructor(private ar:ActivatedRoute) { }
  ngOnInit() {
    let {goodsID} = this.ar.snapshot.params
    console.log(goodsID)
  }

}
``` 
###### 参数订阅
 ``` 
 通过.subscribe()方法订阅监听函数 即可在函数内拿到最新的路径参数  
 
  例如:
      constructor(private ar:ActivatedRoute){}
 
      ngOnInit(){
        this.ar.params.subscribe(params=>{})
         this.ar.queryParams.subscribt(params=>{})
       }
 ```
######  辅助路由
```
<router-outlet name="box"></router-outlet>
<button [routerLink]="[{outlets:{box:'bb'}}]">BABABA</button>
<a href="javascript:;" [routerLink]="[{outlets:{box:'ba'}}]">BBBBBB</a>
 {
    path:'bb',
    component: BBComponent,
    outlet:"box" 
  },
  {
    path:'ba',
    component : BAComponent,
    outlet:"box"
  }
```
###### 路由守卫
```
CanActivate: 处理导航到某路由的情况。
    CanDeactivate: 处理从当前路由离开的情况。
    Resolve: 在路由激活之前获取路由数据。

使用路由守卫要在路由模块进行配置
const routes: Routes = [
        ...
        {path:'routerguard',component:RouterguardComponent,
        canActivate:[CanActivate], // 这里！
         },
        ]

        @NgModule({
        imports: [ 
     RouterModule.forRoot(routes)
    ],
    exports: [RouterModule],
     providers: [CanActivate] // 这里！
```
######  接口
```
class stu implements User{
    username="ziz",
    age=18,
    login(){
        
    },
    regsiter(){
        
    }
    
}//给stu类创建一个接口User,接口只定义实现该接口的类需要的方法和属性
```
##### 服务
###### 创建服务
`ng g s services/sayHello.service`
`服务本质是一个类,我们会将业务逻辑封装到类(服务)的一个方法中:`
```
import { Injectable } from '@angular/core';
@Injectable() //'@Injectable':使当前的服务可以调用其它的服务  
export class SayHelloService {
  constructor() { }
     sayHello(a){ //业务逻辑 方法
        console.log("Hello,"+a)
     }
}
```
`providers["xxxxservers"] //提供器,提供服务实例,有则调用,无则创建`
`构造函数内声明实例:sh construct(private sh:servers){}`
`//形参调用方法,`
`this.sh.sayhello("xxx")`
`依赖注入:当某个组件需要服务,由提供器将服务的实例注入该组件`

##### 组件之间传值
###### 子组件传值给父组件
```
###### 子组件输出
定义一个初始值和一个修改值的方法:  
count:number=0;
add(){this.count++}
在子组件的结构中绑定一个触发修改值方法的事件:  
<button (click)="add()"></button>
给出输出的装饰器@Output(),  
定义一个属性通知父级值发生改变(注意声明EventEmitter发送数据的类型的类型):  
countChange:EventEmitter<number>=new EventEmitter()  
this.countChange.emit(count)//实例调用emit方法将count发送出去

###### 父组件接收  
<app-child (countChange)="getCount($event)"></app-child>//绑定自定义事件 'countChange'让事件调用父组件的函数,  
//这个函数是用于父组件接收子组件传过来的值($event: 子组件发送过来的数据)的,
然后在父组件内写好getCount方法:
 getCount(count) {
    console.log(`父组件接收到的值为:${count}`)
  }
```
######  子组件传值给兄弟组件(子->父->子(兄弟)中间人模式)
```
在父组件设置一个属性接收到前一个子组件传给父组件的值:
childCount:number=0;
 getCount(count) {
    console.log(`父组件接收到的值为:${count}`)
    this.childCount=count;
 }
 在父组件结构页面中将'childCount'传给接受参数的子组件:
 <app-child2 [count]="childCount"></app-child2>(用[count]来接收)
 在接受参数的子组件中定义一个输入属性(就是上面用来接收'childCount'的[count]):
 @Input
 count:number
 

```
###### 数据双向绑定
```
<input type="text" [(ngModel)]="username">
```
###### rxjs 响应式编程 ||流对象
```
######## obj.debouceTime(xxx).subscribe()当触发事件间隔时间小于xxxms,事件不会被发送给监听者
######## obj.subscribe()//流对象订阅方法,可以绑定监听函数,每次流对象触发事件时,该函数绑定的监听函数即被触发
######## obj.map()//对流对象里面的值进行2次处理
######## obj.filter()//流对象过滤器,允许的值才能继续进行处理.并且可在特定情况下拦截事件的触发
```
###### FormsMoudle
```
首先在app.moudle.ts中导入'ReactiveFormsModule'
 imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule,
    ReactiveFormsModule
  ]  
  绑定一个对象,这个对象是'FormControl'的实例:  
  username:FormControl = new FormControl()//括号内可以给定默认值 
  将username属性和输入框进行绑定
  <input type="text" [formControl]="username"/>
  在'username'对象下有'valueChanges'流对象,这个流对象会在'input'的'value'值发生改变时触发subscribe函数
  this.username.debouceTime(500).subscribe(value=>{
    console.log(value) 
  })//当两次事件触发事件时间间隔小于500ms时,那么两次事件将不会被发送给subscribe

```
