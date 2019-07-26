1. 什么是session?

    用于服务端的 会话控制.

2. 什么是会话?
   一次请求(request)与一次响应(response),就形成了一次会话

3. 为什么要使用session?

   因为http属于短链接,无状态,所以服务端加入了session机制,来解决这一问题.
   
    每一次访问,服务器会创建一个线程,给每一个线程赋予一个编号,记录在服务器端,
    通过响应带回浏览器,保存到cookie中;
    
4. session的缺点?
     容易被劫持,伪造,丢失.不能跨域,解决方案 jwt (JSON Web Token)  (组件)
     ???
5. session优点?
     使用简单,使用方式都是一样.session是键值对

    
  还能实现 服务器的页面之间传值
  
  页面传值的方式有哪些一些?
   1.session 能实现页面之间的传值(服务器)
   2.url地址
   3 cookie,
   4.sessionstorage,localstorage
   5.数据库 a页面插入数据,b页面读取数据
   6.父子窗口window.open与window.opener (容易被浏览器阻止)
    
   
  
   

    