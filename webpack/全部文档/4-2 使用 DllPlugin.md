<h1 id="4-2-使用-dllplugin">4-2 使用 DllPlugin</h1>
<h2 id="认识-dll">认识 DLL</h2>
<p>在介绍 <a href="https://webpack.js.org/plugins/dll-plugin/" target="_blank">DllPlugin</a> 前先给大家介绍下 DLL。
用过 Windows 系统的人应该会经常看到以 <code>.dll</code> 为后缀的文件，这些文件称为<strong>动态链接库</strong>，在一个动态链接库中可以包含给其他模块调用的函数和数据。</p>
<p>要给 Web 项目构建接入动态链接库的思想，需要完成以下事情：</p>
<ul>
<li>把网页依赖的基础模块抽离出来，打包到一个个单独的动态链接库中去。一个动态链接库中可以包含多个模块。</li>
<li>当需要导入的模块存在于某个动态链接库中时，这个模块不能被再次被打包，而是去动态链接库中获取。</li>
<li>页面依赖的所有动态链接库需要被加载。</li>
</ul>
<p>为什么给 Web 项目构建接入动态链接库的思想后，会大大提升构建速度呢？
原因在于包含大量复用模块的动态链接库只需要编译一次，在之后的构建过程中被动态链接库包含的模块将不会在重新编译，而是直接使用动态链接库中的代码。
由于动态链接库中大多数包含的是常用的第三方模块，例如 react、react-dom，只要不升级这些模块的版本，动态链接库就不用重新编译。</p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>Webpack 已经内置了对动态链接库的支持，需要通过2个内置的插件接入，它们分别是：</p>
<ul>
<li>DllPlugin 插件：用于打包出一个个单独的动态链接库文件。</li>
<li>DllReferencePlugin 插件：用于在主要配置文件中去引入 DllPlugin 插件打包好的动态链接库文件。</li>
</ul>
<p>下面以基本的 React 项目为例，为其接入 DllPlugin，在开始前先来看下最终构建出的目录结构：</p>
<pre><code>├── main.js
├── polyfill.dll.js
├── polyfill.manifest.json
├── react.dll.js
└── react.manifest.json
</code></pre><p>其中包含两个动态链接库文件，分别是：</p>
<ul>
<li><code>polyfill.dll.js</code> 里面包含项目所有依赖的 polyfill，例如 Promise、fetch 等 API。</li>
<li><code>react.dll.js</code> 里面包含 React 的基础运行环境，也就是 react 和 react-dom 模块。</li>
</ul>
<p>以 <code>react.dll.js</code> 文件为例，其文件内容大致如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">var</span> _dll_react = (<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">modules</span>) </span>{
  <span class="hljs-comment">// ... 此处省略 webpackBootstrap 函数代码</span>
}([
  <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">module, exports, __webpack_require__</span>) </span>{
    <span class="hljs-comment">// 模块 ID 为 0 的模块对应的代码</span>
  },
  <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">module, exports, __webpack_require__</span>) </span>{
    <span class="hljs-comment">// 模块 ID 为 1 的模块对应的代码</span>
  },
  <span class="hljs-comment">// ... 此处省略剩下的模块对应的代码 </span>
]));
</code></pre>
<p>可见一个动态链接库文件中包含了大量模块的代码，这些模块存放在一个数组里，用数组的索引号作为 ID。
并且还通过 <code>_dll_react</code> 变量把自己暴露在了全局中，也就是可以通过 <code>window._dll_react</code> 可以访问到它里面包含的模块。</p>
<p>其中 <code>polyfill.manifest.json</code> 和 <code>react.manifest.json</code> 文件也是由 DllPlugin 生成出，用于描述动态链接库文件中包含哪些模块，
以 <code>react.manifest.json</code> 文件为例，其文件内容大致如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-comment">// 描述该动态链接库文件暴露在全局的变量名称</span>
  <span class="hljs-string">&quot;name&quot;</span>: <span class="hljs-string">&quot;_dll_react&quot;</span>,
  <span class="hljs-string">&quot;content&quot;</span>: {
    <span class="hljs-string">&quot;./node_modules/process/browser.js&quot;</span>: {
      <span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">0</span>,
      <span class="hljs-string">&quot;meta&quot;</span>: {}
    },
    <span class="hljs-comment">// ... 此处省略部分模块</span>
    <span class="hljs-string">&quot;./node_modules/react-dom/lib/ReactBrowserEventEmitter.js&quot;</span>: {
      <span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">42</span>,
      <span class="hljs-string">&quot;meta&quot;</span>: {}
    },
    <span class="hljs-string">&quot;./node_modules/react/lib/lowPriorityWarning.js&quot;</span>: {
      <span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">47</span>,
      <span class="hljs-string">&quot;meta&quot;</span>: {}
    },
    <span class="hljs-comment">// ... 此处省略部分模块</span>
    <span class="hljs-string">&quot;./node_modules/react-dom/lib/SyntheticTouchEvent.js&quot;</span>: {
      <span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">210</span>,
      <span class="hljs-string">&quot;meta&quot;</span>: {}
    },
    <span class="hljs-string">&quot;./node_modules/react-dom/lib/SyntheticTransitionEvent.js&quot;</span>: {
      <span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">211</span>,
      <span class="hljs-string">&quot;meta&quot;</span>: {}
    },
  }
}
</code></pre>
<p>可见 <code>manifest.json</code> 文件清楚地描述了与其对应的 <code>dll.js</code> 文件中包含了哪些模块，以及每个模块的路径和 ID。</p>
<p><code>main.js</code> 文件是编译出来的执行入口文件，当遇到其依赖的模块在 <code>dll.js</code> 文件中时，会直接通过 <code>dll.js</code> 文件暴露出的全局变量去获取打包在 <code>dll.js</code> 文件的模块。
所以在 <code>index.html</code> 文件中需要把依赖的两个 <code>dll.js</code> 文件给加载进去，<code>index.html</code> 内容如下：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">&quot;app&quot;</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入依赖的动态链接库文件--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./dist/polyfill.dll.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./dist/react.dll.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入执行入口文件--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./dist/main.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<p>以上就是所有接入 DllPlugin 后最终编译出来的代码，接下来教你如何实现。</p>
<h3 id="构建出动态链接库文件">构建出动态链接库文件</h3>
<p>构建输出的以下这四个文件 </p>
<pre><code>├── polyfill.dll.js
├── polyfill.manifest.json
├── react.dll.js
└── react.manifest.json
</code></pre><p>和以下这一个文件 </p>
<pre><code>├── main.js
</code></pre><p>是由两份不同的构建分别输出的。</p>
<p>动态链接库文件相关的文件需要由一份独立的构建输出，用于给主构建使用。新建一个 Webpack 配置文件 <code>webpack_dll.config.js</code> 专门用于构建它们，文件内容如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> DllPlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/DllPlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-comment">// JS 执行入口文件</span>
  entry: {
    <span class="hljs-comment">// 把 React 相关模块的放到一个单独的动态链接库</span>
    react: [<span class="hljs-string">&apos;react&apos;</span>, <span class="hljs-string">&apos;react-dom&apos;</span>],
    <span class="hljs-comment">// 把项目需要所有的 polyfill 放到一个单独的动态链接库</span>
    polyfill: [<span class="hljs-string">&apos;core-js/fn/object/assign&apos;</span>, <span class="hljs-string">&apos;core-js/fn/promise&apos;</span>, <span class="hljs-string">&apos;whatwg-fetch&apos;</span>],
  },
  output: {
    <span class="hljs-comment">// 输出的动态链接库的文件名称，[name] 代表当前动态链接库的名称，</span>
    <span class="hljs-comment">// 也就是 entry 中配置的 react 和 polyfill</span>
    filename: <span class="hljs-string">&apos;[name].dll.js&apos;</span>,
    <span class="hljs-comment">// 输出的文件都放到 dist 目录下</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;dist&apos;</span>),
    <span class="hljs-comment">// 存放动态链接库的全局变量名称，例如对应 react 来说就是 _dll_react</span>
    <span class="hljs-comment">// 之所以在前面加上 _dll_ 是为了防止全局变量冲突</span>
    library: <span class="hljs-string">&apos;_dll_[name]&apos;</span>,
  },
  plugins: [
    <span class="hljs-comment">// 接入 DllPlugin</span>
    <span class="hljs-keyword">new</span> DllPlugin({
      <span class="hljs-comment">// 动态链接库的全局变量名称，需要和 output.library 中保持一致</span>
      <span class="hljs-comment">// 该字段的值也就是输出的 manifest.json 文件 中 name 字段的值</span>
      <span class="hljs-comment">// 例如 react.manifest.json 中就有 &quot;name&quot;: &quot;_dll_react&quot;</span>
      name: <span class="hljs-string">&apos;_dll_[name]&apos;</span>,
      <span class="hljs-comment">// 描述动态链接库的 manifest.json 文件输出时的文件名称</span>
      path: path.join(__dirname, <span class="hljs-string">&apos;dist&apos;</span>, <span class="hljs-string">&apos;[name].manifest.json&apos;</span>),
    }),
  ],
};
</code></pre>
<h3 id="使用动态链接库文件">使用动态链接库文件</h3>
<p>构建出的动态链接库文件用于给其它地方使用，在这里也就是给执行入口使用。</p>
<p>用于输出 <code>main.js</code> 的主 Webpack 配置文件内容如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);
<span class="hljs-keyword">const</span> DllReferencePlugin = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;webpack/lib/DllReferencePlugin&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  entry: {
    <span class="hljs-comment">// 定义入口 Chunk</span>
    main: <span class="hljs-string">&apos;./main.js&apos;</span>
  },
  output: {
    <span class="hljs-comment">// 输出文件的名称</span>
    filename: <span class="hljs-string">&apos;[name].js&apos;</span>,
    <span class="hljs-comment">// 输出文件都放到 dist 目录下</span>
    path: path.resolve(__dirname, <span class="hljs-string">&apos;dist&apos;</span>),
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      {
        <span class="hljs-comment">// 项目源码使用了 ES6 和 JSX 语法，需要使用 babel-loader 转换</span>
        test: <span class="hljs-regexp">/\.js$/</span>,
        use: [<span class="hljs-string">&apos;babel-loader&apos;</span>],
        exclude: path.resolve(__dirname, <span class="hljs-string">&apos;node_modules&apos;</span>),
      },
    ]
  },
  plugins: [
    <span class="hljs-comment">// 告诉 Webpack 使用了哪些动态链接库</span>
    <span class="hljs-keyword">new</span> DllReferencePlugin({
      <span class="hljs-comment">// 描述 react 动态链接库的文件内容</span>
      manifest: <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./dist/react.manifest.json&apos;</span>),
    }),
    <span class="hljs-keyword">new</span> DllReferencePlugin({
      <span class="hljs-comment">// 描述 polyfill 动态链接库的文件内容</span>
      manifest: <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;./dist/polyfill.manifest.json&apos;</span>),
    }),
  ],
  devtool: <span class="hljs-string">&apos;source-map&apos;</span>
};
</code></pre>
<blockquote>
<p>注意：在 <code>webpack_dll.config.js</code> 文件中，DllPlugin 中的 name 参数必须和 output.library 中保持一致。
原因在于 DllPlugin 中的 name 参数会影响输出的 manifest.json 文件中 name 字段的值，
而在 <code>webpack.config.js</code> 文件中 DllReferencePlugin 会去 manifest.json 文件读取 name 字段的值，
把值的内容作为在从全局变量中获取动态链接库中内容时的全局变量名。</p>
</blockquote>
<h3 id="执行构建">执行构建</h3>
<p>在修改好以上两个 Webpack 配置文件后，需要重新执行构建。
重新执行构建时要注意的是需要先把动态链接库相关的文件编译出来，因为主 Webpack 配置文件中定义的 DllReferencePlugin 依赖这些文件。</p>
<p>执行构建时流程如下：</p>
<ol>
<li>如果动态链接库相关的文件还没有编译出来，就需要先把它们编译出来。方法是执行 <code>webpack --config webpack_dll.config.js</code> 命令。</li>
<li>在确保动态链接库存在时，才能正常的编译出入口执行文件。方法是执行 <code>webpack</code> 命令。这时你会发现构建速度有了非常大的提升。</li>
</ol>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/4-2使用DllPlugin.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                