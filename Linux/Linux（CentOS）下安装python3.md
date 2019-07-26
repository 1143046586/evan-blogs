#### 1、python官网下载Linux版本的源代码文件
python官网下载Linux版本的二进制文件，二进制文件后缀为:tar.xz
#### 2、使用FileZilla 将python源代码文件上传至云服务器
链接协议：SFTP
端口：空

#### 3、解压文件
进入python源代码文件所在文件夹,并解压文件
```
cd /fzq/
tar -vxf python-3.7.3.tar.xz
```
#### 4、编译安装
安装时不能卸载CentOS原有python版本，不然会导致yum不可用
```
cd /fzq/python-3.7.3/
<!--设置安装路径-->
./configure --prefix=/fzq/python3    //  /home/python3为安装路径
<!--编译-->
```
在终端中输入命令：
```
make
```
之后，再输入命令：
```
make install
```
#### 4、建立软链接
（1）备份python2的软链接,在终端中输入命令：
```
sudo mv /usr/bin/python /usr/bin/python2
sudo mv /usr/bin/pip /usr/bin/pip2
```
建立python3的软链接
```
sudo ln -s /fzq/python3/bin/python3 /usr/bin/python
sudo ln -s /fzq/python3/bin/pip3 /usr/bin/pip
```
#### 5、配置yum
==值得注意：因为在Centos中，yum源使用的是Python2.7，替换为Python3以后，yum源无法正常工作。所以我们需要修改yum配置文件。==

首先，更改文件权限，在终端输入命令：
```
sudo chmod 777 /usr/bin/yum
```
在终端输入命令：
```
vim /usr/bin/yum
```
将首行#!/usr/bin/python 改为#!/usr/bin/python2.7
```
vim /usr/libexec/urlgrabber-ext-down
```
将首行#!/usr/bin/python 改为#!/usr/bin/python2.7
```
vim /usr/bin/yum-config-manager
```
将首行#!/usr/bin/python 改为#!/usr/bin/python2.7
#### 6、安装pip
首先安装setuptools工具
```
cd /fzq
wget   https://bootstrap.pypa.io/ez_setup.py 
python ez_setup.py
unzip setuptools-33.1.1.zip
cd setuptools-33.1.1.zip
python setup.py install 
```
如果安装时报错ModuleNotFoundError: No module named '_ctypes'的解决办法
```
yum install libffi-devel
cd Python-3.7.0    //python3安装文件,重新编译
make
make install
```
如果安装时报错 RuntimeError: Compression requires the (missing) zlib module 的解决办法
```
yum install -y zlib
yum install -y zlib-devel 
cd Python-3.7.0    //python3安装文件,重新编译
make
make install
```
然后
```
cd setuptools-33.1.1
python setup.py install 
```
然后官网下载 **pip-19.1.1.tar.gz** 文件到本地

将 **pip-19.1.1.tar.gz** 上传到云服务器上文件
```
cd /fzq
tar -xf pip-19.1.1.tar.gz
```
安装pip
```
cd /fzq/pip-19.1.1
python setup.py install 
python2 setup.py install 
```
建立软链接
```
rm /usr/bin/pip         //删除python2安装pip时自动生成的软链接
sudo ln -s /fzq/python3/bin/pip3 /usr/bin/pip
sudo ln -s /fzq/python3/bin/pip3 /usr/bin/pip3
```

