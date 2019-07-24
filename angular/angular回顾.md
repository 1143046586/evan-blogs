#### 使用脚手架搭建项目
##### 下载脚手架
        npm i angular-cli -g

###### 下载成功 可以用ng-v来确认版本号
##### 创建项目
        ng new (项目名) --routing
###### 后面带--routing会自动创建路由模块,也可以先创建项目后在手动创建
        ng g m app-routing --flat --module=app
###### 但是要自己引入RouterModule, Routes,并导出RouterModule很麻烦,建议在项目创建时直接创建路由



##### 创建类
        ng g cl (类的路径)

###### 类创建完成后会自动导出
        export class Goods {
    constructor(public name:string,public price:number,public goodsId:string){

    }
}
###### private、protected、public和的区别
    1.public    所有组件都可以用
    2.protected 只有自己和子组件可以用
    3.private   只有自己可以用

### 组件
##### 创建组件
        ng g c (组件路径)
###### 组件创建完成后会导出并挂载到主模块上

        import { Component, OnInit } from '@angular/core';

        @Component({
        selector: 'app-home',//组件名
        templateUrl: './home.component.html',//组件模板
        styleUrls: ['./home.component.css']//组件样式
        })
        export class HomeComponent implements OnInit {

        constructor() { }

         ngOnInit() {
     }
    
    }
###### 使用时在父组件的模板里用标签表示
        <app-home><app-home>
        
##### 组件传值
        aaa:Array<Goods>=[new Goods("商品1",100,"1"),new Goods("商品2",200,"2"),new Goods("商品3",300,"3")
        ]
        <app-home [list]="aaa"></app-home>
        <app-home list="{{aaa}}"></app-home>
##### 接收
        @Input() aaa 

###### @Input()可以接收参数,接收参数后传值属性名必须保持一致 @Input()后的变量名必须和传入的变量名一致

###### 传入方法也是一样的
        add(){
            ...
        }
        
        <app-home [add]="add"></app-home>
###### 接收
        @Input() add
        
        
### 路由
    
        const routes: Routes = [
        { path: '', redirectTo:"/home",pathMatch:"full" },//重定向
        { path: 'list/:id', component: ListComponent},//路径传参
        {path:"home",component:HomeComponent,children:[...]},//子路由
        {path:"a",component:AComponent,outlet:"box"}//辅助路由
        ];
        
###### 路由显示
        <router-outlet></router-outlet>//主路由显示
        <router-outlet name="box"></router-outlet>//辅助路由显示
        
###### 路由跳转
        <button routerLink="/home">home</button>//主路由跳转
        <button routerLink="/list" [queryParams]="{id:1}">list</button>//主路由问号传参跳转
        <button [routerLink]="['/list','1']">list</button>//主路由路径传参跳转
        <button [routerLink]="[{outlets:{box:'a'}}]">box a</button>//辅助路由跳转
        
###### 编程式导航

        this.router.navigateByUrl("home")
        this.router.navigate(["home"])
###### 路由传参的接收
         constructor(private ar:ActivatedRoute) { }
         
         this.ar.snapshot.queryParams//问号传参
         this.ar.snapshot.params//路径传参
###### 参数订阅

###### 通过.subscribe()方法订阅监听函数 即可在函数内拿到最新的路径参数例如:
       constructor(private ar:ActivatedRoute){}
 
      
        this.ar.params.subscribe(params=>{})// 路径传参订阅
        this.ar.queryParams.subscribt(params=>{})//问号传参订阅
      
##### 路由守卫

            CanActivate: 处理导航到某路由的情况。
        CanDeactivate: 处理从当前路由离开的情况。
        Resolve: 在路由激活之前获取路由数据。
###### 使用路由守卫要在路由模块进行配置
        
        const routes: Routes = [
            ...
            {path:'routerguard',component:RouterguardComponent,
            canActivate:[CanActivate], // 这里！！！！！！！！
             },
            ]
 
            @NgModule({
            imports: [ 
         RouterModule.forRoot(routes)
        ],
        exports: [RouterModule],
         providers: [CanActivate] // 这里！！！！！！！



