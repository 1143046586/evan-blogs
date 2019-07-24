<h1 id="4-4-使用-paralleluglifyplugin">4-4 使用 ParallelUglifyPlugin</h1>
<p>在使用 Webpack 构建出用于发布到线上的代码时，都会有压缩代码这一流程。
最常见的 JavaScript 代码压缩工具是 <a href="https://github.com/mishoo/UglifyJS2" target="_blank">UglifyJS</a>，并且 Webpack 也内置了它。</p>
<p>用过 UglifyJS 的你一定会发现在构建用于开发环境的代码时很快就能完成，但在构建用于线上的代码时构建一直卡在一个时间点迟迟没有反应，其实卡住的这个时候就是在进行代码压缩。</p>
<p>由于压缩 JavaScript 代码需要先把代码解析成用 Object 抽象表示的 AST 语法树，再去应用各种规则分析和处理 AST，导致这个过程计算量巨大，耗时非常多。</p>
<p>为什么不把在<a href="4-3使用HappyPack.html">4-3 使用 HappyPack</a>中介绍过的多进程并行处理的思想也引入到代码压缩中呢？</p>
<p><a href="https://github.com/gdborton/webpack-parallel-uglify-plugin" target="_blank">ParallelUglifyPlugin</a> 就做了这个事情。
当 Webpack 有多个 JavaScript 文件需要输出和压缩时，原本会使用 UglifyJS 去一个个挨着压缩再输出，
但是 ParallelUglifyPlugin 则会开启多个子进程，把对多个文件的压缩工作分配给多个子进程去完成，每个子进程其实还是通过 UglifyJS 去压缩代码，但是变成了并行执行。
所以 ParallelUglifyPlugin 能更快的完成对多个文件的压缩工作。</p>
<p>使用 ParallelUglifyPlugin 也非常简单，把原来 Webpack 配置文件中内置的 UglifyJsPlugin 去掉后，再替换成 ParallelUglifyPlugin，相关代码如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> DefinePlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/DefinePlugin&apos;</span>);
<span class="hljs-keyword">const</span> ParallelUglifyPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack-parallel-uglify-plugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-comment">// 使用 ParallelUglifyPlugin 并行压缩输出的 JS 代码</span>
    <span class="hljs-keyword">new</span> ParallelUglifyPlugin({
      <span class="hljs-comment">// 传递给 UglifyJS 的参数</span>
      uglifyJS: {
        output: {
          <span class="hljs-comment">// 最紧凑的输出</span>
          beautify: <span class="hljs-literal">false</span>,
          <span class="hljs-comment">// 删除所有的注释</span>
          comments: <span class="hljs-literal">false</span>,
        },
        compress: {
          <span class="hljs-comment">// 在UglifyJs删除没有用到的代码时不输出警告</span>
          warnings: <span class="hljs-literal">false</span>,
          <span class="hljs-comment">// 删除所有的 `console` 语句，可以兼容ie浏览器</span>
          drop_console: <span class="hljs-literal">true</span>,
          <span class="hljs-comment">// 内嵌定义了但是只用到一次的变量</span>
          collapse_vars: <span class="hljs-literal">true</span>,
          <span class="hljs-comment">// 提取出出现多次但是没有定义成变量去引用的静态值</span>
          reduce_vars: <span class="hljs-literal">true</span>,
        }
      },
    }),
  ],
};
</code></pre>
<p>在通过 <code>new ParallelUglifyPlugin()</code> 实例化时，支持以下参数：</p>
<ul>
<li><code>test</code>：使用正则去匹配哪些文件需要被 ParallelUglifyPlugin 压缩，默认是 <code>/.js$/</code>，也就是默认压缩所有的 .js 文件。</li>
<li><code>include</code>：使用正则去命中需要被 ParallelUglifyPlugin 压缩的文件。默认为 <code>[]</code>。</li>
<li><code>exclude</code>：使用正则去命中不需要被 ParallelUglifyPlugin 压缩的文件。默认为 <code>[]</code>。</li>
<li><code>cacheDir</code>：缓存压缩后的结果，下次遇到一样的输入时直接从缓存中获取压缩后的结果并返回。cacheDir 用于配置缓存存放的目录路径。默认不会缓存，想开启缓存请设置一个目录路径。</li>
<li><code>workerCount</code>：开启几个子进程去并发的执行压缩。默认是当前运行电脑的 CPU 核数减去1。</li>
<li><code>sourceMap</code>：是否输出 Source Map，这会导致压缩过程变慢。</li>
<li><code>uglifyJS</code>：用于压缩 ES5 代码时的配置，Object 类型，直接透传给 UglifyJS 的参数。</li>
<li><code>uglifyES</code>：用于压缩 ES6 代码时的配置，Object 类型，直接透传给 UglifyES 的参数。</li>
</ul>
<p>其中的 <code>test</code>、<code>include</code>、<code>exclude</code> 与配置 Loader 时的思想和用法一样。</p>
<blockquote>
<p><a href="https://github.com/mishoo/UglifyJS2/tree/harmony" target="_blank">UglifyES</a> 是 UglifyJS 的变种，专门用于压缩 ES6 代码，它们两都出自于同一个项目，并且它们两不能同时使用。</p>
<p>UglifyES 一般用于给比较新的 JavaScript 运行环境压缩代码，例如用于 ReactNative 的代码运行在兼容性较好的 JavaScriptCore 引擎中，为了得到更好的性能和尺寸，采用 UglifyES 压缩效果会更好。</p>
<p>ParallelUglifyPlugin 同时内置了 UglifyJS 和 UglifyES，也就是说 ParallelUglifyPlugin 支持并行压缩 ES6 代码。</p>
</blockquote>
<p>接入 ParallelUglifyPlugin 后，项目需要安装新的依赖：</p>
<pre><code class="lang-bash">npm i -D webpack-parallel-uglify-plugin
</code></pre>
<p>安装成功后，重新执行构建你会发现速度变快了许多。如果设置 <code>cacheDir</code> 开启了缓存，在之后的构建中会变的更快。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/4-4使用ParallelUglifyPlugin.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                