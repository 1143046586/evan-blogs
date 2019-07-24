### 一、命令行安装命令
全局安装webpack

    npm i webpack@3.11.0 -g
安装 css加载器

    npm i style-loader css-loader -D
安装ES6语法转ES5解释器

    npm i babel-loader@7.1.4 babel-core babel-preset-env -D
安装less模块及less转译css加载器

    npm i less less-loader -D
安装web服务器

    npm i webpack-dev-server@2.9.7 -g
启动web服务器,inline参数表示自动刷新

    webpack-dev-server --inline
安装HTML页面自动添加script、link标签的插件

    npm i html-webpack-plugin -D
局部安装webpack

    npm i webpack@3.11.0 -D
安装将css单独抽取为一个css文件的插件    

    npm i extract-text-webpack-plugin -D
安装ES6提案2语法解析工具,例如...展开符

    npm i babel-preset-stage-2 -D
    //在babelrc文件中的preset选项中添加stage-2选项
    
### 二、webpack常用配置
```js
let Hwp = require("html-webpack-plugin")    //引入自动复制HTML文件的插件
let Ext = require("extract-text-webpack-plugin")    //引入将css代码单独提取到一个css文件的插件
let webpack = require("webpack")    //引入本地安装的webpack,有些插件需要用到

module.exports = {
    //入口文件
    entry: __dirname + "/src/main.js",
    output: {
        //出口文件夹
        path: __dirname + "/dist/",
        // 出口文件名
        filename:"app.js",
        publicPath : "/"  //指定输出的基础路径
    },
    module: {
        // 定义文件解析规则
        rules: [
            {
                //
                test: /\.css$/,     //以.css后缀的文件
                // loader:"style-loader!css-loader"    //先用css-loader抓取css，然后用style-loader将css插入到页面的style标签中
                loader:Ext.extract("css-loader")    //当使用extract-text-webpack-plugin插件时先用css-loader抓取css，
                                                    //然后导入给extract-text-webpack-plugin插件
            },
            {
                test: /\.less$/,    //解析less文件
                // loader:"style-loader!css-loader!less-loader" //先用less-loader转译less文件，再用css-loader抓取css，
                                                                //然后用style-loader将css插入到页面的style标签中
                                                                
                loader:Ext.extract("css-loader!less-loader")    //当使用extract-text-webpack-plugin插件时,先用less-loader转译less文件，
                                                                //再用css-loader抓取css，然后导入给extract-text-webpack-plugin插件
            },
            {
                test: /\.js$/,
                //排除node_module文件夹里面的文件
                exclude: /node_module/,
                loader:"babel-loader"
            }
        ]
    },
    //配置web服务器
    devServer: {
        contentBase: __dirname + "/dist/",  //定义服务器的根目录
        port: 3000,     //定义服务器端口
        inline:true,	    //定义是否自动刷新
        proxy : {           //设置反向代理
            "/109-35" : {
                target : "http://route.showapi.com",
                secure : false
            }
        },
        publicPath : "/",   //指定请求的基础路径,"/"表示根路径
        historyApiFallback : true,  //为true时,所有请求都将指向index.html,对开发单页面程序时开启
        disableHostCheck : true  //不检查hostname,例如 localhost 与 127.0.0.1
    },
    //配置插件
    plugins: [
        new Hwp({
            template: "src/index.html",     //定义html入口文件
            filename: "index.html",         //定义出入位置的HTML文件名,路径为webpack的出口路径
            inject:true	                    //是否自动添加script标签
        }),
        new Ext("app.css"), //使用extract-text-webpack-plugin插件抽取css代码到独立文件中  其中app.css为自定义文件名
		new webpack.ProvidePlugin({ //将$符配置为全局变量  如此一来 在文件中就可以无需引入 直接使用$符了
			$ : "jquery"
		}),
		new webpack.BannerPlugin('版权所有，翻版必究') //版权申明,生成在js的头部jdoc中

    ],
    devtool:"source-map",   //资源地图模式,用于bug报错定位
    resolve: {
        alias: { //配置程序  在引入"vue"时  实际指向的是vue模块下dist目录中的vue.js文件
            "vue": "vue/dist/vue.js"    //vue.js为编译前的文件，runtime为编译后的版本，使用webpack应用vue.js
        }
    },
}
```
# 参考书籍

>
>[webpack入门](https://segmentfault.com/a/1190000006178770)
>-
>[深入浅出webpack](http://webpack.wuhaolin.cn/)
>-
-

-

-