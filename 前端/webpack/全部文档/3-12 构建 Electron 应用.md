<h1 id="3-12-构建-electron-应用">3-12 构建 Electron 应用</h1>
<h2 id="认识-electron">认识 Electron</h2>
<p><a href="https://electron.atom.io" target="_blank">Electron</a> 可以让你使用开发 Web 的技术去开发跨平台的桌面端应用，由 Github 主导和开源，大家熟悉的 Atom 和 VSCode 编辑器就是使用 Electron 开发的。</p>
<p>Electron 是 Node.js 和 Chromium 浏览器的结合体，用 Chromium 浏览器显示出的 Web 页面作为应用的 GUI，通过 Node.js 去和操作系统交互。
当你在 Electron 应用中的一个窗口操作时，实际上是在操作一个网页。当你的操作需要通过操作系统去完成时，网页会通过 Node.js 去和操作系统交互。</p>
<p>采用这种方式开发桌面端应用的优点有：</p>
<ul>
<li>降低开发门槛，只需掌握网页开发技术和 Node.js 即可，大量的 Web 开发技术和现成库可以复用于 Electron；</li>
<li>由于 Chromium 浏览器和 Node.js 都是跨平台的，Electron 能做到写一份代码在不同的操作系统运行。</li>
</ul>
<p>在运行 Electron 应用时，会从启动一个主进程开始。主进程的启动是通过 Node.js 去执行一个入口 JavaScript 文件实现的，这个入口文件 <code>main.js</code> 内容如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> { app, BrowserWindow } = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;electron&apos;</span>)

<span class="hljs-comment">// 保持一个对于 window 对象的全局引用，如果你不这样做，</span>
<span class="hljs-comment">// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭</span>
<span class="hljs-keyword">let</span> win

<span class="hljs-comment">// 打开主窗口</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">createWindow</span>(<span class="hljs-params"></span>) </span>{
  <span class="hljs-comment">// 创建浏览器窗口</span>
  win = <span class="hljs-keyword">new</span> BrowserWindow({ width: <span class="hljs-number">800</span>, height: <span class="hljs-number">600</span> })

  <span class="hljs-comment">// 加载应用的 index.html</span>
  <span class="hljs-keyword">const</span> indexPageURL = <span class="hljs-string">`file://<span class="hljs-subst">${__dirname}</span>/dist/index.html`</span>;
  win.loadURL(indexPageURL);

  <span class="hljs-comment">// 当 window 被关闭，这个事件会被触发</span>
  win.on(<span class="hljs-string">&apos;closed&apos;</span>, () =&gt; {
    <span class="hljs-comment">// 取消引用 window 对象</span>
    win = <span class="hljs-literal">null</span>
  })
}

<span class="hljs-comment">// Electron 会在创建浏览器窗口时调用这个函数。</span>
app.on(<span class="hljs-string">&apos;ready&apos;</span>, createWindow)

<span class="hljs-comment">// 当全部窗口关闭时退出</span>
app.on(<span class="hljs-string">&apos;window-all-closed&apos;</span>, () =&gt; {
  <span class="hljs-comment">// 在 macOS 上，除非用户用 Cmd + Q 确定地退出</span>
  <span class="hljs-comment">// 否则绝大部分应用会保持激活</span>
  <span class="hljs-keyword">if</span> (process.platform !== <span class="hljs-string">&apos;darwin&apos;</span>) {
    app.quit()
  }
})
</code></pre>
<p>主进程启动后会一直驻留在后台运行，你眼睛所看得的和操作的窗口并不是主进程，而是由主进程新启动的窗口子进程。</p>
<p>应用从启动到退出有一系列生命周期事件，通过 <code>electron.app.on()</code> 函数去监听生命周期事件，在特定的时刻做出反应。
例如在 <code>app.on(&apos;ready&apos;)</code> 事件中通过 <code>BrowserWindow</code> 去展示应用的主窗口，具体用法见 <a href="https://github.com/electron/electron/blob/master/docs-translations/zh-CN/api/browser-window.md" target="_blank">BrowserWindow 的 API 文档</a>。</p>
<p>启动的窗口其实是一个网页，启动时会去加载在 <code>loadURL</code> 中传入的网页地址。
每个窗口都是一个单独的网页进程，窗口之间的通信需要借助主进程传递消息。</p>
<p><img src="img/3-12electron-arch.png" alt="Electron 应用架构图"></p>
<p>总体来说开发 Electron 应用和开发 Web 应用很相似，区别在于 Electron 的运行环境同时内置了浏览器和 Node.js 的 API，在开发网页时除了可以使用浏览器提供的 API 外，还可以使用 Node.js 提供的 API。</p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>接下来做一个简单的 Electron 应用，要求为应用启动后显示一个主窗口，在主窗口里有一个按钮，点击这个按钮后新显示一个窗口，且使用 React 开发网页。</p>
<p>由于 Electron 应用中的每一个窗口对应一个网页，所以需要开发2个网页，分别是主窗口的 <code>index.html</code> 和新打开的窗口 <code>login.html</code>。
也就是说项目由2个单页应用组成，这和<a href="3-10管理多个单页应用.html">3-10管理多个单页应用</a> 中的项目非常相似，让我们来把它改造成一个 Electron 应用。</p>
<p>需要改动的地方如下：</p>
<ul>
<li>在项目根目录下新建主进程的入口文件 <code>main.js</code>，内容和上面提到的一致；</li>
<li>主窗口网页的代码如下：</li>
</ul>
<pre><code class="lang-js"><span class="hljs-keyword">import</span> React, { Component } <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react&apos;</span>;
<span class="hljs-keyword">import</span> { render } <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react-dom&apos;</span>;
<span class="hljs-keyword">import</span> { remote } <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;electron&apos;</span>;
<span class="hljs-keyword">import</span> path <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;path&apos;</span>;
<span class="hljs-keyword">import</span> <span class="hljs-string">&apos;./index.css&apos;</span>;

<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">App</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Component</span> </span>{

  <span class="hljs-comment">// 在按钮被点击时</span>
  handleBtnClick() {
    <span class="hljs-comment">// 新窗口对应的页面的 URI 地址</span>
    <span class="hljs-keyword">const</span> modalPath = path.join(<span class="hljs-string">&apos;file://&apos;</span>, remote.app.getAppPath(), <span class="hljs-string">&apos;dist/login.html&apos;</span>);
    <span class="hljs-comment">// 新窗口的大小</span>
    <span class="hljs-keyword">let</span> win = <span class="hljs-keyword">new</span> remote.BrowserWindow({ width: <span class="hljs-number">400</span>, height: <span class="hljs-number">320</span> })
    win.on(<span class="hljs-string">&apos;close&apos;</span>, <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) </span>{
      <span class="hljs-comment">// 窗口被关闭时清空资源</span>
      win = <span class="hljs-literal">null</span>
    })
    <span class="hljs-comment">// 加载网页</span>
    win.loadURL(modalPath)
    <span class="hljs-comment">// 显示窗口</span>
    win.show()
  }

  render() {
    <span class="hljs-keyword">return</span> (
      <span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">div</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>Page Index<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">button</span> <span class="hljs-attr">onClick</span>=<span class="hljs-string">{this.handleBtnClick}</span>&gt;</span>Open Page Login<span class="hljs-tag">&lt;/<span class="hljs-name">button</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span></span>
    )
  }
}

render(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">App</span>/&gt;</span></span>, <span class="hljs-built_in">window</span>.document.getElementById(<span class="hljs-string">&apos;app&apos;</span>));
</code></pre>
<p>其中最关键的部分在于在按钮点击事件里通过 <code>electron</code> 库里提供的 API 去新打开一个窗口，并加载网页文件所在的地址。</p>
<p>页面部分的代码已经修改完成，接下来修改构建方面的代码。
这里构建需要做到以下几点：</p>
<ul>
<li>构建出2个可在浏览器里运行的网页，分别对应2个窗口的界面；</li>
<li>由于在网页的 JavaScript 代码里可能会有调用 Node.js 原生模块或者 <code>electron</code> 模块，也就是输出的代码依赖这些模块。但由于这些模块都是内置支持的，构建出的代码不能把这些模块打包进去。</li>
</ul>
<p>要完成以上要求非常简单，因为 Webpack 内置了对 Electron 的支持。
只需要给 Webpack 配置文件加上一行代码即可，如下：</p>
<pre><code class="lang-js">target: <span class="hljs-string">&apos;electron-renderer&apos;</span>,
</code></pre>
<p>这句配置曾在<a href="../2配置/2-7其它配置项.html#Target">2-7其它配置项-Target</a>中提到，意思是指让 Webpack 构建出用于 Electron 渲染进程用的 JavaScript 代码，也就是这2个窗口需要的网页代码。</p>
<p>以上修改都完成后重新执行 Webpack 构建，对应的网页需要的代码都输出到了项目根目录下的 <code>dist</code> 目录里。</p>
<p>为了以 Electron 应用的形式运行，还需要安装新依赖：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 安装 Electron 执行环境到项目中</span>
npm i -D electron
</code></pre>
<p>安装成功后在项目目录下执行 <code>electron .</code> 你就能成功看到启动的桌面应用了，效果如图：</p>
<p><img src="img/3-12electron-app.png" alt="Electron 应用运行效果图"></p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-12构建Electron应用.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                