<h1 id="3-5-使用-postcss">3-5 使用 PostCSS</h1>
<h2 id="认识-postcss">认识 PostCSS</h2>
<p><a href="http://postcss.org" target="_blank">PostCSS</a> 是一个 CSS 处理工具，和 SCSS 不同的地方在于它通过插件机制可以灵活的扩展其支持的特性，而不是像 SCSS 那样语法是固定的。
PostCSS 的用处非常多，包括给 CSS 自动加前缀、使用下一代 CSS 语法等，目前越来越多的人开始用它，它很可能会成为 CSS 预处理器的最终赢家。</p>
<blockquote>
<p>PostCSS 和 CSS 的关系就像 Babel 和 JavaScript 的关系，它们解除了语法上的禁锢，通过插件机制来扩展语言本身，用工程化手段给语言带来了更多的可能性。</p>
<p>PostCSS 和 SCSS 的关系就像 Babel 和 TypeScript 的关系，PostCSS 更加灵活、可扩张性强，而 SCSS 内置了大量功能而不能扩展。</p>
</blockquote>
<p></p><p>想深入学习 深入 PostCSS，推荐《深入PostCSS Web设计》：</p>
<a href="http://union-click.jd.com/jdc?d=hKtbgE" target="_blank">
<img src="http://img12.360buyimg.com/n1/jfs/t6037/101/5599226387/412098/e3f2d019/596cf567Ncc4bbbb4.jpg">
</a><p></p>
<p>为了更直观的展示 PostCSS，让我们来看一些例子。</p>
<p>给 CSS 自动加前缀，增加各浏览器的兼容性：</p>
<pre><code class="lang-css"><span class="hljs-comment">/*输入*/</span>
<span class="hljs-selector-tag">h1</span> {
  <span class="hljs-attribute">display</span>: flex;
}

<span class="hljs-comment">/*输出*/</span>
<span class="hljs-selector-tag">h1</span> {
  <span class="hljs-attribute">display</span>: -webkit-box;
  <span class="hljs-attribute">display</span>: -webkit-flex;
  <span class="hljs-attribute">display</span>: -ms-flexbox;
  <span class="hljs-attribute">display</span>: flex;
}
</code></pre>
<p>使用下一代 CSS 语法：</p>
<pre><code class="lang-css"><span class="hljs-comment">/*输入*/</span>
<span class="hljs-selector-pseudo">:root</span> {
  <span class="hljs-attribute">--red</span>: <span class="hljs-number">#d33</span>;
}

<span class="hljs-selector-tag">h1</span> {
  <span class="hljs-attribute">color</span>: <span class="hljs-built_in">var</span>(--red);
}


<span class="hljs-comment">/*输出*/</span>
<span class="hljs-selector-tag">h1</span> { 
  <span class="hljs-attribute">color</span>: <span class="hljs-number">#d33</span>;
}
</code></pre>
<p>PostCSS 全部采用 JavaScript 编写，运行在 Node.js 之上，即提供了给 JavaScript 代码调用的模块，也提供了可执行的文件。
在 PostCSS 启动时，会从目录下的 <code>postcss.config.js</code> 文件中读取所需配置，所以需要新建该文件，文件内容大致如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-comment">// 需要使用的插件列表</span>
    <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;postcss-cssnext&apos;</span>)
  ]
}
</code></pre>
<p>其中的 <a href="http://cssnext.io" target="_blank">postcss-cssnext</a> 插件可以让你使用下一代 CSS 语法编写代码，再通过 PostCSS 转换成目前的浏览器可识别的 CSS，并且该插件还包含给 CSS 自动加前缀的功能。</p>
<blockquote>
<p>目前 Chrome 等现代浏览器已经能完全支持 cssnext 中的所有语法，也就是说按照 cssnext 语法写的 CSS 在不经过转换的情况下也能在浏览器中直接运行。 </p>
</blockquote>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>虽然使用 PostCSS 后文件后缀还是 <code>.css</code> 但这些文件必须先交给 <a href="https://github.com/postcss/postcss-loader" target="_blank">postcss-loader</a> 处理一遍后再交给 css-loader。</p>
<p>接入 PostCSS 相关的 Webpack 配置如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 使用 PostCSS 处理 CSS 文件</span>
        test: <span class="hljs-regexp">/\.css/</span>,
        use: [<span class="hljs-string">&apos;style-loader&apos;</span>, <span class="hljs-string">&apos;css-loader&apos;</span>, <span class="hljs-string">&apos;postcss-loader&apos;</span>],
      },
    ]
  },
};
</code></pre>
<p>接入 PostCSS 给项目带来了新的依赖需要安装，如下：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 安装 Webpack Loader 依赖</span>
npm i -D postcss-loader css-loader style-loader
<span class="hljs-comment"># 根据你使用的特性安装对应的 PostCSS 插件依赖</span>
npm i -D postcss-cssnext
</code></pre>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-5使用PostCSS.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                