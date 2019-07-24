<h1 id="3-18-使用-webpack-dev-middleware">3-18 使用 Webpack Dev Middleware</h1>
<p>在 <a href="../1入门/1-6使用DevServer.html">1-6 使用 DevServer</a> 中介绍过的 DevServer 是一个方便开发的小型 HTTP 服务器，
DevServer 其实是基于 <a href="https://github.com/webpack/webpack-dev-middleware" target="_blank">webpack-dev-middleware</a> 和 <a href="https://expressjs.com" target="_blank">Expressjs</a> 实现的，
而 webpack-dev-middleware 其实是 Expressjs 的一个中间件。</p>
<p>也就是说，实现 DevServer 基本功能的代码大致如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> express = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;express&apos;</span>);
<span class="hljs-keyword">const</span> webpack = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack&apos;</span>);
<span class="hljs-keyword">const</span> webpackMiddleware = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack-dev-middleware&apos;</span>);

<span class="hljs-comment">// 从 webpack.config.js 文件中读取 Webpack 配置 </span>
<span class="hljs-keyword">const</span> config = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./webpack.config.js&apos;</span>);
<span class="hljs-comment">// 实例化一个 Expressjs app</span>
<span class="hljs-keyword">const</span> app = express();

<span class="hljs-comment">// 用读取到的 Webpack 配置实例化一个 Compiler</span>
<span class="hljs-keyword">const</span> compiler = webpack(config);
<span class="hljs-comment">// 给 app 注册 webpackMiddleware 中间件</span>
app.use(webpackMiddleware(compiler));
<span class="hljs-comment">// 启动 HTTP 服务器，服务器监听在 3000 端口 </span>
app.listen(<span class="hljs-number">3000</span>);
</code></pre>
<p>从以上代码可以看出，从 <code>webpack-dev-middleware</code> 中导出的 <code>webpackMiddleware</code> 是一个函数，该函数需要接收一个 Compiler 实例。
在 <a href="3-17通过Node.jsAPI启动Webpack.html">3-17 通过 Node.js API 启动 Webpack</a> 中曾介绍过 Webpack API 导出的 <code>webpack</code> 函数会返回一个Compiler 实例。</p>
<p><code>webpackMiddleware</code> 函数的返回结果是一个 Expressjs 的中间件，该中间件有以下功能：</p>
<ul>
<li>接收来自 Webpack Compiler 实例输出的文件，但不会把文件输出到硬盘，而是保存在内存中；</li>
<li>往 Expressjs app 上注册路由，拦截 HTTP 收到的请求，根据请求路径响应对应的文件内容；</li>
</ul>
<p>通过 webpack-dev-middleware 能够将 DevServer 集成到你现有的 HTTP 服务器中，让你现有的 HTTP 服务器能返回 Webpack 构建出的内容，而不是在开发时启动多个 HTTP 服务器。
这特别适用于后端接口服务采用 Node.js 编写的项目。 </p>
<h2 id="webpack-dev-middleware-支持的配置项">Webpack Dev Middleware 支持的配置项</h2>
<p>在 Node.js 中调用 webpack-dev-middleware 提供的 API 时，还可以给它传入一些配置项，方法如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// webpackMiddleware 函数的第二个参数为配置项</span>
app.use(webpackMiddleware(compiler, {
    <span class="hljs-comment">// webpack-dev-middleware 所有支持的配置项</span>
    <span class="hljs-comment">// 只有 publicPath 属性为必填，其它都是选填项</span>
    <span class="hljs-comment">// Webpack 输出资源绑定在 HTTP 服务器上的根目录，</span>
    <span class="hljs-comment">// 和 Webpack 配置中的 publicPath 含义一致 </span>
    publicPath: <span class="hljs-string">&apos;/assets/&apos;</span>,
    <span class="hljs-comment">// 不输出 info 类型的日志到控制台，只输出 warn 和 error 类型的日志</span>
    noInfo: <span class="hljs-literal">false</span>,
    <span class="hljs-comment">// 不输出任何类型的日志到控制台</span>
    quiet: <span class="hljs-literal">false</span>,
    <span class="hljs-comment">// 切换到懒惰模式，这意味着不监听文件变化，只会在请求到时再去编译对应的文件，</span>
    <span class="hljs-comment">// 这适合页面非常多的项目。</span>
    lazy: <span class="hljs-literal">true</span>,
    <span class="hljs-comment">// watchOptions</span>
    <span class="hljs-comment">// 只在非懒惰模式下才有效</span>
    watchOptions: {
        aggregateTimeout: <span class="hljs-number">300</span>,
        poll: <span class="hljs-literal">true</span>
    },
    <span class="hljs-comment">// 默认的 URL 路径, 默认是 &apos;index.html&apos;.</span>
    index: <span class="hljs-string">&apos;index.html&apos;</span>,
    <span class="hljs-comment">// 自定义 HTTP 头</span>
    headers: {<span class="hljs-string">&apos;X-Custom-Header&apos;</span>: <span class="hljs-string">&apos;yes&apos;</span>},
    <span class="hljs-comment">// 给特定文件后缀的文件添加 HTTP mimeTypes ，作为文件类型映射表</span>
    mimeTypes: {<span class="hljs-string">&apos;text/html&apos;</span>: [<span class="hljs-string">&apos;phtml&apos;</span>]},
    <span class="hljs-comment">// 统计信息输出样式</span>
    stats: {
        colors: <span class="hljs-literal">true</span>
    },
    <span class="hljs-comment">// 自定义输出日志的展示方法</span>
    reporter: <span class="hljs-literal">null</span>,
    <span class="hljs-comment">// 开启或关闭服务端渲染</span>
    serverSideRender: <span class="hljs-literal">false</span>,
}));
</code></pre>
<h2 id="webpack-dev-middleware-与模块热替换">Webpack Dev Middleware 与模块热替换</h2>
<p>DevServer 提供了一个方便的功能，可以做到在监听到文件发生变化时自动替换网页中的老模块，以做到实时预览。
DevServer 虽然是基于 webpack-dev-middleware 实现的，但 webpack-dev-middleware 并没有实现模块热替换功能，而是 DevServer 自己实现了该功能。</p>
<p>为了在使用 webpack-dev-middleware 时也能使用模块热替换功能去提升开发效率，需要额外的再接入 <a href="https://github.com/glenjamin/webpack-hot-middleware" target="_blank">webpack-hot-middleware</a>。
需要做以下修改才能实现模块热替换。</p>
<p>第2步：修改 <code>webpack.config.js</code> 文件，加入 <code>HotModuleReplacementPlugin</code> 插件，修改如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> HotModuleReplacementPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/HotModuleReplacementPlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  entry: [
    <span class="hljs-comment">// 为了支持模块热替换，注入代理客户端</span>
    <span class="hljs-string">&apos;webpack-hot-middleware/client&apos;</span>,
    <span class="hljs-comment">// JS 执行入口文件</span>
    <span class="hljs-string">&apos;./src/main.js&apos;</span>
  ],
  output: {
    <span class="hljs-comment">// 把所有依赖的模块合并输出到一个 bundle.js 文件</span>
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
  },
  plugins: [
    <span class="hljs-comment">// 为了支持模块热替换，生成 .hot-update.json 文件</span>
    <span class="hljs-keyword">new</span> HotModuleReplacementPlugin(),
  ],
  devtool: <span class="hljs-string">&apos;source-map&apos;</span>,
};
</code></pre>
<p>该修改其实就是相当于完成了在 <a href="../4优化/4-6开启模块热替换.html">4-6 开启模块热替换</a> 中提到的 <code>webpack-dev-server --hot</code> 的工作。</p>
<p>第1步：修改 HTTP 服务器代码 <code>server.js</code> 文件，接入 <code>webpack-hot-middleware</code> 中间件，修改如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> express = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;express&apos;</span>);
<span class="hljs-keyword">const</span> webpack = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack&apos;</span>);
<span class="hljs-keyword">const</span> webpackMiddleware = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack-dev-middleware&apos;</span>);

<span class="hljs-comment">// 从 webpack.config.js 文件中读取 Webpack 配置</span>
<span class="hljs-keyword">const</span> config = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./webpack.config.js&apos;</span>);
<span class="hljs-comment">// 实例化一个 Expressjs app</span>
<span class="hljs-keyword">const</span> app = express();

<span class="hljs-comment">// 用读取到的 Webpack 配置实例化一个 Compiler</span>
<span class="hljs-keyword">const</span> compiler = webpack(config);
<span class="hljs-comment">// 给 app 注册 webpackMiddleware 中间件</span>
app.use(webpackMiddleware(compiler));
<span class="hljs-comment">// 为了支持模块热替换，响应用于替换老模块的资源</span>
app.use(<span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack-hot-middleware&apos;</span>)(compiler));
<span class="hljs-comment">// 把项目根目录作为静态资源目录，用于服务 HTML 文件</span>
app.use(express.static(<span class="hljs-string">&apos;.&apos;</span>));
<span class="hljs-comment">// 启动 HTTP 服务器，服务器监听在 3000 端口</span>
app.listen(<span class="hljs-number">3000</span>, () =&gt; {
  <span class="hljs-built_in">console</span>.info(<span class="hljs-string">&apos;成功监听在 3000&apos;</span>);
});
</code></pre>
<p>第3步：修改执行入口文件 <code>main.js</code>，加入替换逻辑，在文件末尾加入以下代码：</p>
<pre><code class="lang-js"><span class="hljs-keyword">if</span> (<span class="hljs-built_in">module</span>.hot) {
  <span class="hljs-built_in">module</span>.hot.accept();
}
</code></pre>
<p>第4步：安装新引入的依赖：</p>
<pre><code class="lang-bash">npm i -D webpack-dev-middleware webpack-hot-middleware express
</code></pre>
<p>安装成功后，通过 <code>node ./server.js</code> 就能启动一个类似于 DevServer 那样支持模块热替换的自定义 HTTP 服务了。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-18使用WebpackDevMiddleware.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                