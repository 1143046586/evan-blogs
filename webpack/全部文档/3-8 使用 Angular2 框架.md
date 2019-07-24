<h1 id="3-8-使用-angular2-框架">3-8 使用 Angular2 框架</h1>
<h2 id="认识-angular2">认识 Angular2</h2>
<p><a href="https://angular.io" target="_blank">Angular2</a> 是 <a href="https://angularjs.org" target="_blank">AngularJS</a> 的下一个版本，它继承了 AngularJS 中的部分思想又加入了一些新的改进。
Angular2 相比于 React 和 Vue 来说要复杂很多，这三者的出发点都是组件化和数据驱动视图，但 Angular2 多出了以下概念：</p>
<ul>
<li>模块（NgModule）这里的模块不是指 JavaScript 或者其它编程语言里的模块化，而是指 Angular2 里提出的独有的用法。</li>
<li>注解（Decorator）通过注解语法 <code>@XXX</code> 来给一个 Class 附加元数据。</li>
<li>服务（Service）按照功能划分，把项目中可复用的重复代码封装成一个个服务以方便给其它模块使用。服务可以包含函数、常值等，常见的有日志服务、数据服务、应用程序配置等。</li>
<li>依赖注入（Dependency Injection）也叫控制反转（Inversion of Control），是面向对象编程中的一种设计原则，可以用来减低代码之间的耦合度。</li>
</ul>
<p>Angular2 引入的这些思想都是为了分解和简化大型项目的难度，但对于开发小项目来说这些可能都是累赘，对于初学者来说可能会难以掌握。
在语言选择上虽然 Angular2 官方对 TypeScript 和 JavaScript 都提供了支持，但通常选择 Angular2 的项目都会使用 TypeScript，
原因在于 Angular2 本身就是使用 TypeScript 开发，在项目中使用 TypeScript 相比 JavaScript 开发体验会好很多。</p>
<p></p><p>想深入学习 Angular2，推荐《迈向Angular 2：基于TypeScript的高性能SPA框架》：</p>
<a href="http://union-click.jd.com/jdc?d=SFEPja" target="_blank">
<img src="http://img11.360buyimg.com/n1/jfs/t2818/331/3977755135/157483/7b060b4d/57a42310Nbab48a67.jpg">
</a><p></p>
<p>看到这你可能会被 Angular2 的复杂所吓到，先来看看如何用 Angular2 来开发 <code>Hello,Webpack</code>。</p>
<p>这个应用只有一个视图组件 <code>AppComponent</code> 用于渲染 <code>Hello,Webpack</code>，组件代码如下：</p>
<pre><code class="lang-typescript"><span class="hljs-keyword">import</span> {Component} from <span class="hljs-string">&apos;@angular/core&apos;</span>;

<span class="hljs-comment">// 通过注解的方式描述清楚了这个视图组件所需的模版、样式、数据、逻辑。</span>
@Component({
  <span class="hljs-comment">// 标签名称</span>
  selector: <span class="hljs-string">&apos;app-root&apos;</span>,
  <span class="hljs-comment">// HTML 模版</span>
  template: <span class="hljs-string">&apos;&lt;h1&gt;{{msg}}&lt;/h1&gt;&apos;</span>,
  <span class="hljs-comment">// CSS 样式</span>
  styles: [<span class="hljs-string">&apos;h1{ color:red; }&apos;</span>]
})
<span class="hljs-keyword">export</span> <span class="hljs-keyword">class</span> AppComponent {
  msg = <span class="hljs-string">&apos;Hello,Webpack&apos;</span>;
}
</code></pre>
<p>光有组件还不够，还需要实例化 <code>AppComponent</code> 视图组件，并把它渲染到 DOM 中去。Angular2 规定可运行的应用至少有一个 NgModule 也就是需要一个根 NgModule。
这个根 NgModule 描述了如何启动应用，代码如下：</p>
<pre><code class="lang-typescript"><span class="hljs-comment">// 让 Angular2 正常运行需要的 polyfill</span>
<span class="hljs-keyword">import</span> <span class="hljs-string">&apos;core-js/es6/reflect&apos;</span>;
<span class="hljs-keyword">import</span> <span class="hljs-string">&apos;core-js/es7/reflect&apos;</span>;
<span class="hljs-keyword">import</span> <span class="hljs-string">&apos;zone.js/dist/zone&apos;</span>;
<span class="hljs-comment">// Angular2 框架核心模块</span>
<span class="hljs-keyword">import</span> {NgModule} from <span class="hljs-string">&apos;@angular/core&apos;</span>;
<span class="hljs-keyword">import</span> {BrowserModule} from <span class="hljs-string">&apos;@angular/platform-browser&apos;</span>;
<span class="hljs-keyword">import</span> {platformBrowserDynamic} from <span class="hljs-string">&apos;@angular/platform-browser-dynamic&apos;</span>;
<span class="hljs-comment">// 项目自定义视图组件</span>
<span class="hljs-keyword">import</span> {AppComponent} from <span class="hljs-string">&apos;./app.component&apos;</span>;

@NgModule({
  <span class="hljs-comment">// 声明该 NgModule 所依赖的组件、指令、管道</span>
  declarations: [AppComponent],
  <span class="hljs-comment">// 该 NgModule 所依赖的其它 NgModule</span>
  imports: [BrowserModule],
  <span class="hljs-comment">// 应用的根视图组件，只有根 NgModule 需要设置</span>
  bootstrap: [AppComponent]
})
<span class="hljs-keyword">class</span> AppModule {
}

<span class="hljs-comment">// 从 AppModule 启动应用</span>
platformBrowserDynamic().bootstrapModule(AppModule);
</code></pre>
<p>Angular2 应用启动后会去解析当前 DOM 树，找出名叫 <code>app-root</code> 的 HTML 标签，然后以这个找出的标签为 Angular2 应用的运行容器。
为此还需要改造 <code>index.html</code> 文件，插入 <code>app-root</code> HTML 标签，代码如下：</p>
<pre><code class="lang-html"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">&quot;UTF-8&quot;</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">app-root</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">app-root</span>&gt;</span>
<span class="hljs-comment">&lt;!--导入 Webpack 输出的 JavaScript 文件--&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">&quot;./dist/bundle.js&quot;</span>&gt;</span><span class="undefined"></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
<p>要让 <code>Hello,Webpack</code> 跑起来，需要安装以下模块：</p>
<pre><code class="lang-bash"><span class="hljs-comment"># Angular2 框架基础核心模块</span>
npm i -S @angular/core @angular/common
<span class="hljs-comment"># Angular2 框架浏览器环境运行库，类似于 react-dom</span>
npm i -S @angular/platform-browser 
<span class="hljs-comment"># 要让 Angular2 正常跑起来所依赖的运行环境和 polyfill</span>
npm i -S core-js rxjs zone.js 
<span class="hljs-comment"># 在浏览器里运行过程中动态的编译 HTML 模版</span>
npm i -S @angular/platform-browser-dynamic @angular/compiler
</code></pre>
<p>以上是一个最小的能正常运行的 Angular2 应用，可见 Angular2 的依赖很多，使用繁杂。</p>
<h2 id="接入-webpack">接入 Webpack</h2>
<p>由于 Angular2 应用采用 TypeScript 开发，构建和前面讲过的<a href="3-2使用TypeScript语言.html">(3.2 使用 TypeScript 语言</a> 类似，不同在于 <code>tsconfig.json</code> 配置。
由于 Angular2 项目中采用了注解的语法，而且 <code>@angular/platform-browser</code> 源码中有许多 DOM 操作，配置需要修改为如下：</p>
<pre><code class="lang-json">{
  <span class="hljs-string">&quot;compilerOptions&quot;</span>: {
    <span class="hljs-string">&quot;target&quot;</span>: <span class="hljs-string">&quot;es5&quot;</span>,
    <span class="hljs-string">&quot;module&quot;</span>: <span class="hljs-string">&quot;commonjs&quot;</span>,
    <span class="hljs-string">&quot;sourceMap&quot;</span>: <span class="hljs-literal">true</span>,
    <span class="hljs-comment">// 开启对 注解 的支持</span>
    <span class="hljs-string">&quot;experimentalDecorators&quot;</span>: <span class="hljs-literal">true</span>,
    <span class="hljs-comment">// Angular2 依赖新的 JavaScript API 和 DOM 操作</span>
    <span class="hljs-string">&quot;lib&quot;</span>: [
      <span class="hljs-string">&quot;es2015&quot;</span>,
      <span class="hljs-string">&quot;dom&quot;</span>
    ]
  },
  <span class="hljs-string">&quot;exclude&quot;</span>: [
    <span class="hljs-string">&quot;node_modules/*&quot;</span>
  ]
}
</code></pre>
<p>其它配置保持和在<a href="3-2使用TypeScript语言.html">3.2 使用 TypeScript 语言</a>的一致，安装好前面提到的 Angular2 框架依赖的模块后，重新执行构建打开网页，你会看到由 Angular2 渲染出来的 <code>Hello,Webpack</code>。</p>
<blockquote>
<p>本实例<a href="http://webpack.wuhaolin.cn/3-8使用Angular2框架.zip" target="_blank">提供项目完整代码</a></p>
</blockquote>

                                
                                