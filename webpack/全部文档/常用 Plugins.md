<h1 id="常用-plugins">常用 Plugins</h1>
<p>本节将对本书用到的及其它常用 Plugin 做一个汇总，以方便你在这快速查找到你需要的 Plugin。</p>
<h3 id="用于修改行为">用于修改行为</h3>
<ul>
<li><strong><a href="https://webpack.js.org/plugins/define-plugin/" target="_blank">define-plugin</a></strong>：定义环境变量，在<a href="../4优化/4-7区分环境.html">4-7区分环境</a>中有介绍。</li>
<li><strong><a href="https://webpack.js.org/plugins/context-replacement-plugin/" target="_blank">context-replacement-plugin</a></strong>：修改 <code>require</code> 语句在寻找文件时的默认行为。</li>
<li><strong><a href="https://webpack.js.org/plugins/ignore-plugin/" target="_blank">ignore-plugin</a></strong>：用于忽略部分文件。</li>
</ul>
<h3 id="用于优化">用于优化</h3>
<ul>
<li><strong><a href="https://webpack.js.org/plugins/commons-chunk-plugin/" target="_blank">commons-chunk-plugin</a></strong>：提取公共代码，在<a href="../4优化/4-11提取公共代码.html">4-11提取公共代码</a>中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/extract-text-webpack-plugin" target="_blank">extract-text-webpack-plugin</a></strong>：提取 JavaScript 中的 CSS 代码到单独的文件中，在<a href="../1入门/1-5使用Plugin.html">1-5使用 Plugin</a> 中有介绍。 </li>
<li><strong><a href="https://github.com/gajus/prepack-webpack-plugin" target="_blank">prepack-webpack-plugin</a></strong>：通过 Facebook 的 Prepack 优化输出的 JavaScript 代码性能，在 <a href="../4优化/4-13使用Prepack.html">4-13使用 Prepack</a> 中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/uglifyjs-webpack-plugin" target="_blank">uglifyjs-webpack-plugin</a></strong>：通过 UglifyES 压缩 ES6 代码，在 <a href="../4优化/4-8压缩代码.html">4-8压缩代码</a>中有介绍。</li>
<li><strong><a href="https://github.com/gdborton/webpack-parallel-uglify-plugin" target="_blank">webpack-parallel-uglify-plugin</a></strong>：多进程执行 UglifyJS 代码压缩，提升构建速度。</li>
<li><strong><a href="https://www.npmjs.com/package/imagemin-webpack-plugin" target="_blank">imagemin-webpack-plugin</a></strong>：压缩图片文件。</li>
<li><strong><a href="https://www.npmjs.com/package/webpack-spritesmith" target="_blank">webpack-spritesmith</a></strong>：用插件制作雪碧图。</li>
<li><strong><a href="https://webpack.js.org/plugins/module-concatenation-plugin/" target="_blank">ModuleConcatenationPlugin</a></strong>：开启 Webpack Scope Hoisting 功能，在<a href="../4优化/4-14开启ScopeHoisting.html">4-14开启 ScopeHoisting</a>中有介绍。 </li>
<li><strong><a href="https://webpack.js.org/plugins/dll-plugin/" target="_blank">dll-plugin</a></strong>：借鉴 DDL 的思想大幅度提升构建速度，在<a href="../4优化/4-2使用DllPlugin.html">4-2使用 DllPlugin</a>中有介绍。</li>
<li><strong><a href="https://webpack.js.org/plugins/hot-module-replacement-plugin/" target="_blank">hot-module-replacement-plugin</a></strong>：开启模块热替换功能。</li>
</ul>
<h3 id="其它">其它</h3>
<ul>
<li><strong><a href="https://github.com/oliviertassinari/serviceworker-webpack-plugin" target="_blank">serviceworker-webpack-plugin</a></strong>：给网页应用增加离线缓存功能，在<a href="../3实战/3-14构建离线应用.html">3-14 构建离线应用</a>中有介绍。</li>
<li><strong><a href="https://github.com/JaKXz/stylelint-webpack-plugin" target="_blank">stylelint-webpack-plugin</a></strong>：集成 stylelint 到项目中，在<a href="../3实战/3-16检查代码.html">3-16检查代码</a>中有介绍。</li>
<li><strong><a href="https://github.com/webpack-contrib/i18n-webpack-plugin" target="_blank">i18n-webpack-plugin</a></strong>：给你的网页支持国际化。</li>
<li><strong><a href="https://webpack.js.org/plugins/provide-plugin/" target="_blank">provide-plugin</a></strong>：从环境中提供的全局变量中加载模块，而不用导入对应的文件。</li>
<li><strong><a href="https://github.com/gwuhaolin/web-webpack-plugin" target="_blank">web-webpack-plugin</a></strong>：方便的为单页应用输出 HTML，比 html-webpack-plugin 好用。</li>
</ul>

                                
                                