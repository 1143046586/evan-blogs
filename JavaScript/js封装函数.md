### 1,Unicode字符转utf-8
```
var regex_num_set = /\&#x\w+;/g;
str = str.replace(regex_num_set, function (_, $1) {
    let strTemp = "0x" + _.substring(3)
    return String.fromCharCode(parseInt(strTemp, 16));
});
```