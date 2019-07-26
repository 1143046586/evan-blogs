### 一.前期准备
官网下载Mongodb包

本文使用 mongodb-linux-x86_64-debian92-4.0.3.tgz

### 二.mongodb安装和配置
解压和复制mongodb至目录 /usr/local/mongodb

进入目录
```
cd  /usr/local/mongodb
tar -zxvf mongodb-linux-x86_64-amazon-3.6.2.tgz
sudo mv mongodb-linux-x86_64-amazon-3.6.2 mongodb
```
#### 2.1 系统profile配置
修改/etc/profile文件，

   sudo vi /etc/profile
在文件最后插入下面内容

    export MONGODB_HOME=/usr/local/mongodb
    export PATH=$PATH:$MONGODB_HOME/bin
保存后，重启系统配置

    source /etc/profile
    
终端查看输入$PATH命令查看mongodb的bin目录是否被包含在全局路径

#### 2.2 mongodb启动配置
新建配置文件 目录可以定义，进入编辑

   sudo  vi /etc/mongodb.conf --
    
拷贝配置放到文件里
```
#日志文件位置
logpath=/data/mongodb/logs/mongodb.log  （自行创建路径及文件）

# 以追加方式写入日志
logappend=true

# 是否以守护进程方式运行
fork = true

# 默认27017
port = 27017
bind_ip = 127.0.0.1
journal=true
# 数据库文件位置
dbpath=/data/mongodb/db  （自行创建路径）

# 启用定期记录CPU利用率和 I/O 等待
#cpu = true

# 是否以安全认证方式运行，默认是不认证的非安全方式
#noauth = true
#auth = true

# 详细记录输出
#verbose = true

# Inspect all client data for validity on receipt (useful for
# developing drivers)用于开发驱动程序时验证客户端请求
#objcheck = true

# Enable db quota management
# 启用数据库配额管理
#quota = true
# 设置oplog记录等级
# Set oplogging level where n is
#   0=off (default)
#   1=W
#   2=R
#   3=both
#   7=W+some reads
#diaglog=0

# Diagnostic/debugging option 动态调试项
#nocursors = true

# Ignore query hints 忽略查询提示
#nohints = true
# 禁用http界面，默认为localhost：28017
#nohttpinterface = true

# 关闭服务器端脚本，这将极大的限制功能
# Turns off server-side scripting.  This will result in greatly limited
# functionality
#noscripting = true
# 关闭扫描表，任何查询将会是扫描失败
# Turns off table scans.  Any query that would do a table scan fails.
#notablescan = true
# 关闭数据文件预分配
# Disable data file preallocation.
#noprealloc = true
# 为新数据库指定.ns文件的大小，单位:MB
# Specify .ns file size for new databases.
# nssize =

# Replication Options 复制选项
# in replicated mongo databases, specify the replica set name here
#replSet=setname
# maximum size in megabytes for replication operation log
#oplogSize=1024
# path to a key file storing authentication info for connections
# between replica set members
#指定存储身份验证信息的密钥文件的路径
#keyFile=/path/to/keyfile
```
按个人喜好修改配置 并且保存

### 三.mongodb测试
#### 3.1 启动mongod数据库服务

    mongod -f /etc/mongodb.conf
    
--这里的conf文件是之前新建的.conf文件路径 如果不同请修改
#### 3.2 进入mongodb数据库

    mongo

如打印信息说明成功 pidof mongod 可以验证 或者top看

#### 3.3 插入1个数据

    use lalala

    db.test.insert({title:'哈哈',Tags:['anime','game']});

    show collections

    db.test.find()

    show dbs -- 只有有东西才显示 默认test没东西所以你看不到

### 四 建立root账号
mongo 默认谁都可以访问修改 但只有本地 所以我们要配置下

use 哪个就是用哪个数据库 没有就新建 mongo默认会生成一些数据库 如 admin ,config
```
    show dbs -- 看看有哪些数据库

    use admin
    -- 建立root并且分配角色
    db.createUser({user:"root",pwd:"123456",roles:["root","userAdminAnyDatabase"]})
    
    db.createUser({user: "fzq", pwd: "1995feng320", roles: [{ role: "readWrite", db: "blog" }]}) //新建外网访问数据库用户
    
    db.system.users.find() -- 查看系统的用户
```
如果有了就输入

    exit
    
命令退出

前面建的mongodb.conf里 把#auth = true前面的#号去掉，启用安全验证。

    mongod --shutdown -f /etc/mongodb.conf --关闭服务

    mongod -f /etc/mongodb.conf --开启服务

    show dbs - 报错 不可以以匿名方式显示

    use admin

    db.auth("root","123456") -- 注意引号 他是个字符串

返回1 说明身份通过你的身份就是root

    use nana -- 以root的身份建立个nana数据库

    db.createUser({user:'admin',pwd:'123456', roles: [ "readWrite", "dbAdmin" ]}) -- 为nana创建个管理员用户admin

    show users -- 看用户

    db.logout() -- 登出root 或者exit

    db.websize.insert({title:'哈哈',Tags:['anime','game']}); -- 没权限

    db.auth("admin","123456") -- 用nana的admin

    db.websize.insert({title:'dorodoroLab',Tags:['dorodro','lab']}); -- 这次有权限

    db.websize.find() --插入成功

这里要注意的是 createUser 根据 use在哪个数据库 show users信息就生成在哪 不然通过db.auth可能找不到 

下面命令也一样

    db.grantRolesToUser( "admin" , [ "readWrite", "dbAdmin","useAdmin" ]) -- 用户新授权 
也可以用db.updateUser

    db.dropUser('admin') --删除用户

root用户可以使用admin全局管理用户 通过db.system.users.find() db.system.users.remove() 等
如果懒的记请用 db.help() 去看

### 五.mongodb开机启动
#### 5.1设置mongodb.service启动服务
进入/lib/systemd/system文件夹修改里面的mongodb.service文件（没有就新建）

    cd /lib/systemd/system

    sudo vi mongodb.service
输入下面内容：
```
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
```
#### 5.2 设置mongodb.service权限

    sudo chmod 777 mongodb.service

    systemctl daemon-reload

#### 5.3 系统mongodb.service操作命令
启动服务

    systemctl start mongodb.service
关闭服务

    systemctl stop mongodb.service
开机启动

    systemctl enable mongodb.service

上面命令如果出现下面错误：

    Unit mongodb.service is masked
    
可以使用下面命令进行解决

    systemctl unmask mongodb.service
#### 5.4 mongodb.service启动测试
mongo 127.0.0.1