<h1 id="3-7-使用-vue-框架">3-7 使用 Vue 框架</h1>
<p><a href="https://cn.vuejs.org" target="_blank">Vue</a> 是一个渐进式的 MVVM 框架，相比于 React、Angular 它更灵活轻量。
它不会强制性地内置一些功能和语法，你可以根据自己的需要一点点地添加功能。
虽然采用 Vue 的项目能用可直接运行在浏览器环境里的代码编写，但为了方便编码大多数项目都会采用 Vue 官方的<a href="https://cn.vuejs.org/v2/guide/single-file-components.html#介绍" target="_blank">单文件组件</a>的写法去编写项目。
由于直接引用 Vue 是古老和成熟的做法，本书只专注于讲解如何用 Webpack 构建 Vue 单文件组件。</p>
<h2 id="认识-vue">认识 Vue</h2>
<p>Vue 和 React 一样，它们都推崇组件化和由数据驱动视图的思想，视图和数据绑定在一起，数据改变视图会跟着改变，而无需直接操作视图。
还是以前面的 <code>Hello,Webpack</code> 为例，来看下 Vue 版本的实现。</p>
<p><code>App.vue</code> 文件代表一个单文件组件，它是项目唯一的组件，也是根组件：</p>
<pre><code class="lang-html"><span class="hljs-comment">&lt;!--渲染模版--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">template</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>{{ msg }}<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">template</span>&gt;</span>

<span class="hljs-comment">&lt;!--样式描述--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">style</span> <span class="hljs-attr">scoped</span>&gt;</span><span class="css">
  <span class="hljs-selector-tag">h1</span> {
    <span class="hljs-attribute">color</span>: red;
  }
</span><span class="hljs-tag">&lt;/<span class="hljs-name">style</span>&gt;</span>

<span class="hljs-comment">&lt;!--组件逻辑--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span>&gt;</span><span class="javascript">
  <span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
    data() {
      <span class="hljs-keyword">return</span> {
        msg: <span class="hljs-string">&apos;Hello,Webpack&apos;</span>
      }
    }
  }
</span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
</code></pre>
<p>Vue 的单文件组件通过一个类似 HTML 文件的 <code>.vue</code> 文件就能描述清楚一个组件所需的模版、样式、逻辑。</p>
<p><code>main.js</code> 入口文件：</p>
<pre><code class="lang-js"><span class="hljs-keyword">import</span> Vue <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;vue&apos;</span>
<span class="hljs-keyword">import</span> App <span class="hljs-keyword">from</span> <span class="hljs-string">&apos;./App.vue&apos;</span>

<span class="hljs-keyword">new</span> Vue({
  el: <span class="hljs-string">&apos;#app&apos;</span>,
  render: h =&gt; h(App)
});
</code></pre>
<p>入口文件创建一个 Vue 的根实例，在 ID 为 <code>app</code> 的 DOM 节点上渲染出上面定义的 <code>App</code> 组件。</p>
<p></p><p>想深入学习 深入 Vue，推荐《Vue.js权威指南》：</p>
<a href="http://union-click.jd.com/jdc?d=Fdmv9t" target="_blank">
<img src="http://img14.360buyimg.com/n1/jfs/t3049/211/1135504925/129375/cc072102/57c6862aN329b23fd.jpg">
</a><p></p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>目前最成熟和流行的开发 Vue 项目的方式是采用 ES6 加 Babel 转换，这和基本的采用 ES6 开发的项目很相似，差别在于要解析 <code>.vue</code> 格式的单文件组件。
好在 Vue 官方提供了对应的 <a href="https://vue-loader.vuejs.org/zh-cn/" target="_blank">vue-loader</a> 可以非常方便的完成单文件组件的转换。</p>
<p>修改 Webpack 相关配置如下：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>: {
  rules: [
    {
      test: <span class="hljs-regexp">/\.vue$/</span>,
      use: [<span class="hljs-string">&apos;vue-loader&apos;</span>],
    },
  ]
}
</code></pre>
<p>安装新引入的依赖：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># Vue 框架运行需要的库</span>
npm i -S vue
<span class="hljs-comment"># 构建所需的依赖</span>
npm i -D vue-loader css-loader vue-template-compiler
</code></pre>
<p>在这些依赖中，它们的作用分别是：</p>
<ul>
<li><code>vue-loader</code>：解析和转换 <code>.vue</code> 文件，提取出其中的逻辑代码 <code>script</code>、样式代码 <code>style</code>、以及 HTML 模版 <code>template</code>，再分别把它们交给对应的 Loader 去处理。</li>
<li><code>css-loader</code>：加载由 <code>vue-loader</code> 提取出的 CSS 代码。</li>
<li><code>vue-template-compiler</code>：把 <code>vue-loader</code> 提取出的 HTML 模版编译成对应的可执行的 JavaScript 代码，这和 React 中的 JSX 语法被编译成 JavaScript 代码类似。预先编译好 HTML 模版相对于在浏览器中再去编译 HTML 模版的好处在于性能更好。</li>
</ul>
<p>重新启动构建你就能看到由 Vue 渲染出的 <code>Hello,Webpack</code> 了。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-7使用Vue框架Babel.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>
<h2 id="使用-typescript-编写-vue-应用">使用 TypeScript 编写 Vue 应用</h2>
<p>从 Vue 2.5.0+ 版本开始，提供了对 TypeScript 的良好支持，使用 TypeScript 编写 Vue 是一个很好的选择，因为 TypeScript 能检查出一些潜在的错误。
下面讲解如何用 Webpack 构建使用 TypeScript 编写的 Vue 应用。</p>
<p>新增 <code>tsconfig.json</code> 配置文件，内容如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;compilerOptions&quot;</span>: {
    <span class="hljs-comment">// 构建出 ES5 版本的 JavaScript，与 Vue 的浏览器支持保持一致</span>
    <span class="hljs-string">&quot;target&quot;</span>: <span class="hljs-string">&quot;es5&quot;</span>,
    <span class="hljs-comment">// 开启严格模式，这可以对 `this` 上的数据属性进行更严格的推断</span>
    <span class="hljs-string">&quot;strict&quot;</span>: <span class="hljs-literal">true</span>,
    <span class="hljs-comment">// TypeScript 编译器输出的 JavaScript 采用 es2015 模块化，使 Tree Shaking 生效</span>
    <span class="hljs-string">&quot;module&quot;</span>: <span class="hljs-string">&quot;es2015&quot;</span>,
    <span class="hljs-string">&quot;moduleResolution&quot;</span>: <span class="hljs-string">&quot;node&quot;</span>
  }
}
</code></pre>
<p>以上代码中的 <code>&quot;module&quot;: &quot;es2015&quot;</code> 是为了 Tree Shaking 优化生效，阅读 <a href="../4优化/4-10使用TreeShaking.html">4-10 使用 TreeShaking</a> 进一步了解。</p>
<p>修改 <code>App.vue</code> 脚本部分内容如下：</p>
<pre><code class="lang-html"><span class="hljs-comment">&lt;!--组件逻辑--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">lang</span>=<span class="hljs-string">&quot;ts&quot;</span>&gt;</span><span class="javascript">
  <span class="hljs-keyword">import</span> Vue <span class="hljs-keyword">from</span> <span class="hljs-string">&quot;vue&quot;</span>;

  <span class="hljs-comment">// 通过 Vue.extend 启用 TypeScript 类型推断</span>
  <span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> Vue.extend({
    data() {
      <span class="hljs-keyword">return</span> {
        msg: <span class="hljs-string">&apos;Hello,Webpack&apos;</span>,
      }
    },
  });
</span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
</code></pre>
<p>注意 script 标签中的 <code>lang=&quot;ts&quot;</code> 是为了指明代码的语法是 TypeScript。</p>
<p>修改 <code>main.ts</code> 执行入口文件为如下：</p>
<pre><code class="lang-typescript"><span class="hljs-keyword">import</span> Vue from <span class="hljs-string">&apos;vue&apos;</span>
<span class="hljs-keyword">import</span> App from <span class="hljs-string">&apos;./App.vue&apos;</span>

<span class="hljs-keyword">new</span> Vue({
  el: <span class="hljs-string">&apos;#app&apos;</span>,
  render: h =&gt; h(App)
});
</code></pre>
<p>由于 TypeScript 不认识 <code>.vue</code> 结尾的文件，为了让其支持 <code>import App from &apos;./App.vue&apos;</code> 导入语句，还需要以下文件 <code>vue-shims.d.ts</code> 去定义 <code>.vue</code> 的类型：</p>
<pre><code class="lang-typescript"><span class="hljs-comment">// 告诉 TypeScript 编译器 .vue 文件其实是一个 Vue  </span>
<span class="hljs-keyword">declare</span> <span class="hljs-keyword">module</span> &quot;*.vue&quot; {
  <span class="hljs-keyword">import</span> Vue from <span class="hljs-string">&quot;vue&quot;</span>;
  <span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> Vue;
}
</code></pre>
<p>Webpack 配置需要修改两个地方，如下：</p>
<pre><code class="lang-js"><span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">&apos;path&apos;</span>);

<span class="hljs-built_in">module</span>.exports = {
  resolve: {
    <span class="hljs-comment">// 增加对 TypeScript 的 .ts 和 .vue 文件的支持</span>
    extensions: [<span class="hljs-string">&apos;.ts&apos;</span>, <span class="hljs-string">&apos;.js&apos;</span>, <span class="hljs-string">&apos;.vue&apos;</span>, <span class="hljs-string">&apos;.json&apos;</span>],
  },
  <span class="hljs-built_in">module</span>: {
    rules: [
      <span class="hljs-comment">// 加载 .ts 文件</span>
      {
        test: <span class="hljs-regexp">/\.ts$/</span>,
        loader: <span class="hljs-string">&apos;ts-loader&apos;</span>,
        exclude: <span class="hljs-regexp">/node_modules/</span>,
        options: {
          <span class="hljs-comment">// 让 tsc 把 vue 文件当成一个 TypeScript 模块去处理，以解决 moudle not found 的问题，tsc 本身不会处理 .vue 结尾的文件</span>
          appendTsSuffixTo: [<span class="hljs-regexp">/\.vue$/</span>],
        }
      },
    ]
  },
};
</code></pre>
<p>除此之外还需要安装新引入的依赖：</p>
<pre><code class="lang-bash">npm i -D ts-loader typescript
</code></pre>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-7使用Vue框架TypeScript.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                