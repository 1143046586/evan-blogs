### 插入数据
```
insert into qingdaohang.errlog (errtime,errdetails)values("1212","这个是错误！")
```
### limit查询返回部分数据
```
 SELECT * FROM table LIMIT 5,10;     // 检索记录行 6-15 ,注意，10为偏移量 
 
 SELECT * FROM table LIMIT 95,-1;    // 检索记录行 96-结尾.
 
 SELECT * FROM table LIMIT 5;   //检索前 5 个记录行 //也就是说，LIMIT n 等价于 LIMIT 0,n。
 ```
 
### 查询字段为空的选项
```
select * from bizhi.imginfo where imgsmallpath is null
```

### 查询或设置值为null或不为null
```
//imgsmallpath将不为空的项设置为空
UPDATE imginfo set imgsmallpath=null WHERE imgsmallpath is not null
//imgsmallpath将为空的项设置为1
UPDATE imginfo set imgsmallpath=1 WHERE imgsmallpath is null
```
### 超时等待锁
事务没有提交导致 锁等待Lock wait timeout exceeded异常的处理办法

    select * from information_schema.innodb_trx

kill 对应的线程号就ok了
