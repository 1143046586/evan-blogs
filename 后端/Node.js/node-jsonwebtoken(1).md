官网: https://jwt.io/

A.生成
```js  
    const jwt = require("jsonwebtoken");
    let token = jwt.sign({
            uname : "abc123"
        },
        "secret",
        {
            expiresIn : 60
        }
    )
```    
    
B.校验
```js

//var token = req.body.token || req.query.token || req.headers['x-access-token'];
    let token = qs.parse(req.headers.cookie).sercet;

    jwt.verify(token, "secret", function(err, decoded){
        console.log(decoded);
        if(!err){
            console.log(decoded.uname);  //会输出123，如果过了60秒，则有错误。
        }
    });
```
    
    
    

1).生成私钥
在指定的文件夹下生成 私钥可以

```js
    ssh-keygen -t rsa -b 2048 -f       private.key
```
2).生成公钥
    
```js 
    openssl rsa -in private.key -pubout -outform pem -out public.key
```

3).就要使用 RS256

```js
    let token = jwt.sign({name : "admin"}, private.toString(), {
    expiresIn : 60 * 60,
    algorithm:'RS256'  // 注意使用rs256 , algorithm (default: HS256)
});
```

解

```js
    jwt.verify(token, public.toString(), {algorithms : ['RS256']}, function(err, data){
    console.log(data);
})
```

