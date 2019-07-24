### 安装
    npm i redux react-redux
### 设计思想
    1.Web 应用是一个状态机，视图与状态是一一对应的。
    2.所有的状态，保存在一个对象里面。
### 引入中间件
    import {Provider} from "react-redux"--main.js
    import {connect} from "react-redux"--容器组件
    import {createStore} from "redux"--store.js
    
### 基本概念
    1.Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store。Redux 提供createStore这个函数，用来生成 Store。
    2. State Store对象包含所有数据 通过store.getState()拿到数据
    3.Action 就是 View 发出的通知，表示 State 应该要发生变化了。
    4.store.dispatch()是 View 发出 Action 的唯一方法
    5. Reducer Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。
    6.store.subscribe()
    Store允许使用store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。
    7.解除监听 this. unsubscribe = store.subscribe();
    this.unsubscribe;
    
### 工作流程
    首先，用户发出 Action。然后，Store 自动调用 Reducer，并且传入两个参数：
    当前 State 和收到的 Action。 Reducer 会返回新的 State 。
    State 一旦有变化，Store 就会调用监听函数。

![image](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)


    
    
