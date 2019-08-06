### ListView
ListView 是一个线性布局的widgets 列表.
```
ListView -extends->BoxScrollView -extends->ScrollView -extends->StatelessWidget
```
在构建ListView时有4中选择：

>利用ListView构造函数。此构造函数适合于具有少量子元素的列表视图，因为上种方法创建的ListView 会将子Item一次性绘制出来，如果子Item 多的话，会造成页面卡顿。

>ListView.builder利用IndexedWidgetBuilder来按需构造。这个构造函数适合于具有大量（或无限）子视图的列表视图，因为构建器只绘制可见的Item。

>使用ListView.separated构造函数，采用两个IndexedWidgetBuilder：itemBuilder根据需要构建子项separatorBuilder类似地构建出现在子项之间的分隔符子项。此构造函数适用于具有固定数量的子控件的列表视图。

>使用ListView.custom的SliverChildDelegate构造，它提供了定制子模型的其他方面的能力。 例如，SliverChildDelegate可以控制用于估计实际上不可见的孩子的大小的算法。

#### 1 加载少量数据时可以使用构造函数
本方法创建的ListView ，会将子Item一次性绘制出来
```dart
ListView(
    //item 高度会适配 item填充的内容的高度
    shrinkWrap: true,
    padding: EdgeInsets.all(20.0),
    children: <Widget>[
        new Text(
        "test",
        style: new TextStyle(fontSize: 18.0, color: Colors.red),
        ),
        new Text(
        "${list[0].age}",
        style: new TextStyle(fontSize: 18.0, color: Colors.green),
        ),
        new Text(
        "${list[0].content}",
        style: new TextStyle(fontSize: 18.0, color: Colors.blue),
        ),
    ],
);
```
#### 2 ListView.builder() 常用
假如有 1000 个列表，初始渲染时并不会所有都渲染，而只会特定数量的 item
![https://img-blog.csdnimg.cn/20190703093946727.gif](https://img-blog.csdnimg.cn/20190703093946727.gif)
```dart
ListView.builder(
    // item 的个数
    itemCount: 20,
    //设置滑动方向 Axis.horizontal 水平  默认 Axis.vertical 垂直
    scrollDirection: Axis.vertical,
    //内间距
    padding: EdgeInsets.all(10.0),
    //是否倒序显示 默认正序 false  倒序true
    reverse: false,
    //false，如果内容不足，则用户无法滚动 而如果[primary]为true，它们总是可以尝试滚动。
    primary: true,
    //确定每一个item的高度 会让item加载更加高效
    itemExtent: 50.0,
    //item 高度会适配 item填充的内容的高度 多用于嵌套listView中 内容大小不确定 比如 垂直布局中 先后放入文字 listView （需要Expend包裹否则无法显示无穷大高度 但是需要确定listview高度 shrinkWrap使用内容适配不会） 文字
    shrinkWrap: true,
    //滑动类型设置
    //new AlwaysScrollableScrollPhysics() 总是可以滑动 NeverScrollableScrollPhysics禁止滚动 BouncingScrollPhysics 内容超过一屏 上拉有回弹效果 ClampingScrollPhysics 包裹内容 不会有回弹
    //        cacheExtent: 30.0,  //cacheExtent  设置预加载的区域   cacheExtent 强制设置为了 0.0，从而关闭了“预加载”
    physics: new ClampingScrollPhysics(),
    //滑动监听
    //        controller ,
    itemBuilder: (BuildContext context, int index) {
        //设置子条目
        return ListTile(
        title: Text("title $index"),
        // item 标题
        leading: Icon(Icons.keyboard),
        // item 前置图标
        subtitle: Text("subtitle $index"),
        // item 副标题
        trailing: Icon(Icons.keyboard_arrow_right),
        // item 后置图标
        isThreeLine: false,
        // item 是否三行显示
        dense: true,
        // item 直观感受是整体大小
        contentPadding: EdgeInsets.all(10.0),
        // item 内容内边距
        enabled: true,
        onTap: () {
            print('点击:$index');
        },
        // item onTap 点击事件
        onLongPress: () {
            print('长按:$index');
        },
        // item onLongPress 长按事件
        selected: false, // item 是否选中状态
        );
    },
),
```
ListTile 是Flutter 提供好的常用widget ，包括文字，icon，点击事件
#### 3 ListView.separated()
带分割线的item，separated 相比较于 builder，又多了一个参数 separatorBuilder ，用于控制列表各个元素的间隔如何渲染。
![](https://img-blog.csdnimg.cn/20190703094222313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psMTg2MDM1NDM1NzI=,size_16,color_FFFFFF,t_70)
```dart
ListView.separated(
    scrollDirection: Axis.vertical,
    itemCount: 100, // item 的个数
    separatorBuilder: (BuildContext context, int index) => Divider(height:1.0,color: Colors.blue),  // 添加分割线
    itemBuilder: (BuildContext context, int index) {
        return ListTile(
            title:  Text("title $index"), // item 标题
            leading: Icon(Icons.keyboard), // item 前置图标
            subtitle: Text("subtitle $index"), // item 副标题
            trailing: Icon(Icons.keyboard_arrow_right),// item 后置图标
            isThreeLine:false,  // item 是否三行显示
            dense:true,                // item 直观感受是整体大小
            contentPadding: EdgeInsets.all(10.0),// item 内容内边距
            enabled:true,
            onTap:(){print('点击:$index');},// item onTap 点击事件
            onLongPress:(){print('长按:$index');},// item onLongPress 长按事件
            selected:false,     // item 是否选中状态
        );
    },
),
```
#### 4 ListView.custom
```dart
ListView.custom(
    scrollDirection: Axis.vertical,
    childrenDelegate:SliverChildBuilderDelegate((BuildContext context, int index) {
        return Container(
            height: 50.0,
            alignment: Alignment.center,
            color: Colors.lightBlue[100 * (index % 9)],
            child: Text('list item $index'),
        );
    }, childCount: 50),
),
```
#### 注意
**在ListView为column等多子元素的小部件的子元素时，ListView不能直接与其他子元素并列，会报没有限制高度错误，应在ListView外面包一层Container并设置高度**