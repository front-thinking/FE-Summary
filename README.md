# 收集各种前端面试中被问到的问题，以方便大家找工作的时候做准备，参考答案在下面，欢迎补充、完善！
## HTML
 1. ie的某些兼容性问题
 2. HTML5的新特性
 3. canvas画图
 4. doctype的作用
 5. HTML5中引进的data-有什么作用
 6. HTML标准模式和怪异模式有什么不同
 7. 写出你常用的HTML标签

## JavaScript
 1. 同源策略及跨域请求的方法和原理（比较JSONP和document.domain的不同及优劣，以及HTML5的跨域方案）
 2. JavaScript数据类型
 3. JavaScript字符串转化
 4. JSONP原理及优缺点
 5. XMLHttpRequest
 6. 事件委托
 7. 前端模块化（AMD和CommonJS的原理及异同，requirejs的用法）
 8. session
 9. Cookie
 10. seaJS的用法及原理，依赖加载的原理、初始化、实现等
 11. this问题
 12. 模块化原理（作用域）
 13. JavaScript动画算法
 14. 拖拽的实现
 15. JavaScript原型链
 16. 闭包及闭包的用处
 17. 常见算法的JS实现（例如：实现将两个不同长度的数组组合，顺序越乱越好，以及其的复杂度）
 18. 事件冒泡和事件捕获
 19. 浏览器检测（能力检测、怪癖检测等）
 20. JavaScript代码测试
 21. call与apply的作用及不同
 22. bind的用法
 23. 变量名提升
 24. == 与 ===
 25. "use strict"作用
 26. AJAX请求的细节
 27. 函数柯里化（Currying）
 28. NodeJS健壮性方面的实践（子进程等）
 29. NodeJS能否用利用多核实现在计算性能上的劣势等
 30. jQuery链式调用的原理
 31. ES6及jQuery新引进的Promise有什么用处
 32. NodeJS的优缺点及使用场景
 33. JS中random的概率问题
 34. 客户端存储及他们的异同（例如：cookie, sessionStorage和localStorage等）
 35. AngularJS的文件管理及打包（包括模板打包及请求、JS的打包和请求等）
 36. AngularJS的JS模块管理及实践
 37. 在你的Angular App页面里随意加一个JS文件，会有什么影响
 38. AngularJS directive及自己如何定义directive
 39. AngularJS双向绑定的原理及实现
 40. 你如何测试你的JS代码
 41. DOM1\DOM2\DOM3都有什么不同
 42. XSS
 43. 常用数组方法
 44. js数组去重复项

## CSS
 1. 圣杯布局
 2. CSS合并方法
 3. 盒子模型
 4. CSS定位
 5. CSS动画原理
 6. CSS3动画（简单动画的实现，如旋转等）
 7. CSS不同选择器的权重(CSS层叠的规则)
 8. flexbox布局
 9. 块级元素和行内元素的异同
 10. CSS在性能优化方面的实践（比方说选择器的效率等）
 11. CSS打包压缩的方法
 12. 使用CSS预处理的优缺点（比方说Sass和Compass等）
 13. * { box-sizing: border-box; }这条CSS规则是干嘛的，有什么优点
 14. CSS浮动的原理及清除浮动的方法及优缺点
 15. CSS垂直居中的方法

## 综合
 1. HTTP状态码
 2. Cach-Control
 3. 项目经历及作用和用到的技术等
 4. SEO
 5. 一个页面从输入 URL 到页面加载完的过程中都发生了什么事情？
 6. 常见组件的实现（如让你实现图片轮播、时间计时等）
 7. HTTP头部包含的信息及作用
 8. HTML\CSS\JS在处理浏览器兼容性方面的实践
 9. 前端发展的方向及你的了解和尝试（例如：组件化、工程化、前后端分离、前端质量体系、数据可视化、前端工具及生态圈、前端安全、下一代类库框架等）
 10. 前端工作需要注重的哪些点儿及你在这方面的理解和实践（如：用户体验、性能优化等）
 11. 前端MVC与后端MVC的异同及你对前端MVC的理解（个人在实践方面的理解）
 12. 什么是面向对象编程及面向过程编程，它们的异同和优缺点
 13. 从你自己的理解来看，你是如何理解面向对象编程的，它解决了什么问题，有什么作用
 14. 你对前端的理解？你为什么学前端？
 15. “渐进增强”和“优雅降级”
 16. 什么是“FOUC”及如何避免（http://blog.csdn.net/kongtoubudui/article/details/12975401）
 17. 页面性能优化方法及其原理
 18. POST和GET的异同
 19. 你是如何了解到并且学习一门技术的
 20. 讲一下你读过的关于前端技术的书
 21. 你未来三年的计划
 22. 响应式布局
 23. 文件上传的实现
 24. 雅虎性能优化的15条规则

------
# 答案

## HTML
1. ie的某些兼容性问题<br>
2. HTML5的新特性<br>
3. canvas画图<br>
4. doctype的作用<br>
答案：doctype告诉浏览器它使用了什么文档类型。它指出阅读程序应该用什么规则集来解释文档中的标记。XHTML中有三种，包括过度型、严格型、框架型。HTML4严格。随着XML的流行，HTML推出了XHTML标准，其中严格模式严格遵守了XML的规范，例如属性必须有值、标签必须闭合等，同时也抛弃了一些不推荐的标签。而XHTML过度版本，则稍微比严格模式松散些，一些不推荐的标签依然可用外。当页面有框架时，则应该使用框架型。再就是HTML5的版本。使用HTML5的Doctype会默认触发标准模式。
5. HTML5中引进的data-有什么作用<br>
答案：data-是HTML5新增的一个自定义属性。用以方便开发者在HTML中自定义一些属性来存储和操作数据，同时又不违背HTML的规范，不会带来一些副作用。data-自定义属性非常简单，如下例：<article  id="electriccars"  data-columns="3"  data-index-number="12314"  data-parent="cars">...</article>。通过JS去获取自定义属性非常简单，可以通过 getAttribute()传递全部名称来获取，也可以使用 dataset 属性集来获取。如：var article = document.querySelector('#electriccars');article.dataset.columns; // "3"  article.dataset.indexNumber ;// "12314"  article.dataset.parent ;//
6. 浏览器标准模式和怪异模式有什么不同<br>
答案：由于历史的原因，各个浏览器在对页面的渲染上存在差异，甚至同一浏览器在不同版本中，对页面的渲染也不同。在W3C标准出台以前，浏览器在对页面的渲染上没有统一规范，产生了差异(Quirks mode或者称为Compatibility Mode)；由于W3C标准的推出，浏览器渲染页面有了统一的标准(CSScompat或称为Strict mode也有叫做Standars mode)，这就是二者最简单的区别。
   W3C标准推出以后，浏览器都开始采纳新标准，但存在一个问题就是如何保证旧的网页还能继续浏览，在标准出来以前，很多页面都是根据旧的渲染方法编写的，如果用的标准来渲染，将导致页面显示异常。为保持浏览器渲染的兼容性，使以前的页面能够正常浏览，浏览器都保留了旧的渲染方法（如：微软的IE）。这样浏览器渲染上就产生了Quircks mode和Standars mode，两种渲染方法共存在一个浏览器上。火狐一直工作在标准模式下，但IE（6，7，8）标准模式与怪异模式差别很大，主要体现在对盒子模型的解释上，这个很重要，下面就重点说这个。那么浏览器究竟该采用哪种模式渲染呢？这就引出的DTD，既是网页的头部声明，浏览器会通过识别DTD而采用相对应的渲染模式：1. 浏览器要使老旧的网页正常工作，但这部分网页是没有doctype声明的，所以浏览器对没有doctype声明的网页采用quirks mode解析。 2. 对于拥有doctype声明的网页，什么浏览器采用何种模式解析，这里有一张详细列表可参考：http://hsivonen.iki.fi/doctype。3. 对于拥有doctype声明的网页，这里有几条简单的规则可用于判断：对于那些浏览器不能识别的doctype声明，浏览器采用strict mode解析。4. 在doctype声明中，没有使用DTD声明或者使用HTML4以下（不包括HTML4）的DTD声明时，基本所有的浏览器都是使用quirks mode呈现，其他的则使用strict mode解析。5. 可以这么说，在现有有doctype声明的网页，绝大多数是采用strict mode进行解析的。6. 在ie6中，如果在doctype声明前有一个xml声明(比如:<?xml version=”1.0″ encoding=”iso-8859-1″?>)，则采用quirks mode解析。这条规则在ie7中已经移除了。
7. 写出你常用的HTML标签<br>
答案：根据自己的使用和掌握来写吧。参考http://blog.csdn.net/ithomer/article/details/5277162。


## JavaScript
1. 同源策略及跨域请求的方法和原理（比较JSONP和document.domain的不同及优劣，以及HTML5的跨域方案）<br>
答案：同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。这里的同源指的是：同协议，同域名和同端口。这里说的js跨域是指通过js在不同的域之间进行数据传输或通信，比如用ajax向一个不同的域请求数据，或者通过js获取页面中不同域的框架中(iframe)的数据。只要协议、域名、端口有任何一个不同，都被当作是不同的域。
     JSONP：一句话就是利用script标签绕过同源策略，获得一个类似这样的数据，jsonpcallback是页面存在的回调方法，参数就是想得到的json。
     document.domain:浏览器都的同源策略，其限制之一就是第一种方法中我们说的不能通过ajax的方法去请求不同源中的文档。 它的第二个限制是浏览器中不同域的框架之间是不能进行js的交互操作的。有一点需要说明，不同的框架之间（父子或同辈），是能够获取到彼此的window对象的，但头疼的是你却不能使用获取到的window对象的属性和方法(html5中的postMessage方法是一个例外，还有些浏览器比如ie6也可以使用top、parent等少数几个属性)，总之，你可以当做是只能获取到一个几乎无用的window对象。document.domain开始派上用场，只要将同一域下不同子域的document.domain设置为共同的父域，则这个时候我们就可以访问对应window的各种属性和方法了。例如：www.example.com父域下的www.lib.example.com和www.hr.example.com两个子域，将对应页面的document.domain设为example.com即可。
     （参考：http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html和http://www.cnblogs.com/2050/p/3191744.html及http://adam.kahtava.com/journal/2010/03/18/the-same-origin-policy-jsonp-vs-the-documentdomain-property/）
2. JavaScript数据类型<br>
答案： JavaScript中有5种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number和String。还有1种复杂数据类型——Object，Object本质上是由一组无序的名值对组成的。
3. JavaScript字符串转化<br>
答案：熟悉基本的字符串操作函数，参考http://www.cnblogs.com/front-Thinking/p/4398447.html
4. JSONP原理及优缺点<br>
答案：具体JSONP的原理可参考1。JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。
5. XMLHttpRequest<br>
答案：http://www.cnblogs.com/beniao/archive/2008/03/29/1128914.html
6. 事件委托<br>
答案：http://www.ituring.com.cn/article/467。
7. 前端模块化（AMD和CommonJS的原理及异同，requirejs的用法）<br>
答案：[使用AMD\CommonJS\ES Harmony编写模块化的JavaScript](http://justineo.github.io/singles/writing-modular-js/)/和[RequireJS中文网](http://www.requirejs.cn/)
8. session<br>
9. Cookie<br>
答案：8与9的知识可以参考：http://www.cnblogs.com/shiyangxt/archive/2008/10/07/1305506.html和http://www.cnblogs.com/Darren_code/archive/2011/11/24/Cookie.html<br>
常见的cookie操作包括创建cookie、添加cookie、删除cookie等，相应函数参考：<br>

```javascript
//添加（daysToLive大于0）cookie/删除（daysToLive为0）cookie
function setcookie(name,value,daysToLive){
  var cookie = name + "=" + encodeURIComponent(value);
  if(typeof daysToLive === "number"){
    cookie += ";max-age=" + (daysToLive*60*60*24);
  }
  document.cookie = cookie;
}
//解析cookie,直接getcookie()[name]获取对应的name cookie
function getcookie(){
  var cookie = {};
  var all = document.cookie;
  if(all === ""){
    return false;
  }
  var list = all.split(";");
  for(var i=0;i < list.length; i++){
    var cookie = list[i];
    var p = cookie.indexOf("=");
    var name = cookie.substring(0,p);
    var value = cookie.substring(p+1);
    value = decodeURIComponent(value);
    cookie[name] = value;
  }
  return cookie;
}
```
<br>
10. seaJS的用法及原理，依赖加载的原理、初始化、实现等<br>
11. this问题<br>
答案：http://www.cnblogs.com/front-Thinking/p/4364337.html<br>
12. 模块化原理（作用域）<br>
13. JavaScript动画算法<br>
14. 拖拽的实现<br>
15. JavaScript原型链<br>
16. 闭包及闭包的用处<br>
答案：http://www.cnblogs.com/front-Thinking/p/4317020.html<br>
17. 常见算法的JS实现（例如：实现将两个不同长度的数组组合，顺序越乱越好，以及其的复杂度）<br>
18. 事件冒泡和事件捕获<br>
答案：http://www.quirksmode.org/js/events_order.html#link4<br>
19. 浏览器检测（能力检测、怪癖检测等）<br>
20. JavaScript代码测试<br>
21. call与apply的作用及不同<br>
22. bind的用法<br>
23. 变量名提升<br>
24. == 与 ===<br>
25. "use strict"作用<br>
26. AJAX请求的细节<br>
27. 函数柯里化（Currying）<br>
28. NodeJS健壮性方面的实践（子进程等）<br>
29. NodeJS能否用利用多核实现在计算性能上的劣势等<br>
30. jQuery链式调用的原理<br>
31. ES6及jQuery新引进的Promise有什么用处<br>
32. NodeJS的优缺点及使用场景<br>
33. JS中random的概率问题<br>
34. 客户端存储及他们的异同（例如：cookie, sessionStorage和localStorage等）<br>
35. AngularJS的文件管理及打包（包括模板打包及请求、JS的打包和请求等）<br>
36. AngularJS的JS模块管理及实践<br>
37. 在你的Angular App页面里随意加一个JS文件，会有什么影响<br>
38. AngularJS directive及自己如何定义directive<br>
39. AngularJS双向绑定的原理及实现<br>
40. 你如何测试你的JS代码<br>
41. DOM1\DOM2\DOM3都有什么不同<br>
42. XSS<br>
答案：1. [《浅谈javascript函数劫持》](http://www.xfocus.net/articles/200712/963.html) 2. [《xss零碎指南》](http://www.cnblogs.com/hustskyking/p/xss-snippets.html)<br>
43. 常用数组方法<br>
44. js数组去重复项<br>
答案：[js数组去重复项的四种方法](http://www.cnblogs.com/novus/archive/2011/06/30/1921132.html)

## CSS
1. 圣杯布局<br>
答案：http://www.elonglau.com/33.html
2. CSS合并方法<br>
答案：避免使用@import引入多个css文件，可以使用CSS工具将CSS合并为一个CSS文件，例如使用Sass\Compass等。
3. 盒子模型<br>
答案：http://www.w3school.com.cn/css/css_boxmodel.asp
4. CSS定位<br>
答案：http://www.cnblogs.com/fsjohnhuang/p/3967350.html
5. CSS动画原理<br>
答案：http://blog.csdn.net/dojotoolkit/article/details/6859754
6. CSS3动画（简单动画的实现，如旋转等）<br>
7. CSS不同选择器的权重(CSS层叠的规则)<br>
答案：首先，找出所有应用到该标签的所有规则。然后按照下面的规则进行应用：1、！important规则最重要，大于其它规则；2、行内样式规则，加1000；3、对于选择器中给定的各个ID属性值，加100；4、对于选择器中给定的各个类属性、属性选择器或者伪类选择器，加10；5、对于选择其中给定的各个元素标签选择器，加1；6、通配符和结合符给予特殊性没有任何贡献；7、如果权值一样，则按照样式规则的先后顺序来应用，顺序靠后的覆盖靠前的规则。
8. flexbox布局<br>
9. 块级元素和行内元素的异同<br>
10. CSS在性能优化方面的实践（比方说选择器的效率等）<br>
11. CSS打包压缩的方法<br>
12. 使用CSS预处理的优缺点（比方说Sass和Compass等）<br>
13. * { box-sizing: border-box; }这条CSS规则是干嘛的，有什么优点<br>
14. CSS浮动的原理及清除浮动的方法及优缺点
答案：[css-float浮动的深入研究、详解及拓展](http://www.zhangxinxu.com/wordpress/2010/01/css-float%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%B7%B1%E5%85%A5%E7%A0%94%E7%A9%B6%E3%80%81%E8%AF%A6%E8%A7%A3%E5%8F%8A%E6%8B%93%E5%B1%95%E4%B8%80/)和[那些年我们一起清除过的浮动](http://www.iyunlu.com/view/css-xhtml/55.html)及[css清除浮动各种方法](http://www.cnblogs.com/mizzle/archive/2011/07/14/2105961.html)
15. CSS水平垂直居中的方法
答案：[CSS垂直居中总结](http://www.cnblogs.com/dojo-lzz/p/4419596.html)和[CSS水平垂直居中总结](http://www.cnblogs.com/dojo-lzz/p/4419596.html)

## 综合
1. HTTP状态码<br>
答案：http://baike.baidu.com/link?url=2Mur0ZYizeDAwgnKKu_REjgsbW-vVbKNA44xxHuz7C4sn6YBtIn2KCMPSSIqQJC0BQsQNxMKQE-EcS8iZPEWg_
2. Cach-Control<br>
答案：http://baike.baidu.com/link?url=I2l51auZpAcJ8F0-ozRZUWRcCatmQz7PCZ8vdbEzHvCz_yJKcSSeDmn2cDWfOhrUIqL3KRa7wueujDcEZ9QBN_

| 方法        | 描述  |
| --------   | :----  |
| 打开新窗口	     | 如果指定cache-control的值为private、no-cache、must-revalidate,那么打开新窗口访问时都会重新访问服务器。而如果指定了max-age值,那么在此值内的时间里就不会重新访问服务器,例如：Cache-control: max-age=5 表示当访问此网页后的5秒内再次访问不会去服务器.     |
| 在地址栏回车        |  如果值为private或must-revalidate,则只有第一次访问时会访问服务器,以后就不再访问。如果值为no-cache,那么每次都会访问。如果值为max-age,则在过期之前不会重复访问。   |
| 按后退按扭        |    如果值为private、must-revalidate、max-age,则不会重访问,而如果为no-cache,则每次都重复访问  |
| 按刷新按扭        |    无论为何值,都会重复访问.  |

<br>
3. 项目经历及作用和用到的技术等<br>
4. SEO<br>
5. 一个页面从输入 URL 到页面加载完的过程中都发生了什么事情？<br>
6. 常见组件的实现（如让你实现图片轮播、时间计时等）<br>
7. HTTP头部包含的信息及作用<br>
8. HTML\CSS\JS在处理浏览器兼容性方面的实践<br>
9. 前端发展的方向及你的了解和尝试（例如：组件化、工程化、前后端分离、前端质量体系、数据可视化、前端工具及生态圈、前端安全、下一代类库框架等）<br>
10. 前端工作需要注重的哪些点儿及你在这方面的理解和实践（如：用户体验、性能优化等）<br>
11. 前端MVC与后端MVC的异同及你对前端MVC的理解（个人在实践方面的理解）<br>
12. 什么是面向对象编程及面向过程编程，它们的异同和优缺点<br>
13. 从你自己的理解来看，你是如何理解面向对象编程的，它解决了什么问题，有什么作用<br>
14. 你对前端的理解？你为什么学前端？<br>
15. “渐进增强”和“优雅降级”<br>
16. 什么是“FOUC”及如何避免（http://blog.csdn.net/kongtoubudui/article/details/12975401）<br>
17. 页面性能优化方法及其原理<br>
18. POST和GET的异同<br>
19. 你是如何了解到并且学习一门技术的<br>
20. 讲一下你读过的关于前端技术的书<br>
21. 你未来三年的计划<br>
22. 响应式布局<br>
23. 文件上传的实现<br>
24. 雅虎性能优化的15条规则<br>
