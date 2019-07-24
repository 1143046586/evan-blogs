<h1 id="3-4-使用-scss-语言">3-4 使用 SCSS 语言</h1>
<h2 id="认识-scss">认识 SCSS</h2>
<p><a href="http://sass-lang.com" target="_blank">SCSS</a> 可以让你用更灵活的方式写 CSS。
它是一种 CSS 预处理器，语法和 CSS 相似，但加入了变量、逻辑、等编程元素，代码类似这样：</p>
<pre><code class="lang-scss"><span class="hljs-variable">$blue</span>: <span class="hljs-number">#1875e7</span>;　
<span class="hljs-selector-tag">div</span> {
  <span class="hljs-attribute">color</span>: <span class="hljs-variable">$blue</span>;
}
</code></pre>
<blockquote>
<p>SCSS 又叫 SASS，区别在于 SASS 语法类似 Ruby，而 SCSS 语法类似 CSS，对于熟悉 CSS 的前端工程师来说会更喜欢 SCSS。</p>
</blockquote>
<p>采用 SCSS 去写 CSS 的好处在于可以方便地管理代码，抽离公共的部分，通过逻辑写出更灵活的代码。
和 SCSS 类似的 CSS 预处理器还有 <a href="http://lesscss.org" target="_blank">LESS</a> 等。</p>
<p></p><p>想深入学习 SCSS，推荐《Sass与Compass实战》：</p>
<a href="http://union-click.jd.com/jdc?d=MDHQeE" target="_blank">
<img src="http://img11.360buyimg.com/n1/g16/M00/01/01/rBEbRlNrEikIAAAAAAWkTy4WIEwAAANFwMiR2wABaRn525.jpg">
</a><p></p>
<p>使用 SCSS 可以提升编码效率，但是必须把 SCSS 源代码编译成可以直接在浏览器环境下运行的 CSS 代码。
SCSS 官方提供了多种语言实现的编译器，由于本书更倾向于前端工程师使用的技术栈，所以主要来介绍下 <a href="https://github.com/sass/node-sass" target="_blank">node-sass</a>。</p>
<p>node-sass 核心模块是由 C++ 编写，再用 Node.js 封装了一层，以供给其它 Node.js 调用。
node-sass 还支持通过命令行调用，先安装它到全局：</p>
<pre><code class="lang-bash">npm i -g node-sass
</code></pre>
<p>再执行编译命令：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 把 main.scss 源文件编译成 main.css</span>
node-sass main.scss main.css
</code></pre>
<p>你就能在源码同目录下看到编译后的 <code>main.css</code> 文件。</p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>由于需要把 SCSS 源代码转换成 CSS 代码，在<a href="../1入门/1-4使用Loader.html">1-4 使用Loader</a>中曾介绍过转换文件最适合的方式是使用 Loader，Webpack 官方提供了对应的 <a href="https://github.com/webpack-contrib/sass-loader" target="_blank">sass-loader</a>。</p>
<p>Webpack 接入 sass-loader 相关配置如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 增加对 SCSS 文件的支持</span>
        test: <span class="hljs-regexp">/\.scss/</span>,
        <span class="hljs-comment">// SCSS 文件的处理顺序为先 sass-loader 再 css-loader 再 style-loader</span>
        use: [<span class="hljs-string">&apos;style-loader&apos;</span>, <span class="hljs-string">&apos;css-loader&apos;</span>, <span class="hljs-string">&apos;sass-loader&apos;</span>],
      },
    ]
  },
};
</code></pre>
<p>以上配置通过正则 <code>/\.scss/</code> 匹配所有以 <code>.scss</code> 为后缀的 SCSS 文件，再分别使用3个 Loader 去处理。具体处理流程如下：</p>
<ol>
<li>通过 sass-loader 把 SCSS 源码转换为 CSS 代码，再把 CSS 代码交给 css-loader 去处理。</li>
<li>css-loader 会找出 CSS 代码中的 <code>@import</code> 和 <code>url()</code> 这样的导入语句，告诉 Webpack 依赖这些资源。同时还支持 CSS Modules、压缩 CSS 等功能。处理完后再把结果交给 style-loader 去处理。</li>
<li>style-loader 会把 CSS 代码转换成字符串后，注入到 JavaScript 代码中去，通过 JavaScript 去给 DOM 增加样式。如果你想把 CSS 代码提取到一个单独的文件而不是和 JavaScript 混在一起，可以使用<a href="../1入门/1-5使用Plugin.html">1-5 使用Plugin</a> 中介绍过的 ExtractTextPlugin。</li>
</ol>
<p>由于接入 sass-loader，项目需要安装这些新的依赖：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 安装 Webpack Loader 依赖</span>
npm i -D  sass-loader css-loader style-loader
<span class="hljs-comment"># sass-loader 依赖 node-sass</span>
npm i -D node-sass
</code></pre>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-4使用SCSS语言.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                