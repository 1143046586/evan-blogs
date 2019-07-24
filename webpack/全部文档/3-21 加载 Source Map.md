<h1 id="3-21-加载-source-map">3-21 加载 Source Map</h1>
<p>由于在开发过程中经常会使用新语言去开发项目，最后会把源码转换成能在浏览器中直接运行的 JavaScript 代码。
这样做虽能提升开发效率，在调试代码的过程中你会发现生成的代码可读性非常差，这给代码调试带来了不便。</p>
<p>Webpack 支持为转换生成的代码输出对应的 Source Map 文件，以方便在浏览器中能通过源码调试。
控制 Source Map 输出的 Webpack 配置项是 <code>devtool</code>，它有很多选项，下面来一一详细介绍。</p>
<table>
<thead>
<tr>
<th>devtool</th>
<th>含义</th>
</tr>
</thead>
<tbody>
<tr>
<td>空</td>
<td>不生成 Source Map</td>
</tr>
<tr>
<td>eval</td>
<td>每个 module 会封装到 eval 里包裹起来执行，并且会在每个 eval 语句的末尾追加注释 <code>//# sourceURL=webpack:///./main.js</code></td>
</tr>
<tr>
<td>source-map</td>
<td>会额外生成一个单独 Source Map 文件，并且会在 JavaScript 文件末尾追加 <code>//# sourceMappingURL=bundle.js.map</code></td>
</tr>
<tr>
<td>hidden-source-map</td>
<td>和 source-map 类似，但不会在 JavaScript 文件末尾追加 <code>//# sourceMappingURL=bundle.js.map</code></td>
</tr>
<tr>
<td>inline-source-map</td>
<td>和 source-map 类似，但不会额外生成一个单独 Source Map 文件，而是把 Source Map 转换成 base64 编码内嵌到 JavaScript 中</td>
</tr>
<tr>
<td>eval-source-map</td>
<td>和 eval 类似，但会把每个模块的 Source Map 转换成 base64 编码内嵌到 eval 语句的末尾，例如 <code>//# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW...</code></td>
</tr>
<tr>
<td>cheap-source-map</td>
<td>和 source-map 类似，但生成的 Source Map 文件中没有列信息，因此生成速度更快</td>
</tr>
<tr>
<td>cheap-module-source-map</td>
<td>和 cheap-source-map 类似，但会包含 Loader 生成的 Source Map</td>
</tr>
</tbody>
</table>
<p>其实以上表格只是列举了 devtool 可能取值的一部分，
它的取值其实可以由 <code>source-map</code>、<code>eval</code>、<code>inline</code>、<code>hidden</code>、<code>cheap</code>、<code>module</code> 这六个关键字随意组合而成。
这六个关键字每个都代表一种特性，它们的含义分别是：</p>
<ul>
<li>eval：用 <code>eval</code> 语句包裹需要安装的模块；</li>
<li>source-map：生成独立的 Source Map 文件；</li>
<li>hidden：不在 JavaScript 文件中指出 Source Map 文件所在，这样浏览器就不会自动加载 Source Map；</li>
<li>inline：把生成的 Source Map 转换成 base64 格式内嵌在 JavaScript 文件中；</li>
<li>cheap：生成的 Source Map 中不会包含列信息，这样计算量更小，输出的 Source Map 文件更小；同时 Loader 输出的 Source Map 不会被采用；</li>
<li>module：来自 Loader 的 Source Map 被简单处理成每行一个模块；</li>
</ul>
<h2 id="该如何选择">该如何选择</h2>
<p>Devtool 配置项提供的这么多选项看似简单，但很多人搞不清楚它们之间的差别和应用场景。</p>
<p>如果你不关心细节和性能，只是想在不出任何差错的情况下调试源码，可以直接设置成 <code>source-map</code>，但这样会造成两个问题：</p>
<ul>
<li><code>source-map</code> 模式下会输出质量最高最详细的 Source Map，这会造成构建速度缓慢，特别是在开发过程需要频繁修改的时候会增加等待时间；</li>
<li><code>source-map</code> 模式下会把 Source Map 暴露出去，如果构建发布到线上的代码的 Source Map 暴露出去就等于源码被泄露；</li>
</ul>
<p>为了解决以上两个问题，可以这样做：</p>
<ul>
<li>在开发环境下把 <code>devtool</code> 设置成 <code>cheap-module-eval-source-map</code>，因为生成这种 Source Map 的速度最快，能加速构建。由于在开发环境下不会做代码压缩，Source Map 中即使没有列信息也不会影响断点调试；</li>
<li>在生产环境下把 <code>devtool</code> 设置成 <code>hidden-source-map</code>，意思是生成最详细的 Source Map，但不会把 Source Map 暴露出去。由于在生产环境下会做代码压缩，一个 JavaScript 文件只有一行，所以需要列信息。</li>
</ul>
<blockquote>
<p>在生产环境下通常不会把 Source Map 上传到 HTTP 服务器让用户获取，而是上传到 JavaScript 错误收集系统，在错误收集系统上根据 Source Map 和收集到的 JavaScript 运行错误堆栈计算出错误所在源码的位置。</p>
<p>不要在生产环境下使用 <code>inline</code> 模式的 Source Map， 因为这会使 JavaScript 文件变得很大，而且会泄露源码。</p>
</blockquote>
<h2 id="加载现有的-source-map">加载现有的 Source Map</h2>
<p>有些从 Npm 安装的第三方模块是采用 ES6 或者 TypeScript 编写的，它们在发布时会同时带上编译出来的 JavaScript 文件和对应的 Source Map 文件，以方便你在使用它们出问题的时候调试它们；</p>
<p>默认情况下 Webpack 是不会去加载这些附加的 Source Map 文件的，Webpack 只会在转换过程中生成 Source Map。
为了让 Webpack 加载这些附加的 Source Map 文件，需要安装 <a href="https://github.com/webpack-contrib/source-map-loader" target="_blank">source-map-loader</a> 。
使用方法如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        test: <span class="hljs-regexp">/\.js$/</span>,
        <span class="hljs-comment">// 只加载你关心的目录下的 Source Map，以提升构建速度</span>
        include: [path.resolve(root, <span class="hljs-string">&apos;node_modules/some-components/&apos;</span>)],
        use: [<span class="hljs-string">&apos;source-map-loader&apos;</span>],
        <span class="hljs-comment">// 要把 source-map-loader 的执行顺序放到最前面，如果在 source-map-loader 之前有 Loader 转换了该 JavaScript 文件，会导致 Source Map 映射错误</span>
        enforce: <span class="hljs-string">&apos;pre&apos;</span>
      }
    ]
  }
};
</code></pre>
<blockquote>
<p>由于 source-map-loader 在加载 Source Map 时计算量很大，因此要避免让该 Loader 处理过多的文件，不然会导致构建速度缓慢。
通常会采用 <code>include</code> 去命中只关心的文件。</p>
</blockquote>
<p>再安装新引入的依赖：</p>
<pre><code class="lang-bash">npm i -D <span class="hljs-built_in">source</span>-map-loader
</code></pre>
<p>重启 Webpack 后，你就能在浏览器中调试 <code>node_modules/some-components/</code> 目录下的源码了。</p>

                                
                                