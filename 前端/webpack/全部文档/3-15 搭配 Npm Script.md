<h1 id="3-15-搭配-npm-script">3-15 搭配 Npm Script</h1>
<h2 id="认识-npm-script">认识 Npm Script</h2>
<p><a href="https://docs.npmjs.com/misc/scripts" target="_blank">Npm Script</a> 是一个任务执行者。
Npm 是在安装 Node.js 时附带的包管理器，Npm Script 则是 Npm 内置的一个功能，允许在 <code>package.json</code> 文件里面使用 <code>scripts</code> 字段定义任务：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;scripts&quot;</span>: {
    <span class="hljs-string">&quot;dev&quot;</span>: <span class="hljs-string">&quot;node dev.js&quot;</span>,
    <span class="hljs-string">&quot;pub&quot;</span>: <span class="hljs-string">&quot;node build.js&quot;</span>
  }
}
</code></pre>
<p>里面的 <code>scripts</code> 字段是一个对象，每一个属性对应一段脚本，以上定义了两个任务 <code>dev</code> 和 <code>pub</code>。
Npm Script 底层实现原理是通过调用 Shell 去运行脚本命令，例如执行 <code>npm run pub</code> 命令等同于执行命令 <code>node build.js</code>。</p>
<p>Npm Script 还有一个重要的功能是能运行安装到项目目录里的 <code>node_modules</code> 里的可执行模块，例如在通过命令</p>
<pre><code class="lang-bash">npm i -D webpack
</code></pre>
<p>将 Webpack 安装到项目中后，是无法直接在项目根目录下通过命令 <code>webpack</code> 去执行 Webpack 构建的，而是要通过命令 <code>./node_modules/.bin/webpack</code> 去执行。</p>
<p>Npm Script 能方便的解决这个问题，只需要在 <code>scripts</code> 字段里定义一个任务，例如：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;scripts&quot;</span>: {
    <span class="hljs-string">&quot;build&quot;</span>: <span class="hljs-string">&quot;webpack&quot;</span>
  }
}
</code></pre>
<p>Npm Script 会先去项目目录下的 <code>node_modules</code> 中寻找有没有可执行的 <code>webpack</code> 文件，如果有就使用本地的，如果没有就使用全局的。
所以现在执行 Webpack 构建只需要通过执行 <code>npm run build</code> 去实现。</p>
<h2 id="webpack-为什么需要-npm-script">Webpack 为什么需要 Npm Script</h2>
<p>Webpack 只是一个打包模块化代码的工具，并没有提供任何任务管理相关的功能。
但在实际场景中通常不会是只通过执行 <code>webpack</code> 就能完成所有任务的，而是需要多个任务才能完成。</p>
<p>举一个常见的例子，要求如下：</p>
<ol>
<li>在开发阶段为了提高开发体验，使用 DevServer 做开发，并且需要输出 Source Map 以方便调试，同时还需要开启自动刷新功能。</li>
<li>为了减小发布到线上的代码尺寸，在构建出发布到线上的代码时，需要压缩输出的代码。</li>
<li>在构建完发布到线上的代码后，需要把构建出的代码提交给发布系统。</li>
</ol>
<p>可以看出要求1和要求2是相互冲突的，其中任务3又依赖任务2。要满足以上三个要求，需要定义三个不同的任务。</p>
<p>接下来通过 Npm Script 来定义上面的3个任务：</p>
<pre><code class="lang-json"><span class="hljs-string">&quot;scripts&quot;</span>: {
  <span class="hljs-string">&quot;dev&quot;</span>: <span class="hljs-string">&quot;webpack-dev-server --open&quot;</span>,
  <span class="hljs-string">&quot;dist&quot;</span>: <span class="hljs-string">&quot;NODE_ENV=production webpack --config webpack_dist.config.js&quot;</span>,
  <span class="hljs-string">&quot;pub&quot;</span>: <span class="hljs-string">&quot;npm run dist &amp;&amp; rsync dist&quot;</span>
},
</code></pre>
<p>含义分别是：</p>
<ul>
<li><code>dev</code> 代表用于开发时执行的任务，通过 DevServer 去启动构建。所以在开发项目时只需执行 <code>npm run dev</code>。</li>
<li><code>dist</code> 代表构建出用于发布到线上去的代码，输出到 <code>dist</code> 目录中。其中的 <code>NODE_ENV=production</code> 是为了在运行任务时注入环境变量。</li>
<li><code>pub</code> 代表先构建出用于发布到线上去的代码，再同步 <code>dist</code> 目录中的文件到发布系统(如何同步文件需根据你所使用的发布系统而定)。所以在开发完后需要发布时只需执行 <code>npm run pub</code>。</li>
</ul>
<p>使用 Npm Script 的好处是把一连串复杂的流程简化成了一个简单的命令，需要时只需要执行对应的那个简短的命令，而不用去手动的重复整个流程。
这会大大的提高我们的效率和降低出错率。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-15搭配NpmScript.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                