#### 1.检测系统内部有没有安装其他的mysql数据库
```
rpm -qa | grep mysql
```
然后如果有的话删除这些mysql yum remove 查出来的所有名字

#### 2.彻底删除系统中mysql的目录
```
find / -name mysql
```
将查出的所有目录删掉 rm -rf 查到的路径

#### 3.下载mysql的rpm包
```
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
#### 4.安装mysql源
```
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```
#### 5.重要一步

先尝试安装
```
yum install -y mysql-community-server
```
如果可以安装那么安装之后查询mysql版本 mysql -V

显示5.7版本则安装成功

如果安装成立5.6

那么修改mqsql源里面的一些 vi /etc/yum.repos.d/mysql-community.repo ：比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可。

这里是将5.7的修改为1 5.6的修改为0
```
# Enable to use MySQL 5.5
[mysql55-community]
name=MySQL 5.5 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.5-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

# Enable to use MySQL 5.6
[mysql56-community]
name=MySQL 5.6 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/7/$basearch/
enabled=0 #####################################
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1 #######################################
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-tools-preview]
name=MySQL Tools Preview
baseurl=http://repo.mysql.com/yum/mysql-tools-preview/el/7/$basearch/
enabled=0
```
再次安装 
```
yum install mysql-community-server
```
#### 6.启动mysql服务
```
systemctl start mysqld
```
#### 7.mysql开启启动
```
systemctl enable mysqld
```
#### 8.查看mysql密码
```
grep 'temporary password' /var/log/mysqld.log
```
如果你什么都没有查到

直接进入mysql
```
mysql -uroot -p
```
如果查到了，则使用查到的密码进行登录
```
mysql -uroot -pCW9dtNXktl?d
```
#### 9.修改密码
首先需要设置密码的验证强度等级
```
set global validate_password_policy=0;

//关于 mysql 密码策略相关参数；
//1）、validate_password_length  固定密码的总长度；
//2）、validate_password_dictionary_file 指定密码验证的文件路径；
//3）、validate_password_mixed_case_count  整个密码中至少要包含大/小写字母的总个数；
//4）、validate_password_number_count  整个密码中至少要包含阿拉伯数字的个数；
//5）、validate_password_policy 指定密码的强度验证等级，默认为 MEDIUM；
//  关于 validate_password_policy 的取值：
//      0/LOW：只验证长度；
//      1/MEDIUM：验证长度、数字、大小写、特殊字符；
//      2/STRONG：验证长度、数字、大小写、特殊字符、字典文件；
//6）、validate_password_special_char_count 整个密码中至少要包含特殊字符的个数；
```
设置自己的密码
MySQL版本5.7.6版本以前用户可以使用如下命令：
```
mysql> SET PASSWORD = PASSWORD('Xiaoming250'); 
```
MySQL版本5.7.6版本开始的用户可以使用如下命令：
```
mysql> ALTER USER USER() IDENTIFIED BY 'Xiaoming250';
```
修改初始密码后可使用
```
 SHOW VARIABLES LIKE 'validate_password%';
```
命令查看密码策略
#### 10.修改root用户远程访问
```
mysql -uroot -p  输入密码登录
grant all on *.* to 'root'@'%' identified by 'Xiaoming250';
```
all 表示所有的权限（读、写、查询、删除等等操作），

. 前面的 * 表示所有的数据库，后面的 * 表示所有的表，

identified by 后面跟密码，用单引号括起来。

这里的user1指的是localhost上的user1

用户和主机的IP之间有一个@，另外主机IP那里可以用%替代，表示所有主机