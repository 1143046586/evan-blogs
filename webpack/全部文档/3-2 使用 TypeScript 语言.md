<h1 id="3-2-使用-typescript-语言">3-2 使用 TypeScript 语言</h1>
<h2 id="认识-typescript">认识 TypeScript</h2>
<p><a href="http://www.typescriptlang.org" target="_blank">TypeScript</a> 是 JavaScript 的一个超集，主要提供了类型检查系统和对 ES6 语法的支持，但不支持新的 API。
目前没有任何环境支持运行原生的 TypeScript 代码，必须通过构建把它转换成 JavaScript 代码后才能运行。</p>
<p></p><p>想深入学习 TypeScript，推荐《Learning TypeScript中文版》：</p>
<a href="http://union-click.jd.com/jdc?d=R5StrZ" target="_blank">
<img src="http://img13.360buyimg.com/n1/jfs/t7579/323/998195052/27290/9a6b8b9b/59994fb3N86192622.jpg">
</a><p></p>
<p>改造下前面用过的例子 <code>Hello,Webpack</code>，用 TypeScript 重写 JavaScript。由于 TypeScript 是 JavaScript 的超集，直接把后缀 <code>.js</code> 改成 <code>.ts</code> 是可以的。
但为了体现出 TypeScript 的不同，我们重写 JavaScript 代码为如下，加入类型检查：</p>
<pre><code class="lang-typescript"><span class="hljs-comment">// show.ts</span>
<span class="hljs-comment">// 操作 DOM 元素，把 content 显示到网页上</span>
<span class="hljs-comment">// 通过 ES6 模块规范导出 show 函数</span>
<span class="hljs-comment">// 给 show 函数增加类型检查 </span>
<span class="hljs-keyword">export</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">show</span>(<span class="hljs-params">content: <span class="hljs-built_in">string</span></span>) </span>{
  <span class="hljs-built_in">window</span>.document.getElementById(<span class="hljs-string">&apos;app&apos;</span>).innerText = <span class="hljs-string">&apos;Hello,&apos;</span> + content;
}
<span class="hljs-comment">// main.ts</span>
<span class="hljs-comment">// 通过 ES6 模块规范导入 show 函数</span>
<span class="hljs-keyword">import</span> {show} from <span class="hljs-string">&apos;./show&apos;</span>;
<span class="hljs-comment">// 执行 show 函数</span>
show(<span class="hljs-string">&apos;Webpack&apos;</span>);
</code></pre>
<p>TypeScript 官方提供了能把 TypeScript 转换成 JavaScript 的编译器。
你需要在当前项目根目录下新建一个用于配置编译选项的 <code>tsconfig.json</code> 文件，编译器默认会读取和使用这个文件，配置文件内容大致如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;compilerOptions&quot;</span>: {
    <span class="hljs-string">&quot;module&quot;</span>: <span class="hljs-string">&quot;commonjs&quot;</span>, <span class="hljs-comment">// 编译出的代码采用的模块规范</span>
    <span class="hljs-string">&quot;target&quot;</span>: <span class="hljs-string">&quot;es5&quot;</span>, <span class="hljs-comment">// 编译出的代码采用 ES 的哪个版本</span>
    <span class="hljs-string">&quot;sourceMap&quot;</span>: <span class="hljs-literal">true</span> <span class="hljs-comment">// 输出 Source Map 方便调试</span>
  },
  <span class="hljs-string">&quot;exclude&quot;</span>: [ <span class="hljs-comment">// 不编译这些目录里的文件</span>
    <span class="hljs-string">&quot;node_modules&quot;</span>
  ]
}
</code></pre>
<p>通过 <code>npm install -g typescript</code> 安装编译器到全局后，你可以通过 <code>tsc hello.ts</code> 命令编译出 <code>hello.js</code> 和 <code>hello.js.map</code> 文件。</p>
<h2 id="减少代码冗余">减少代码冗余</h2>
<p>TypeScript 编译器会有和在 <a href="3-1使用ES6语言.html">3-1 使用ES6语言</a>中 Babel 一样的问题：在把 ES6 语法转换成 ES5 语法时需要注入辅助函数，
为了不让同样的辅助函数重复的出现在多个文件中，可以开启 TypeScript 编译器的 <code>importHelpers</code> 选项，修改 <code>tsconfig.json</code> 文件如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;compilerOptions&quot;</span>: {
    <span class="hljs-string">&quot;importHelpers&quot;</span>: <span class="hljs-literal">true</span>
  }
}
</code></pre>
<p>该选项的原理和 Babel 中介绍的 <code>babel-plugin-transform-runtime</code> 非常类似，会把辅助函数换成如下导入语句：</p>
<pre><code class="lang-js"><span class="hljs-keyword">var</span> _tslib = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;tslib&apos;</span>);
_tslib._extend(target);
</code></pre>
<p>这会导致编译出的代码依赖 <code>tslib</code> 这个迷你库，但避免了代码冗余。</p>
<h2 id="集成-webpack">集成 Webpack</h2>
<p>要让 Webpack 支持 TypeScript，需要解决以下2个问题：</p>
<ol>
<li>通过 Loader 把 TypeScript 转换成 JavaScript。</li>
<li>Webpack 在寻找模块对应的文件时需要尝试 <code>ts</code> 后缀。</li>
</ol>
<p>对于问题1，社区已经出现了几个可用的 Loader，推荐速度更快的 <a href="https://github.com/s-panferov/awesome-typescript-loader" target="_blank">awesome-typescript-loader</a>。
对于问题2，根据<a href="../2配置/2-4Resolve.html#extensions">2-4 Resolve 中的 extensions</a> 我们需要修改默认的 <code>resolve.extensions</code> 配置项。</p>
<p>综上，相关 Webpack 配置如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// 执行入口文件</span>
  entry: <span class="hljs-string">&apos;./main&apos;</span>,
  output: {
    filename: <span class="hljs-string">&apos;bundle.js&apos;</span>,
    path: path.resolve(__dirname, <span class="hljs-string">&apos;./dist&apos;</span>),
  },
  resolve: {
    <span class="hljs-comment">// 先尝试 ts 后缀的 TypeScript 源码文件</span>
    extensions: [<span class="hljs-string">&apos;.ts&apos;</span>, <span class="hljs-string">&apos;.js&apos;</span>] 
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        test: <span class="hljs-regexp">/\.ts$/</span>,
        loader: <span class="hljs-string">&apos;awesome-typescript-loader&apos;</span>
      }
    ]
  },
  devtool: <span class="hljs-string">&apos;source-map&apos;</span>,<span class="hljs-comment">// 输出 Source Map 方便在浏览器里调试 TypeScript 代码</span>
};
</code></pre>
<p>在运行构建前需要安装上面用到的依赖：</p>
<pre><code class="lang-bash">npm i -D typescript awesome-typescript-loader
</code></pre>
<p>安装成功后重新执行构建，你将会在 <code>dist</code> 目录看到输出的 JavaScript 文件 <code>bundle.js</code>，和对应的 Source Map 文件 <code>bundle.js.map</code>。
在浏览器里打开 <code>index.html</code> 页面后，来开发工具里可以看到和调试用 TypeScript 编写的源码。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-2使用TypeScript语言.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                