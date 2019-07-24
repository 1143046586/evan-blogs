<h1 id="常用-loaders">常用 Loaders</h1>
<p>本节将对本书用到的及其它常用 Loader 做一个汇总，以方便你在这快速查找到你需要的 Loader。</p>
<h3 id="加载文件">加载文件</h3>
<ul>
<li><strong><a href="https://github.com/webpack-contrib/raw-loader" target="_blank">raw-loader</a></strong>：把文本文件的内容加载到代码中去，在 <a href="../3实战/3-20加载SVG.html">3-20加载SVG</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/file-loader" target="_blank">file-loader</a></strong>：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件，在 <a href="../3实战/3-19加载图片.html">3-19加载图片</a>、<a href="../3实战/3-20加载SVG.html">3-20加载 SVG</a>、<a href="../4优化/4-9CDN加速.html">4-9 CDN 加速</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/url-loader" target="_blank">url-loader</a></strong>：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去，在 <a href="../3实战/3-19加载图片.html">3-19加载图片</a>、<a href="../3实战/3-20加载SVG.html">3-20加载 SVG</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/source-map-loader" target="_blank">source-map-loader</a></strong>：加载额外的 Source Map 文件，以方便断点调试，在 <a href="../3实战/3-21加载SourceMap.html">3-21加载 Source Map</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/svg-inline-loader" target="_blank">svg-inline-loader</a></strong>：把压缩后的 SVG 内容注入到代码中，在 <a href="../3实战/3-20加载SVG.html">3-20加载 SVG</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/node-loader" target="_blank">node-loader</a></strong>：加载 Node.js 原生模块 <code>.node</code> 文件。</li>
<li><strong><a href="https://github.com/tcoopman/image-webpack-loader" target="_blank">image-loader</a></strong>：加载并且压缩图片文件。</li>
<li><strong><a href="https://github.com/webpack-contrib/json-loader" target="_blank">json-loader</a></strong>：加载 JSON 文件。</li>
<li><strong><a href="https://github.com/okonet/yaml-loader" target="_blank">yaml-loader</a></strong>：加载 YAML 文件。</li>
</ul>
<h3 id="编译模版">编译模版</h3>
<ul>
<li><strong><a href="https://github.com/pugjs/pug-loader" target="_blank">pug-loader</a></strong>：把 Pug 模版转换成 JavaScript 函数返回。</li>
<li><strong><a href="https://github.com/pcardune/handlebars-loader" target="_blank">handlebars-loader</a></strong>：把 Handlebars 模版编译成函数返回。 </li>
<li><strong><a href="https://github.com/okonet/ejs-loader" target="_blank">ejs-loader</a></strong>：把 EJS 模版编译成函数返回。</li>
<li><strong><a href="https://github.com/AlexanderPavlenko/haml-loader" target="_blank">haml-loader</a></strong>：把 HAML 代码转换成 HTML。 </li>
<li><strong><a href="https://github.com/peerigon/markdown-loader" target="_blank">markdown-loader</a></strong>：把 Markdown 文件转换成 HTML。</li>
</ul>
<h3 id="转换脚本语言">转换脚本语言</h3>
<ul>
<li><strong><a href="https://github.com/babel/babel-loader" target="_blank">babel-loader</a></strong>：把 ES6 转换成 ES5，在<a href="../3实战/3-1使用ES6语言.html">3-1使用 ES6 语言</a>中有介绍。</li>
<li><strong><a href="https://github.com/TypeStrong/ts-loader" target="_blank">ts-loader</a></strong>：把 TypeScript 转换成 JavaScript，在<a href="../3实战/3-2使用TypeScript语言.html">3-2使用 TypeScript 语言</a>中有遇到。</li>
<li><strong><a href="https://github.com/s-panferov/awesome-typescript-loader" target="_blank">awesome-typescript-loader</a></strong>：把 TypeScript 转换成 JavaScript，性能要比 ts-loader 好。 </li>
<li><strong><a href="https://github.com/webpack-contrib/coffee-loader" target="_blank">coffee-loader</a></strong>：把 CoffeeScript 转换成 JavaScript。</li>
</ul>
<h3 id="转换样式文件">转换样式文件</h3>
<ul>
<li><strong><a href="https://github.com/webpack-contrib/css-loader" target="_blank">css-loader</a></strong>：加载 CSS，支持模块化、压缩、文件导入等特性。</li>
<li><strong><a href="https://github.com/webpack-contrib/style-loader" target="_blank">style-loader</a></strong>：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。</li>
<li><strong><a href="https://github.com/webpack-contrib/sass-loader" target="_blank">sass-loader</a></strong>：把 SCSS/SASS 代码转换成 CSS，在<a href="../3实战/3-4使用SCSS语言.html">3-4使用 SCSS 语言</a>中有介绍。</li>
<li><strong><a href="https://github.com/postcss/postcss-loader" target="_blank">postcss-loader</a></strong>：扩展 CSS 语法，使用下一代 CSS，在<a href="../3实战/3-5使用PostCSS.html">3-5使用 PostCSS</a>中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/less-loader" target="_blank">less-loader</a></strong>：把 Less 代码转换成 CSS 代码。</li>
<li><strong><a href="https://github.com/shama/stylus-loader" target="_blank">stylus-loader</a></strong>：把 Stylus 代码转换成 CSS 代码。</li>
</ul>
<h3 id="检查代码">检查代码</h3>
<ul>
<li><strong><a href="https://github.com/MoOx/eslint-loader" target="_blank">eslint-loader</a></strong>：通过 ESLint 检查 JavaScript 代码，在 <a href="../3实战/3-16检查代码.html">3-16检查代码</a>中有介绍。</li>
<li><strong><a href="https://github.com/wbuchwalter/tslint-loader" target="_blank">tslint-loader</a></strong>：通过 TSLint 检查 TypeScript 代码。</li>
<li><strong><a href="https://github.com/webpack-contrib/mocha-loader" target="_blank">mocha-loader</a></strong>：加载 Mocha 测试用例代码。</li>
<li><strong><a href="https://github.com/webpack-contrib/coverjs-loader" target="_blank">coverjs-loader</a></strong>：计算测试覆盖率。</li>
</ul>
<h3 id="其它">其它</h3>
<ul>
<li><strong><a href="https://github.com/vuejs/vue-loader" target="_blank">vue-loader</a></strong>：加载 Vue.js 单文件组件，在<a href="../3实战/3-7使用Vue框架.html">3-7使用 Vue 框架</a>中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/i18n-loader" target="_blank">i18n-loader</a></strong>：加载多语言版本，支持国际化。</li>
<li><strong><a href="https://github.com/cherrry/ignore-loader" target="_blank">ignore-loader</a></strong>：忽略掉部分文件，在<a href="../3实战/3-11构建同构应用.html">3-11构建同构应用</a>中有介绍。</li>
<li><strong><a href="https://github.com/gwuhaolin/ui-component-loader" target="_blank">ui-component-loader</a></strong>：按需加载 UI 组件库，例如在使用 antd UI 组件库时，不会因为只用到了 Button 组件而打包进所有的组件。</li>
</ul>

                                
                                