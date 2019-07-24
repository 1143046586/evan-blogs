<h1 id="4-13-使用-prepack">4-13 使用 Prepack</h1>
<h2 id="认识-prepack">认识 Prepack</h2>
<p>在前面的优化方法中提到了代码压缩和分块，这些都是在网络加载层面的优化，除此之外还可以优化代码在运行时的效率，<a href="https://prepack.io" target="_blank">Prepack</a> 就是为此而生。</p>
<p>Prepack 由 Facebook 开源，它采用较为激进的方法：在保持运行结果一致的情况下，改变源代码的运行逻辑，输出性能更高的 JavaScript 代码。
实际上 Prepack 就是一个部分求值器，编译代码时提前将计算结果放到编译后的代码中，而不是在代码运行时才去求值。</p>
<p>以如下源码为例：</p>
<pre><code class="lang-js"><span class="hljs-keyword">import</span> React, {Component} <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react&apos;</span>;
<span class="hljs-keyword">import</span> {renderToString} <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;react-dom/server&apos;</span>;

<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">hello</span>(<span class="hljs-params">name</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-string">&apos;hello &apos;</span> + name;
}

<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Button</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">Component</span> </span>{
  render() {
    <span class="hljs-keyword">return</span> hello(<span class="hljs-keyword">this</span>.props.name);
  }
}

<span class="hljs-built_in">console</span>.log(renderToString(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">Button</span> <span class="hljs-attr">name</span>=<span class="hljs-string">&apos;webpack&apos;</span>/&gt;</span>));
</span></code></pre>
<p>被 Prepack 转化后竟然直接输出如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">console</span>.log(<span class="hljs-string">&quot;hello webpack&quot;</span>);
</code></pre>
<p>可以看出 Prepack 通过在编译阶段预先执行了源码得到执行结果，再直接把运行结果输出来以提升性能。</p>
<p>Prepack 的工作原理和流程大致如下：</p>
<ol>
<li>通过 Babel 把 JavaScript 源码解析成抽象语法树（AST），以方便更细粒度地分析源码；</li>
<li>Prepack 实现了一个 JavaScript 解释器，用于执行源码。借助这个解释器 Prepack 才能掌握源码具体是如何执行的，并把执行过程中的结果返回到输出中。</li>
</ol>
<p>从表面上看去这似乎非常美好，但实际上 Prepack 还不够成熟与完善。Prepack 目前还处于初期的开发阶段，局限性也很大，例如：</p>
<ul>
<li>不能识别 DOM API 和 部分 Node.js API，如果源码中有调用依赖运行环境的 API 就会导致 Prepack 报错；</li>
<li>存在优化后的代码性能反而更低的情况；</li>
<li>存在优化后的代码文件尺寸大大增加的情况。</li>
</ul>
<p>总之，现在把 Prepack 用于线上环境还为时过早。</p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>Prepack 需要在 Webpack 输出最终的代码之前，对这些代码进行优化，就像 UglifyJS 那样。
因此需要通过新接入一个插件来为 Webpack 接入 Prepack，幸运的是社区中已经有人做好了这个插件：<a href="https://github.com/gajus/prepack-webpack-plugin" target="_blank">prepack-webpack-plugin</a>。</p>
<p>接入该插件非常简单，相关配置代码如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> PrepackWebpackPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;prepack-webpack-plugin&apos;</span>).default;

<span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-keyword">new</span> PrepackWebpackPlugin()
  ]
};
</code></pre>
<p>重新执行构建你就会看到输出的被 Prepack 优化后的代码。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/4-13使用Prepack.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                