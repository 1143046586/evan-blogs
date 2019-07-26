<h1 id="3-6-使用-react-框架">3-6 使用 React 框架</h1>
<h2 id="react-语法特征">React 语法特征</h2>
<p>使用了 React 项目的代码特征有 JSX 和 Class 语法，例如：</p>
<pre><code class="lang-jsx"><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Button</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Component</span> </span>{
  render() {
    <span class="hljs-keyword">return</span> <span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>Hello,Webpack<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span></span>
  }
}
</code></pre>
<blockquote>
<p>在使用了 React 的项目里 JSX 和 Class 语法并不是必须的，但使用新语法写出的代码看上去更优雅。</p>
</blockquote>
<p>其中 JSX 语法是无法在任何现有的 JavaScript 引擎中运行的，所以在构建过程中需要把源码转换成可以运行的代码，例如：</p>
<pre><code class="lang-jsx"><span class="hljs-comment">// 原 JSX 语法代码</span>
<span class="hljs-keyword">return</span> <span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>Hello,Webpack<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span></span>

<span class="hljs-comment">// 被转换成正常的 JavaScript 代码</span>
<span class="hljs-keyword">return</span> React.createElement(<span class="hljs-string">&apos;h1&apos;</span>, <span class="hljs-literal">null</span>, <span class="hljs-string">&apos;Hello,Webpack&apos;</span>)
</code></pre>
<p>目前 Babel 和 TypeScript 都提供了对 React 语法的支持，下面分别来介绍如何在使用 Babel 或 TypeScript 的项目中接入 React 框架。</p>
<p></p><p>想深入学习 深入 React，推荐《React 精髓》：</p>
<a href="http://union-click.jd.com/jdc?d=NLTvYa" target="_blank">
<img src="http://img11.360buyimg.com/n1/jfs/t2935/9/345124300/315949/c3e6ecc1/57568847Ndcfccba0.jpg">
</a><p></p>
<h2 id="react-与-babel">React 与 Babel</h2>
<p>要在使用 Babel 的项目中接入 React 框架是很简单的，只需要加入 React 所依赖的 Presets <a href="https://babeljs.io/docs/plugins/preset-react/" target="_blank">babel-preset-react</a>。
接下来通过修改前面讲过的<a href="3-1使用ES6语言.html">3-1 使用 ES6 语言</a>中的项目，为其接入 React 框架。</p>
<p>通过以下命令：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 安装 React 基础依赖</span>
npm i -D react react-dom
<span class="hljs-comment"># 安装 babel 完成语法转换所需依赖</span>
npm i -D babel-preset-react
</code></pre>
<p>安装新的依赖后，再修改 <code>.babelrc</code> 配置文件加入 React Presets</p>
<pre><code class="lang-json"><span class="hljs-string">&quot;presets&quot;</span>: [
    <span class="hljs-string">&quot;react&quot;</span>
],
</code></pre>
<p>就完成了一切准备工作。</p>
<p>再修改 <code>main.js</code> 文件如下：</p>
<pre><code class="lang-jsx"><span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react&apos;</span>;
<span class="hljs-keyword">import</span> { Component } <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react&apos;</span>;
<span class="hljs-keyword">import</span> { render } <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react-dom&apos;</span>;

<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Button</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Component</span> </span>{
  render() {
    <span class="hljs-keyword">return</span> <span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>Hello,Webpack<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span></span>
  }
}

render(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">Button</span>/&gt;</span></span>, <span class="hljs-built_in">window</span>.document.getElementById(<span class="hljs-string">&apos;app&apos;</span>));
</code></pre>
<p>重新执行构建打开网页你将会发现由 React 渲染出来的 <code>Hello,Webpack</code>。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-6使用React框架Babel.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>
<hr>
<h2 id="react-与-typescript">React 与 TypeScript</h2>
<p>TypeScript 相比于 Babel 的优点在于它原生支持 JSX 语法，你不需要重新安装新的依赖，只需修改一行配置。
但 TypeScript 的不同在于：</p>
<ul>
<li>使用了 JSX 语法的文件后缀必须是 <code>tsx</code>。</li>
<li>由于 React 不是采用 TypeScript 编写的，需要安装 <code>react</code> 和 <code>react-dom</code> 对应的 TypeScript 接口描述模块 <code>@types/react</code> 和 <code>@types/react-dom</code> 后才能通过编译。</li>
</ul>
<p>接下来通过修改<a href="3-2使用TypeScript语言.html">3-2 使用 TypeScript 语言</a>中讲过的的项目，为其接入 React 框架。
修改 TypeScript 编译器配置文件 <code>tsconfig.json</code> 增加对 JSX 语法的支持，如下：</p>
<pre><code class="lang-js">{
  <span class="hljs-string">&quot;compilerOptions&quot;</span>: {
    <span class="hljs-string">&quot;jsx&quot;</span>: <span class="hljs-string">&quot;react&quot;</span> <span class="hljs-comment">// 开启 jsx ，支持 React</span>
  }
}
</code></pre>
<p>由于 <code>main.js</code> 文件中存在 JSX 语法，再把 <code>main.js</code> 文件重命名为 <code>main.tsx</code>，同时修改文件内容为在上面 <em>React 与 Babel</em> 里所采用的 React 代码。
同时为了让 Webpack 对项目里的 ts 与 tsx 原文件都采用 <code>awesome-typescript-loader</code> 去转换，
需要注意的是 Webpack Loader 配置的 <code>test</code> 选项需要匹配到 tsx 类型的文件，并且 <code>extensions</code> 中也要加上 <code>.tsx</code>，配置如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// TS 执行入口文件</span>
  entry: <span class="hljs-string">&apos;./main&apos;</span>,
  output: {
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  },
  resolve: {
    <span class="hljs-comment">// 先尝试 ts，tsx 后缀的 TypeScript 源码文件 </span>
    extensions: [<span class="hljs-string">&apos;.ts&apos;</span>, <span class="hljs-string">&apos;.tsx&apos;</span>, <span class="hljs-string">&apos;.js&apos;</span>,] 
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 同时匹配 ts，tsx 后缀的 TypeScript 源码文件 </span>
        test: <span class="hljs-regexp">/\.tsx?$/</span>,
        loader: <span class="hljs-string">&apos;awesome-typescript-loader&apos;</span>
      }
    ]
  },
  devtool: <span class="hljs-string">&apos;source-map&apos;</span>,<span class="hljs-comment">// 输出 Source Map 方便在浏览器里调试 TypeScript 代码</span>
};
</code></pre>
<p>通过</p>
<pre><code class="lang-bash">npm i react react-dom @types/react @types/react-dom
</code></pre>
<p>安装新的依赖后重启构建，重新打开网页你将会发现由 React 渲染出来的 <code>Hello,Webpack</code>。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-6使用React框架TypeScript.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                