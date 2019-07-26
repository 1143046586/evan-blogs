### 1、data深层数据修改页面不刷新
```
data(){
    obj:{
        arr1:["早", "早", "早", "早"],  //修改其中某个值
        arr2:[
            {
                year: 0,    //修改值
                month: 0,
            },
            {
                year: 0,
                month: 0,
            }
        ]
    }
}
```
解决办法：
```
this.$set(this.obj.arr1,index,newValue)
this.$set(this.obj.arr2[1],year,newValue)