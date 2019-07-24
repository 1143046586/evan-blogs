### 一、Ajax的介绍
#### 1, Ajax的概念

AJAX (阿贾克斯 Asynchronous Javascript And Xml ) 异步JavaScript和XML，是指一种创建交互式网页应用的网页开发技术, 可以访问服务器数据的局部刷新的技术. AJAX不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术. 

虽然 Ajax 中的 x 代表的是 XML，但 Ajax 通信和数据格式无关，也就是说这种技术不一定使用 XML。相反目前常用的数据格式是JSON

#### 2, Ajax的作用

允许客户端脚本发送HTTP请求, 去请求服务器的数据来创建动态网页, 可以在不重新加载整个网页的情况下, 对网页的某部分进行更新. 也称局部刷新(常见的例子:分页, 用户名即时验证, 聊天等);

#### 3, Ajax的优势

JavaScript, XML, HTML，CSS在 AJAX 中使用的 Web 标准已被良好定义，并被所有的主流浏览器支持.

支持异步请求。

能够向服务器请求额外的数据而无须卸载页面(无需刷新页面)

#### 4, Ajax相关的概念

进程: 程序从开始到结束的一次执行过程 , 一个基于操作系统的应用程序中一般至少会有一个进程.

线程: 一个进程当中程序同时运行的多个分支, 每个进程中至少会有一个主线程,可以有多个分线程(子线程), 多个不同的线程是可以同时执行的(并行执行).

多线程: 多个线程并发执行的一种技术.

注: 每个运行的程序操作系统都会至少分配一个进程。一个进程里面至少包含一个或多个线程

同步: 在主线程中按步骤顺序执行, 做完前一件事情, 才能下一件事情, 不能同时做多件事情.

异步: 与同步处理相对, 异步处理不用阻塞当前线程来等待处理完成, 而是允许同时可以做多件事情, 换句话说就是由多个不同的线程在同时执行(同时执行, 也叫并发).

#### 5, Ajax的核心
Ajax 技术核心是JavaScript对象XMLHttpRequest(简称 XHR), 它是一种支持异步请求数据的技术.

XHR的出现, 提供了向服务器发送请求和解析服务器响应提供了流畅的接口. 能够以异步方式从服务器获取更多的信息, 这就意味着, 用户只要触发某一事件, 在不刷新网页的情况下, 更新服务器最新的数据。

#### 6, 创建XHR对象

    var xhr = new XMLHttpRequest();
    console.log(xhr);   //XMLHttpRequest

XHR对象支持IE7+、Firefox、Opera、Chrome 和 Safari 等浏览器, 但是不支持IE6.

如果要支持IE6, 则需要使用ActiveX对象通过传入参数Microsoft.XMLHTTP来实现.

    var xhr = new ActiveXObject("Microsoft.XMLHTTP");
#### 7, XMLHttpRequest对象的属性和方法
**XHR提供的方法:**

open(): 准备好需要发送给服务器的内容.

接收三个参数: 要发送的请求类型(get/post), 请求的URL和是否异步.

    如:xhr.open(‘get’, ‘demo.json’, false);  //URL为demo.json的get请求, false表示同步.

send(): 向服务器发送请求.

接收一个参数:请求体发送的数据. 如果是get方式请求则填null.

    如: xhr.send(null);	 

**XHR提供的属性:**

当请求发送到服务器端后, 服务器就会向客户端发送响应, 收到响应数据后, 响应的数据会自动填充XHR对象的属性.一共有四个属性：

    status: 响应的HTTP状态码 (重要)
    statusText: HTTP状态码的说明 (重要)
    responseText:  作为响应主体被返回的文本 (重要)
    responseXML: 如果响应主体内容类型是"text/xml"或"application/xml"，则返回包含响应数据的 XML文档,否则是null

其中status属性: 

HTTP状态码, 一般我们只需要关心HTTP状态代码为200则表示请求服务器成功.

### 二、同步请求
```
//创建XHR对象
function createXHR() {
        //IE7+ 谷歌, 火狐, Safari等
        if (window.XMLHttpRequest) {
                return new XMLHttpRequest();
        }
        return new ActiveXObject("Microsoft.XMLHTTP"); //IE6
}
var xhr = createXHR();

//使用open()方法,设置请求方式为get,请求url为demo.json, false表示同步请求 
xhr.open('get', 'demo.json', false); 

//向服务器发送请求
xhr.send(null);
if (xhr.status == 200) {	  //如果返回成功了
      console.log(xhr.responseText); //调出服务器返回的数据
} else {
      console.log(‘error.  状态代码: ’ + xhr.status + ‘错误信息: ' + xhr.statusText);
}
```
### 三、异步请求
前面我们使用同步请求比较简单, 但是实际项目开发中一般不用, 因为同步会阻塞主线程, 所以我们常用的是异步请求;

使用异步调用的时候, 需要触发readystatechange事件, 然后检测readyState属性的值即可, readyState属性的值有5个, 最常用的是最后一个值:4,表示已经接收到了全部响应数据.
```
0 － （未初始化）还没有调用send()方法 
1 － （载入）已调用send()方法，正在发送请求 
2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
3 － （交互）正在解析响应内容 
4 － （完成）响应内容解析完成，可以在客户端调用了 
```
异步请求:
```
//使用之前写好的createXHR()函数
xhr = createXHR(); 
//使用open()方法, 设置true表示异步请求 
xhr.open('get', 'demo.json', true); 
//向服务器发送请求
xhr.send(null); 
//监听状态改变
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {  
          if (xhr.status == 200) {  
	  console.log(xhr.responseText); 
          } else {
	  consolel.log(‘error. 状态码: ’ + xhr.status + ‘错误信息:’+ xhr.statusText);
          }
     }
};
```
abort()方法: 

取消异步请求, 如果需要取消某异步请求, 则在send()之后再调用, 写在send()之前调用会报错

    //取消异步请求
    xhr.abort();

### 四、HTTP协议
HTTP是一个属于应用层的面向对象的协议, 由于其简捷,快速的方式, 适用于分布式超媒体信息系统.  
        
HTTP协议的主要特点有:  Client/Server  B/S: 浏览器/服务器

    1, 支持客户端/服务端模式. 即请求(request)-响应(response)模式
    2, 简单快速,  客户端向服务端发送请求时, 只需要传送请求方式和路径即可,所以简单,
    由于HTTP协议简单, 使得HTTP服务器的程序规模小, 因而速度很快;
    3, 灵活, 传输数据类型种类多
    4, 无连接,  请求一次服务器后立刻断开连接, 即非长连接 即短连接
    5, 无状态,  HTTP协议对事务处理没有记忆能力; (cookie+session)解决无状态
	
HTTP协议的请求方式: GET, POST, HEAD, PUT等
HTTP包含: 请求头和请求体

### 五、GET请求: 

在通过HTTP协议向服务器请求的过程中, 有两种最常用的请求方式, 分别是: GET和POST. 

在Ajax使用的过程中，GET的使用频率又要比POST高得多. GET请求最常用于向服务器获取数据, 也可以将少量字符串参数提交给服务器.
如: 

    xhr.open('get', ‘login.php?username=zhang&pwd=123456', true);
GET请求通过url传递参数，相对于POST请求，GET请求相对速度较快,但安全性较低，一般用来向服务器请求不敏感的信息，传输的内容收限制,只能传字符串.

通过URL后的问号?给服务器传递键值对数据, 服务器接收到请求后可以从中获取到对应的数据

### 六、POST请求
#### 1 ,请求头   

请求头包含HTTP的头部信息, 即服务器返回的响应头信息和客户端发送出去的请求头信息. 我们可以获取响应头(response)信息或者设置请求头(request)信息。

使用 getAllResponseHeaders()获取整个响应头信息

    console.log(xhr.getAllResponseHeaders());

使用 getResponseHeader()获取单个响应头信息

    console.log(xhr.getResponseHeader('Content-Type'));

使用 setRequestHeader()设置单个请求头信息 ＼

    xhr.setRequestHeader(“MyHeader”, Zhang); //放在open方法之后, send方法之前

#### 2，字符编码和解码
特殊字符传参产生的问题可以使用encodeURIComponent()进行编码处理, 中文字符的返回及传参, 可以将页面保存和设置为utf-8格式即可.

字符编码

    var url = encodeURIComponent(name) + ‘=‘ + encodeURIComponent(value); 

字符解码

    var url2 = decodeURIComponent(url); 

#### 3，post请求
POST请求可以包含非常多的数据, 我们在使用表单提交的时候, 很多就是使用的POST传输方式。

    xhr.open('post', 'demo.php', true);

POST请求向服务器发送的数据, 不会跟在URL后面, 而是通过send()方法向服务器提交数据。

    xhr.send('name=Zhang&age=100');

POST请求和Web表单提交不同, 需要使用XHR来模仿表单提交.

    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

从性能上来讲POST请求比GET请求消耗更多一些, 用相同数据比较, GET最多比 POST快两倍. 这也是我们GET请求的使用率大于POST的原因.

POST相对于GET的优点是一次传输的内容可以比较多,格式也可以多样！

### 七、Promise
它的作用就是解决 异步的回调,常见于解决回调地狱问题

回调地狱: 太多回调,嵌套回调...

Promise 的状态 :

pending(等待)

resolved(已解决的) success

rejected(拒绝)  error

#### 1 ,Promise语法
```
var p = new Promise((resolve, rejecte) =>{
        setTimeout(function(){
            if(1 === 1){
                resolve("你答对了")
            } else {
                rejecte("你答错了")
            }
        })
    })

    p.then(resolve =>{
        //成功
        console.log(resolve);
    }, rejecte =>{
        //失败
        console.log(rejecte);
    })
```
#### 2，Promise的链式调用
当Promise的取值函数返回一个新的Promise时,可以在后面继续使用then命令进行处理，当Promise状态变为rejected时会跳到下面最近的rejected状态处理函数，rejected函数可继续返回新的Promise继续使用then进行处理.
```
var p = new Promise((resolve, rejecte) =>{
        setTimeout(function(){
            if(1 === 1){
                resolve("你答对了")
            } else {
                rejecte("你答错了")
            }
        })
    })

    p.then(resolve =>{
        //成功
        console.log(resolve);
        return new Promise((resolve, rejecte) =>{
            setTimeout(function(){
                if(2 == 1){
                    resolve("你答对了")
                } else {
                    rejecte("你答错了")
                }
            })
        })
    }).then(resolve=>{
        
    },rejecte =>{
        //失败      不管是第一个Promise失败还是第二个Promise失败都会跳到此处
        console.log(rejecte);
    })
```
#### 3，Promise的并发与赛跑
```
var p1 = new Promise(function(aa, bb){
        setTimeout(function(){
            aa("p1的ok")
        },5000)
    });
    var p2 = new Promise(function(aa, bb){
        setTimeout(function(){
            aa("p2的ok");
        },1000)
    });

    //Promise的all 并发执行 两者值都进行处理
    Promise.all([p1,p2]).then(function(res){
        console.log(res);
    })

    //赛跑,谁先执行,拿谁
    Promise.race([p1,p2]).then(function(res){
        console.log(res);
    })
```

实现Ajax异步的依赖调用

例如： 实现先注册 后登陆
```
var p = new Promise(function(resolve, reject){
      ajax({
	success: function(d){
    	      resolve(d);
	} 
      })  
})
p.then(function(data){
      ajax({
	success: function(d){
  	      console.log("登录成功");					
	} 
      }) 		
})
```
### 八、跨域
#### 1 ,同源策略
同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。

同源策略，它是由Netscape提出的一个著名的安全策略，现在所有支持JavaScript的浏览器都会使用这个策略。所谓同源是指，域名，协议，端口相同。

解决Ajax跨域问题一般有以下两种方式：

1，JSONP

2，由服务器端支持跨域 
#### 2，JSONP
**什么是JSONP**

JSON是一种数据交换格式，而JSONP是一种依靠开发人员的聪明才智创造出的一种非官方跨域数据交互协议。

jsonp的原理：利用src的属性,能请求到外部资源,并返回json的数据;

JSONP的优点：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。

JSONP的缺点：它只支持GET请求而不支持POST等其它类型的HTTP请求；
```
function jsonp(url, data, callbackName, fn){
    var cbName = "jquery_" + parseInt(Math.random() * 10000);
    // 生成随机函数名cbName= jquery_1234
    window[cbName] = fn;    //为传进来的函数赋予随机名
    //1.jsonp ,首先要先定义函数;

    //2.加载外部的资源  //并将随机函数名传递过去
    var scrEle = document.createElement("script");
    //先写callbackName,后写data的参数,data有没有,都不会影响
    scrEle.src = url + "?" + callbackName + "=" + cbName + "&" + param(data);  
    document.body.appendChild(scrEle);  //加载引入的Js文件后,引入的js文件调用传过去的函数名,并传入参数
    // 3.加载数据后,就要清除 script标签
    scrEle.onload=function(){
        this.remove();
    }
}
```
**JSONP实现跨域的原理：**

JSONP是一种非正式传输协议，该协议的一个要点就是允许用户传递一个callback参数给服务端，然后服务端返回数据时会将这个callback参数作为函数名来包裹住JSON数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了。
JSONP本身就是一个get请求，而script节点本身也是一个get请求，这个思想是通过后端的配合（后端输出的 response text必须符合js语法）更好的利用了get请求而已。 
而前端封装一个方法，通过这个方法把请求注册的回调指向全局的一个具体名字的函数，同时把具体名字函数的函数名和参数通过get请求传递给后端而已。
#### 3，由服务器端支持跨域
后台文件设置支持跨域（PHP）

    header('Access-Control-Allow-Origin:*');
.

.

.

.


 