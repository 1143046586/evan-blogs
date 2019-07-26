### 1.首先更新一下仓库

    sudo apt-get update
### 2.安装mysql

    sudo apt-get install -y mysql-server mysql-client
### 3.检查mysql是否已运行

    sudo netstat -tap | grep mysql
有显示就是已运行

### 4.查询默认的用户名和密码

(提示:sudo命令会让你输入你的用户密码,就是你的开机密码,并且你输入的时候看不到自己输入的内容,不要以为电脑卡住了,输完回车就行)

    sudo cat /etc/mysql/debian.cnf
显示出来的内容格式如下(我的密码,跟你不一样)
```
baiguo@baiguo-PC:~$ sudo cat /etc/mysql/debian.cnf 
# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = F64nKZ233QkzL8v9
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = F64nKZ233QkzL8v9
socket   = /var/run/mysqld/mysqld.sock
```
其中user代表的用户名,password代表的默认生成的密码

### 5.利用默认账号密码登录
(-p后面是我的密码,你们要把自己的密码放在那里,并且-p跟密码之间无空格)

    mysql -u debian-sys-maint -pF64nKZ233QkzL8v9
成功进入mysql,有如下显示:
```
baiguo@baiguo-PC:~$ mysql -u debian-sys-maint -pF64nKZ233QkzL8v9

mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.21-1 (Debian)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
### 6.现在更改密码的设置.
(原封不动复制过去,漏一个字你就完了),这是最重要的部分,很多教程都没有这个,所以才不管用

    update mysql.user set plugin="mysql_native_password" where user="root";
成功后结果如下:
```
mysql> update mysql.user set plugin="mysql_native_password" where user="root";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
```
### 7.设置root的密码

    update mysql.user set authentication_string=password('这里是你的密码') where user='root'and Host = 'localhost';
密码自己设置

### 8.退出数据库

    exit
### 9.重新启动数据库

    sudo service mysql restart
### 10.用自己设置好的密码登录

    mysql -u root -p
输入密码,成功登录,如下.
```
baiguo@baiguo-PC:~$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.21-1 (Debian)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
--------------------- 
作者：baiguoxiong 

来源：CSDN 

原文：https://blog.csdn.net/baiguoxiong/article/details/82936890 

版权声明：本文为博主原创文章，转载请附上博文链接！