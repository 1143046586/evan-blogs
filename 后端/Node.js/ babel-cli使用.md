参照资源

https://excaliburhan.com/post/babel-preset-and-plugins.html

https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/user-handbook.md#toc-configuring-babel


第一步:安装

```js
    npm install babel-cli babel-preset-env --dev
```

第二步:新建一个文件
  .babelrc   前面有点
```js
    
    {
      "presets": ["env"],
      "plugins": ["transform-runtime"]
    }

```
或者

```js
    
    {
      "presets": ["env","es2015", "stage-2"],
      "plugins": ["transform-runtime"]
    }

```
第三部:修改配置文件
packge.json

```js
     "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "nodemon --exec babel-node server.js"
    }

```

第四部: 执行

```js
    npm start
```