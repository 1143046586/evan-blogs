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

