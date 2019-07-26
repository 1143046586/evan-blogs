### 一 链接数据库
##### 1.建立数据库链接
    $con=new mysqli('localhost', 'fzq', '1995feng320', 'test');
创建一个新的数据库链接并将标识符赋给$conn

    localhost : 链接的数据库地址

    fzq : 数据库用户名

    1995feng320 : 数据库密码

    test : 将要操作的数据库名称
##### 2.验证数据库是否链接成功
```
if(!$con)
    die("connect error:".$mysqli->connect_error;
else
    echo "success connect mysql\n";
```
当链接成功建立即$con不为空时，输出链接成功，否则输出链接错误和输出连接错误提示($mysqli->connect_error).
##### 3.设置链接使用的字符编码
```
$con->query("set names utf-8");
```
通常我们会先设置一下当前连接传输数据使用的字符编码，一般的我们会使用utf8编码。
##### 4.切换数据库
```
$con->select_db("world");
```
切换将要操作的数据库为：world

### 二 关闭数据库
```
$con->close();
```
可用来关闭非持久的数据库链接，一般可以不使用，数据库链接会在脚本关闭时自动关闭链接．
### 三 数据库操作
```
$sql="select*from userinfo where name='".$name."'";
$result=$conn->query($sql);
```
使用query函数对数据库进行操作，并将返回值赋给$result , $sql为发送到数据库的sql操作语句.

mysql_query() 仅对 SELECT，SHOW，EXPLAIN 或 DESCRIBE 语句返回一个资源标识符（上文的$result），如果查询执行不正确则返回 FALSE。

对于其它类型的 SQL 语句，mysql_query() 在执行成功时返回 TRUE，出错时返回 FALSE。

非 FALSE 的返回值意味着查询是合法的并能够被服务器执行。这并不说明任何有关影响到的或返回的行数。很有可能一条查询执行成功了但并未影响到或并未返回任何行。
##### 1.查询语句

    $sql="select name from userinfo where name='".$name."'";
    
该sql语句表示从userinfo数据表的name字段查找name为$name变量值得结果
    
==sql语句不区分大小写，where用来连接条件判断的子语句，name键值对后面跟字符串类型的变量时,需要在变量前后连接引号。==
##### 2.插入语句

    $sql="INSERT INTO userinfo (name,passWord,email,phoneNumber,QQ) VALUES ('".$name."','".$passWord."','".$email."','".$phoneNumber."','".$QQ."')";
    
user：为插入数据的数据表

(name, age, class)：为将要传入数据表的三个字段

values('$name', '$age', '$class')：为传入数据表的对应的三个值。

在mysql中，执行插入语句以后，可以得到自增的主键id,通过PHP的mysql_insert_id函数可以获取该id。

    $con->insert_id
    
这个id的作用非常大，通常可以用来判断是否插入成功，或者作为关联ID进行其他的数据操作。

### 四 查询结果处理
##### 1. $result->fetch_row()

    $row = $result->fetch_row()
    
每执行一次，都将从查询返回的结果集中取出一行数据，以一维数字索引数组的方式返回出来并赋给变量$row，当前一次已经取到最后一条数据的时候，返回空结果。

     while ($row = $result->fetch_row()) {
        printf ("%s (%s)\n", $row[0], $row[1]);
    }

可以使用while循环依次输出结果集。
##### 2. $result->fetch_array(参数)
    
    $row = $result->fetch_array()
    
每执行一次，都将从查询返回的结果集中取出一行数据，以一维索引数组和关联数组的方式返回出来并赋给变量$row。
