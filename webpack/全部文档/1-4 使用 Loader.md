<h1 id="1-4-使用-loader">1-4 使用 Loader</h1>
<p>在上一节中使用 Webpack 构建了一个采用 CommonJS 规范的模块化项目，本节将继续优化这个网页的 UI，为项目引入 CSS 代码让文字居中显示，<code>main.css</code> 的内容如下：</p>
<pre><code class="lang-css"><span class="hljs-selector-id">#app</span>{
  <span class="hljs-attribute">text-align</span>: center;
}
</code></pre>
<p>Webpack 把一切文件看作模块，CSS 文件也不例外，要引入 <code>main.css</code> 需要像引入 JavaScript 文件那样，修改入口文件 <code>main.js</code> 如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 通过 CommonJS 规范导入 CSS 模块</span>
<span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./main.css&apos;</span>);
<span class="hljs-comment">// 通过 CommonJS 规范导入 show 函数</span>
<span class="hljs-keyword">const</span> show = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./show.js&apos;</span>);
<span class="hljs-comment">// 执行 show 函数</span>
show(<span class="hljs-string">&apos;Webpack&apos;</span>);
</code></pre>
<p>但是这样修改后去执行 Webpack 构建是会报错的，因为 Webpack 不原生支持解析 CSS 文件。要支持非 JavaScript 类型的文件，需要使用 Webpack 的 Loader 机制。Webpack的配置修改使用如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// JavaScript 执行入口文件</span>
  entry: <span class="hljs-string">&apos;./main.js&apos;</span>,
  output: {
    <span class="hljs-comment">// 把所有依赖的模块合并输出到一个 bundle.js 文件</span>
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
    <span class="hljs-comment">// 输出文件都放到 dist 目录下</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 用正则去匹配要用该 loader 转换的 CSS 文件</span>
        test: <span class="hljs-regexp">/\.css$/</span>,
        use: [<span class="hljs-string">&apos;style-loader&apos;</span>, <span class="hljs-string">&apos;css-loader?minimize&apos;</span>],
      }
    ]
  }
};
</code></pre>
<p>Loader 可以看作具有文件转换功能的翻译员，配置里的 <code>module.rules</code> 数组配置了一组规则，告诉 Webpack 在遇到哪些文件时使用哪些 Loader 去加载和转换。
如上配置告诉 Webpack 在遇到以 <code>.css</code> 结尾的文件时先使用 <code>css-loader</code> 读取 CSS 文件，再交给 <code>style-loader</code> 把 CSS 内容注入到 JavaScript 里。
在配置 Loader 时需要注意的是：</p>
<ul>
<li><code>use</code> 属性的值需要是一个由 Loader 名称组成的数组，Loader 的执行顺序是由后到前的；</li>
<li>每一个 Loader 都可以通过 URL querystring 的方式传入参数，例如 <code>css-loader?minimize</code> 中的 <code>minimize</code> 告诉 <code>css-loader</code> 要开启 CSS 压缩。</li>
</ul>
<p>想知道 Loader 具体支持哪些属性，则需要我们查阅文档，例如 <code>css-loader</code> 还有很多用法，我们可以在 <a href="https://github.com/webpack-contrib/css-loader" target="_blank">css-loader 主页</a> 上查到。</p>
<p>在重新执行 Webpack 构建前要先安装新引入的 Loader：</p>
<pre><code class="lang-bash">npm i -D style-loader css-loader
</code></pre>
<p>安装成功后重新执行构建时，你会发现 <code>bundle.js</code> 文件被更新了，里面注入了在 <code>main.css</code> 中写的 CSS，而不是会额外生成一个 CSS 文件。
但是重新刷新 <code>index.html</code> 网页时将会发现 <code>Hello,Webpack</code> 居中了，样式生效了！
也许你会对此感到奇怪，第一次看到 CSS 被写在了 JavaScript 里！这其实都是 <code>style-loader</code> 的功劳，它的工作原理大概是把 CSS 内容用 JavaScript 里的字符串存储起来，
在网页执行 JavaScript 时通过 DOM 操作动态地往 <code>HTML head</code> 标签里插入 <code>HTML style</code> 标签。
也许你认为这样做会导致 JavaScript 文件变大并导致加载网页时间变长，想让 Webpack 单独输出 CSS 文件。下一节 <a href="1-5使用Plugin.html">1-5 使用Plugin</a> 将教你如何通过 Webpack Plugin 机制来实现。</p>
<blockquote>
<p>本实例 <a href="http://webpack.wuhaolin.cn/1-4使用Loader.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>
<p>给 Loader 传入属性的方式除了有 querystring 外，还可以通过 Object 传入，以上的 Loader 配置可以修改为如下：</p>
<pre><code class="lang-js">use: [
  <span class="hljs-string">&apos;style-loader&apos;</span>, 
  {
    loader:<span class="hljs-string">&apos;css-loader&apos;</span>,
    options:{
      minimize:<span class="hljs-literal">true</span>,
    }
  }
]
</code></pre>
<p>除了在 <code>webpack.config.js</code> 配置文件中配置 Loader 外，还可以在源码中指定用什么 Loader 去处理文件。
以加载 CSS 文件为例，修改上面例子中的 <code>main.js</code> 如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;style-loader!css-loader?minimize!./main.css&apos;</span>);
</code></pre>
<p>这样就能指定对 <code>./main.css</code> 这个文件先采用 css-loader 再采用 style-loader 转换。</p>

                                
                                