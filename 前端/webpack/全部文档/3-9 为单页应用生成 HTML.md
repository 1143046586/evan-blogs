<h1 id="3-9-为单页应用生成-html">3-9 为单页应用生成 HTML</h1>
<h2 id="引入问题">引入问题</h2>
<p>在<a href="3-6使用React框架.html">3-6 使用 React 框架</a>中，是用最简单的 <code>Hello,Webpack</code> 作为例子让大家理解，
这个例子里因为只输出了一个 <code>bundle.js</code> 文件，所以手写了一个 <code>index.html</code> 文件去引入这个 <code>bundle.js</code>，才能让应用在浏览器中运行起来。</p>
<p>在实际项目中远比这复杂，一个页面常常有很多资源要加载。接下来举一个实战中的例子，要求如下：</p>
<ol>
<li>项目采用 ES6 语言加 React 框架。</li>
<li>给页面加入 <a href="https://analytics.google.com/analytics/web/" target="_blank">Google Analytics</a>，这部分代码需要内嵌进 HEAD 标签里去。</li>
<li>给页面加入 <a href="https://disqus.com" target="_blank">Disqus</a> 用户评论，这部分代码需要异步加载以提升首屏加载速度。</li>
<li>压缩和分离 JavaScript 和 CSS 代码，提升加载速度。</li>
</ol>
<p>在开始前先来看看该应用最终发布到线上的代码：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
  <span class="hljs-comment">&lt;!--注入 Chunk app 依赖的 CSS--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">style</span> <span class="hljs-attr">rel</span>=<span class="hljs-string">&quot;stylesheet&quot;</span>&gt;</span><span class="css"><span class="hljs-selector-tag">h1</span>{<span class="hljs-attribute">color</span>:red}</span><span class="hljs-tag">&lt;/<span class="hljs-name">style</span>&gt;</span>
  <span class="hljs-comment">&lt;!--内嵌 google_analytics 中的 JavaScript 代码--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">script</span>&gt;</span><span class="javascript">
(<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">i,s,o,g,r,a,m</span>)</span>{i[<span class="hljs-string">&apos;GoogleAnalyticsObject&apos;</span>]=r;i[r]=i[r]||<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
(i[r].q=i[r].q||[]).push(<span class="hljs-built_in">arguments</span>)},i[r].l=<span class="hljs-number">1</span>*<span class="hljs-keyword">new</span> <span class="hljs-built_in">Date</span>();a=s.createElement(o),
m=s.getElementsByTagName(o)[<span class="hljs-number">0</span>];a.async=<span class="hljs-number">1</span>;a.src=g;m.parentNode.insertBefore(a,m)
})(<span class="hljs-built_in">window</span>,<span class="hljs-built_in">document</span>,<span class="hljs-string">&apos;script&apos;</span>,<span class="hljs-string">&apos;https://www.google-analytics.com/analytics.js&apos;</span>,<span class="hljs-string">&apos;ga&apos;</span>);
ga(<span class="hljs-string">&apos;create&apos;</span>, <span class="hljs-string">&apos;UA-XXXXX-Y&apos;</span>, <span class="hljs-string">&apos;auto&apos;</span>);
ga(<span class="hljs-string">&apos;send&apos;</span>, <span class="hljs-string">&apos;pageview&apos;</span>);
  </span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
  <span class="hljs-comment">&lt;!--异步加载 Disqus 评论--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">async</span>=<span class="hljs-string">&quot;&quot;</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;https://dive-into-webpack.disqus.com/embed.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入 app 依赖的 JS--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;app_746f32b2.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-comment">&lt;!--Disqus 评论容器--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;disqus_thread&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<blockquote>
<p>HTML 应该是被压缩过的，这里为了方便大家阅读而格式化了 HTML，并且加入了注释。</p>
</blockquote>
<p>构建出的目录结构为：</p>
<pre><code>dist
├── app_792b446e.js
└── index.html
</code></pre><p>可以看到部分代码被内嵌进了 HTML 的 HEAD 标签中，部分文件的文件名称被打上根据文件内容算出的 Hash 值，并且加载这些文件的 URL 地址也被正常的注入到了 HTML 中。
如果你还采用手写 <code>index.html</code> 文件去完成以上要求，这就会使工作变得复杂、易错，项目难以维护。
本节教你如何自动化的生成这个符合要求的 <code>index.html</code>。</p>
<h2 id="解决方案">解决方案</h2>
<p>推荐一个用于方便的解决以上问题的 Webpack 插件 <a href="https://github.com/gwuhaolin/web-webpack-plugin" target="_blank">web-webpack-plugin</a>。
该插件已经被社区上许多人使用和验证，解决了大家的痛点获得了很多好评，下面具体介绍如何用它来解决上面的问题。</p>
<p>首先，修改 Webpack 配置为如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> UglifyJsPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/optimize/UglifyJsPlugin&apos;</span>);
<span class="hljs-keyword">const</span> ExtractTextPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;extract-text-webpack-plugin&apos;</span>);
<span class="hljs-keyword">const</span> DefinePlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/DefinePlugin&apos;</span>);
<span class="hljs-keyword">const</span> { WebPlugin } = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;web-webpack-plugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  entry: {
    app: <span class="hljs-string">&apos;./main.js&apos;</span><span class="hljs-comment">// app 的 JavaScript 执行入口文件</span>
  },
  output: {
    filename: <span class="hljs-string">&apos;[name]_[chunkhash:8].js&apos;</span>,<span class="hljs-comment">// 给输出的文件名称加上 Hash 值</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        test: <span class="hljs-regexp">/\.js$/</span>,
        use: [<span class="hljs-string">&apos;babel-loader&apos;</span>],
        <span class="hljs-comment">// 排除 node_modules 目录下的文件，</span>
        <span class="hljs-comment">// 该目录下的文件都是采用的 ES5 语法，没必要再通过 Babel 去转换</span>
        exclude: path.resolve(__dirname, <span class="hljs-string">&apos;node_modules&apos;</span>),
      },
      {
        test: <span class="hljs-regexp">/\.css/</span>,<span class="hljs-comment">// 增加对 CSS 文件的支持</span>
        <span class="hljs-comment">// 提取出 Chunk 中的 CSS 代码到单独的文件中</span>
        use: ExtractTextPlugin.extract({
          use: [<span class="hljs-string">&apos;css-loader?minimize&apos;</span>] <span class="hljs-comment">// 压缩 CSS 代码</span>
        }),
      },
    ]
  },
  plugins: [
    <span class="hljs-comment">// 使用本文的主角 WebPlugin，一个 WebPlugin 对应一个 HTML 文件</span>
    <span class="hljs-keyword">new</span> WebPlugin({
      template: <span class="hljs-string">&apos;./template.html&apos;</span>, <span class="hljs-comment">// HTML 模版文件所在的文件路径</span>
      filename: <span class="hljs-string">&apos;index.html&apos;</span> <span class="hljs-comment">// 输出的 HTML 的文件名称</span>
    }),
    <span class="hljs-keyword">new</span> ExtractTextPlugin({
      filename: <span class="hljs-string">`[name]_[contenthash:8].css`</span>,<span class="hljs-comment">// 给输出的 CSS 文件名称加上 Hash 值</span>
    }),
    <span class="hljs-keyword">new</span> DefinePlugin({
      <span class="hljs-comment">// 定义 NODE_ENV 环境变量为 production，以去除源码中只有开发时才需要的部分</span>
      <span class="hljs-string">&apos;process.env&apos;</span>: {
        NODE_ENV: <span class="hljs-built_in">JSON</span>.stringify(<span class="hljs-string">&apos;production&apos;</span>)
      }
    }),
    <span class="hljs-comment">// 压缩输出的 JavaScript 代码</span>
    <span class="hljs-keyword">new</span> UglifyJsPlugin({
      <span class="hljs-comment">// 最紧凑的输出</span>
      beautify: <span class="hljs-literal">false</span>,
      <span class="hljs-comment">// 删除所有的注释</span>
      comments: <span class="hljs-literal">false</span>,
      compress: {
        <span class="hljs-comment">// 在UglifyJs删除没有用到的代码时不输出警告</span>
        warnings: <span class="hljs-literal">false</span>,
        <span class="hljs-comment">// 删除所有的 `console` 语句，可以兼容ie浏览器</span>
        drop_console: <span class="hljs-literal">true</span>,
        <span class="hljs-comment">// 内嵌定义了但是只用到一次的变量</span>
        collapse_vars: <span class="hljs-literal">true</span>,
        <span class="hljs-comment">// 提取出出现多次但是没有定义成变量去引用的静态值</span>
        reduce_vars: <span class="hljs-literal">true</span>,
      }
    }),
  ],
};
</code></pre>
<p>以上配置中，大多数都是按照前面已经讲过的内容增加的配置，例如：</p>
<ul>
<li>增加对 CSS 文件的支持，提取出 Chunk 中的 CSS 代码到单独的文件中，压缩 CSS 文件；</li>
<li>定义 <code>NODE_ENV</code> 环境变量为 <code>production</code>，以去除源码中只有开发时才需要的部分；</li>
<li>给输出的文件名称加上 Hash 值；</li>
<li>压缩输出的 JavaScript 代码。</li>
</ul>
<p>但最核心的部分在于 <code>plugins</code> 里的：</p>
<pre><code class="lang-js"><span class="hljs-keyword">new</span> WebPlugin({
  template: <span class="hljs-string">&apos;./template.html&apos;</span>, <span class="hljs-comment">// HTML 模版文件所在的文件路径</span>
  filename: <span class="hljs-string">&apos;index.html&apos;</span> <span class="hljs-comment">// 输出的 HTML 的文件名称</span>
})
</code></pre>
<p>其中 <code>template: &apos;./template.html&apos;</code> 所指的模版文件 <code>template.html</code> 的内容是：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
  <span class="hljs-comment">&lt;!--注入 Chunk app 中的 CSS--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">link</span> <span class="hljs-attr">rel</span>=<span class="hljs-string">&quot;stylesheet&quot;</span> <span class="hljs-attr">href</span>=<span class="hljs-string">&quot;app?_inline&quot;</span>&gt;</span>
  <span class="hljs-comment">&lt;!--注入 google_analytics 中的 JavaScript 代码--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./google_analytics.js?_inline&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
  <span class="hljs-comment">&lt;!--异步加载 Disqus 评论--&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;https://dive-into-webpack.disqus.com/embed.js&quot;</span> <span class="hljs-attr">async</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入 Chunk app 中的 JS--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-comment">&lt;!--Disqus 评论容器--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;disqus_thread&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<p>该文件描述了哪些资源需要被以何种方式加入到输出的 HTML 文件中。</p>
<p>以 <code>&lt;link rel=&quot;stylesheet&quot; href=&quot;app?_inline&quot;&gt;</code> 为例，按照正常引入 CSS 文件一样的语法来引入 Webpack 生产的代码。
<code>href</code> 属性中的 <code>app?_inline</code> 可以分为两部分，前面的 <code>app</code> 表示 CSS 代码来自名叫 <code>app</code> 的 Chunk 中，后面的 <code>_inline</code> 表示这些代码需要被内嵌到这个标签所在的位置。</p>
<p>同样的 <code>&lt;script src=&quot;./google_analytics.js?_inline&quot;&gt;&lt;/script&gt;</code> 表示 JavaScript 代码来自相对于当前模版文件 <code>template.html</code> 的本地文件 <code>./google_analytics.js</code>，
而且文件中的 JavaScript 代码也需要被内嵌到这个标签所在的位置。</p>
<p>也就是说资源链接 URL 字符串里问号前面的部分表示资源内容来自哪里，后面的 querystring 表示这些资源注入的方式。</p>
<p>除了 <code>_inline</code> 表示内嵌外，还支持以下属性：</p>
<ul>
<li><code>_dist</code> 只有在生产环境下才引入该资源</li>
<li><code>_dev</code> 只有在开发环境下才引入该资源</li>
<li><code>_ie</code> 只有IE浏览器才需要引入的资源，通过 <code>[if IE]&gt;resource&lt;![endif]</code> 注释实现</li>
</ul>
<p>这些属性之间可以搭配使用，互不冲突。例如 <code>app?_inline&amp;_dist</code> 表示只在生产环境下才引入该资源，并且需要内嵌到 HTML 里去。</p>
<p><code>WebPlugin</code> 插件还支持一些其它更高级的用法，详情可以访问该<a href="https://github.com/gwuhaolin/web-webpack-plugin" target="_blank">项目主页</a>阅读文档。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-9为单页应用生成HTML.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                