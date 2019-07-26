<h1 id="1-5-使用-plugin">1-5 使用 Plugin</h1>
<p>Plugin 是用来扩展 Webpack 功能的，通过在构建流程里注入钩子实现，它给 Webpack 带来了很大的灵活性。</p>
<p>在上一节中通过 Loader 加载了 CSS 文件，本节将通过 Plugin 把注入到 <code>bundle.js</code> 文件里的 CSS 提取到单独的文件中，配置修改如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> ExtractTextPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;extract-text-webpack-plugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// JavaScript 执行入口文件</span>
  entry: <span class="hljs-string">&apos;./main.js&apos;</span>,
  output: {
    <span class="hljs-comment">// 把所有依赖的模块合并输出到一个 bundle.js 文件</span>
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
    <span class="hljs-comment">// 把输出文件都放到 dist 目录下</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 用正则去匹配要用该 loader 转换的 CSS 文件</span>
        test: <span class="hljs-regexp">/\.css$/</span>,
        use: ExtractTextPlugin.extract({
          <span class="hljs-comment">// 转换 .css 文件需要使用的 Loader</span>
          use: [<span class="hljs-string">&apos;css-loader&apos;</span>],
        }),
      }
    ]
  },
  plugins: [
    <span class="hljs-keyword">new</span> ExtractTextPlugin({
      <span class="hljs-comment">// 从 .js 文件中提取出来的 .css 文件的名称</span>
      filename: <span class="hljs-string">`[name]_[contenthash:8].css`</span>,
    }),
  ]
};
</code></pre>
<p>要让以上代码运行起来，需要先安装新引入的插件：</p>
<pre><code class="lang-bash">npm i -D extract-text-webpack-plugin
</code></pre>
<p>安装成功后重新执行构建，你会发现 dist 目录下多出一个 <code>main_1a87a56a.css</code> 文件，<code>bundle.js</code> 里也没有 CSS 代码了，再把该 CSS 文件引入到 <code>index.html</code> 里就完成了。</p>
<p>从以上代码可以看出， Webpack 是通过 <code>plugins</code> 属性来配置需要使用的插件列表的。
<code>plugins</code> 属性是一个数组，里面的每一项都是插件的一个实例，在实例化一个组件时可以通过构造函数传入这个组件支持的配置属性。</p>
<p>例如 <code>ExtractTextPlugin</code> 插件的作用是提取出 JavaScript 代码里的 CSS 到一个单独的文件。
对此你可以通过插件的 <code>filename</code> 属性，告诉插件输出的 CSS 文件名称是通过 <code>[name]_[contenthash:8].css</code> 字符串模版生成的，里面的 <code>[name]</code> 代表文件名称， <code>[contenthash:8]</code> 代表根据文件内容算出的8位 hash 值，
还有很多配置选项可以在 <a href="https://github.com/webpack-contrib/extract-text-webpack-plugin" target="_blank">ExtractTextPlugin</a> 的主页上查到。</p>
<blockquote>
<p>本实例 <a href="http://webpack.wuhaolin.cn/1-5使用Plugin.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                