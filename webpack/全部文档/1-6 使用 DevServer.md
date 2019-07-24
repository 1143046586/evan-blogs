<h1 id="1-6-使用-devserver">1-6 使用 DevServer</h1>
<p>前面的几节只是让 Webpack 正常运行起来了，但在实际开发中你可能会需要：</p>
<ol>
<li>提供 HTTP 服务而不是使用本地文件预览；</li>
<li>监听文件的变化并自动刷新网页，做到实时预览；</li>
<li>支持 Source Map，以方便调试。</li>
</ol>
<p>对于这些， Webpack 都为你考虑好了。Webpack 原生支持上述第2、3点内容，再结合官方提供的开发工具 <a href="https://webpack.js.org/configuration/dev-server/" target="_blank">DevServer</a> 也可以很方便地做到第1点。
DevServer 会启动一个 HTTP 服务器用于服务网页请求，同时会帮助启动 Webpack ，并接收 Webpack 发出的文件更变信号，通过 WebSocket 协议自动刷新网页做到实时预览。</p>
<p>下面为之前的小项目 <code>Hello,Webpack</code> 继续集成 DevServer。
首先需要安装 DevServer：</p>
<pre><code class="lang-bash">npm i -D webpack-dev-server
</code></pre>
<p>安装成功后执行 <code>webpack-dev-server</code> 命令， DevServer 就启动了，这时你会看到控制台有一串日志输出：</p>
<pre><code>Project is running at http://localhost:8080/
webpack output is served from /
</code></pre><p>这意味着 DevServer 启动的 HTTP 服务器监听在 <code>http://localhost:8080/</code> ，DevServer 启动后会一直驻留在后台保持运行，访问这个网址你就能获取项目根目录下的 <code>index.html</code>。
用浏览器打开这个地址你会发现页面空白错误原因是 <code>./dist/bundle.js</code> 加载404了。
同时你会发现并没有文件输出到 <code>dist</code> 目录，原因是 DevServer 会把 Webpack 构建出的文件保存在内存中，在要访问输出的文件时，必须通过 HTTP 服务访问。
由于 DevServer 不会理会 <code>webpack.config.js</code> 里配置的 <code>output.path</code> 属性，所以要获取 <code>bundle.js</code> 的正确 URL 是 <code>http://localhost:8080/bundle.js</code>，对应的 <code>index.html</code> 应该修改为：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入 DevServer 输出的 JavaScript 文件--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;bundle.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<h2 id="实时预览">实时预览</h2>
<p>接着上面的步骤，你可以试试修改 <code>main.js main.css show.js</code> 中的任何一个文件，保存后你会发现浏览器会被自动刷新，运行出修改后的效果。</p>
<p>Webpack 在启动时可以开启监听模式，开启监听模式后 Webpack 会监听本地文件系统的变化，发生变化时重新构建出新的结果。Webpack 默认是关闭监听模式的，你可以在启动 Webpack 时通过 <code>webpack --watch</code> 来开启监听模式。</p>
<p>通过 DevServer 启动的 Webpack 会开启监听模式，当发生变化时重新执行完构建后通知 DevServer。
DevServer 会让 Webpack 在构建出的 JavaScript 代码里注入一个代理客户端用于控制网页，网页和 DevServer 之间通过 WebSocket 协议通信，
以方便 DevServer 主动向客户端发送命令。
DevServer 在收到来自 Webpack 的文件变化通知时通过注入的客户端控制网页刷新。</p>
<p>如果尝试修改 <code>index.html</code> 文件并保存，你会发现这并不会触发以上机制，导致这个问题的原因是 Webpack 在启动时会以配置里的 <code>entry</code> 为入口去递归解析出 <code>entry</code> 所依赖的文件，只有 <code>entry</code> 本身和依赖的文件才会被 Webpack 添加到监听列表里。
而 <code>index.html</code> 文件是脱离了 JavaScript 模块化系统的，所以 Webpack 不知道它的存在。</p>
<h2 id="模块热替换">模块热替换</h2>
<p>除了通过重新刷新整个网页来实现实时预览，DevServer 还有一种被称作模块热替换的刷新技术。
模块热替换能做到在不重新加载整个网页的情况下，通过将被更新过的模块替换老的模块，再重新执行一次来实现实时预览。
模块热替换相对于默认的刷新机制能提供更快的响应和更好的开发体验。
模块热替换默认是关闭的，要开启模块热替换，你只需在启动 DevServer 时带上 <code>--hot</code> 参数，重启 DevServer 后再去更新文件就能体验到模块热替换的神奇了。</p>
<h2 id="支持-source-map">支持 Source Map</h2>
<p>在浏览器中运行的 JavaScript 代码都是编译器输出的代码，这些代码的可读性很差。如果在开发过程中遇到一个不知道原因的 Bug，则你可能需要通过断点调试去找出问题。
在编译器输出的代码上进行断点调试是一件辛苦和不优雅的事情，
调试工具可以通过 <a href="https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/" target="_blank">Source Map</a> 映射代码，让你在源代码上断点调试。
Webpack 支持生成 Source Map，只需在启动时带上 <code>--devtool source-map</code> 参数。
加上参数重启 DevServer 后刷新页面，再打开 Chrome 浏览器的开发者工具，就可在 Sources 栏中看到可调试的源代码了。</p>
<p><img src="img/1-6source-map.png" alt="图1.6.1 在开发者工具中调试 Source Map"></p>
<blockquote>
<p>本实例 <a href="http://webpack.wuhaolin.cn/1-6使用DevServer.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                