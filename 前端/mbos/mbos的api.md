### 1、请求数据
```
mbos.eas.invokeScript({
    needShowLoading:false,      //不显示加载中图标
    name:"getData",             //服务端函数名
    param:[],
    success:function(e){
        console.log(e)
    },
    error:function(){
    }
})
```