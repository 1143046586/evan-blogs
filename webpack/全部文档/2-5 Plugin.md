<h1 id="2-5-plugin">2-5 Plugin</h1>
<p>Plugin 用于扩展 Webpack 功能，各种各样的 Plugin 几乎让 Webpack 可以做任何构建相关的事情。</p>
<h2 id="配置-plugin">配置 Plugin</h2>
<p>Plugin 的配置很简单，<code>plugins</code> 配置项接受一个数组，数组里每一项都是一个要使用的 Plugin 的实例，Plugin 需要的参数通过构造函数传入。</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> CommonsChunkPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/optimize/CommonsChunkPlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-comment">// 所有页面都会用到的公共代码提取到 common 代码块中</span>
    <span class="hljs-keyword">new</span> CommonsChunkPlugin({
      name: <span class="hljs-string">&apos;common&apos;</span>,
      chunks: [<span class="hljs-string">&apos;a&apos;</span>, <span class="hljs-string">&apos;b&apos;</span>]
    }),
  ]
};
</code></pre>
<p>使用 Plugin 的难点在于掌握 Plugin 本身提供的配置项，而不是如何在 Webpack 中接入 Plugin。</p>
<p>几乎所有 Webpack 无法直接实现的功能都能在社区找到开源的 Plugin 去解决，你需要善于使用搜索引擎去寻找解决问题的方法。</p>

                                
                                