<h1 id="3-17-通过-nodejs-api-启动-webpack">3-17 通过 Node.js API 启动 Webpack</h1>
<p>Webpack 除了提供可执行的命令行工具外，还提供可在 Node.js 环境中调用的库。
通过 Webpack 暴露的 API，可直接在 Node.js 程序中调用 Webpack 执行构建。</p>
<p></p><p>想深入学习 深入 Node.js，推荐《Node.js实战》：</p>
<a href="http://union-click.jd.com/jdc?d=wFYZhH" target="_blank">
<img src="http:////img12.360buyimg.com/n1/g16/M00/00/1B/rBEbRlNq7V8IAAAAAAYFS_3OMQsAAAKwAFddy0ABgVj894.jpg">
</a><p></p>
<p>通过 API 去调用并执行 Webpack 比直接通过可执行文件启动更加灵活，可用在一些特殊场景，下面将教你如何使用 Webpack 提供的 API。</p>
<blockquote>
<p>Webpack 其实是一个 Node.js 应用程序，它全部通过 JavaScript 开发完成。
在命令行中执行 <code>webpack</code> 命令其实等价于执行 <code>node ./node_modules/webpack/bin/webpack.js</code>。</p>
</blockquote>
<h2 id="安装和使用-webpack-模块">安装和使用 Webpack 模块</h2>
<p>在调用 Webpack API 前，需要先安装它：</p>
<pre><code class="lang-bash">npm i -D webpack
</code></pre>
<p>安装成功后，可以采用以下代码导入 Webpack 模块：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> webpack = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack&apos;</span>);

<span class="hljs-comment">// ES6 语法</span>
<span class="hljs-keyword">import</span> webpack <span class="hljs-keyword">from</span> <span class="hljs-string">&quot;webpack&quot;</span>;
</code></pre>
<p>导出的 <code>webpack</code> 其实是一个函数，使用方法如下：</p>
<pre><code class="lang-js">webpack({
  <span class="hljs-comment">// Webpack 配置，和 webpack.config.js 文件一致</span>
}, (err, stats) =&gt; {
  <span class="hljs-keyword">if</span> (err || stats.hasErrors()) {
    <span class="hljs-comment">// 构建过程出错</span>
  }
  <span class="hljs-comment">// 成功执行完构建</span>
});
</code></pre>
<p>如果你的 Webpack 配置写在 <code>webpack.config.js</code> 文件中，可以这样使用：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 读取 webpack.config.js 文件中的配置</span>
<span class="hljs-keyword">const</span> config = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./webpack.config.js&apos;</span>);
webpack(config , callback);
</code></pre>
<h2 id="以监听模式运行">以监听模式运行</h2>
<p>以上使用 Webpack API 的方法只能执行一次构建，无法以监听模式启动 Webpack，为了在使用 API 时以监听模式启动，需要获取 Compiler 实例，方法如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 如果不传 callback 回调函数，就会返回一个 Compiler 实例，用于让你去控制启动，而不是像上面那样立即启动</span>
<span class="hljs-keyword">const</span> compiler = webpack(config);

<span class="hljs-comment">// 调用 compiler.watch 以监听模式启动，返回的 watching 用于关闭监听</span>
<span class="hljs-keyword">const</span> watching = compiler.watch({
  <span class="hljs-comment">// watchOptions</span>
  aggregateTimeout: <span class="hljs-number">300</span>,
},(err, stats)=&gt;{
  <span class="hljs-comment">// 每次因文件发生变化而重新执行完构建后</span>
});

<span class="hljs-comment">// 调用 watching.close 关闭监听 </span>
watching.close(()=&gt;{
  <span class="hljs-comment">// 在监听关闭后</span>
});
</code></pre>
<p>其中的 watchOptions 就是在 <a href="../2配置/2-7其它配置项.html">2-7 其它配置项</a> 中介绍过的 <em>Watch 和 WatchOptions</em>。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-17通过Node.jsAPI启动Webpack.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                