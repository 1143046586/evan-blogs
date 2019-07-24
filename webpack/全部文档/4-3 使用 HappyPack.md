<h1 id="4-3-使用-happypack">4-3 使用 HappyPack</h1>
<p>由于有大量文件需要解析和处理，构建是文件读写和计算密集型的操作，特别是当文件数量变多后，Webpack 构建慢的问题会显得严重。
运行在 Node.js 之上的 Webpack 是单线程模型的，也就是说 Webpack 需要处理的任务需要一件件挨着做，不能多个事情一起做。</p>
<p>文件读写和计算操作是无法避免的，那能不能让 Webpack 同一时刻处理多个任务，发挥多核 CPU 电脑的威力，以提升构建速度呢？</p>
<p><a href="https://github.com/amireh/happypack" target="_blank">HappyPack</a> 就能让 Webpack 做到这点，它把任务分解给多个子进程去并发的执行，子进程处理完后再把结果发送给主进程。</p>
<blockquote>
<p>由于 JavaScript 是单线程模型，要想发挥多核 CPU 的能力，只能通过多进程去实现，而无法通过多线程实现。</p>
</blockquote>
<h2 id="使用-happypack">使用 HappyPack</h2>
<p>分解任务和管理线程的事情 HappyPack 都会帮你做好，你所需要做的只是接入 HappyPack。
接入 HappyPack 的相关代码如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> ExtractTextPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;extract-text-webpack-plugin&apos;</span>);
<span class="hljs-keyword">const</span> HappyPack = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;happypack&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        test: <span class="hljs-regexp">/\.js$/</span>,
        <span class="hljs-comment">// 把对 .js 文件的处理转交给 id 为 babel 的 HappyPack 实例</span>
        use: [<span class="hljs-string">&apos;happypack/loader?id=babel&apos;</span>],
        <span class="hljs-comment">// 排除 node_modules 目录下的文件，node_modules 目录下的文件都是采用的 ES5 语法，没必要再通过 Babel 去转换</span>
        exclude: path.resolve(__dirname, <span class="hljs-string">&apos;node_modules&apos;</span>),
      },
      {
        <span class="hljs-comment">// 把对 .css 文件的处理转交给 id 为 css 的 HappyPack 实例</span>
        test: <span class="hljs-regexp">/\.css$/</span>,
        use: ExtractTextPlugin.extract({
          use: [<span class="hljs-string">&apos;happypack/loader?id=css&apos;</span>],
        }),
      },
    ]
  },
  plugins: [
    <span class="hljs-keyword">new</span> HappyPack({
      <span class="hljs-comment">// 用唯一的标识符 id 来代表当前的 HappyPack 是用来处理一类特定的文件</span>
      id: <span class="hljs-string">&apos;babel&apos;</span>,
      <span class="hljs-comment">// 如何处理 .js 文件，用法和 Loader 配置中一样</span>
      loaders: [<span class="hljs-string">&apos;babel-loader?cacheDirectory&apos;</span>],
      <span class="hljs-comment">// ... 其它配置项</span>
    }),
    <span class="hljs-keyword">new</span> HappyPack({
      id: <span class="hljs-string">&apos;css&apos;</span>,
      <span class="hljs-comment">// 如何处理 .css 文件，用法和 Loader 配置中一样</span>
      loaders: [<span class="hljs-string">&apos;css-loader&apos;</span>],
    }),
    <span class="hljs-keyword">new</span> ExtractTextPlugin({
      filename: <span class="hljs-string">`[name].css`</span>,
    }),
  ],
};
</code></pre>
<p>以上代码有两点重要的修改：</p>
<ul>
<li>在 Loader 配置中，所有文件的处理都交给了 <code>happypack/loader</code> 去处理，使用紧跟其后的 querystring <code>?id=babel</code> 去告诉 <code>happypack/loader</code> 去选择哪个 HappyPack 实例去处理文件。</li>
<li>在 Plugin 配置中，新增了两个 HappyPack 实例分别用于告诉 <code>happypack/loader</code> 去如何处理 .js 和 .css 文件。选项中的 <code>id</code> 属性的值和上面 querystring 中的 <code>?id=babel</code> 相对应，选项中的 <code>loaders</code> 属性和 Loader 配置中一样。</li>
</ul>
<p>在实例化 HappyPack 插件的时候，除了可以传入 <code>id</code> 和 <code>loaders</code> 两个参数外，HappyPack 还支持如下参数：</p>
<ul>
<li><code>threads</code> 代表开启几个子进程去处理这一类型的文件，默认是3个，类型必须是整数。</li>
<li><code>verbose</code> 是否允许 HappyPack 输出日志，默认是 <code>true</code>。</li>
<li><code>threadPool</code> 代表共享进程池，即多个 HappyPack 实例都使用同一个共享进程池中的子进程去处理任务，以防止资源占用过多，相关代码如下：</li>
</ul>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> HappyPack = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;happypack&apos;</span>);
<span class="hljs-comment">// 构造出共享进程池，进程池中包含5个子进程</span>
<span class="hljs-keyword">const</span> happyThreadPool = HappyPack.ThreadPool({ size: <span class="hljs-number">5</span> });

<span class="hljs-built_in">module</span>.exports = {
  plugins: [
    <span class="hljs-keyword">new</span> HappyPack({
      <span class="hljs-comment">// 用唯一的标识符 id 来代表当前的 HappyPack 是用来处理一类特定的文件</span>
      id: <span class="hljs-string">&apos;babel&apos;</span>,
      <span class="hljs-comment">// 如何处理 .js 文件，用法和 Loader 配置中一样</span>
      loaders: [<span class="hljs-string">&apos;babel-loader?cacheDirectory&apos;</span>],
      <span class="hljs-comment">// 使用共享进程池中的子进程去处理任务</span>
      threadPool: happyThreadPool,
    }),
    <span class="hljs-keyword">new</span> HappyPack({
      id: <span class="hljs-string">&apos;css&apos;</span>,
      <span class="hljs-comment">// 如何处理 .css 文件，用法和 Loader 配置中一样</span>
      loaders: [<span class="hljs-string">&apos;css-loader&apos;</span>],
      <span class="hljs-comment">// 使用共享进程池中的子进程去处理任务</span>
      threadPool: happyThreadPool,
    }),
    <span class="hljs-keyword">new</span> ExtractTextPlugin({
      filename: <span class="hljs-string">`[name].css`</span>,
    }),
  ],
};
</code></pre>
<p>接入 HappyPack 后，你需要给项目安装新的依赖：</p>
<pre><code class="lang-bash">npm i -D happypack
</code></pre>
<p>安装成功后重新执行构建你就会看到以下由 HappyPack 输出的日志：</p>
<pre><code>Happy[babel]: Version: 4.0.0-beta.5. Threads: 3
Happy[babel]: All set; signaling webpack to proceed.
Happy[css]: Version: 4.0.0-beta.5. Threads: 3
Happy[css]: All set; signaling webpack to proceed.
</code></pre><p>说明你的 HappyPack 配置生效了，并且可以得知 HappyPack 分别启动了3个子进程去并行的处理任务。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/4-3使用HappyPack.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>
<h2 id="happypack-原理">HappyPack 原理</h2>
<p>在整个 Webpack 构建流程中，最耗时的流程可能就是 Loader 对文件的转换操作了，因为要转换的文件数据巨多，而且这些转换操作都只能一个个挨着处理。
HappyPack 的核心原理就是把这部分任务分解到多个进程去并行处理，从而减少了总的构建时间。</p>
<p>从前面的使用中可以看出所有需要通过 Loader 处理的文件都先交给了 <code>happypack/loader</code> 去处理，收集到了这些文件的处理权后 HappyPack 就好统一分配了。</p>
<p>每通过 <code>new HappyPack()</code> 实例化一个 HappyPack 其实就是告诉 HappyPack 核心调度器如何通过一系列 Loader 去转换一类文件，并且可以指定如何给这类转换操作分配子进程。</p>
<p>核心调度器的逻辑代码在主进程中，也就是运行着 Webpack 的进程中，核心调度器会把一个个任务分配给当前空闲的子进程，子进程处理完毕后把结果发送给核心调度器，它们之间的数据交换是通过进程间通信 API 实现的。</p>
<p>核心调度器收到来自子进程处理完毕的结果后会通知 Webpack 该文件处理完毕。</p>

                                
                                