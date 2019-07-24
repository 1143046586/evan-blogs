<div data-v-4f8894a8="" data-id="5ce155af6fb9a07ed136a5af" itemprop="articleBody" class="article-content"><p></p><figure><img alt="banner" class="lazyload inited loaded" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acfee7031de2c7?imageslim" data-width="668" data-height="630" src="https://user-gold-cdn.xitu.io/2019/5/19/16acfee7031de2c7?imageslim"><figcaption></figcaption></figure><p></p>
<h1 class="heading" data-id="heading-0">situation</h1>
<p>一个常见的的场景，</p>
<p>主页 <ruby>--&gt;<rt>前进</rt></ruby> 列表页<ruby>--&gt;<rt>前进</rt></ruby> 详情页，详情页 <ruby>--&gt;<rt>返回</rt></ruby> 主页 <ruby>--&gt;<rt>返回</rt></ruby> 列表页</p>
<p>我们希望，</p>
<p>从 详情页 <ruby>--&gt;<rt>返回</rt></ruby> 列表页 的时候页面的状态是缓存，不用重新请求数据，提升用户体验。</p>
<p>从 列表页 <ruby>--&gt;<rt>返回</rt></ruby> 主页 的时候页面，注销掉列表页，以在进入不同的列表页的时候，获取最新的数据。</p>
<h1 class="heading" data-id="heading-1">task</h1>
<p>今天 让我们来实现这个需求。</p>
<p>在 代码的世界里 解决问题的 方法 从来都不止一种。</p>
<p>比如，从数组中找到一个的值索引：</p>
<p>可以用最基础的 for循环 遍历，也可以用indexOf这种工具函数，还可以用findIndex forEach等更语义化的高阶函数来遍历。</p>
<p>我参考了大量的文章，爬了官方的文档，之后，找到了好几种办法，下面，让我们来一个个分析。</p>
<p><strong>let's begin</strong> ， 我们开始吧。🙌</p>
<ol>
<li>
<p>解决掉提需求的人</p>
<p>😂 emm... ，相对于我们的人生，可能代码会更好掌控吧。</p>
<blockquote>
<p>程序员活在自己想象的王国里</p>
<p>我刚接触电脑就发现电脑的妙处，电脑远没有人那么复杂。如果你的程序写得好，你就可以和电脑处好关系，就可以指挥电脑干你
想干的事。这个时候你是十足的主宰。 每每你坐在电脑面前，你就是在你的王国里巡行，这样的日子简直就是天堂般的日子。
电脑里的世界很大，编程人是活在自己想象的王国里。你可以想象到电脑里细微到每一个字节、每一个比特的东西。</p>
</blockquote>
<blockquote>
<p>-- 雷军(没错就是小米那个雷军)</p>
</blockquote>
<p>我很认同雷军的说法，</p>
<p>人生能找到一个 热爱 的东西很难得，你 热爱代码 并 专注于它的话，</p>
<p>终有一天，</p>
<p>它会以各种各样的方式回报给你：工资，认同感，成就感，和你一起进步，心里共鸣的人。</p>
<p>你若盛开，蝴蝶自来。诸君共勉 (能骗一个相信，就少一个沙雕和我抢妹子🤣)</p>
</li>
</ol>
<p></p><figure><img alt="沙雕" class="lazyload inited loaded" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acfef91567de63?imageslim" data-width="160" data-height="118" src="https://user-gold-cdn.xitu.io/2019/5/19/16acfef91567de63?imageslim"><figcaption></figcaption></figure><p></p>
<ol start="2">
<li>
<p>睡服♂提需求的人</p>
<p>😂emm..., 看着镜子里那张的脸，这辈子应该是没办法从脸上得到任何的便利了...</p>
</li>
</ol>
<p></p><figure><img alt="富婆" class="lazyload inited loaded" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acff5d30ff80d6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="469" data-height="158" src="https://user-gold-cdn.xitu.io/2019/5/19/16acff5d30ff80d6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1"><figcaption></figcaption></figure><p></p>
<ol start="3">
<li>
<p>keep-alive</p>
<p><code>keep-alive</code>是<code>Vue</code>提供的一个抽象组件，主要用于保留组件状态或避免重新渲染。</p>
<p></p><figure><img class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acff709701145a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="593" data-height="74" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;593&quot; height=&quot;74&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p>但是 <code>keep-alive</code>会把其包裹的所有组件都缓存起来。</p>
</li>
</ol>
<h1 class="heading" data-id="heading-2">action</h1>
<p>我们把需求分解成2步来实现</p>
<h2 class="heading" data-id="heading-3">1.把需要缓存和不需要缓存的视图组件区分开</h2>
<p>思路如下图:</p>
<p></p><figure><img alt="router区分" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acffc9192eb757?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="1189" data-height="714" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;1189&quot; height=&quot;714&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<ol>
<li>写2个 <code>router-view</code> 出口</li>
</ol>
<pre><code class="hljs html copyable" lang="html"><span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span>&gt;</span>
    <span class="hljs-comment">&lt;!-- 需要缓存的视图组件 --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">router-view</span> <span class="hljs-attr">v-if</span>=<span class="hljs-string">"$route.meta.keepAlive"</span>&gt;</span>
  <span class="hljs-tag">&lt;/<span class="hljs-name">router-view</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span>

<span class="hljs-comment">&lt;!-- 不需要缓存的视图组件 --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">router-view</span> <span class="hljs-attr">v-if</span>=<span class="hljs-string">"!$route.meta.keepAlive"</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">router-view</span>&gt;</span>
<span class="copy-code-btn">复制代码</span></code></pre><ol start="2">
<li>在<code>Router</code>里定义好需要缓存的视图组件</li>
</ol>
<pre><code class="hljs js copyable" lang="js"><span class="hljs-keyword">new</span> Router({
    <span class="hljs-attr">routes</span>: [
        {
            <span class="hljs-attr">path</span>: <span class="hljs-string">'/'</span>,
            <span class="hljs-attr">name</span>: <span class="hljs-string">'index'</span>,
            <span class="hljs-attr">component</span>: <span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> <span class="hljs-keyword">import</span>(<span class="hljs-string">'./views/keep-alive/index.vue'</span>),
            <span class="hljs-attr">meta</span>: {
                <span class="hljs-attr">deepth</span>: <span class="hljs-number">0.5</span>
            }
        },
        {
            <span class="hljs-attr">path</span>: <span class="hljs-string">'/list'</span>,
            <span class="hljs-attr">name</span>: <span class="hljs-string">'list'</span>,
            <span class="hljs-attr">component</span>: <span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> <span class="hljs-keyword">import</span>(<span class="hljs-string">'./views/keep-alive/list.vue'</span>),
            <span class="hljs-attr">meta</span>: {
                <span class="hljs-attr">deepth</span>: <span class="hljs-number">1</span>
                keepAlive: <span class="hljs-literal">true</span> <span class="hljs-comment">//需要被缓存</span>
            }
        },
        {
            <span class="hljs-attr">path</span>: <span class="hljs-string">'/detail'</span>,
            <span class="hljs-attr">name</span>: <span class="hljs-string">'detail'</span>,
            <span class="hljs-attr">component</span>: <span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> <span class="hljs-keyword">import</span>(<span class="hljs-string">'./views/keep-alive/detail.vue'</span>),
            <span class="hljs-attr">meta</span>: {
                <span class="hljs-attr">deepth</span>: <span class="hljs-number">2</span>
            }
        }
    ]
})
<span class="copy-code-btn">复制代码</span></code></pre><h2 class="heading" data-id="heading-4">2.按需keep-alive</h2>
<p></p><figure><img alt="keep-alive" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad00649bb73773?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="555" data-height="177" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;555&quot; height=&quot;177&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p></p><figure><img alt="include" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad008732ddf825?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="577" data-height="64" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;577&quot; height=&quot;64&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p>我们从官方文档提供的api入手,</p>
<p><code>keep-alive</code>组件如果设置了 <code>include</code> ，就只有和 <code>include</code> 匹配的组件会被缓存，</p>
<p>所以思路就是，动态修改 <code>include</code> 数组来实现按需缓存。</p>
<p></p><figure><img alt="include" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad009cfdc4879b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="1028" data-height="508" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;1028&quot; height=&quot;508&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<pre><code class="hljs html copyable" lang="html"><span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span> <span class="hljs-attr">:include</span>=<span class="hljs-string">"include"</span>&gt;</span>
    <span class="hljs-comment">&lt;!-- 需要缓存的视图组件 --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">router-view</span> <span class="hljs-attr">v-if</span>=<span class="hljs-string">"$route.meta.keepAlive"</span>&gt;</span>
  <span class="hljs-tag">&lt;/<span class="hljs-name">router-view</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span>

<span class="hljs-comment">&lt;!-- 不需要缓存的视图组件 --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">router-view</span> <span class="hljs-attr">v-if</span>=<span class="hljs-string">"!$route.meta.keepAlive"</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">router-view</span>&gt;</span>
<span class="copy-code-btn">复制代码</span></code></pre><p>让我们在app.vue里监听路由的变化,</p>
<pre><code class="hljs js copyable" lang="js"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  <span class="hljs-attr">name</span>: <span class="hljs-string">"app"</span>,
  <span class="hljs-attr">data</span>: <span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> ({
    <span class="hljs-attr">include</span>: []
  }),
  <span class="hljs-attr">watch</span>: {
    $route(to, <span class="hljs-keyword">from</span>) {
      <span class="hljs-comment">//如果 要 to(进入) 的页面是需要 keepAlive 缓存的，把 name push 进 include数组</span>
      <span class="hljs-keyword">if</span> (to.meta.keepAlive) {
        !<span class="hljs-keyword">this</span>.include.includes(to.name) &amp;&amp; <span class="hljs-keyword">this</span>.include.push(to.name);
      }
      <span class="hljs-comment">//如果 要 form(离开) 的页面是 keepAlive缓存的，</span>
      <span class="hljs-comment">//再根据 deepth 来判断是前进还是后退</span>
      <span class="hljs-comment">//如果是后退</span>
      <span class="hljs-keyword">if</span> (<span class="hljs-keyword">from</span>.meta.keepAlive &amp;&amp; to.meta.deepth &lt; <span class="hljs-keyword">from</span>.meta.deepth) {
        <span class="hljs-keyword">var</span> index = <span class="hljs-keyword">this</span>.include.indexOf(<span class="hljs-keyword">from</span>.name);
        index !== <span class="hljs-number">-1</span> &amp;&amp; <span class="hljs-keyword">this</span>.include.splice(index, <span class="hljs-number">1</span>);
      }
    }
  }
};
<span class="copy-code-btn">复制代码</span></code></pre><p>需要注意的是
</p><figure><img class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad018aed5b0a02?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="626" data-height="99" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;626&quot; height=&quot;99&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure>
<code>keep-alive</code>需要其包裹的组件有name属性，<p></p>
<p>我们上面的代码中的 <code>push</code> 和 <code>splice</code> 的是 <code>router</code> 的 <code>name</code>。</p>
<p>所以建议大家把 <code>route</code> 的 <code>name</code>属性设置和 <code>route</code>对应<code>component</code>的<code>name</code>设成一样的。</p>
<p></p><figure><img alt="就是这个意思" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad01ec0d57e21d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="1280" data-height="458" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;1280&quot; height=&quot;458&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<h1 class="heading" data-id="heading-5">result</h1>
<p>让我们验证一下我们的成果</p>
<p></p><figure><img alt="banner" class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16acfee7031de2c7?imageslim" data-width="668" data-height="630" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;668&quot; height=&quot;630&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p></p><figure><img class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad0240ea84a2e8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="691" data-height="407" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;691&quot; height=&quot;407&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p>在 <a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvue-devtools" rel="nofollow noopener noreferrer">vue-devtool</a> 里，灰色的组件，代表是缓存状态的组件，注意观察<code>list</code>的变化。</p>
<h1 class="heading" data-id="heading-6">写在最后</h1>
<ol>
<li>实现按需 <code>keep-alive</code> ，网上有方法，通过修改 <code>route</code> 配置里的 <code>meta</code>里的 <code>keepAlive</code> 值来实现。</li>
</ol>
<p></p><figure><img class="lazyload inited" data-src="https://user-gold-cdn.xitu.io/2019/5/19/16ad02b3cd17da4f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" data-width="972" data-height="697" src="data:image/svg+xml;utf8,&lt;?xml version=&quot;1.0&quot;?&gt;&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; version=&quot;1.1&quot; width=&quot;972&quot; height=&quot;697&quot;&gt;&lt;/svg&gt;"><figcaption></figcaption></figure><p></p>
<p>直接修改 <code>meta</code> 的值，可能会出现上图的情况，keep-alive里有一直有一个缓存的 list,正常的 <code>rotuer-view</code> 里也有一个,</p>
<p>复现这个问题需要很长得篇幅，感兴趣的朋友可以自己去爬一下坑。</p>
<ol start="2">
<li>还有得方法是 通过在keep-alive 的视图组件在退出 rotuer 的时候，调用this.$destory() 直接摧毁组件，这会导致组件没法在缓存，这个bug ，在官方issue有提到。</li>
</ol>
<h1 class="heading" data-id="heading-7">参考资料</h1>
<blockquote>
<p><a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvue%2Fissues%2F6509" rel="nofollow noopener noreferrer">官方issue</a></p>
</blockquote>
<blockquote>
<p><a target="_blank" href="https://juejin.im/post/5b5eb85d6fb9a04fb745e8f8" rel="">Vue项目全局配置页面缓存，实现按需读取缓存
</a></p>
</blockquote>
<h1 class="heading" data-id="heading-8">关于动画</h1>
<p>谢谢大家的点赞和回复，我看好多人想要demo的源码这里分享一下。</p>
<p>因为我公司目前还没有,vue的项目给我施展拳脚。我当时写demo的时候，想的也不是动画方面的封装和优化，</p>
<p>所以分享的demo里的代码里，关于动画，会有很多重复的代码和丑陋的实现。还请多见谅</p>
<p><a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Fshare.weiyun.com%2F5XiSQDy" rel="nofollow noopener noreferrer">源码链接</a></p>
<p>关于demo动画的一些分析，可以看我的前2篇文章</p>
</div>