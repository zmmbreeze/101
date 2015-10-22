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
No Rules! Just Tools!



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
--
引入了无限滚动特性

页面滚动时速度变的很慢！

jQuery 1.4.2 升级到 1.4.4<!-- .element: class="fragment" data-fragment-index="1" -->
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


---


## Selector
--
```
.portal .lbf-combobox #user-id.lbf-combobox-label {
    /* ... */
}
```
解析顺序：[从右到左](http://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left/5813672#5813672)
<!-- .element: class="fragment" data-fragment-index="1" -->
<!-- 包括jQuery(Sizzle)也是RTL -->
--
```
#user-id {
    /* ... */
}
```
> CSS selector matching is now reasonably fast for the absolute majority of common selectors that used to be slow at the time of the profiler implementation. This time is also included into the Timeline "Recalculate Style" event.
> As such, I believe the CSS selector profiler is not as useful as it [might have been] used to and can safely be dropped. This will also reduce the fraction of developers trying to micro-optimize already fast selectors.

Chrome 的 CSS 选择器匹配性能已经足够快了，Chrome 30中[移除了自己的 CSS selector性能分析器](https://code.google.com/p/chromium/issues/detail?id=265486)
```
[Github 遇到的 CSS 性能挑战](https://speakerdeck.com/jonrohan/githubs-css-performance)
--
### 避免冲突
```
/* index_header.css */
.header .current { background: #FEFEFE; }

/* index_list.css */
.current  { background: blue; }
```
--
### [OOCSS](http://oocss.org/) / [SMACSS](https://smacss.com/) / [BEM](https://en.bem.info/)
--
### BEM
![](./demo/bem.png)
--
```
/* Block */
.menu {
    display: block;
}

/* Element */
.menu__item {
    float: left;
}

/* Modifier */
.menu__item_current {
    background-color: #EFEFEF;
}
```
--
#### 优点
- 减少后代选择器
- 易重用，可扩展
--
#### 但是很丑
--
### [AMCSS](https://amcss.github.io/)
![](./demo/amcss.png)
--
### 缺点
- 各种 JS 库支持不够好
- 开发者的习惯很难改

<!-- 作者弃坑了，转而做 CSS Modules -->
--
### [CSS Modules](http://glenmaddern.com/articles/css-modules)
```
/* components/submit-button.css */
.common { /* font-sizes, padding, border-radius */ }
.normal { composes: common; /* blue color, light blue background */ }
.error { composes: common; /* red color, light red background */ }
```
```
.components_submit_button__common__abc5436 { /* font-sizes, padding, border-radius */ }
.components_submit_button__normal__def6547 { /* blue color, light blue background */ }
.components_submit_button__error__1638bcd { /* red color, light red background */ }
```
<!-- .element: class="fragment" data-fragment-index="1" -->
--
<!-- 个人习惯 -->
```
/* util.less */
.clearfix() { /* 清除浮动代码 */ }
```
```
/* common.less */
/* `g-` 作为全局模块的前缀 */
.g-header {
    .clearfix();
}
```
```
/* page/index.less */
/* `page-` 作为页面class的前缀 */
.page-index {
    /* 页面模块Block名 */
    .header {
        .clearfix();

        /* 页面模块中的Element */
        &-menu {
            /* ... */
        }
        /* 尽可能使用标签，确保 HTML 语义化 */
        &-menu li {
            float: left;
        }
        /* Element的状态名 */
        /* 非Block命名尽量简写，`cur === current` */
        &-menu .header-menu-cur {
            float: left;
        }
    }
}
```
--
Javascript 用 ID 选择器，CSS 用 Class 选择器

尽量做到“行为和样式分离”



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


## DOM 操作
--
- `relayout / reflow` - 重新计算节点的位置
- `repaint` - 重新绘制节点到屏幕上
<!-- relayout 之后一定触发 repaint -->
--
![](./demo/webkitflow.png)
![](./demo/geckoflow.jpg)
<!-- http://www.phpied.com/rendering-repaint-reflowrelayout-restyle/ -->
<!-- http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/ -->
--
### layout thrashing
![](./demo/layoutthrashing.png)
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

此时第二次读操作执行前，会强迫浏览器 relayout 一次，这样才能计算得到新的 height
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

读写分离，减少 layout thrashing
--
```
function call1() {
    // 读
    var height1 = element1.clientHeight;
    // 写
    element1.style.height = (height1 + 2) + 'px';
}
function call2() {
    // 读
    var height2 = element2.clientHeight;
    // 写
    element2.style.height = (height2 + 2) + 'px';
}

call1();
call2();
```

不一定 DOM 操作都在一个方法体里面
--
```
function call1() {
    // 读
    var height1 = element1.clientHeight;
    // 写
    requestAnimationFrame(function() {
        element1.style.height = (height1 + 2) + 'px';
    });
}
function call2() {
    // 读
    var height2 = element2.clientHeight;
    // 写
    requestAnimationFrame(function() {
        element2.style.height = (height2 + 2) + 'px';
    });
}
```
<!-- http://wilsonpage.co.uk/preventing-layout-thrashing/ -->
[Fast dom](https://github.com/wilsonpage/fastdom)
--
话说回来，一般调用没那么频繁。只是特殊情况下（动画）需要注意优化
--
### No Rules! Just Tools!


---


## CSS 兼容性 Hack
--
### IE 条件注释
```
<!--[if IE 6]>
	这段文字只在IE6浏览器显示
<![endif]-->
```
--
### 属性前缀 Hack

| Selector | IE6(s) | IE7(s) | IE8(s) | IE9(s) | IE10(s) |
| -------- | ------ | ------ | ------ | ------ | ------- |
| `color:red`     | Y | Y | Y | Y | Y |
| `color:red\0`   | N | N | Y | Y | Y |
| `color:red\9\0` | N | N | N | Y | Y |
| `*color:red`    | Y | Y | N | N | N |
| `_color:red`    | Y | N | N | N | N |
--
### [CSS Hack Table](http://swordair.com/tools/css-hack-table/)
--
CSS 会忽略不支持的属性或选择器
<!-- http://stackoverflow.com/questions/13816764/invalid-css-selector-causes-rule-to-be-dropped-what-is-the-rationale -->
<!-- http://stackoverflow.com/questions/5426261/border-radius-causing-naughty-errors-in-firebug-unknown-property-declaratio -->
<!-- Fault tolerance: https://en.wikipedia.org/wiki/Fault_tolerance#Terminology -->
--
```
.test1 {
    background-color: #FFF;                    /* 不支持rgba */
    background-color: rgba(255, 255, 255, .8); /* 支持rgba */
}
.test2 {
    background-image: url(top.png);
    /* IE9+ 不支持多背景 */
    background-image: url(data:image/svg+xml;base64,....), none;
}
```


---


## 布局
--
```
.block {
    /* 块级元素，新开始一行并且尽可能撑满容器 */
    display: block;
}
.inline {
    /* 行内元素，一行之内横向的排列，宽高不起作用 */
    display: inline;
    width: 1000px; /* 无效 */
}
.hide {
    /* 隐藏元素，没有高宽 */
    display: none;
}
.inline-block {
    /* 行内元素，一行之内横向的排列，宽高起作用 */
    display: inline-block;
    height: 24px; /* 有效 */
}
```
--
### 盒模型
```
.box {
    margin: 10px;
    padding: 10px;
    width: 100px;
    height: 100px;
    border: 1px solid #CCC;
}
```

`[box-sizing](http://zh.learnlayout.com/box-sizing.html)`
<!-- .element: class="fragment" data-fragment-index="1" -->
--
![](./demo/catboxmodel.jpg)
<!-- 盒模型就像集装箱里面的盒子一样，盒子间的距离是 margin，盒子外壳的厚度是
border，盒子内的货物的高宽是 width 与 height，货物与盒子的间距是 padding -->
--
![](./demo/boxmodel.png)

仅仅是普通文档流，一般是从左至右、从上到下
<!-- .element: class="fragment" data-fragment-index="1" -->


---


## 浮动元素
--
1. 浮动元素脱离文档流。对于它的父元素来说，浮动元素是不存在的（父元素不会自适应以包裹浮动元素，所以需要清除浮动）
2. 一个浮动元素的位置会尽可能的靠近他父元素的左上角或者右上角
3. 浮动元素前面定义的元素会把浮动元素挤到下面
4. 先声明的浮动元素有优先靠近父元素左上角或者右上角（规则2）位置的权利
5. 规则2的拓展，如果有多个相同方向的浮动元素，浮动元素也会尽可能的靠近左上角或者右上角，直到父元素宽度没法放下这个元素的时候，这个元素才会被挤下去
6. 行内元素添加浮动属性会变成块级元素
7. 浮动元素不会跑出父元素的边界
--
### 清除浮动


---


[学习CSS布局](http://zh.learnlayout.com/)


---



---


[](http://csstriggers.com/)
![](./demo/repaintrelayout.jpg)




