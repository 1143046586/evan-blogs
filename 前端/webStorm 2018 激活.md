# webStorm 2018 激活
今天早上一更新webStorm，之前在网上找到的方式基本都没用了，后来摸索了一小阵儿。找到解决办法了：

    先下载破解补丁
    修改路径
    然后获取激活码
    重启OK
    
## 下载破解补丁
打开网址（[IntelliJ IDEA 注册码](http://idea.lanyus.com/)），下载补丁

然后将补丁复制到安装目录的bin目录下（下面是补丁路径，不一定是你们的，要改）

    E:\WebStorm 2017.2.5\bin
==PS：一定要把这个jar包复制到这个bin目录，不然后面操作可能无法进行。==
## 修改路径
修改同目录下的 WebStorm.exe.vmoptions 和WebStorm64.exe.vmoptions，这两个文件一个是32位的，一个是64位的，建议同步修改。

用文本编辑器打开之后，在文件最上面加一行代码 ：（这个也是我的路径，你们要看着改，尤其是补丁的名字要对）
    
    -javaagent:E:\WebStorm 2017.2.5\bin\JetbrainsCrack-2.5.6.jar
    
配置好之后，保存文件。再此启动WebStorm。
## 获取激活码
打开网址（ [IntelliJ IDEA 注册码](http://idea.lanyus.com/) ），我们能看到下面的界面，直接点击获取激活码，将生成的激活码粘贴到WebStorm激活对话框中的【Lisence Code】输入框，点击OK即可破解。
