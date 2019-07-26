### Vue简介
每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的：

    var vm = new Vue({
    // 选项
    })
虽然没有完全遵循 MVVM 模型，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例。

当创建一个 Vue 实例时，你可以传入一个选项对象。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。作为参考，你也可以在 API 文档 中浏览完整的选项列表。

一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。

### 1. vue起步
通过script标签引入 Vue：

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
## 数据与方法
当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
```
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
```
当这些数据改变时，视图会进行重渲染。值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的。也就是说如果你后续对vm添加一个新的属性，是不会被渲染的。


#### 1.数据渲染 v-text、v-html、{{}}
实例化一个Vue对象，el为绑定的dom节点，data为dom节点内的数据，vue会自动将数据渲染到页面中！

    <div id="app">{{ message }}</div>
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!'
        }
    })
写法二：

    <div id="app" v-text="message"></div>
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!'
        }
    })
    

#### 2.事件方法v-on
（v-on:可缩写为@）

    <div id="app" v-on:click="handle"></div>
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!'
        },
        methods:{
            handle:function(e){
                this.message="hello world"
            }
        }
    })
为节点绑定一个点击事件，处理函数是vue实例里的handle方法。
#### 3.属性绑定v-bind
使用v-bind指令将dom节点的属性值绑定为vue实例的数据。（v-bind:可缩写为：）

    <div id="app" v-bind:title="title"></div>
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!',
            title:"这是一个div"
        }
    })
#### 4.双向绑定v-model
使用v-model指令进行数据的双向绑定，修改input框里面的值时，vue实例的数据也会变化，同时页面上绑定相同数据的也会发生变化。

    <div id="app">{{message}}</div>
    <input v-model="message" />
    
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!'
        }
    })

#### 5.计算属性computed
当某个数据是由其他数据计算出来的，可以使用vue实例的computed属性进行处理。
   
    <div id="app">
        <input v-model="firstName" />
        <input v-model="lastName" />
        <p>{{fullName}}</p>
    </div>
    
    var app = new Vue({
        el: '#app',
        data: {
            firstName: 'feng',
            lastName: 'zhiqing'
        }
        computed:{
            fullName:function(){
                return this.firstName+" "+this.lastName
            }
        }
    })
    
#### 6.侦听器watch
watch能够监听某一数据是否发生变化，当发生变化时将执行watch监听器里相应数据的处理函数。

    <div id="app">
        <input v-model="firstName" />
        <input v-model="lastName" />
        <p>{{fullName}}</p>
        <p>{{count}}</p>
    </div>
    
    var app = new Vue({
        el: '#app',
        data: {
            firstName: 'feng',
            lastName: 'zhiqing',
            count:0
        }
        computed:{
            fullName:function(){
                return this.firstName+" "+this.lastName
            }
        },
        watch:{
            fullName:function(){
                this.count++
            }
        }
        
    })
    
#### 7.v-if指令
v-if指令可以根据绑定的属性的布尔值决定是否在页面上显示这个dom节点，为false时，dom节点将被移除。

    <div id="app">
        <p v-if="show">{{fullName}}</p>
    </div>
    
    var app = new Vue({
        el: '#app',
        data: {
            fullName: 'fengzhiqing',
            show:true,
        }
    })
#### 8.v-show指令
v-show指令可以根据绑定的属性的布尔值决定是否在页面上显示这个dom节点，为false时，dom节点将被添加display：node进行隐藏，性能比v-if略好。

    <div id="app">
        <p v-if="show">{{fullName}}</p>
    </div>
    
    var app = new Vue({
        el: '#app',
        data: {
            fullName: 'fengzhiqing',
            show:true,
        }
    })
    
#### 9.v-for指令
v-for指令可以对vue实力的一个数组属性进行遍历，并将遍历的结果按某种规则显示出来。

    <div id="app">
        <li v-for="(item,index) of list"  :key="index">{{item}}</li>
    </div>
    
    var app = new Vue({
        el: '#app',
        data: {
            fullName: 'fengzhiqing',
            list:[1,2,3,4],
        }
    })

上面的例子表示对list属性进行值和索引的遍历，按照索引的顺序将值显示出来。（按值得顺序的话，值不能重复,vue中按key的值来区分生成的不同的li，所以key的值必须要有且不相同。）