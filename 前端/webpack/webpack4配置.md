### 一、命令行安装命令
全局安装webpack、webpack-cli
```
npm install --global webpack webpack-cli
```
本地安装webpack、webpack-cli
```
npm install webpack webpack-cli -D
```
安装web服务
```
 npm i webpack-dev-server -D
```
安装HTML打包插件
```
npm install html-webpack-plugin --save-dev
```
### 二、配置项目
在package.json中配置scripts字段
```
"scripts": {
    "dev": "webpack --config=./config/webpack.dev.js",
    "build": "webpack --config=./config/webpack.pro.js",
    "server": "webpack-dev-server --config=./config/webpack.dev.js"
  }
```
根目录新建文件路径
```
    |
    |__ config
    |    |
    |    |__webpack.dev.js
    |    |__webpack.pro.js
    |
    |__ src
    |    |__index.js
    |    |__index.html
    |
    |__ dist
```
### 三、配置webpack配置文件
webpack.dev.js
```
const path =require("path")
const htmlPlugin=require("html-webpack-plugin")
module.exports={
  entry:"./src/index.js",
  output:{
    path:path.resolve(__dirname,"../dist"),
    filename:"app.js"
  },
  mode:"development",
  module:{},
  plugins:[
    new htmlPlugin({
      minify:{
        removeAttributeQuotes:true
      },
      hash:true,
      template:"./src/index.html"
    })
  ],
  loader:{},
  devServer:{
    contentBase:path.resolve(__dirname,"../dist"),
    host:"localhost",
    port:"8090",
    compress:true               //服务器压缩是否开启
  }
}
```
webpack.pro.js
```

```


### 四、参考资料
[webpack4 从零开始](https://blog.csdn.net/w123452222/article/details/81394696)