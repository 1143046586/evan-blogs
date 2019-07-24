<h1 id="1-3-安装-webpack">1-3 安装 Webpack</h1>
<p>在用 Webpack 执行构建任务时需要通过 <code>webpack</code> 可执行文件去启动构建任务，所以需要安装 <code>webpack</code> 可执行文件。
在安装 Webpack 前请确保你的系统安装了5.0.0及以上版本的 <a href="https://nodejs.org" target="_blank">Node.js</a>。</p>
<p>在开始给项目加入构建前，你需要先新建一个 Web 项目，方式包括：</p>
<ul>
<li>新建一个目录，再进入项目根目录执行 <code>npm init</code> 来初始化最简单的采用了模块化开发的项目；</li>
<li>用脚手架工具 <a href="http://yeoman.io" target="_blank">Yeoman</a> 直接快速地生成一个最符合你的需求的项目。</li>
</ul>
<h2 id="安装-webpack-到本项目">安装 Webpack 到本项目</h2>
<p>要安装 Webpack 到本项目，可按照你的需要选择以下任意命令运行：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># npm i -D 是 npm install --save-dev 的简写，是指安装模块并保存到 package.json 的 devDependencies</span>
<span class="hljs-comment"># 安装最新稳定版</span>
npm i -D webpack

<span class="hljs-comment"># 安装指定版本</span>
npm i -D webpack@&lt;version&gt;

<span class="hljs-comment"># 安装最新体验版本</span>
npm i -D webpack@beta
</code></pre>
<p>安装完后你可以通过这些途径运行安装到本项目的 Webpack：</p>
<ul>
<li>在项目根目录下对应的命令行里通过 <code>node_modules/.bin/webpack</code> 运行 Webpack 可执行文件。</li>
<li><p>在 <a href="常见的构建工具及对比/npm_script.md">Npm Script</a> 里定义的任务会优先使用本项目下的 Webpack，代码如下：</p>
<pre><code class="lang-json"><span class="hljs-string">&quot;scripts&quot;</span>: {
    <span class="hljs-string">&quot;start&quot;</span>: <span class="hljs-string">&quot;webpack --config webpack.config.js&quot;</span>
}
</code></pre>
</li>
</ul>
<h2 id="安装-webpack-到全局">安装 Webpack 到全局</h2>
<p>安装到全局后你可以在任何地方共用一个 Webpack 可执行文件，而不用各个项目重复安装，安装方式如下：</p>
<pre><code class="lang-bash">npm i -g webpack
</code></pre>
<p>虽然介绍了以上两种安装方式，但是我们推荐安装到本项目，原因是可防止不同项目依赖不同版本的 Webpack 而导致冲突。</p>
<h2 id="使用-webpack">使用 Webpack</h2>
<p>下面通过 Webpack 构建一个采用 CommonJS 模块化编写的项目，该项目有个网页会通过 JavaScript 在网页中显示 <code>Hello,Webpack</code>。</p>
<p>运行构建前，先把要完成该功能的最基础的 JavaScript 文件和 HTML 建立好，需要如下文件：</p>
<p>页面入口文件 <code>index.html</code></p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入 Webpack 输出的 JavaScript 文件--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./dist/bundle.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<p>JS 工具函数文件 <code>show.js</code></p>
<pre><code class="lang-js"><span class="hljs-comment">// 操作 DOM 元素，把 content 显示到网页上</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">show</span>(<span class="hljs-params">content</span>) </span>{
  <span class="hljs-built_in">window</span>.document.getElementById(<span class="hljs-string">&apos;app&apos;</span>).innerText = <span class="hljs-string">&apos;Hello,&apos;</span> + content;
}

<span class="hljs-comment">// 通过 CommonJS 规范导出 show 函数</span>
<span class="hljs-built_in">module</span>.exports = show;
</code></pre>
<p>JS 执行入口文件 <code>main.js</code></p>
<pre><code class="lang-js"><span class="hljs-comment">// 通过 CommonJS 规范导入 show 函数</span>
<span class="hljs-keyword">const</span> show = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./show.js&apos;</span>);
<span class="hljs-comment">// 执行 show 函数</span>
show(<span class="hljs-string">&apos;Webpack&apos;</span>);
</code></pre>
<p>Webpack 在执行构建时默认会从项目根目录下的 <code>webpack.config.js</code> 文件读取配置，所以你还需要新建它，其内容如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// JavaScript 执行入口文件</span>
  entry: <span class="hljs-string">&apos;./main.js&apos;</span>,
  output: {
    <span class="hljs-comment">// 把所有依赖的模块合并输出到一个 bundle.js 文件</span>
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
    <span class="hljs-comment">// 输出文件都放到 dist 目录下</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  }
};
</code></pre>
<p>由于 Webpack 构建运行在 Node.js 环境下，所以该文件最后需要通过 CommonJS 规范导出一个描述如何构建的 <code>Object</code> 对象。</p>
<p>此时项目目录如下：</p>
<pre><code>|-- index.html
|-- main.js
|-- show.js
|-- webpack.config.js
</code></pre><p>一切文件就绪，在项目根目录下执行 <code>webpack</code> 命令运行 Webpack 构建，你会发现目录下多出一个 <code>dist</code> 目录，里面有个 <code>bundle.js</code> 文件，
<code>bundle.js</code> 文件是一个可执行的 JavaScript 文件，它包含页面所依赖的两个模块 <code>main.js</code> 和 <code>show.js</code> 及内置的 <code>webpackBootstrap</code> 启动函数。
这时你用浏览器打开 <code>index.html</code> 网页将会看到 <code>Hello,Webpack</code>。</p>
<p>Webpack 是一个打包模块化 JavaScript 的工具，它会从 <code>main.js</code> 出发，识别出源码中的模块化导入语句，
递归的寻找出入口文件的所有依赖，把入口和其所有依赖打包到一个单独的文件中。
从 Webpack2 开始，已经内置了对 ES6、CommonJS、AMD 模块化语句的支持。</p>
<p>至此你已经学会了 Webpack，接下来我们将探索 Webpack 的更多功能。</p>
<blockquote>
<p>本实例 <a href="http://webpack.wuhaolin.cn/1-3安装与使用.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                