<h1 id="5-5-调试-webpack">5-5 调试 Webpack</h1>
<p>在编写 Webpack 的 Plugin 和 Loader 时，可能执行结果会和你预期的不一样，就和你平时写代码遇到了奇怪的 Bug 一样。
对于无法一眼看出问题的 Bug，通常需要调试程序源码才能找出问题所在。</p>
<p>虽然可以通过 <code>console.log</code> 的方式完成调试，但这种方法非常不方便也不优雅，本节将教你如何断点调试 <a href="http://webpack.wuhaolin.cn/5-1工作原理概括.zip" target="_blank">5-1工作原理概括</a> 中的插件代码。
由于 Webpack 运行在 Node.js 之上，调试 Webpack 就相对于调试 Node.js 程序。</p>
<h2 id="在-webstorm-中调试">在 Webstorm 中调试</h2>
<p>Webstorm 集成了 Node.js 的调试工具，因此使用 Webstorm 调试 Webpack 非常简单。</p>
<h3 id="1-设置断点">1. 设置断点</h3>
<p>在你认为可能出现问题的地方设下断点，点击编辑区代码左侧出现红点表示设置了断点，如图：</p>
<p><img src="img/5-5设置断点.png" alt="图5-5-1 设置断点"></p>
<h3 id="2-配置执行入口">2. 配置执行入口</h3>
<p>告诉 Webstorm 如何启动 Webpack，由于 Webpack 实际上就是一个 Node.js 应用，因此需要新建一个 Node.js 类型的执行入口。</p>
<p><img src="img/5-5配置执行入口.png" alt="图5-5-2 配置执行入口"></p>
<p>以上配置中有三点需要注意：</p>
<ul>
<li><code>Name</code> 设置成了 <code>debug webpack</code>，就像设置了一个别名，方便记忆和区分；</li>
<li><code>Working directory</code> 设置为需要调试的插件所在的项目的根目录；</li>
<li><code>JavaScript file</code> 即 Node.js 的执行入口文件，设置为 Webpack 的执行入口文件 <code>node_modules/webpack/bin/webpack.js</code>。</li>
</ul>
<h3 id="3-启动调试">3. 启动调试</h3>
<p>经过以上两步，准备工作已经完成，下面启动调试，启动时选中前面设置的 <code>debug webpack</code>，方法如图</p>
<p><img src="img/5-5启动Webpack.png" alt="图5-5-3 启动 Webpack"></p>
<h3 id="4-执行到断点">4. 执行到断点</h3>
<p>启动后程序就会停在断点所在的位置，在这里你可以方便的查看变量当前的状态，找出问题，如图：</p>
<p><img src="img/5-5执行到断点.png" alt="图5-5-4 执行到断点"></p>
<blockquote>
<p>VSCode 的断点调试方法和 Webstorm 很类似，这里就不重复介绍。</p>
<p>除了以上调试方法外，你还可以通过 Node.js 自带的 <a href="https://nodejs.org/api/debugger.html" target="_blank"> Debugger</a> 断点调试。</p>
</blockquote>

                                
                                