# 前端101
[MZhou](https://github.com/zmmbreeze) / [@mmzhou](http://twitter.com/mmzhou)


---


![](./demo/timing.png)
--
- Stalled / Blocking - 请求发起之前等待的时间总和。包含了用于处理代理的时间。另外，如果有已经建立好的连接，那么这个时间还包括等待已建立连接被复用的时间，这个遵循 Chrome 对同一源最大6个TCP连接的规则。<a href="http://fex.baidu.com/blog/2015/01/chrome-stalled-problem-resolving-process/" target="_blank">Chrome的bug导致stalled了21秒</a>
- Proxy Negotiation - 处理代理的时间
- DNS Lookup - 查找DNS的时间。页面上每个新的域都需要一次完整的寻路来完成DNS查找
- Initial Connection / Connecting - 建立链接的时间，包括TCP三次握手及重试握手，还有处理SSL
- SSL - 处理 SSL 握手
- Request Sent / Sending - 请求发送的时间
- Waiting (<abbr title="Time To First Byte">TTFB</abbr>) - Time To First Byte，等待首个字节返回的时间
- Content Download / Downloading - 全部内容下载完成的时间
--
### [Performance API](http://javascript.ruanyifeng.com/bom/performance.html)
--
### 多普勒测速

[![](./demo/doppler.png)](http://velocity.oreilly.com.cn/2011/ppts/MobilePerformanceVelocity2011_DavidWei.pdf)
    <!-- Round trip time (RTT)  -->
--
### [Yahoo的军规](https://developer.yahoo.com/performance/rules.html)
--
<pre><code>&lt;!-- BAD: 会block外部script请求 --&gt;
&lt;script src="//example.com/test.js"&gt;&lt;/script&gt;

&lt;!-- GOOD: 外部script会异步加载 --&gt;
&lt;script&gt;
    var script = document.createElement('script');
    script.src = "//example.com/test.js";
    document.getElementsByTagName('head')[0].appendChild(script);
&lt;/script&gt;</code></pre>
<!-- https://www.igvita.com/2014/05/20/script-injected-async-scripts-considered-harmful/ -->
--
<pre><code>&lt;link href="http://example.com/test.css?rtt=2" rel="stylesheet"&gt;
&lt;!-- body 内容 --&gt;
&lt;script&gt;
var script = document.createElement('script');
script.src = "http://example.com/test.js?rtt=1&a";
document.getElementsByTagName('head')[0].appendChild(script);
&lt;/script&gt;

&lt;script&gt;
var script = document.createElement('script');
script.src = "http://example.com/test.js?rtt=1&b";
document.getElementsByTagName('head')[0].appendChild(script);
&lt;/script&gt;</code></pre>
--
[![](./demo/script-1.jpeg)](http://output.jsbin.com/qefefiyi/9/quiet)

Javascript 可以操作 CSSOM，所以需要等到 css 完全加载解析完毕之后才能执行 script 标签
<!-- .element: class="fragment" data-fragment-index="1" -->
--
<pre><code>&lt;link href="http://example.com/test.css?rtt=2" rel="stylesheet"&gt;
&lt;!-- body 内容 --&gt;
&lt;script src="http://example.com/test.js?rtt=1&a"&gt;&lt;/script&gt;
&lt;script src="http://example.com/test.js?rtt=1&b" &gt;&lt;/script&gt;</code></pre>
--
[![](./demo/script-2.jpeg)](http://output.jsbin.com/qefefiyi/8/quiet)

浏览器（包括 IE8/9 和 Android 2.3/2.2）会预解析查找可以下载的外部文件，并行下载，串行执行
<!-- .element: class="fragment" data-fragment-index="1" -->
--
<pre><code>&lt;!-- 现代浏览器使用 'async'，旧IE使用 'defer' --&gt;
&lt;script src="//somehost.com/awesome-widget.js" async defer&gt;&lt;/script&gt;</code></pre>

并行下载，不会 block DOM 不能确保执行顺序
<!-- .element: class="fragment" data-fragment-index="1" -->
--
No Rules! Use Tools!



---


## HTML语义化
--
- p - <small>段落</small>
- h1,h2,h3,h4,h5,h6 - <small>层级标题</small>
- strong,em - <small>强调</small>
- ins - <small>插入</small>
- del - <small>删除</small>
- abbr - <small>缩写</small>
- code - <small>代码标识</small>
- cite - <small>引述来源作品的标题</small>
- q - <small>引用</small>
- blockquote - <small>一段或长篇引用</small>
- ul - <small>无序列表</small>
- ol - <small>有序列表</small>
- dl,dt,dd - <small>定义列表</small>
- table - <small>表格，列表</small>
--
- article - <small>独立的文档、页面、应用、站点</small>
- section - <small>按主题将内容分组，层级标题，并非「语义化的 div」。当你希望这个元素的内容体现在文档的提纲 (outline) 中时，用 section 是合适的。</small>
- nav - <small>导航</small>
- aside - <small>与周围内容关系不太密切的内容 (eg. 广告 / 侧边栏内容）</small>
- header - <small>一组介绍性描述或导航信息（eg. 目录 / 搜索框 / logo）</small>
- footer - <small>底部元素，代表最近的父级区块内容的页脚</small>
- address - <small>联系人信息</small>
<!-- 基本拷贝于 http://justineo.github.io/slideshows/semantic-html/#/6/1 ，感谢E0大大的整理 -->

[更多详细信息](http://justineo.github.io/slideshows/semantic-html/#/6/1)
--
## CSS、JAVASCRIPT的引入
--
<pre><code>&lt;link rel="stylesheet" href="page.css"/&gt;
&lt;script src="page.js"&gt;&lt;/script&gt;</code></pre>
--
### protocol-relative URL

<pre><code>&lt;link rel="stylesheet" href="//qq.com/page.css"&gt;</code></pre>

使用 protocol-relative URL 引入 CSS，在 IE7/8 下，会发两次请求。<!-- .element: class="fragment" data-fragment-index="1" -->


---


## JAVASCRIPT 单线程
--
![](./demo/javascript-single.jpg)
--
```
setTimeout(function () {
    console.log(1);
}, 0);
console.log(2);
```


---


## [11年 Twitter 改版](http://ejohn.org/blog/learning-from-twitter/)
引入了无线滚动（infinite scroll）后页面滚动时速度变的很慢！<!-- .element: class="fragment" data-fragment-index="1" -->
--
jQuery 1.4.2 升级到 1.4.4
--
### 定位bug

```
$(window).bind('scroll', function () {
    if (nearBottomOfPage()) {
        // load more tweets ...
    }
});
```

```
$details.find('.details-pane-outer');
```
<!-- .element: class="fragment" data-fragment-index="1" -->
--
jQuery 1.4.3开始选择器引擎 Sizzle 会优先使用 `querySelectorAll`
--
```
// 1.4.2
document.getElementsByClassName('details-pane-outer');
// 1.4.4
document.querySelectorAll('details-pane-outer');</code></pre>
```
--
```
var divs = document.getElementsByTagName("div");
var i = 0;

while(i < divs.length){
    document.body.appendChild(document.createElement("div"));
    i++;
}
```

这是一个死循环！<!-- .element: class="fragment" data-fragment-index="1" -->
--
- Live NodeList 快
- Static NodeList 慢
--
[为什么 `getElementsByTagName()` 比 `querySelectorAll()` 快？](https://www.nczonline.net/blog/2010/09/28/why-is-getelementsbytagname-faster-that-queryselectorall/)
--
- DOM 查询结果需要重用时一定要缓存
- 绑定重复触发的事件时（例如window scroll 事件）一定要做 throttle 或 debounce


---


## throttle 和 debounce
--
- [debounce](http://underscorejs.org/#debounce) - 阻止事件触发直到N段时间后
- [throttle](http://underscorejs.org/#throttle) - 限制事件触发频率
--
<p data-height="268" data-theme-id="20219" data-slug-hash="GpyXxV" data-default-tab="result" data-user="zmmbreeze" class='codepen'>See the Pen <a href='http://codepen.io/zmmbreeze/pen/GpyXxV/'>The Difference Between Throttling, Debouncing, and Neither</a> by mzhou (<a href='http://codepen.io/zmmbreeze'>@zmmbreeze</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
--


---


## 记录页面的 `A` 标签的点击事件
```
$('a').click(function () {
    // 记录操作
});
```
<!-- .element: class="fragment" data-fragment-index="1" -->
```
var links = document.getElementsByTagName('a');
for (var i = 0, l = links.length; i < l; i++) {
    links[i].onclick = (function (link) {
        // 记录操作
    })(links[i])
}
```
<!-- .element: class="fragment" data-fragment-index="2" -->

链接多了之后很慢！
<!-- .element: class="fragment" data-fragment-index="3" -->
--
### 事件代理
--
### 捕获与冒泡
![](./demo/delegate.png)
--
```
document.body.addEventListener('click', function (e) {
    e.preventDefault();
    var target = e.target;
    var isLink = target.nodeName === 'A';
    if (isLink) {
        // 记录操作
    }
}, false);
```
```
$('body').on('click', 'a', function () {
    // 记录操作
});
```
<!-- .element: class="fragment" data-fragment-index="1" -->
--
### 优势

- 能处理动态更新的DOM元素
- DOM元素很多时，有性能优势
--


---


## 字符串拼接
--
```
var result = ''
    + '<h1>' + title + '</h1>'
    + '<p>' + content + '</p>';</code></pre>
```

```
var result = [
    '<h1>' + title + '</h1>',
    '<p>' + content + '</p>'
].join('');
```
<!-- .element: class="fragment" data-fragment-index="1" -->
--
`Array.join` > +操作符

只是在旧浏览器下（IE7-），现代浏览器中差不多
<!-- .element: class="fragment" data-fragment-index="1" -->

[性能测试](http://jsperf.com/connect-string-array-with-join-and-loop)
<!-- .element: class="fragment" data-fragment-index="2" -->
--
`String.replace` 或者模板引擎更好

```
var tpl = ''
    + '<h1>{title}</h1>'
    + '<p>{content}</p>';

var template = function (tpl, data) {
    return tpl.replace(/{([^}]+)}/g, function (r, $0) {
        return data[$0] || '';
    });
};

var result = template(tpl, {
    title: '标题',
    content: '内容'
});
```
--
### 优势

- 直观可读性好
- 模板字符串可复用
- 模板默认提供转义，更安全
- JS压缩引擎可以合并字符串，压缩后没有 `+` 操作符
--
### ES2015 / ES6 的模板字符串
```
var data = {
    title: '标题',
    content: '内容'
};

var result = `<h1>${data.title}</h1>
<p>${data.content}</p>`;
```


---


## 正则表达式
--
- [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [String.prototype.match()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)
- [String.prototype.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
- [RegExp.prototype.exec()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)


---


## DOM 操作费时
--
### `repaint` 与 `relayout / reflow`
![](./demo/webkitflow.png)
![](./demo/geckoflow.jpg)
<!-- http://www.phpied.com/rendering-repaint-reflowrelayout-restyle/ -->
<!-- http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/ -->
--
- `relayout / reflow` - 重新计算节点的位置
- `repaint` - 重新绘制节点到屏幕上
<!-- relayout 之后一定触发 repaint -->
--
### layout thrashing
![](layoutthrashing.png)
--
```
// 读
var height = element.clientHeight;
// 写
element.style.height = (height + 2) + 'px';
// 写
element.style.height = (height + 3) + 'px';
```

浏览器优化之后，会把DOM操作放到一个队列里面。在将来的某个时候执行（例如屏幕刷新前）
<!-- .element: class="fragment" data-fragment-index="1" -->
--
```
// 读
var height1 = element1.clientHeight;
// 写
element1.style.height = (height1 + 2) + 'px';
// 读
var height2 = element2.clientHeight;
// 写
element2.style.height = (height2 + 2) + 'px';
```

此时第二次读操作执行前，会强迫浏览器 relayout 一次，这样才能计算得到新的 `height`
<!-- .element: class="fragment" data-fragment-index="1" -->
--
```
// 读
var height1 = element1.clientHeight;
// 读
var height2 = element2.clientHeight;
// 写
element1.style.height = (height1 + 2) + 'px';
// 写
element2.style.height = (height2 + 2) + 'px';
```

读写分离，减少 layout thrashing。但是显示很骨感，不一定DOM操作都在一个方法体里面
--
```
// 读
var height1 = element1.clientHeight;
// 写
requestAnimationFrame(function() {
    element1.style.height = (height1 + 2) + 'px';
});
// 读
var height2 = element2.clientHeight;
// 写
requestAnimationFrame(function() {
    element2.style.height = (height2 + 2) + 'px';
});
```
<!-- http://wilsonpage.co.uk/preventing-layout-thrashing/ -->
[Fast dom](https://github.com/wilsonpage/fastdom)
--
话说回来，一般调用没那么频繁。只是特殊情况下（动画）需要注意优化
--
### No Rules! Use Tools!


---


[](http://csstriggers.com/)
![](./demo/repaintrelayout.jpg)




