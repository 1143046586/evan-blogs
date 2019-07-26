### 一、vuex是什么？

先引用vuex官网的话：

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。

它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

状态管理模式、集中式存储管理 一听就很高大上，蛮吓人的。

在我看来 vuex 就是把需要共享的变量全部存储在一个对象里面，然后将这个对象放在顶层组件中供其他组件使用。

这么说吧，将vue想作是一个js文件、组件是函数，那么vuex就是一个全局变量，只是这个“全局变量”包含了一些特定的规则而已。

在vue的组件化开发中，经常会遇到需要将当前组件的状态传递给其他组件。父子组件通信时，我们通常会采用 props + emit 这种方式。但当通信双方不是父子组件甚至压根不存在相关联系，或者一个状态需要共享给多个组件时，就会非常麻烦，数据也会相当难维护，这对我们开发来讲就很不友好。vuex 这个时候就很实用，不过在使用vuex之后也带来了更多的概念和框架，需慎重！

### 二、vuex里面都有些什么内容？
Talk is cheap,Show me the code. 
先来一段代码间隔下这么多的文字：
```
const store = new Vuex.Store({
    state: {
        name: 'weish',
        age: 22
    },
    getters: {
        personInfo(state) {
            return `My name is ${state.name}, I am ${state.age}`;
        }
    }
    mutations: {
        SET_AGE(state, age) {
            commit(age, age);
        }
    },
    actions: {
        nameAsyn({commit}) {
            setTimeout(() => {
                commit('SET_AGE', 18);
            }, 1000);
        }
    },
    modules: {
        a: modulesA
    }
}
```
这个就是最基本也是完整的vuex代码；vuex 包含有五个基本的对象：

#### 1.State 

　　vuex的状态管理，需要依赖它的状态树，官网说：

　　Vuex 使用单一状态树——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 (SSOT)”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

　　简单粗暴理解： 我们要把我们需要做状态管理的量放到这里来，然后在后面的操作动它

　　我们来声明一个state：
```
const state = { 
    blogTitle: '迩伶贰blog',
    views: 10,
    blogNumber: 100,
    total: 0,
    todos: [
        {id: 1, done: true, text: '我是码农'},
        {id: 2, done: false, text: '我是码农202号'},
        {id: 3, done: true, text: '我是码农202号'}
    ]
}
```
#### 2. Mutation

　我们有了state状态树，我们要改变它的状态（值），就必须用vue指定唯一方法 mutation，官网说：
 
更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。


每个 mutation 都会传入一个包含所有状态的state参数，以便mutation对state进行修改,第二个参数为comment传过来的参数。

 1 mutation必须是纯函数 (传入相同的参数,得到的结果也一定相同)
 2 mutation必须是同步函数  

　简单粗暴理解：任何不以mutation的方式改变state的值，都是耍流氓（违法，vuex严格模式下会报错）

我们来一个mutation：
```
const mutation = {
    addViews (state，options) {
        state.views++
    },
    blogAdd (state，options) {
        state.blogNumber++
    },
    clickTotal (state，options) {
        state.total++
    }
}
```
#### 3.Action

　　action 的作用跟mutation的作用是一致的，它提交mutation，从而改变state，是改变state的一个增强版，官网说：

Action 类似于 mutation，不同在于：

Action 提交的是 mutation，而不是直接变更状态。

Action 可以包含任意异步操作也可以是非纯函数。

action包含两个参数，第一个是context，context提供了用来提交mutation的commit方法，第二个是用户通过dispatch传过来的参数。

　　我们来一个action：
```
    const actions = {
        addViews (context，options) {
        context.commit('addViews',options)
    },
    clickTotal (context，options) {
        context.commit('clickTotal',options)
    },
    blogAdd (context，options) {
        context.commit('blogAdd',options)
    }
}
```
#### 4.Getter

官网说：有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数。减少我们对这些状态数据的操作

简单粗暴理解：状态树上的数据比较复杂了，我们使用的时候要简化操作，类似于vue的computed；

我们来一个getter：
```
const getters = {
    getToDo (state) {
    return state.todos.filter(item => item.done === true)
    // filter 迭代过滤器 将每个item的值 item.done == true 挑出来， 返回的是一个数组
    }
}
```
### 三、mapState、mapMutations、mapAction
#### 1，mapState

表面意思:mapState是state的辅助函数

实际作用:当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们批量生成计算属性，减少代码的书写量


在使用mapState之前,要在组件中导入这个辅助函数.

    import { mapState } from 'vuex'
    
它两种用法，或接受一个对象，或接受一个数组。

当为数组时,组件中直接使用数组中的值映射state中相对应得的值，

例如
```
<template>
  <div id="example">
    {{count}}
    {{str}}
  </div>
</template>
<script>
import { mapState } from 'vuex'
export default {
  data () {
    return {
      str: '国籍',
    }
  },
  computed: mapState(["count"]),
}
</script>
```
当我们需要对mapState中映射的state中的值进行相应逻辑计算或重命名时，显然数组形式以及不满足需求，所以我们需要用到对象形式的使用方法。

使用例子：
```
<template>
  <div id="example">
    {{count}}
    {{dataCount}}
    <div>{{sex}}</div>
    <div>{{from}}</div>
    <div>{{myCmpted}}</div>
  </div>
</template>
<script>
import { mapState } from 'vuex'
export default {
  data () {
    return {
      str: '国籍',
      dataCount: this.$store.state.count
    }
  },
  computed: mapState({
    count: 'Acount', // 第一种写法，将state中的Acount映射为count，在组件中使用this.count获取state中的count
    sex: (state) => state.sex, // 第二种写法，等同于第一种
    //返回data和state中的属性计算后的值
    from: function (state) { // 用普通函数this指向vue实例,要注意
      return this.str + ':' + state.from
    },
    // 注意下面的写法看起来和上面相同,事实上箭头函数的this指针并没有指向vue实例,因此不要滥用箭头函数
    // from: (state) => this.str + ':' + state.from
    
    //computed原有的计算属性
    myCmpted: function () {
      return '测试' + this.str
    }
  }),
  created () {
    // 写个定时器，发现computed依旧保持了只要内部有相关属性发生改变不管是当前实例data中的改变，还是vuex中的值改变都会触发dom和值更新
    setTimeout(() => {
      this.str = '国家'
    }, 1000)
  }
}
</script>
```
  在使用的时候,computed接收mapState函数的返回值,你可以用三种方式去指定返回值于state中的值得映射关系,具体可以看注释.

  事实上第二种和第三种是同一种,只是前者用了ES6的偷懒语法,箭头函数,在偷懒的时候要注意一个问题,this指针的指向问题,我已经在很多篇文章中提到不要在vue中为了偷懒使用箭头函数,会导致很多很难察觉的错误,如果你在用到state的同时还需要借助当前vue实例的this,请务必使用常规写法.

  当然computed不会因为引入mapState辅助函数而失去原有的功能---计算属性,只是写法会有一些奇怪,如果你已经写了一大堆的computed计算属性,做了一半发现你要引入vuex,还想使用mapState辅助函数的方便,你可以需要做下列事情.
```
//之前的computed
computed:{
    fn1(){ return ...},
    fn2(){ return ...},
    fn3(){ return ...}
    ........
}
//引入mapState辅助函数之后
 
computed:mapState({
    //原先的计算属性
    fn1(){ return ...},
    fn2(){ return ...},
    fn3(){ return ...}
    ......
    //混入的state属性
    count:'count'
    .......
})
```
从上述写法可以看出来,这与我们原先写computed中的计算存在差异，那有没有一种既能像原先使用computed计算属性以避免对代码的修改，又能使用mapState呢？ES6语法中的...对象展开运算符为我们提供了这项方便。
```
//之前的computed
computed:{
    fn1(){ return ...},
    fn2(){ return ...},
    fn3(){ return ...}
    ........
}

//引入mapState辅助函数之后
computed:{
    //原来的继续保留
    fn1(){ return ...},
    fn2(){ return ...},
    fn3(){ return ...}
    ......
    //插入的mapState，通过展开运算符将mapState返回的对象张开并混入原先的computed中
    ...mapState({ 
        count:'count'
    })
}
```
### 2，mapMutations、mapActions、mapGetter

mapMutations、mapActions、mapGetter的用法与mapState用法相似！

mapGetter同样是批量引用并附加在computed中，而mapMutations、mapActions则是附加在methods中。
## 四、模块modules
前面我们讲到的例子都在一个状态树里进行，当一个项目比较大时，所有的状态都集中在一起会得到一个比较大的对象，进而显得臃肿，难以维护。为了解决这个问题，Vuex允许我们将store分割成模块（module），每个module有自己的state，mutation，action，getter，甚至还可以往下嵌套模块，下面我们看一个典型的模块化例子
```
//文件A
export let moduleA = {
    state: {....},
    mutations: {....},
    actions: {....},
    getters: {....}
}

//文件B
export let moduleB = {
    state: {....},
    mutations: {....},
    actions: {....},
    getters: {....}
}

//主文件
import Vuex from "vuex"
import Vue from "vue"
import {moduleA} from "./文件A"
import {moduleB} from "./文件B"
Vue.use(Vuex)
export let store = new Vuex.Store({
    modules: {
        a: moduleA,
        b: moduleB
    }
})

//使用时
this.$store.state.a.xxx // 使用moduleA的状态
this.$store.state.b.xxx // 使用moduleB的状态
this.$store.commit("xxx")   //actions为全局函数,不需要指定模块
this.$store.dispatch("xxx") //mutations为全局函数,不需要指定模块
this.$store.getters.xxx //getters为全局函数,不需要指定模块
```
模块内部的mutation和getter，接收的第一参数（state）是模块的局部状态对象,rootState为根节点状态

使用mapState、mapMutations、mapActions、mapGetter时写法
```
computed : {
    ...mapState("home",["title","newList"]) //需在第一个参数指定模块名
    ...mapGetter(["title","newList"])
},
methods : {
    ...mapMutations({
        addGoods:"addItem"  //mutations为全局函数,无影响
    })
    ...mapActions({
        addGoods:"addItem"  //actions为全局函数,无影响
    })
},
```
上面所有的例子中，模块内部的action、mutation、getter是注册在全局命名空间的，如果你在moduleA和moduleB里分别声明了命名相同的action或者mutation或者getter（叫some），当你使用store.commit('some')，A和B模块会同时响应。
### 命名空间
如果希望提高模块可重用性，你可以在子模块添加namespaced:true的属性，使其成为命名空间模块。当模块被注册后，它的所有getter，action，mutation都会自动根据模块注册的路径调用整个命名，例如：
```
const store = new Vuex.Store({
    modules: {
        account: {
            namespaced: true,
            state: {...},           // 模块内的状态已经是嵌套的，namespaced不会有影响
            getters: {              // 每一条注释为调用方法
                isAdmin () { ... }  // getters['account/isAdmin']
            },
            actions: {
                login () {...}      // dispatch('account/login')
            },
            mutations: {
                login () {...}      // commit('account/login')
            },
            modules: {              // 继承父模块的命名空间
                myPage : {
                    state: {...},
                    getters: {
                        profile () {...}        // getters['account/profile']
                    }
                },
                posts: {            
                    namespaced: true,           // 进一步嵌套命名空间
                    getters: {
                        popular () {...}        // getters['account/posts/popular']
                    }
                }
            }
        }
    }
})
```
启用了命名空间的getter和action会变成局部化的getter，dispatch和commit。在外部调用时需添加命名空间前缀,但是在vuex模块内使用模块内容时不需要再同一模块内添加空间名前缀，更改namespaced属性后不需要修改模块内的代码

### 在命名空间模块内访问全局内容
如果你希望使用全局state和getter，roorState和rootGetter会作为第三和第四参数传入getter，也会通过context对象的属性传入action若需要在全局命名空间内分发action或者提交mutation，将{ root: true }作为第三参数传给dispatch或commit即可。
```
modules: {
    foo: {
        namespaced: true,
        getters: {       // 在这个被命名的模块里，getters被局部化了,你可以使用getter的第四个参数来调用 'rootGetters'
            someGetter (state, getters, rootSate, rootGetters) {
                getters.someOtherGetter // -> 局部的getter， ‘foo/someOtherGetter'
                rootGetters.someOtherGetter // -> 全局getter, 'someOtherGetter'
            }
        },
        actions: {      // 在这个模块里，dispatch和commit也被局部化了,他们可以接受root属性以访问跟dispatch和commit
            smoeActino ({dispatch, commit, getters, rootGetters }) {
                getters.someGetter                              // 'foo/someGetter'
                rootGetters.someGetter                          // 'someGetter'
                dispatch('someOtherAction')                     // 'foo/someOtherAction'
                dispatch('someOtherAction', null, {root: true}) //‘someOtherAction'
                commit('someMutation')                          // 'foo/someMutation'
                commit('someMutation', null, { root: true })    // someMutation
            }
        }
    }
}
```
### 带命名空间的绑定函数
前面说过，带了命名空间后，调用时必须要写上命名空间，但是这样就比较繁琐，尤其涉及到多层嵌套时

下面我们看下一般写法

```
computed: {
    ...mapState({
        a: state => state.some.nested.module.a,
        b: state => state.some.nested.module.b
    }),
},
methods: {
    ...mapActions([
        'some/nested/module/foo',
        'some/nested/module/bar'
    ])
}
```
对于这种情况，你可以将模块的命名空间作为第一个参数传递给上述函数，这样所有的绑定会自动将该模块作为上下文。简化写就是
```
computed: {
    ...mapStates('some/nested/module', {
    a: state => state.a,
    b: state => state.b
    })
},
methods: {
    ...mapActions('some/nested/module',[
        'foo',
        'bar'
    ])
}
```