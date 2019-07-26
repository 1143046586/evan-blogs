<h1 id="2-3-module">2-3 Module</h1>
<p><code>module</code> 配置如何处理模块。</p>
<h2 id="配置-loader">配置 Loader</h2>
<p><code>rules</code> 配置模块的读取和解析规则，通常用来配置 Loader。其类型是一个数组，数组里每一项都描述了如何去处理部分文件。
配置一项 <code>rules</code> 时大致通过以下方式：</p>
<ol>
<li>条件匹配：通过 <code>test</code> 、 <code>include</code> 、 <code>exclude</code> 三个配置项来命中 Loader 要应用规则的文件。</li>
<li>应用规则：对选中后的文件通过 <code>use</code> 配置项来应用 Loader，可以只应用一个 Loader 或者按照从后往前的顺序应用一组 Loader，同时还可以分别给 Loader 传入参数。</li>
<li>重置顺序：一组 Loader 的执行顺序默认是从右到左执行，通过 <code>enforce</code> 选项可以让其中一个 Loader 的执行顺序放到最前或者最后。</li>
</ol>
<p>下面来通过一个例子来说明具体使用方法：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>: {
  rules: [
    {
      <span class="hljs-comment">// 命中 JavaScript 文件</span>
      test: <span class="hljs-regexp">/\.js$/</span>,
      <span class="hljs-comment">// 用 babel-loader 转换 JavaScript 文件</span>
      <span class="hljs-comment">// ?cacheDirectory 表示传给 babel-loader 的参数，用于缓存 babel 编译结果加快重新编译速度</span>
      use: [<span class="hljs-string">&apos;babel-loader?cacheDirectory&apos;</span>],
      <span class="hljs-comment">// 只命中src目录里的js文件，加快 Webpack 搜索速度</span>
      include: path.resolve(__dirname, <span class="hljs-string">&apos;src&apos;</span>)
    },
    {
      <span class="hljs-comment">// 命中 SCSS 文件</span>
      test: <span class="hljs-regexp">/\.scss$/</span>,
      <span class="hljs-comment">// 使用一组 Loader 去处理 SCSS 文件。</span>
      <span class="hljs-comment">// 处理顺序为从后到前，即先交给 sass-loader 处理，再把结果交给 css-loader 最后再给 style-loader。</span>
      use: [<span class="hljs-string">&apos;style-loader&apos;</span>, <span class="hljs-string">&apos;css-loader&apos;</span>, <span class="hljs-string">&apos;sass-loader&apos;</span>],
      <span class="hljs-comment">// 排除 node_modules 目录下的文件</span>
      exclude: path.resolve(__dirname, <span class="hljs-string">&apos;node_modules&apos;</span>),
    },
    {
      <span class="hljs-comment">// 对非文本文件采用 file-loader 加载</span>
      test: <span class="hljs-regexp">/\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/</span>,
      use: [<span class="hljs-string">&apos;file-loader&apos;</span>],
    },
  ]
}
</code></pre>
<p>在 Loader 需要传入很多参数时，你还可以通过一个 Object 来描述，例如在上面的 babel-loader 配置中有如下代码：</p>
<pre><code class="lang-js">use: [
  {
    loader:<span class="hljs-string">&apos;babel-loader&apos;</span>,
    options:{
      cacheDirectory:<span class="hljs-literal">true</span>,
    },
    <span class="hljs-comment">// enforce:&apos;post&apos; 的含义是把该 Loader 的执行顺序放到最后</span>
    <span class="hljs-comment">// enforce 的值还可以是 pre，代表把 Loader 的执行顺序放到最前面</span>
    enforce:<span class="hljs-string">&apos;post&apos;</span>
  },
  <span class="hljs-comment">// 省略其它 Loader</span>
]
</code></pre>
<p>上面的例子中 <code>test include exclude</code> 这三个命中文件的配置项只传入了一个字符串或正则，其实它们还都支持数组类型，使用如下：</p>
<pre><code class="lang-js">{
  test:[
    <span class="hljs-regexp">/\.jsx?$/</span>,
    <span class="hljs-regexp">/\.tsx?$/</span>
  ],
  include:[
    path.resolve(__dirname, <span class="hljs-string">&apos;src&apos;</span>),
    path.resolve(__dirname, <span class="hljs-string">&apos;tests&apos;</span>),
  ],
  exclude:[
    path.resolve(__dirname, <span class="hljs-string">&apos;node_modules&apos;</span>),
    path.resolve(__dirname, <span class="hljs-string">&apos;bower_modules&apos;</span>),
  ]
}
</code></pre>
<p>数组里的每项之间是<strong>或</strong>的关系，即文件路径符合数组中的任何一个条件就会被命中。</p>
<h2 id="noparse">noParse</h2>
<p><code>noParse</code> 配置项可以让 Webpack 忽略对部分没采用模块化的文件的递归解析和处理，这样做的好处是能提高构建性能。
原因是一些库例如 jQuery 、ChartJS 它们庞大又没有采用模块化标准，让 Webpack 去解析这些文件耗时又没有意义。</p>
<p><code>noParse</code> 是可选配置项，类型需要是 <code>RegExp</code>、<code>[RegExp]</code>、<code>function</code> 其中一个。</p>
<p>例如想要忽略掉 jQuery 、ChartJS，可以使用如下代码：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 使用正则表达式</span>
noParse: <span class="hljs-regexp">/jquery|chartjs/</span>

<span class="hljs-comment">// 使用函数，从 Webpack 3.0.0 开始支持</span>
noParse: (content)=&gt; {
  <span class="hljs-comment">// content 代表一个模块的文件路径</span>
  <span class="hljs-comment">// 返回 true or false</span>
  <span class="hljs-keyword">return</span> <span class="hljs-regexp">/jquery|chartjs/</span>.test(content);
}
</code></pre>
<blockquote>
<p>注意被忽略掉的文件里不应该包含 <code>import</code> 、 <code>require</code> 、 <code>define</code> 等模块化语句，不然会导致构建出的代码中包含无法在浏览器环境下执行的模块化语句。</p>
</blockquote>
<h2 id="parser">parser</h2>
<p>因为 Webpack 是以模块化的 JavaScript 文件为入口，所以内置了对模块化 JavaScript 的解析功能，支持 AMD、CommonJS、SystemJS、ES6。
<code>parser</code> 属性可以更细粒度的配置哪些模块语法要解析哪些不解析，和 <code>noParse</code> 配置项的区别在于 <code>parser</code> 可以精确到语法层面，
而 <code>noParse</code> 只能控制哪些文件不被解析。
<code>parser</code> 使用如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>: {
  rules: [
    {
      test: <span class="hljs-regexp">/\.js$/</span>,
      use: [<span class="hljs-string">&apos;babel-loader&apos;</span>],
      parser: {
      amd: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 AMD</span>
      commonjs: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 CommonJS</span>
      system: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 SystemJS</span>
      harmony: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 ES6 import/export</span>
      requireInclude: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 require.include</span>
      requireEnsure: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 require.ensure</span>
      requireContext: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 require.context</span>
      browserify: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 browserify</span>
      requireJs: <span class="hljs-literal">false</span>, <span class="hljs-comment">// 禁用 requirejs</span>
      }
    },
  ]
}
</code></pre>

                                
                                