
   最近在完成MySql项目集成的情况下，需要增加批量更新的功能，根据网上的资料整理了一下，很好用，都测试过，可以直接使用。

   mysql 批量更新共有以下四种办法

### 1、.replace into 批量更新
```
replace into test_tbl (id,dr) values (1,'2'),(2,'3'),...(x,'y');
```
   例子：
   ```
replace into book (`Id`,`Author`,`CreatedTime`,`UpdatedTime`) values
(1,'张飞','2016-12-12 12:20','2016-12-12 12:20'),
(2,'关羽','2016-12-12 12:20','2016-12-12 12:20');
   ```

### 2、insert into ...on duplicate key update批量更新
```
insert into test_tbl (id,dr) values (1,'2'),(2,'3'),...(x,'y') 
on duplicate key update dr=values(dr);
```
  例子：
  ```
insert into book (`Id`,`Author`,`CreatedTime`,`UpdatedTime`) values
(1,'张飞2','2017-12-12 12:20','2017-12-12 12:20'),
(2,'关羽2','2017-12-12 12:20','2017-12-12 12:20') 
on duplicate key update Author=values(Author),
CreatedTime=values(CreatedTime),UpdatedTime=values(UpdatedTime);
```
replace into  和 insert into on duplicate key update的不同在于：

replace into 操作本质是对重复的记录先delete 后insert，如果更新的字段不全会将缺失的字段置为缺省值，用这个要悠着点否则不小心清空大量数据可不是闹着玩的。
    insert into 则是只update重复记录，不会改变其它字段。

### 3.创建临时表，先更新临时表，然后从临时表中update

    create temporary table tmp(id int(4) primary key,dr varchar(50));
    insert into tmp values  (0,'gone'), (1,'xx'),...(m,'yy');
    update test_tbl, tmp set test_tbl.dr=tmp.dr where test_tbl.id=tmp.id;

   注意：这种方法需要用户有temporary 表的create 权限。

### 4、使用mysql 自带的语句构建批量更新

mysql 实现批量 可以用点小技巧来实现:

    UPDATE yoiurtable
        SET dingdan = CASE id 
            WHEN 1 THEN 3 
            WHEN 2 THEN 4 
            WHEN 3 THEN 5 
        END
    WHERE id IN (1,2,3)

这句sql 的意思是，更新dingdan 字段，如果id=1 则dingdan 的值为3，如果id=2 则dingdan 的值为4……

where部分不影响代码的执行，但是会提高sql执行的效率。确保sql语句仅执行需要修改的行数，这里只有3条数据进行更新，而where子句确保只有3行数据执行。   

    例子：UPDATE book
        SET Author = CASE id 
            WHEN 1 THEN '黄飞鸿' 
            WHEN 2 THEN '方世玉'
            WHEN 3 THEN '洪熙官'
        END
    WHERE id IN (1,2,3)

    如果更新多个值的话，只需要稍加修改：

    UPDATE categories      
        SET dingdan = CASE id 
            WHEN 1 THEN 3 
            WHEN 2 THEN 4 
            WHEN 3 THEN 5 
        END, 
        title = CASE id 
            WHEN 1 THEN 'New Title 1'
            WHEN 2 THEN 'New Title 2'
            WHEN 3 THEN 'New Title 3'
        END
    WHERE id IN (1,2,3)

    例子：UPDATE book
        SET Author = CASE id 
            WHEN 1 THEN '黄飞鸿2' 
            WHEN 2 THEN '方世玉2'
            WHEN 3 THEN '洪熙官2'
        END,
        Code = CASE id 
            WHEN 1 THEN 'HFH2' 
            WHEN 2 THEN 'FSY2'
            WHEN 3 THEN 'HXG2'
        END
    WHERE id IN (1,2,3)
