<h1 id="2-1-entry">2-1 Entry</h1>
<p><code>entry</code>是配置模块的入口，可抽象成输入，Webpack 执行构建的第一步将从入口开始搜寻及递归解析出所有入口依赖的模块。</p>
<p><code>entry</code> 配置是<strong>必填的</strong>，若不填则将导致 Webpack 报错退出。</p>
<h2 id="context">context</h2>
<p>Webpack 在寻找相对路径的文件时会以 <code>context</code> 为根目录，<code>context</code> 默认为执行启动 Webpack 时所在的当前工作目录。
如果想改变 <code>context</code> 的默认配置，则可以在配置文件里这样设置它：</p>
<pre><code class="lang-js"><span class="hljs-built_in">module</span>.exports = {
  context: path.resolve(__dirname, <span class="hljs-string">&apos;app&apos;</span>)
}
</code></pre>
<p>注意， <code>context</code> 必须是一个绝对路径的字符串。
除此之外，还可以通过在启动 Webpack 时带上参数 <code>webpack --context</code> 来设置 <code>context</code>。</p>
<p>之所以在这里先介绍 <code>context</code>，是因为 Entry 的路径和其依赖的模块的路径可能采用相对于 <code>context</code> 的路径来描述，<code>context</code> 会影响到这些相对路径所指向的真实文件。</p>
<h2 id="entry-类型">Entry 类型</h2>
<p>Entry 类型可以是以下三种中的一种或者相互组合：</p>
<table>
<thead>
<tr>
<th>类型</th>
<th>例子</th>
<th>含义</th>
</tr>
</thead>
<tbody>
<tr>
<td>string</td>
<td><code>&apos;./app/entry&apos;</code></td>
<td>入口模块的文件路径，可以是相对路径。</td>
</tr>
<tr>
<td>array</td>
<td><code>[&apos;./app/entry1&apos;, &apos;./app/entry2&apos;]</code></td>
<td>入口模块的文件路径，可以是相对路径。</td>
</tr>
<tr>
<td>object</td>
<td><code>{ a: &apos;./app/entry-a&apos;, b: [&apos;./app/entry-b1&apos;, &apos;./app/entry-b2&apos;]}</code></td>
<td>配置多个入口，每个入口生成一个 Chunk</td>
</tr>
</tbody>
</table>
<p>如果是 <code>array</code> 类型，则搭配 <code>output.library</code> 配置项使用时，只有数组里的最后一个入口文件的模块会被导出。</p>
<h2 id="chunk-名称">Chunk 名称</h2>
<p>Webpack 会为每个生成的 Chunk 取一个名称，Chunk 的名称和 Entry 的配置有关：</p>
<ul>
<li>如果 <code>entry</code> 是一个 <code>string</code> 或 <code>array</code>，就只会生成一个 Chunk，这时 Chunk 的名称是 <code>main</code>；</li>
<li>如果 <code>entry</code> 是一个 <code>object</code>，就可能会出现多个 Chunk，这时 Chunk 的名称是 <code>object</code> 键值对里键的名称。</li>
</ul>
<h2 id="配置动态-entry">配置动态 Entry</h2>
<p>假如项目里有多个页面需要为每个页面的入口配置一个 Entry ，但这些页面的数量可能会不断增长，则这时 Entry 的配置会受到到其他因素的影响导致不能写成静态的值。其解决方法是把 Entry 设置成一个函数去动态返回上面所说的配置，代码如下：</p>
<pre><code class="lang-js"><span class="hljs-comment">// 同步函数</span>
entry: () =&gt; {
  <span class="hljs-keyword">return</span> {
    a:<span class="hljs-string">&apos;./pages/a&apos;</span>,
    b:<span class="hljs-string">&apos;./pages/b&apos;</span>,
  }
};
<span class="hljs-comment">// 异步函数</span>
entry: () =&gt; {
  <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> <span class="hljs-built_in">Promise</span>((resolve)=&gt;{
    resolve({
       a:<span class="hljs-string">&apos;./pages/a&apos;</span>,
       b:<span class="hljs-string">&apos;./pages/b&apos;</span>,
    });
  });
};
</code></pre>

                                
                                