<h1 id="2-2-output">2-2 Output</h1>
<p><code>output</code> 配置如何输出最终想要的代码。<code>output</code> 是一个 <code>object</code>，里面包含一系列配置项，下面分别介绍它们。</p>
<h2 id="filename">filename</h2>
<p><code>output.filename</code> 配置输出文件的名称，为string 类型。
如果只有一个输出文件，则可以把它写成静态不变的：</p>
<pre><code class="lang-js">filename: <span class="hljs-string">&apos;bundle.js&apos;</span>
</code></pre>
<p>但是在有多个 Chunk 要输出时，就需要借助模版和变量了。前面说到 Webpack 会为每个 Chunk取一个名称，可以根据 Chunk 的名称来区分输出的文件名：</p>
<pre><code class="lang-js">filename: <span class="hljs-string">&apos;[name].js&apos;</span>
</code></pre>
<p>代码里的 <code>[name]</code> 代表用内置的 <code>name</code> 变量去替换<code>[name]</code>，这时你可以把它看作一个字符串模块函数，
每个要输出的 Chunk 都会通过这个函数去拼接出输出的文件名称。</p>
<p>内置变量除了 <code>name</code> 还包括：</p>
<table>
<thead>
<tr>
<th>变量名</th>
<th>含义</th>
</tr>
</thead>
<tbody>
<tr>
<td>id</td>
<td>Chunk 的唯一标识，从0开始</td>
</tr>
<tr>
<td>name</td>
<td>Chunk 的名称</td>
</tr>
<tr>
<td>hash</td>
<td>Chunk 的唯一标识的 Hash 值</td>
</tr>
<tr>
<td>chunkhash</td>
<td>Chunk 内容的 Hash 值</td>
</tr>
</tbody>
</table>
<p>其中 <code>hash</code> 和 <code>chunkhash</code> 的长度是可指定的，<code>[hash:8]</code> 代表取8位 Hash 值，默认是20位。</p>
<blockquote>
<p>注意 <a href="https://github.com/webpack-contrib/extract-text-webpack-plugin" target="_blank">ExtractTextWebpackPlugin</a> 插件是使用 <code>contenthash</code> 来代表哈希值而不是 <code>chunkhash</code>，
原因在于 ExtractTextWebpackPlugin 提取出来的内容是代码内容本身而不是由一组模块组成的 Chunk。</p>
</blockquote>
<h2 id="chunkfilename">chunkFilename</h2>
<p><code>output.chunkFilename</code> 配置无入口的 Chunk 在输出时的文件名称。
chunkFilename 和上面的 filename 非常类似，但 chunkFilename 只用于指定在运行过程中生成的 Chunk 在输出时的文件名称。
常见的会在运行时生成 Chunk 场景有在使用 CommonChunkPlugin、使用 <code>import(&apos;path/to/module&apos;)</code> 动态加载等时。
chunkFilename 支持和 filename 一致的内置变量。</p>
<h2 id="path">path</h2>
<p><code>output.path</code> 配置输出文件存放在本地的目录，必须是 string 类型的绝对路径。通常通过 Node.js 的 <code>path</code> 模块去获取绝对路径：</p>
<pre><code class="lang-js">path: path.resolve(__dirname, <span class="hljs-string">&apos;dist_[hash]&apos;</span>)
</code></pre>
<h2 id="publicpath">publicPath</h2>
<p>在复杂的项目里可能会有一些构建出的资源需要异步加载，加载这些异步资源需要对应的 URL 地址。</p>
<p><code>output.publicPath</code> 配置发布到线上资源的 URL 前缀，为string 类型。
默认值是空字符串 <code>&apos;&apos;</code>，即使用相对路径。</p>
<p>这样说可能有点抽象，举个例子，需要把构建出的资源文件上传到 CDN 服务上，以利于加快页面的打开速度。配置代码如下：</p>
<pre><code class="lang-js">filename:<span class="hljs-string">&apos;[name]_[chunkhash:8].js&apos;</span>
publicPath: <span class="hljs-string">&apos;https://cdn.example.com/assets/&apos;</span>
</code></pre>
<p>这时发布到线上的 HTML 在引入 JavaScript 文件时就需要：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&apos;https://cdn.example.com/assets/a_12345678.js&apos;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
</code></pre>
<p>使用该配置项时要小心，稍有不慎将导致资源加载404错误。</p>
<p><code>output.path</code> 和 <code>output.publicPath</code> 都支持字符串模版，内置变量只有一个：<code>hash</code> 代表一次编译操作的 Hash 值。</p>
<h2 id="crossoriginloading">crossOriginLoading</h2>
<p>Webpack 输出的部分代码块可能需要异步加载，而异步加载是通过 <a href="https://zh.wikipedia.org/wiki/JSONP" target="_blank">JSONP</a> 方式实现的。
JSONP 的原理是动态地向 HTML 中插入一个 <code>&lt;script src=&quot;url&quot;&gt;&lt;/script&gt;</code> 标签去加载异步资源。 
<code>output.crossOriginLoading</code> 则是用于配置这个异步插入的标签的 <code>crossorigin</code> 值。</p>
<p>script 标签的 crossorigin 属性可以取以下值：</p>
<ul>
<li><code>anonymous</code>(默认) 在加载此脚本资源时不会带上用户的 Cookies；</li>
<li><code>use-credentials</code> 在加载此脚本资源时会带上用户的 Cookies。</li>
</ul>
<p>通常用设置 crossorigin 来获取异步加载的脚本执行时的详细错误信息。</p>
<h2 id="librarytarget-和-library">libraryTarget 和 library</h2>
<p>当用 Webpack 去构建一个可以被其他模块导入使用的库时需要用到它们。</p>
<ul>
<li><code>output.libraryTarget</code> 配置以何种方式导出库。</li>
<li><code>output.library</code> 配置导出库的名称。</li>
</ul>
<p>它们通常搭配在一起使用。</p>
<p><code>output.libraryTarget</code> 是字符串的枚举类型，支持以下配置。</p>
<h3 id="var-默认">var (默认)</h3>
<p>编写的库将通过 <code>var</code> 被赋值给通过 <code>library</code> 指定名称的变量。</p>
<p>假如配置了 <code>output.library=&apos;LibraryName&apos;</code>，则输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
<span class="hljs-keyword">var</span> LibraryName = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
LibraryName.doSomething();
</code></pre>
<p>假如 <code>output.library</code> 为空，则将直接输出：</p>
<pre><code class="lang-js">lib_code
</code></pre>
<blockquote>
<p>其中 <code>lib_code</code> 代指导出库的代码内容，是有返回值的一个自执行函数。</p>
</blockquote>
<h3 id="commonjs">commonjs</h3>
<p>编写的库将通过 CommonJS 规范导出。</p>
<p>假如配置了 <code>output.library=&apos;LibraryName&apos;</code>，则输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
exports[<span class="hljs-string">&apos;LibraryName&apos;</span>] = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
<span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;library-name-in-npm&apos;</span>)[<span class="hljs-string">&apos;LibraryName&apos;</span>].doSomething();
</code></pre>
<blockquote>
<p>其中 <code>library-name-in-npm</code> 是指模块发布到 Npm 代码仓库时的名称。</p>
</blockquote>
<h3 id="commonjs2">commonjs2</h3>
<p>编写的库将通过 CommonJS2 规范导出，输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
<span class="hljs-built_in">module</span>.exports = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
<span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;library-name-in-npm&apos;</span>).doSomething();
</code></pre>
<blockquote>
<p>CommonJS2 和 CommonJS 规范很相似，差别在于 CommonJS 只能用 <code>exports</code> 导出，而 CommonJS2 在 CommonJS 的基础上增加了 <code>module.exports</code> 的导出方式。</p>
<p>在 <code>output.libraryTarget</code> 为 <code>commonjs2</code> 时，配置 <code>output.library</code> 将没有意义。</p>
</blockquote>
<h3 id="this">this</h3>
<p>编写的库将通过 <code>this</code> 被赋值给通过 <code>library</code> 指定的名称，输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
<span class="hljs-keyword">this</span>[<span class="hljs-string">&apos;LibraryName&apos;</span>] = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
<span class="hljs-keyword">this</span>.LibraryName.doSomething();
</code></pre>
<h3 id="window">window</h3>
<p>编写的库将通过 <code>window</code> 被赋值给通过 <code>library</code> 指定的名称，即把库挂载到 <code>window</code> 上，输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
<span class="hljs-built_in">window</span>[<span class="hljs-string">&apos;LibraryName&apos;</span>] = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
<span class="hljs-built_in">window</span>.LibraryName.doSomething();
</code></pre>
<h3 id="global">global</h3>
<p>编写的库将通过 <code>global</code> 被赋值给通过 <code>library</code> 指定的名称，即把库挂载到 <code>global</code> 上，输出和使用的代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
global[<span class="hljs-string">&apos;LibraryName&apos;</span>] = lib_code;

<span class="hljs-comment">// 使用库的方法</span>
global.LibraryName.doSomething();
</code></pre>
<h2 id="libraryexport">libraryExport</h2>
<p><code>output.libraryExport</code> 配置要导出的模块中哪些子模块需要被导出。
它只有在 <code>output.libraryTarget</code> 被设置成 <code>commonjs</code> 或者 <code>commonjs2</code> 时使用才有意义。</p>
<p>假如要导出的模块源代码是：</p>
<pre><code class="lang-js"><span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> a=<span class="hljs-number">1</span>;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> b=<span class="hljs-number">2</span>;
</code></pre>
<p>现在你想让构建输出的代码只导出其中的 <code>a</code>，可以把 <code>output.libraryExport</code> 设置成 <code>a</code>，那么构建输出的代码和使用方法将变成如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack 输出的代码</span>
<span class="hljs-built_in">module</span>.exports = lib_code[<span class="hljs-string">&apos;a&apos;</span>];

<span class="hljs-comment">// 使用库的方法</span>
<span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;library-name-in-npm&apos;</span>)===<span class="hljs-number">1</span>;
</code></pre>
<blockquote>
<p>以上只是 <code>output</code> 里常用的配置项，还有部分几乎用不上的配置项没有一一列举，你可以在 <a href="https://webpack.js.org/configuration/output/" target="_blank">Webpack 官方文档</a> 上查阅它们。</p>
</blockquote>

                                
                                