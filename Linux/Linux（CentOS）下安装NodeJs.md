#### 1、node官网下载Linux版本的二进制文件
node官网下载Linux版本的二进制文件，二进制文件后缀为:tar.xz
#### 2、使用FileZilla 将node二进制文件上传至云服务器
链接协议：SFTP
端口：空

#### 3、解压文件
进入node二进制文件所在文件夹,并解压文件
```
cd /fzq/
tar -vxf node-10.16.0.tar.xz
```
#### 4、配置环境变量
查看环境变量路径
```
echo $PATH
```
将node和npm的软链接复制到环境变量路径中
```
ln -s /fzq/node-v10.16.0/bin/node /usr/local/bin
ln -s /fzq/node-v10.16.0/bin/npm /usr/local/bin
```
将全局安装目录添加到环境变量中
```
vim /etc/profile
//在文件的最后一行加上：export PATH=“$PATH:/fzq/node-v10.16.0-linux-x64/bin”
source /etc/profile      //立即生效
```