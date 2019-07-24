<h1 id="4-14-开启-scope-hoisting">4-14 开启 Scope Hoisting</h1>
<p>Scope Hoisting 可以让 Webpack 打包出来的代码文件更小、运行的更快，
它又译作 &quot;作用域提升&quot;，是在 Webpack3 中新推出的功能。
单从名字上看不出 Scope Hoisting 到底做了什么，下面来详细介绍它。</p>
<h2 id="认识-scope-hoisting">认识 Scope Hoisting</h2>
<p>让我们先来看看在没有 Scope Hoisting 之前 Webpack 的打包方式。</p>
<p>假如现在有两个文件分别是 <code>util.js</code>:</p>
<pre><code class="lang-js"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> <span class="hljs-string">&apos;Hello,Webpack&apos;</span>;
</code></pre>
<p>和入口文件 <code>main.js</code>:</p>
<pre><code class="lang-js"><span class="hljs-keyword">import</span> str <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;./util.js&apos;</span>;
<span class="hljs-built_in">console</span>.log(str);
</code></pre>
<p>以上源码用 Webpack 打包后输出中的部分代码如下：</p>
<pre><code class="lang-js">[
  (<span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">module, __webpack_exports__, __webpack_require__</span>) </span>{
    <span class="hljs-keyword">var</span> __WEBPACK_IMPORTED_MODULE_0__util_js__ = __webpack_require__(<span class="hljs-number">1</span>);
    <span class="hljs-built_in">console</span>.log(__WEBPACK_IMPORTED_MODULE_0__util_js__[<span class="hljs-string">&quot;a&quot;</span>]);
  }),
  (<span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">module, __webpack_exports__, __webpack_require__</span>) </span>{
    __webpack_exports__[<span class="hljs-string">&quot;a&quot;</span>] = (<span class="hljs-string">&apos;Hello,Webpack&apos;</span>);
  })
]
</code></pre>
<p>在开启 Scope Hoisting 后，同样的源码输出的部分代码如下：</p>
<pre><code class="lang-js">[
  (<span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">module, __webpack_exports__, __webpack_require__</span>) </span>{
    <span class="hljs-keyword">var</span> util = (<span class="hljs-string">&apos;Hello,Webpack&apos;</span>);
    <span class="hljs-built_in">console</span>.log(util);
  })
]
</code></pre>
<p>从中可以看出开启 Scope Hoisting 后，函数申明由两个变成了一个，<code>util.js</code> 中定义的内容被直接注入到了 <code>main.js</code> 对应的模块中。
这样做的好处是：</p>
<ul>
<li>代码体积更小，因为函数申明语句会产生大量代码；</li>
<li>代码在运行时因为创建的函数作用域更少了，内存开销也随之变小。</li>
</ul>
<p>Scope Hoisting 的实现原理其实很简单：分析出模块之间的依赖关系，尽可能的把打散的模块合并到一个函数中去，但前提是不能造成代码冗余。
因此只有那些被引用了一次的模块才能被合并。</p>
<p>由于 Scope Hoisting 需要分析出模块之间的依赖关系，因此源码必须采用 ES6 模块化语句，不然它将无法生效。
原因和<a href="4-10使用TreeShaking.html">4-10 使用 TreeShaking</a> 中介绍的类似。</p>
<h2 id="使用-scope-hoisting">使用 Scope Hoisting</h2>
<p>要在 Webpack 中使用 Scope Hoisting 非常简单，因为这是 Webpack 内置的功能，只需要配置一个插件，相关代码如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> ModuleConcatenationPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/optimize/ModuleConcatenationPlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-comment">// 开启 Scope Hoisting</span>
    <span class="hljs-keyword">new</span> ModuleConcatenationPlugin(),
  ],
};
</code></pre>
<p>同时，考虑到 Scope Hoisting 依赖源码需采用 ES6 模块化语法，还需要配置 <code>mainFields</code>。
原因在 <a href="4-10使用TreeShaking.html">4-10 使用 TreeShaking</a> 中提到过：因为大部分 Npm 中的第三方库采用了 CommonJS 语法，但部分库会同时提供 ES6 模块化的代码，为了充分发挥
Scope Hoisting 的作用，需要增加以下配置：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  resolve: {
    <span class="hljs-comment">// 针对 Npm 中的第三方模块优先采用 jsnext:main 中指向的 ES6 模块化语法的文件</span>
    mainFields: [<span class="hljs-string">&apos;jsnext:main&apos;</span>, <span class="hljs-string">&apos;browser&apos;</span>, <span class="hljs-string">&apos;main&apos;</span>]
  },
};
</code></pre>
<p>对于采用了非 ES6 模块化语法的代码，Webpack 会降级处理不使用 Scope Hoisting 优化，为了知道 Webpack 对哪些代码做了降级处理，
你可以在启动 Webpack 时带上 <code>--display-optimization-bailout</code> 参数，这样在输出日志中就会包含类似如下的日志：</p>
<pre><code>[0] ./main.js + 1 modules 80 bytes {0} [built]
    ModuleConcatenation bailout: Module is not an ECMAScript module
</code></pre><p>其中的 <code>ModuleConcatenation bailout</code> 告诉了你哪个文件因为什么原因导致了降级处理。</p>
<p>也就是说要开启 Scope Hoisting 并发挥最大作用的配置如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> ModuleConcatenationPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/optimize/ModuleConcatenationPlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  resolve: {
    <span class="hljs-comment">// 针对 Npm 中的第三方模块优先采用 jsnext:main 中指向的 ES6 模块化语法的文件</span>
    mainFields: [<span class="hljs-string">&apos;jsnext:main&apos;</span>, <span class="hljs-string">&apos;browser&apos;</span>, <span class="hljs-string">&apos;main&apos;</span>]
  },
  plugins: [
    <span class="hljs-comment">// 开启 Scope Hoisting</span>
    <span class="hljs-keyword">new</span> ModuleConcatenationPlugin(),
  ],
};
</code></pre>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/4-14开启ScopeHoisting.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                