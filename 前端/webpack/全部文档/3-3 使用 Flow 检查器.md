<h1 id="3-3-使用-flow-检查器">3-3 使用 Flow 检查器</h1>
<h2 id="认识-flow">认识 Flow</h2>
<p><a href="https://flow.org" target="_blank">Flow</a> 是一个 Facebook 开源的 JavaScript 静态类型检测器，它是 JavaScript 语言的超集。
你所需要做的就是在需要的地方加上类型检查，例如在两个由不同人开发的模块对接的接口出加上静态类型检查，能在编译阶段就指出部分模块使用不当的问题。
同时 Flow 也能通过类型推断检查出 JavaScript 代码中潜在的 Bug。</p>
<p>Flow 使用效果如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// @flow</span>

<span class="hljs-comment">// 静态类型检查</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">square1</span>(<span class="hljs-params">n: number</span>): <span class="hljs-title">number</span> </span>{
  <span class="hljs-keyword">return</span> n * n;
}
square1(<span class="hljs-string">&apos;2&apos;</span>); <span class="hljs-comment">// Error: square1 需要传入 number 作为参数</span>

<span class="hljs-comment">// 类型推断检查</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">square2</span>(<span class="hljs-params">n</span>) </span>{
  <span class="hljs-keyword">return</span> n * n; <span class="hljs-comment">// Error: 传入的 string 类型不能做乘法运算</span>
}
square2(<span class="hljs-string">&apos;2&apos;</span>);
</code></pre>
<blockquote>
<p>需要注意的时代码中的第一行 <code>// @flow</code> 告诉 Flow 检查器这个文件需要被检查。</p>
</blockquote>
<h2 id="使用-flow">使用 Flow</h2>
<p>以上只是让你了解 Flow 的功能，下面教你如何运行 Flow 去检查代码。
Flow 检测器由高性能跨平台的 <a href="http://ocaml.org" target="_blank">OCaml</a> 语言编写，它的可执行文件可以通过</p>
<pre><code class="lang-bash">npm i -D flow-bin
</code></pre>
<p>安装，安装完成后通过先配置 Npm Script </p>
<pre><code class="lang-json"><span class="hljs-string">&quot;scripts&quot;</span>: {
   <span class="hljs-string">&quot;flow&quot;</span>: <span class="hljs-string">&quot;flow&quot;</span>
}
</code></pre>
<p>再通过 <code>npm run flow</code> 去调用 Flow 执行代码检查。</p>
<p>除此之外你还可以通过</p>
<pre><code class="lang-bash">npm i -g flow-bin
</code></pre>
<p>把 Flow 安装到全局后，再直接通过 <code>flow</code> 命令去执行代码检查。</p>
<p>安装成功后，在项目根目录下执行 Flow 后，Flow 会遍历出所有需要检查的文件并对其进行检查，输出错误结果到控制台，例如：</p>
<pre><code>Error: show.js:6
  6: export function show(content) {
                          ^^^^^^^ parameter `content`. Missing annotation

Found 1 error
</code></pre><p>采用了 Flow 静态类型语法的 JavaScript 是无法直接在目前已有的 JavaScript 引擎中运行的，要让代码可以运行需要把这些静态类型语法去掉。
例如：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 采用 Flow 的源代码</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">foo</span>(<span class="hljs-params">one: any, two: number, three?</span>): <span class="hljs-title">string</span> </span>{}

<span class="hljs-comment">// 去掉静态类型语法后输出代码</span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">foo</span>(<span class="hljs-params">one, two, three</span>) </span>{}
</code></pre>
<p>有两种方式可以做到这点：</p>
<ol>
<li><a href="https://github.com/flowtype/flow-remove-types" target="_blank">flow-remove-types</a> 可单独使用，速度快。</li>
<li><a href="https://babeljs.io/docs/plugins/preset-flow/" target="_blank">babel-preset-flow</a> 与 Babel 集成。</li>
</ol>
<h2 id="集成-webpack">集成 Webpack</h2>
<p>由于使用了 Flow 项目一般都会使用 ES6 语法，所以把 Flow 集成到使用 Webpack 构建的项目里最方便的方法是借助 Babel，
下面来修改在<a href="3-1使用ES6语言.html">3-1 使用 ES6 语言</a>中使用的项目，为其加入 Flow 代码检查。
改动很少，如下：</p>
<ol>
<li>安装 <code>npm i -D babel-preset-flow</code> 依赖到项目。</li>
<li>修改 <code>.babelrc</code> 配置文件，加入 Flow Preset：<pre><code class="lang-js"><span class="hljs-string">&quot;presets&quot;</span>: [
...[],
<span class="hljs-string">&quot;flow&quot;</span>
]
</code></pre>
往源码里加入静态类型后重新构建项目，你会发现采用了 Flow 的源码还是能正常在浏览器中运行。</li>
</ol>
<blockquote>
<p>要明确构建的目的只是为了去除源码中的 Flow 静态类型语法，而代码检查和构建无关。
许多编辑器已经整合 Flow，可以实时在代码中高亮指出 Flow 检查出的问题。</p>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-3使用Flow检查器.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                