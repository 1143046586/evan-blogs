在讲述Flux之前，我们看看之前传统的MVC架构以及在web前端中的一些问题继而思考Flux带来的改变。

我们先来看一下后端的mvc架构，现在假设有这样的场景，用户想查看自己的profile页面，可能会有这样的流程：

在页面上点击profile按钮，

接下来就是一个HTTP请求(/profile?username=jiavan) => 

Controller接收到这一请求并获得请求的内容username=jiavan

然后告知Model需要jiavan的数据 => 

Model返回了jiavan的数据 => 

Controller得到数据返回新的视图，看下流程：
![https://segmentfault.com/img/bVZaYM?w=461&h=241](https://segmentfault.com/img/bVZaYM?w=461&h=241)

然后我们再来看一下前端的mvc架构（AngularJS，BackboneJS）
现在前端中又有这样的场景：

点击导航栏的菜单，

我们会给选中的菜单新增样式同时移除其他兄弟元素的样式，

这里的事件响应函数就是Controller，

我们会在这里处理样式修改逻辑，以及更新Model的数据，

然后新的数据及样式重新渲染界面。

![https://upload-images.jianshu.io/upload_images/5518628-a3f25eec5ec6e916.png?imageMogr2/auto-orient/](https://upload-images.jianshu.io/upload_images/1783332-a8692e05f1f36108.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

在这个场景中，我们一个点击事件，修改的菜单栏的索引位置，修改了页面的显示内容等多个model，

而同时，我们一个model可能控制多个视图，我们这个有道云的文件model，既控制着文件图标的类型，又控制文件阅读的组件，

而在前端页面中，我们的事件处理函数Controller分布在页面的各个地方，

当项目复杂的时候，一个点击事件可能触发页面一连串的变化，而由于数据的在view和model之间的双向流动，又可能会改变一连串model，

所以，当变化和我们预期的不一致时，在如此复杂的监听和触发关系中，梳理数据的流动方式，或调试问题，都非常困难。

这个时候，flux出来拯救我们了，
![https://upload-images.jianshu.io/upload_images/5518628-3b097e607eb42a3a.png?imageMogr2/auto-orient/](https://upload-images.jianshu.io/upload_images/5518628-3b097e607eb42a3a.png?imageMogr2/auto-orient/)

Flux的基本概念：

一个Flux应用由三大部分组成：

（1）dispatcher— —负责分发事件

（2）store— —负责保存数据，同时响应事件并更新数据

（3）view— —负责订阅store中的数据，并使这些数据渲染相应的页面。

Flux流程图：

![https://upload-images.jianshu.io/upload_images/5518628-f242d5d8c85a3e16.png?imageMogr2/auto-orient/](https://upload-images.jianshu.io/upload_images/5518628-f242d5d8c85a3e16.png?imageMogr2/auto-orient/)

在Flux模式中的dispatcher中定义了严格的规则来限定我们对数据的修改操作。只能通过dispatcher来修改store中的state，强化数据修改的纯洁性。

Flux核心思想：
（1）Flux的中心化控制。中心化控制让所有的请求与改变都只能通过action发出，统一由dispatcher进行分配。与MVC数据、逻辑改动源自不同的源头相比，Flux查找问题的困难度要小的多。
优点：
1、View保持整洁，只关心数据，不关心逻辑，只发送指令。
2、中心化控制了所有的数据，发现问题时可以方便查询

（2）Flux把action做了统一归纳