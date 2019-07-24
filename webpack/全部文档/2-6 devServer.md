<h1 id="2-6-devserver">2-6 devServer</h1>
<p><a href="../1入门/1-6使用DevServer.html">1-6 使用DevServer</a> 介绍过用来提高开发效率的 DevServer ，它提供了一些配置项可以改变 DevServer 的默认行为。
要配置 DevServer ，除了在配置文件里通过 <code>devServer</code> 传入参数外，还可以通过命令行参数传入。
注意只有在通过 DevServer 去启动 Webpack 时配置文件里 <code>devServer</code> 才会生效，因为这些参数所对应的功能都是 DevServer 提供的，Webpack 本身并不认识 <code>devServer</code> 配置项。</p>
<h2 id="hot">hot</h2>
<p><code>devServer.hot</code> 配置是否启用 <a href="../1入门/1-6使用DevServer.html#模块热替换">1-6 使用DevServer</a> 中提到的模块热替换功能。
DevServer 默认的行为是在发现源代码被更新后会通过自动刷新整个页面来做到实时预览，开启模块热替换功能后将在不刷新整个页面的情况下通过用新模块替换老模块来做到实时预览。</p>
<h2 id="inline">inline</h2>
<p>DevServer 的实时预览功能依赖一个注入到页面里的代理客户端去接受来自 DevServer 的命令和负责刷新网页的工作。
<code>devServer.inline</code> 用于配置是否自动注入这个代理客户端到将运行在页面里的 Chunk 里去，默认是会自动注入。
DevServer 会根据你是否开启 <code>inline</code> 来调整它的自动刷新策略：</p>
<ul>
<li>如果开启 <code>inline</code>，DevServer 会在构建完变化后的代码时通过代理客户端控制网页刷新。</li>
<li>如果关闭 <code>inline</code>，DevServer 将无法直接控制要开发的网页。这时它会通过 iframe 的方式去运行要开发的网页，当构建完变化后的代码时通过刷新 iframe 来实现实时预览。
但这时你需要去 <code>http://localhost:8080/webpack-dev-server/</code> 实时预览你的网页了。</li>
</ul>
<p>如果你想使用 DevServer 去自动刷新网页实现实时预览，最方便的方法是直接开启 <code>inline</code>。</p>
<h2 id="historyapifallback">historyApiFallback</h2>
<p><code>devServer.historyApiFallback</code> 用于方便的开发使用了 <a href="https://developer.mozilla.org/en-US/docs/Web/API/History" target="_blank">HTML5 History API</a> 的单页应用。
这类单页应用要求服务器在针对任何命中的路由时都返回一个对应的 HTML 文件，例如在访问 <code>http://localhost/user</code> 和 <code>http://localhost/home</code> 时都返回 <code>index.html</code> 文件，
浏览器端的 JavaScript 代码会从 URL 里解析出当前页面的状态，显示出对应的界面。</p>
<p>配置 <code>historyApiFallback</code> 最简单的做法是：</p>
<pre><code class="lang-js">historyApiFallback: <span class="hljs-literal">true</span>
</code></pre>
<p>这会导致任何请求都会返回 <code>index.html</code> 文件，这只能用于只有一个 HTML 文件的应用。</p>
<p>如果你的应用由多个单页应用组成，这就需要 DevServer 根据不同的请求来返回不同的 HTML 文件，配置如下：</p>
<pre><code class="lang-js">historyApiFallback: {
  <span class="hljs-comment">// 使用正则匹配命中路由</span>
  rewrites: [
    <span class="hljs-comment">// /user 开头的都返回 user.html</span>
    { <span class="hljs-keyword">from</span>: <span class="hljs-regexp">/^\/user/</span>, to: <span class="hljs-string">&apos;/user.html&apos;</span> },
    { <span class="hljs-keyword">from</span>: <span class="hljs-regexp">/^\/game/</span>, to: <span class="hljs-string">&apos;/game.html&apos;</span> },
    <span class="hljs-comment">// 其它的都返回 index.html</span>
    { <span class="hljs-keyword">from</span>: <span class="hljs-regexp">/./</span>, to: <span class="hljs-string">&apos;/index.html&apos;</span> },
  ]
}
</code></pre>
<h2 id="contentbase">contentBase</h2>
<p><code>devServer.contentBase</code> 配置 DevServer HTTP 服务器的文件根目录。
默认情况下为当前执行目录，通常是项目根目录，所有一般情况下你不必设置它，除非你有额外的文件需要被 DevServer 服务。
例如你想把项目根目录下的 <code>public</code> 目录设置成 DevServer 服务器的文件根目录，你可以这样配置：</p>
<pre><code class="lang-js">devServer:{
  contentBase: path.join(__dirname, <span class="hljs-string">&apos;public&apos;</span>)
}
</code></pre>
<p>这里需要指出可能会让你疑惑的地方，DevServer 服务器通过 HTTP 服务暴露出的文件分为两类：</p>
<ul>
<li>暴露本地文件。</li>
<li>暴露 Webpack 构建出的结果，由于构建出的结果交给了 DevServer，所以你在使用了 DevServer 时在本地找不到构建出的文件。</li>
</ul>
<p><code>contentBase</code> 只能用来配置暴露本地文件的规则，你可以通过 <code>contentBase:false</code> 来关闭暴露本地文件。</p>
<h2 id="headers">headers</h2>
<p><code>devServer.headers</code> 配置项可以在 HTTP 响应中注入一些 HTTP 响应头，使用如下：</p>
<pre><code class="lang-js">devServer:{
  headers: {
    <span class="hljs-string">&apos;X-foo&apos;</span>:<span class="hljs-string">&apos;bar&apos;</span>
  }
}
</code></pre>
<h2 id="host">host</h2>
<p><code>devServer.host</code> 配置项用于配置 DevServer 服务监听的地址。
例如你想要局域网中的其它设备访问你本地的服务，可以在启动 DevServer 时带上 <code>--host 0.0.0.0</code>。
<code>host</code> 的默认值是 <code>127.0.0.1</code> 即只有本地可以访问 DevServer 的 HTTP 服务。</p>
<h2 id="port">port</h2>
<p><code>devServer.port</code> 配置项用于配置 DevServer 服务监听的端口，默认使用 8080 端口。
如果 8080 端口已经被其它程序占有就使用 8081，如果 8081 还是被占用就使用 8082，以此类推。</p>
<h2 id="allowedhosts">allowedHosts</h2>
<p><code>devServer.allowedHosts</code> 配置一个白名单列表，只有 HTTP 请求的 HOST 在列表里才正常返回，使用如下：</p>
<pre><code class="lang-js">allowedHosts: [
  <span class="hljs-comment">// 匹配单个域名</span>
  <span class="hljs-string">&apos;host.com&apos;</span>,
  <span class="hljs-string">&apos;sub.host.com&apos;</span>,
  <span class="hljs-comment">// host2.com 和所有的子域名 *.host2.com 都将匹配</span>
  <span class="hljs-string">&apos;.host2.com&apos;</span>
]
</code></pre>
<h2 id="disablehostcheck">disableHostCheck</h2>
<p><code>devServer.disableHostCheck</code> 配置项用于配置是否关闭用于 DNS 重绑定的 HTTP 请求的 HOST 检查。
DevServer 默认只接受来自本地的请求，关闭后可以接受来自任何 HOST 的请求。
它通常用于搭配 <code>--host 0.0.0.0</code> 使用，因为你想要其它设备访问你本地的服务，但访问时是直接通过 IP 地址访问而不是 HOST 访问，所以需要关闭 HOST 检查。</p>
<h2 id="https">https</h2>
<p>DevServer 默认使用 HTTP 协议服务，它也能通过 HTTPS 协议服务。
有些情况下你必须使用 HTTPS，例如 HTTP2 和 Service Worker 就必须运行在 HTTPS 之上。
要切换成 HTTPS 服务，最简单的方式是：</p>
<pre><code class="lang-js">devServer:{
  https: <span class="hljs-literal">true</span>
}
</code></pre>
<p>DevServer 会自动的为你生成一份 HTTPS 证书。</p>
<p>如果你想用自己的证书可以这样配置：</p>
<pre><code class="lang-js">devServer:{
  https: {
    key: fs.readFileSync(<span class="hljs-string">&apos;path/to/server.key&apos;</span>),
    cert: fs.readFileSync(<span class="hljs-string">&apos;path/to/server.crt&apos;</span>),
    ca: fs.readFileSync(<span class="hljs-string">&apos;path/to/ca.pem&apos;</span>)
  }
}
</code></pre>
<h2 id="clientloglevel">clientLogLevel</h2>
<p><code>devServer.clientLogLevel</code> 配置在客户端的日志等级，这会影响到你在浏览器开发者工具控制台里看到的日志内容。
<code>clientLogLevel</code> 是枚举类型，可取如下之一的值 <code>none | error | warning | info</code>。
默认为 <code>info</code> 级别，即输出所有类型的日志，设置成 <code>none</code> 可以不输出任何日志。</p>
<h2 id="compress">compress</h2>
<p><code>devServer.compress</code> 配置是否启用 gzip 压缩。<code>boolean</code> 为类型，默认为 <code>false</code>。</p>
<h2 id="open">open</h2>
<p><code>devServer.open</code> 用于在 DevServer 启动且第一次构建完时自动用你系统上默认的浏览器去打开要开发的网页。
同时还提供 <code>devServer.openPage</code> 配置项用于打开指定 URL 的网页。</p>

                                
                                