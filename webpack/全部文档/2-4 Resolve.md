<h1 id="2-4-resolve">2-4 Resolve</h1>
<p>Webpack 在启动后会从配置的入口模块出发找出所有依赖的模块，Resolve 配置 Webpack 如何寻找模块所对应的文件。
Webpack 内置 JavaScript 模块化语法解析功能，默认会采用模块化标准里约定好的规则去寻找，但你也可以根据自己的需要修改默认的规则。</p>
<h2 id="alias">alias</h2>
<p><code>resolve.alias</code> 配置项通过别名来把原导入路径映射成一个新的导入路径。例如使用以下配置：</p>
<pre><code class="lang-js"><span class="hljs-comment">// Webpack alias 配置</span>
resolve:{
  alias:{
    components: <span class="hljs-string">&apos;./src/components/&apos;</span>
  }
}
</code></pre>
<p>当你通过 <code>import Button from &apos;components/button&apos;</code> 导入时，实际上被 <code>alias</code> 等价替换成了 <code>import Button from &apos;./src/components/button&apos;</code>。</p>
<p>以上 alias 配置的含义是把导入语句里的 <code>components</code> 关键字替换成 <code>./src/components/</code>。</p>
<p>这样做可能会命中太多的导入语句，alias 还支持 <code>$</code> 符号来缩小范围到只命中以关键字结尾的导入语句：</p>
<pre><code class="lang-js">resolve:{
  alias:{
    <span class="hljs-string">&apos;react$&apos;</span>: <span class="hljs-string">&apos;/path/to/react.min.js&apos;</span>
  }
}
</code></pre>
<p><code>react$</code> 只会命中以 <code>react</code> 结尾的导入语句，即只会把 <code>import &apos;react&apos;</code> 关键字替换成 <code>import &apos;/path/to/react.min.js&apos;</code>。</p>
<h2 id="mainfields">mainFields</h2>
<p>有一些第三方模块会针对不同环境提供几分代码。
例如分别提供采用 ES5 和 ES6 的2份代码，这2份代码的位置写在 <code>package.json</code> 文件里，如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;jsnext:main&quot;</span>: <span class="hljs-string">&quot;es/index.js&quot;</span>,<span class="hljs-comment">// 采用 ES6 语法的代码入口文件</span>
  <span class="hljs-string">&quot;main&quot;</span>: <span class="hljs-string">&quot;lib/index.js&quot;</span> <span class="hljs-comment">// 采用 ES5 语法的代码入口文件</span>
}
</code></pre>
<p>Webpack 会根据 <code>mainFields</code> 的配置去决定优先采用那份代码，<code>mainFields</code> 默认如下：</p>
<pre><code class="lang-js">mainFields: [<span class="hljs-string">&apos;browser&apos;</span>, <span class="hljs-string">&apos;main&apos;</span>]
</code></pre>
<p>Webpack 会按照数组里的顺序去<code>package.json</code> 文件里寻找，只会使用找到的第一个。</p>
<p>假如你想优先采用 ES6 的那份代码，可以这样配置：</p>
<pre><code class="lang-js">mainFields: [<span class="hljs-string">&apos;jsnext:main&apos;</span>, <span class="hljs-string">&apos;browser&apos;</span>, <span class="hljs-string">&apos;main&apos;</span>]
</code></pre>
<h2 id="extensions">extensions</h2>
<p>在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试访问文件是否存在。
<code>resolve.extensions</code> 用于配置在尝试过程中用到的后缀列表，默认是：</p>
<pre><code class="lang-js">extensions: [<span class="hljs-string">&apos;.js&apos;</span>, <span class="hljs-string">&apos;.json&apos;</span>]
</code></pre>
<p>也就是说当遇到 <code>require(&apos;./data&apos;)</code> 这样的导入语句时，Webpack 会先去寻找 <code>./data.js</code> 文件，如果该文件不存在就去寻找 <code>./data.json</code> 文件，
如果还是找不到就报错。</p>
<p>假如你想让 Webpack 优先使用目录下的 TypeScript 文件，可以这样配置：</p>
<pre><code class="lang-js">extensions: [<span class="hljs-string">&apos;.ts&apos;</span>, <span class="hljs-string">&apos;.js&apos;</span>, <span class="hljs-string">&apos;.json&apos;</span>]
</code></pre>
<h2 id="modules">modules</h2>
<p><code>resolve.modules</code> 配置 Webpack 去哪些目录下寻找第三方模块，默认是只会去 <code>node_modules</code> 目录下寻找。
有时你的项目里会有一些模块会大量被其它模块依赖和导入，由于其它模块的位置分布不定，针对不同的文件都要去计算被导入模块文件的相对路径，
这个路径有时候会很长，就像这样 <code>import &apos;../../../components/button&apos;</code>
这时你可以利用 <code>modules</code> 配置项优化，假如那些被大量导入的模块都在 <code>./src/components</code> 目录下，把 <code>modules</code> 配置成</p>
<pre><code class="lang-js">modules:[<span class="hljs-string">&apos;./src/components&apos;</span>,<span class="hljs-string">&apos;node_modules&apos;</span>]
</code></pre>
<p>后，你可以简单通过 <code>import &apos;button&apos;</code> 导入。</p>
<h2 id="descriptionfiles">descriptionFiles</h2>
<p><code>resolve.descriptionFiles</code> 配置描述第三方模块的文件名称，也就是 <code>package.json</code> 文件。默认如下：</p>
<pre><code class="lang-js">descriptionFiles: [<span class="hljs-string">&apos;package.json&apos;</span>]
</code></pre>
<h2 id="enforceextension">enforceExtension</h2>
<p><code>resolve.enforceExtension</code> 如果配置为 <code>true</code> 所有导入语句都必须要带文件后缀，
例如开启前 <code>import &apos;./foo&apos;</code> 能正常工作，开启后就必须写成 <code>import &apos;./foo.js&apos;</code>。</p>
<h2 id="enforcemoduleextension">enforceModuleExtension</h2>
<p><code>enforceModuleExtension</code> 和 <code>enforceExtension</code> 作用类似，但 <code>enforceModuleExtension</code> 只对 <code>node_modules</code> 下的模块生效。
<code>enforceModuleExtension</code> 通常搭配 <code>enforceExtension</code> 使用，在 <code>enforceExtension:true</code> 时，因为安装的第三方模块中大多数导入语句没带文件后缀，
所以这时通过配置 <code>enforceModuleExtension:false</code> 来兼容第三方模块。</p>

                                
                                