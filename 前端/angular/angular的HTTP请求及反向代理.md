### 反向代理
在项目根目录新建 “proxy.conf.json”文件，文件内容为：
```
{
    "/zhuiszhu" : {
        "target" : "http://39.105.136.190/"
    }
}
```
### http请求
在组件的构造函数中通过依赖注入引入HTTP模块，如下
```
constructor(private http:Http) { }
```
使用http的get、post方法发起请，返回的是一个流对象，http请求是在绑定subscript时才发起请求，如下
```
http.get("http://xxx").map(data=>data.json()).subscribe(res=>{
    console.log(res)
})
```
方便用法：

在服务中进行请求定义，返回一个流对象，然后在控制器中将值赋给组件的一个属性，然后在模板中


