### 一、BOM的概念
	
	1. BOM browser object model 浏览器对象模型，用于操作整个浏览器的对象，
	2. 浏览器的核心对象就是window，换句话说，BOM就是window对象
##### BOM对象的组成部分
	window是顶级对象，一个窗口只有一个window对象
   ![r8NKJID](F7DF29C96C2C46758EC04C323C110B16)

### 二、window对象

	window对象是BOM的核心, window对象表示浏览器窗口的一个对象;
    每个浏览器窗口都有一个window对象与之对应. 
    window对象是BOM的顶层(核心)对象，所有对象都是通过它延伸出来的；

##### window对象包含以下对象
	
	 document(核心):
	 文档对象，让我们可以在js脚本中直接访问页面元素(DOM) 
     history(重要):
     历史对象,包含窗口的浏览历史，可以实现后退
     location(重要):
     包含浏览器当前的地址信息，可以用来刷新本页面或跳转到新页面
     frames:
     框架对象，可以获取页面框架内容
     screen: 
     包含有关客户端显示屏幕的信息
     navigator: 
     导航对象, 包含所有有关访问者浏览器的信息
### 三、history 历史对象

	 window.history.back();//回退
	 window.history.forward();//前进
     window.history.go();//回退 -1  前进1  刷新0

### 四、location 地址信息

	window.loaction.href="地址"；//用于跳转
	window.location.assign(‘http://www.baidu.com’);//跳转到指定的URL, 与href等效
	window.location.reload(true); //强行加载服务器上资源
	window.loaction.hash            //哈希 获取/#锚点信息(用来实现单页面应用程序)
	window.loaction.search          //获取URL地址的?及?后面的数据
	window.loaction.host            //主机名称加端口号
	window.loaction.hostname        //主机名称
	window.loaction.port            //端口号
### 五、navigator 导航对象

	window.navigator.userAgent //返回当前浏览器相关信息(重点)

	//获取地理位置(扩展)
    window.navigator.geolocation.getCurrentPosition(function(ps){
      var y=ps.coords.longitude;
      var x=ps.coords.latitude;
	})

### 六、screen 显示器屏幕对象（分辨率）

screen.height 屏幕的高度

screen.width  屏幕的宽度

screen.availHeight 屏幕减去系统任务栏的高度





