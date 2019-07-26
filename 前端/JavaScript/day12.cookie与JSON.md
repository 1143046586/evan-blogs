### 一、cookie
#### 1, cookie是什么    

      cookie 也叫 HTTPCookie，是客户端与服务器端进行会话(session)使用的一个能够在浏览器本地化存储的技术。
      
cookie只能基于http传输协议产生，无法使用文件系统产生

==cookie就是为了存储 sessionID而诞生==
     
#### 2, cookie的作用

      cookie的作用主要是在浏览器存储少量数据, 利用cookie我们可以实现一些保存数据的功能.  
     比如:
     1, 用户登录的记住密码功能(下次再访问网站时无需输入密码了)；
     
     2, 购物车，加入购物车的商品没有及时付款，使用cookie保存后, 可以在一定时间后再访问网站, 会发现购物车里还有之前的商品列表;
     
	 3.存储一些小的数据  4k 4096个字节
	 
	 
#### 3，Cookie的使用


    cookie由键值对形式的文本组成：name=value。
    
    完整格式为：    
    name=value;[expires=date];[path=路径];[domain=域名];[secure]
     
    其中中括号[]表示该值是可选。 
    name=value: 为你要保存的键值对(必选) 
    expires=date: 表示cookie的失效时间, 默认是浏览器关闭时失效(可选)
    path=路径: 访问路径, 默认为当前文件所在目录(可选)
    domain=域名: 访问域名, 限制在该域名下访问(可选)
    secure: 安全设置, 如果设置了则必须使用https协议才可获取cookie(可选)
    
    
#### 4，cookie的封装

    /**
     * @description 设置cookie操作
     * @param name {String}  cookie键名称
     * @param value {String} cookie的值
     * @param date {Number}  设置有效时间(天)
     * @param path {String}  存储的路径
     * @param domain {String} 指定的域名
     * @param secure {Boolean}  是否安全
     * @returns {string}  返回cookie的字符串结构
     */
    function setCookie(name, value, date, path, domain, secure){
        var strCookie = "";
        if(name != null || name != ""){
            strCookie += encodeURIComponent(name) + "=" + encodeURIComponent(value) + ";";
        }
        if(typeof(date) == "number"){
    
            var tmp = new Date();
            tmp.setDate(tmp.getDate() + Number(date));
            strCookie += "expires=" + tmp + ";"
        }
        if(path){
            strCookie += "path=" + path + ";"
        }
    
        if(domain){
            strCookie += "domain=" + domain + ";"
        }
        if(secure){
            strCookie += "secure"
        }
        return document.cookie = strCookie;
    }
    
    //删除封装
    
    function removeCookie(key){
        setCookie(key, "", -1);
    }

    function getCookieAll(){
        var obj = {};
    
        var strCookie =decodeURIComponent(document.cookie);//name=value
        //name=value; name1=value1
        var arr = strCookie.split(";");
        for(var i = 0; i < arr.length; i++){
            var arr2 = arr[i].trim().split("=");
            obj[arr2[0]] = arr2[1];
        }
        return obj;
    }

    function getCookieByName(key){
        var obj = getCookieAll();
        return obj[key];
    }
    
### JSON
JSON 是一种结构化的数据表示方式(数据格式). 通过JavaScript中的一些模式来表示结构化数据, JSON 并不是 JavaScript 独有的数据格式，其他很多语言都可以对 JSON 进行解析和序列化,  
除了JSON外, 还有XML也是一种数据表示方式; 目前很少使用XML的数据格式, 在这里我们只介绍JSON.

#### 1，JSON解析

      JSON解析就是将JSON字符串变成JSON对象(对象或数组)的过程; 
eval():  但这个方法可能会造成安全问题,解析的数据按js进行执行.

JSON.parse(): 常用该方法进行解析. (兼容IE8+)
    例如: 

          var jsonStr = '[{"name": "a","age" : 1},{"name" : "b","age" : 2}]';	
          var jsonOjb = JSON.parse(jsonStr);   
          console.log(jsonStr);
          console.log(jsonObj);
          
          
#### 2，JSON序列化

      JSON序列化就是将JSON对象(对象或数组)变成JSON字符串的过程; 和JSON解析相反; 
JSON.stringify(): JSON序列化

例如: 

      var jsonObj = [{name : 'a', age : 1},{name : 'b', age : 2}];	
      var jsonStr = JSON.stringify(jsonObj);	
      console.log(jsonStr);	
      console.log(jsonStr);	
      
      
#### encodeURI()，和encodeURIComponent()的区别

    简单 
    encodeURI() 不会对  ;/?:@&=+$,# 编码
    encodeURIComponent() 不会  - _ . ! ~ * ' ( ) 编码
    
    cookie 中 ' ; ' 分号必须编码,所以 只能 encodeURIComponent(),
    例如 有一段字符串 "我爱你;我想你"
     
    document.cookie="aaa=我爱你;我想你" // 实际存进去的只有 "aaa=我爱你"
    
    


