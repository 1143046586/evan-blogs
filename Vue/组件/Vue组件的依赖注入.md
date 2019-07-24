### 作用：
允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效

**==provide==选项允许我们指定我们想要提供给后代组件的数据/方法**

provide选项应该是一个对象或返回一个对象的函数 
```
provide: function () {
  return {
    getMap: this.getMap
  }
}
 
// 或者
 
provide: {
    foo: 'bar'
}
```
然后在任何后代组件里，我们都可以使用 **==inject==** 选项来接收指定的我们想要添加在这个实例上的属性：

inject 选项应该是：

    一个字符串数组，或
    一个对象，对象的 key 是本地的绑定名，value 是：
        在可用的注入内容中搜索用的 key (字符串或 Symbol)，或
        一个对象，该对象的：
            from 属性是在可用的注入内容中搜索用的 key (字符串或 Symbol)
            default 属性是降级情况下使用的 value
inject: ['getMap']

**Vue3.0 + Typescript 环境下**

父组件 provide
```
@Provide()
public componentActivity = this.getProvide()
 
private getProvide() {
    return 'aaaaaa'
}
 
// 或者
 
@Provide()
public componentActivity = {name:'aaaaaa'}
```
后代组件 Inject
```
@Inject()
private componentActivity: string
 
private created() {
   console.info(this.componentActivity)   // 'aaaaaa'
}
 
```

**==然而，依赖注入还是有负面影响的。它将你的应用以目前的组件组织方式耦合了起来，使重构变得更加困难。同时所提供的属性是非响应式的，父组件传值发生变化时，子组件接收的数据是不会对应改变的，如果需要响应传值可以考虑vuex==**
--------------------- 
作者：Caroline_LuoLuo 
来源：CSDN 
原文：https://blog.csdn.net/caroline_Luoluo/article/details/83818922 
版权声明：本文为博主原创文章，转载请附上博文链接！