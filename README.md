## HTML

1. ie的某些兼容性问题
   
2. HTML5的新特性
   
   答案：画板元素、视频元素、地理定位、离线网络程序。 语义：能够让你更恰当地描述你的内容是什么。     连通性：能够让你和服务器之间通过创新的新技术方法进行通信。     离线 & 存储：能够让网页在客户端本地存储数据以及更高效地离线运行。    多媒体：使 video 和 audio 成为了在所有 Web 中的一等公民。    2D/3D 绘图 & 效果：提供了一个更加分化范围的呈现选择。   性能 & 集成：提供了非常显著的性能优化和更有效的计算机硬件使用。     设备访问 Device Access：能够处理各种输入和输出设备。     样式设计: 让作者们来创作更加复杂的主题吧！
   
3. canvas画图
   
   ​
   
4. doctype的作用
   
   答案：doctype告诉浏览器它使用了什么文档类型。它指出阅读程序应该用什么		规则集来解释文档中的标记。XHTML中有三种，包括过度型、严格型、框架型。HTML4严格。随着XML的流行，HTML推出了XHTML标准，其中严格模式严格遵守了XML的规范，例如属性必须有值、标签必须闭合等，同时也抛弃了一些不推荐的标签。而XHTML过度版本，则稍微比严格模式松散些，一些不推荐的标签依然可用外。当页面有框架时，则应该使用框架型。再就是HTML5的版本。使用HTML5的Doctype会默认触发标准模式。
   
5. HTML5中引进的data-有什么作用
   
   答案：data-是HTML5新增的一个自定义属性。用以方便开发者在HTML中自定义一些属性来存储和操作数据，同时又不违背HTML的规范，不会带来一些副作用。data-自定义属性非常简单，如下例：
   
   ``` html
   <article  id="electriccars"  data-columns="3"  data-index-number="12314"  data-parent="cars">...</article>。
   ```
   
   通过JS去获取自定义属性非常简单，可以通过 getAttribute()传递全部名称来获取，也可以使用 dataset 属性集来获取。如：
   
   ``` javascript
   var article = document.querySelector('#electriccars');
   article.dataset.columns; // "3"  
   article.dataset.indexNumber ;// "12314"  
   article.dataset.parent ;//
   ```
   
6. 浏览器标准模式和怪异模式有什么不同
   
   答案：由于历史的原因，各个浏览器在对页面的渲染上存在差异，甚至同一浏览器在不同版本中，对页面的渲染也不同。在W3C标准出台以前，浏览器在对页面的渲染上没有统一规范，产生了差异(Quirks mode或者称为Compatibility Mode)；由于W3C标准的推出，浏览器渲染页面有了统一的标准(CSScompat或称为Strict mode也有叫做Standars mode)，这就是二者最简单的区别。
   
      W3C标准推出以后，浏览器都开始采纳新标准，但存在一个问题就是如何保证旧的网页还能继续浏览，在标准出来以前，很多页面都是根据旧的渲染方法编写的，如果用的标准来渲染，将导致页面显示异常。为保持浏览器渲染的兼容性，使以前的页面能够正常浏览，浏览器都保留了旧的渲染方法（如：微软的IE）。这样浏览器渲染上就产生了Quircks mode和Standars mode，两种渲染方法共存在一个浏览器上。火狐一直工作在标准模式下，但IE（6，7，8）标准模式与怪异模式差别很大，主要体现在对盒子模型的解释上，这个很重要，下面就重点说这个。那么浏览器究竟该采用哪种模式渲染呢？这就引出的DTD，既是网页的头部声明，浏览器会通过识别DTD而采用相对应的渲染模式：1. 浏览器要使老旧的网页正常工作，但这部分网页是没有doctype声明的，所以浏览器对没有doctype声明的网页采用quirks mode解析。 2. 对于拥有doctype声明的网页，什么浏览器采用何种模式解析，这里有一张详细列表可参考：[点击这里](http://hsivonen.iki.fi/doctype)。3. 对于拥有doctype声明的网页，这里有几条简单的规则可用于判断：对于那些浏览器不能识别的doctype声明，浏览器采用strict mode解析。4. 在doctype声明中，没有使用DTD声明或者使用HTML4以下（不包括HTML4）的DTD声明时，基本所有的浏览器都是使用quirks mode呈现，其他的则使用strict mode解析。5. 可以这么说，在现有有doctype声明的网页，绝大多数是采用strict mode进行解析的。6. 在ie6中，如果在doctype声明前有一个xml声明(比如:<?xml version=”1.0″ encoding=”iso-8859-1″?>)，则采用quirks mode解析。这条规则在ie7中已经移除了。
   
7. 写出你常用的HTML标签
   
   答案：根据自己的使用和掌握来写吧。参考http://blog.csdn.net/ithomer/article/details/5277162。
   
8. 为什么要少用iframe
   
   答案：[为什么应该减少使用iframe](http://www.williamlong.info/archives/3136.html)
   
   iframe的创建比其它包括scripts和css的 DOM 元素的创建慢了 1-2 个数量级。
   
   Iframes阻塞页面加载:window 的 onload 事件需要在所有 iframe 加载完毕后(包含里面的元素)才会触发。在 Safari 和 Chrome 里，通过 JavaScript 动态设置 iframe 的 SRC 可以避免这种阻塞情况。
   
   唯一的连接池:有人可能希望 iframe 会有自己独立的连接池，但不是这样的。绝大部分浏览器，主页面和其中的 iframe 是共享这些连接的。这意味着 iframe 在加载资源时可能用光了所有的可用连接，从而阻塞了主页面资源的加载。如果 iframe 中的内容比主页面的内容更重要，这当然是很好的。但通常情况下，iframe 里的内容是没有主页面的内容重要的。这时 iframe 中用光了可用的连接就是不值得的了。一种解决办法是，在主页面上重要的元素加载完毕后，再动态设置 iframe 的 SRC。
       
9. HTML语义化的理解
   
   答案：用正确的标签表达正确的内容，可以增强网页的易用性（如障碍人士访问等）和搜索引擎的爬取和检索。HTML5新增的语义化标签如header\section\article\footer等。
   
10. 行内元素和块级元素的异同

    答案：①行内元素与块级元素直观上的区别，行内元素会在一条直线上排列，都是同一行的，水平方向排列②块级元素可以包含行内元素和块级元素。行内元素不能包含块级元素。③行内元素与块级元素属性的不同，主要是盒模型属性上(行内元素设置width无效，height无效(可以设置line-height)，margin上下无效，padding上下无效)。④行内元素转换为块级元素,通过设置display:block
    
11. 盒模型，及在浏览器兼容方面的异同

    答案：浏览器的盒子模型指的是它的盒子宽度需要包括内容区宽度、内外边距和边框大小，高度类似。一般设置宽度默认是对应内容区宽度。
    
    兼容性方面：在IE7以前的版本中设置宽度是包括：内边距+边框+内容区的。IE7之后跟其它浏览器一样，都是只对应内容区的宽度。可以通过修改box-sizing:border-box来修改。让设置宽度等于内容区和边框和内边距之和。
    
    ​

## JavaScript

1. 同源策略及跨域请求的方法和原理（比较JSONP和document.domain的不同及优劣，以及HTML5的跨域方案）
   
   答案：同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。这里的同源指的是：同协议，同域名和同端口。这里说的js跨域是指通过js在不同的域之间进行数据传输或通信，比如用ajax向一个不同的域请求数据，或者通过js获取页面中不同域的框架中(iframe)的数据。只要协议、域名、端口有任何一个不同，都被当作是不同的域。
   
   jsonpcallback是页面存在的回调方法，参数就是想得到的json。 document.domain:浏览器都的同源策略，其限制之一就是第一种方法中我们说的不能通过ajax的方法去请求不同源中的文档。 它的第二个限制是浏览器中不同域的框架之间是不能进行js的交互操作的。有一点需要说明，不同的框架之间（父子或同辈），是能够获取到彼此的window对象的，但头疼的是你却不能使用获取到的window对象的属性和方法(html5中的postMessage方法是一个例外，还有些浏览器比如ie6也可以使用top、parent等少数几个属性)，总之，你可以当做是只能获取到一个几乎无用的window对象。document.domain开始派上用场，只要将同一域下不同子域的document.domain设置为共同的父域，则这个时候我们就可以访问对应window的各种属性和方法了。例如：www.example.com父域下的www.lib.example.com和www.hr.example.com两个子域，将对应页面的document.domain设为example.com即可。 （参考：[参考1](http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)和[参考2](http://www.cnblogs.com/2050/p/3191744.html)及[参考3](http://adam.kahtava.com/journal/2010/03/18/the-same-origin-policy-jsonp-vs-the-documentdomain-property/)
   
2. JavaScript数据类型
   
   答案： JavaScript中有5种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number和String。还有1种复杂数据类型——Object，Object本质上是由一组无序的名值对组成的。
   
3. JavaScript字符串转化
   
   答案：熟悉基本的字符串操作函数，参考http://www.cnblogs.com/front-Thinking/p/4398447.html
   
4. JSONP原理及优缺点
   
   答案：具体JSONP的原理可参考1，说白了就是插入一个script标签，其src指向跨域接口，返回对应的callback(data)，其中data是json格式，callback是页面已存在的function。JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。
   
5. XMLHttpRequest
   
   答案：[轻松掌握XMLHttpRequest对象](http://www.cnblogs.com/beniao/archive/2008/03/29/1128914.html)
   
6. 事件委托
   
   答案：使用事件委托技术能让你避免对特定的每个节点添加事件监听器；相反，事件监听器是被添加到它们的父元素上。事件监听器会分析从子元素冒泡上来的事件，找到是哪个子元素的事件。
   
7. 前端模块化（AMD和CommonJS的原理及异同，seajs和requirejs的异同和用法）
   
   答案：[使用AMD\CommonJS\ES Harmony编写模块化的JavaScript](http://justineo.github.io/singles/writing-modular-js/)/和[RequireJS中文网](http://www.requirejs.cn/)
   
   [SeaJS和RequireJS最大的不同](http://www.douban.com/note/283566440/)，其中AMD和CMD的区别可以看[玉伯在知乎上的回答](http://www.zhihu.com/question/20351507/answer/14859415)
   
8. session
   
9. Cookie
   
   答案：8与9的知识可以参考：[参考1](http://www.cnblogs.com/shiyangxt/archive/2008/10/07/1305506.html)和[参考2](http://www.cnblogs.com/Darren_code/archive/2011/11/24/Cookie.html)
   
   常见的cookie操作包括创建cookie、添加cookie、删除cookie等，相应函数参考：
   
   ``` ​javascript
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
   
10. seaJS的用法及原理，依赖加载的原理、初始化、实现等

    答案：[模块化开发之sea.js实现原理总结](http://www.lxway.com/85146452.htm)，简言之就是要解决三个问题，分别为：①模块加载（插入script标签来加载模块。你在页面看不到标签是因为模块被加载完后删除了对应的script标签。）②模块依赖（按依赖顺序依赖）③命名冲突（封装一层define，所有的都成为了局部变量，并通过exports暴漏出去）。
    
11. this问题
    
    答案：[别再为了this发愁了------JS中的this机制](http://www.cnblogs.com/front-Thinking/p/4364337.html)
    
12. 模块化原理（作用域）

    答案：其中seajs的原理参考第10题。大致原理都类似。
    
13. JavaScript动画算法
    
14. 拖拽的实现
    
15. JavaScript原型链及JavaScript如何实现继承、类的

    答案：[JS 面向对象之继承 -- 原型链](http://www.cnblogs.com/yangjinjin/archive/2013/02/01/2889368.html),即每个构造函数都有一个原型对象，原型对象包含一个指向构造函数的指针（prototype），而实例则包含一个指向原型对象的内部指针（__proto__），通过将子构造函数的原型指向父构造函数的实例。
    
    类的实现如，[js 基于原型的类实现详解](http://blog.csdn.net/lihongxun945/article/details/8061311)
    
16. 闭包及闭包的用处，以及闭包可能造成的不良后果。
    
    答案：[聊一下JS中的作用域scope和闭包closure](http://www.cnblogs.com/front-Thinking/p/4317020.html)
    
    闭包的优劣详解如，[javascript 闭包的好处及坏处](http://blog.csdn.net/vuturn/article/details/43055279)，简言之就是好处能够实现封装和缓存等，坏处就是消耗内存、不正当使用会造成内存溢出的问题。
    
17. 常见算法的JS实现（如快排、冒泡等）

    答案：[常用排序算法之JavaScript实现](http://www.cnblogs.com/ywang1724/p/3946339.html#3037096)
    
18. 事件冒泡和事件捕获
    
    答案：[事件冒泡和事件捕获](http://www.quirksmode.org/js/events_order.html#link4)
    
19. 浏览器检测（能力检测、怪癖检测等）
    
20. JavaScript代码测试
    
    答案：平时在测试方面做的比较少，一般用JSlint检查一些常见的错误。对于功能性的可能会使用基于karma的Jasmine测试框架来做。
    
21. call与apply的作用及不同
    
    答案：绑定this指针，第二个参数不同，apply是类数组，而call是一些列参数。
    
22. bind的用法，以及如何实现bind的函数和需要注意的点

    答案：
    
23. 变量名提升
    
    答案：参考16中的博客及评论部分。
    
24. == 与 ===
    
    答案：前者隐式类型转换，后者严格对比。
    
25. "use strict"作用

    答案：[Javascript 严格模式详解](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)
    
26. AJAX请求的细节和原理
    
27. 函数柯里化（Currying）
    
28. NodeJS健壮性方面的实践（子进程等）
    
    答案：同29。
    
29. NodeJS能否用利用多核实现在计算性能上的劣势等
    
    答案：[《解读Nodejs多核处理模块cluster》](http://blog.fens.me/nodejs-core-cluster/)
    
30. jQuery链式调用的原理
    
31. ES6及jQuery新引进的Promise有什么用处
    
32. NodeJS的优缺点及使用场景
    
33. JS中random的概率问题
    
34. 客户端存储及他们的异同（例如：cookie, sessionStorage和localStorage等）

    共同点：都是保存在浏览器端，且同源的。
    区别：1、cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。2、cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。3、存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。4、数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。5、作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。6、Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

    
35. AngularJS的文件管理及打包（包括模板打包及请求、JS的打包和请求等）
    
36. AngularJS的JS模块管理及实践
    
37. 在你的Angular App页面里随意加一个JS文件，会有什么影响
    
38. AngularJS directive及自己如何定义directive
    
39. AngularJS双向绑定的原理及实现
    
    答案：AngularJS数据绑定及AngularJS的工作机制，参考《AngularJS up and running》第203页，十三章第一节。
    
40. 你如何测试你的JS代码
    
    答案：平时在测试方面做的比较少，一般用JSlint检查一些常见的错误。对于功能性的可能会使用基于karma的Jasmine测试框架来做。
    
41. DOM1\DOM2\DOM3都有什么不同
    
42. XSS
    
    答案：1. [《浅谈javascript函数劫持》](http://www.xfocus.net/articles/200712/963.html) 2. [《xss零碎指南》](http://www.cnblogs.com/hustskyking/p/xss-snippets.html)
    
43. 常用数组方法和数组算法（如数组去重、求交集、并集等）
    
    答案：[javascript常用数组算法总结](http://www.cnblogs.com/front-Thinking/p/4797440.html)
    
44. js数组去重复项
    
    答案：[js数组去重复项的四种方法](http://www.cnblogs.com/novus/archive/2011/06/30/1921132.html)
    
45. js中的垃圾回收机制
    
    答案：[JavaScript垃圾回收机制](http://www.cnblogs.com/hustskyking/archive/2013/04/27/garbage-collection.html)
    
46. 常见的JS设计模式
    
47. js获取服务器精准时间（客户端如何与服务器时间同步）
    
    答案：思路：简而言之就是发送一个ajax请求，然后获取对应的HTTP Header中的time，由于时延等问题造成时间在JS客户端获取后当前时间已经不再是服务器此时的时间，然后用本地的时间减去获取的服务器的时间，这应该就是时间偏移量。再新建一个时间，加上此偏移量应该就是此时此刻服务器的时间。代码如下：
    
    ``` javascript
    var offset = 0;
    function calcOffset() {
      	var xmlhttp = new ActiveXObject("Msxml2.XMLHTTP");
    	xmlhttp.open("GET", "http://stackoverflow.com/", false);
    	xmlhttp.send();
    
    	var dateStr = xmlhttp.getResponseHeader('Date');
    	var serverTimeMillisGMT = Date.parse(new Date(Date.parse(dateStr)).toUTCString());
    	var localMillisUTC = Date.parse(new Date().toUTCString());
    	offset = serverTimeMillisGMT -  localMillisUTC;  
    }
    function getServerTime() {
    	var date = new Date();
    	date.setTime(date.getTime() + offset);
    	return date;
    }
    ```
    
    或者是：
    
    ``` javascript
    var start = (new Date()).getTime();
    
    var serverTime;//服务器时间
    
    $.ajax({
      url:"XXXX",
    success: function(data,statusText,res){
        	var delay = (new Date()).getTime() - start;
        	serverTime = new Date(res.getResponseHeader('Date')).getTime() + delay;
        	console.log(new Date(serverTime));//标准时间
        	console.log((new Date(serverTime)).toTimeString());//转换为时间字符串
        	console.log(serverTime);//服务器时间毫秒数
    	}
    }）
    ```
    
48. 什么是js中的类数组对象
    
    答案：1、它们都有一个合法的 length 属性(0 到 2**32 - 1 之间的正整数)。2、length 属性的值大于它们的最大索引(index)。
    
49. Node中exports和module.exports的区别
    
    答案：[exports 和 module.exports 的区别](https://cnodejs.org/topic/5231a630101e574521e45ef8#554db35aed6f7db13c84919e)
    
50. 异步编程的了解
    
    答案：
    
51. Grunt和Gulp的区别

    
    

## CSS

1. 圣杯布局
   
   答案：http://www.elonglau.com/33.html
   
2. CSS合并方法
   
   答案：避免使用@import引入多个css文件，可以使用CSS工具将CSS合并为一个CSS文件，例如使用Sass\Compass等。
   
3. 盒子模型
   
   答案：http://www.w3school.com.cn/css/css_boxmodel.asp
   
4. CSS定位
   
   答案：http://www.cnblogs.com/fsjohnhuang/p/3967350.html
   
5. CSS动画原理
   
   答案：http://blog.csdn.net/dojotoolkit/article/details/6859754
   
6. CSS3动画（简单动画的实现，如旋转等）
   
7. CSS不同选择器的权重(CSS层叠的规则)
   
   答案：首先，找出所有应用到该标签的所有规则。然后按照下面的规则进行应用：1、！important规则最重要，大于其它规则；2、行内样式规则，加1000；3、对于选择器中给定的各个ID属性值，加100；4、对于选择器中给定的各个类属性、属性选择器或者伪类选择器，加10；5、对于选择其中给定的各个元素标签选择器，加1；6、通配符和结合符给予特殊性没有任何贡献；7、如果权值一样，则按照样式规则的先后顺序来应用，顺序靠后的覆盖靠前的规则。
   
8. flexbox布局
   
9. 块级元素和行内元素的异同
   
10. CSS在性能优化方面的实践（比方说选择器的效率等）
    
11. CSS打包压缩的方法
    
12. 使用CSS预处理的优缺点（比方说Sass和Compass等）
    
    答案：Css预处理器定义了一种新的语言将Css作为目标生成文件，然后开发者就只要使用这种语言进行编码工作了。预处理器通常可以实现浏览器兼容，变量，结构体等功能，代码更加简洁易于维护。让你用一种编程语言的思维来写CSS样式。但是带来的缺点是需要增加设计工作以及学习熟悉成本。
    
13. { box-sizing: border-box; }这条CSS规则是干嘛的，有什么优点
    
14. CSS浮动的原理及清除浮动的方法及优缺点
    
    答案：而此浮动元素在文档流空出的位置，由后续的(非浮动)元素填充上去：块级元素直接填充上去，若跟浮动元素的范围发生重叠，浮动元素覆盖块级元素。内联元素：有空隙就插入。给元素的float属性赋值后，就是脱离文档流，进行左右浮动，紧贴着父元素(默认为body文本区域)的左右边框。[css-float浮动的深入研究、详解及拓展](http://www.zhangxinxu.com/wordpress/2010/01/css-float%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%B7%B1%E5%85%A5%E7%A0%94%E7%A9%B6%E3%80%81%E8%AF%A6%E8%A7%A3%E5%8F%8A%E6%8B%93%E5%B1%95%E4%B8%80/)和[那些年我们一起清除过的浮动](http://www.iyunlu.com/view/css-xhtml/55.html)及[css清除浮动各种方法](http://www.cnblogs.com/mizzle/archive/2011/07/14/2105961.html)
    
15. CSS水平垂直居中的方法
    
    答案：[CSS垂直居中总结](http://www.cnblogs.com/dojo-lzz/p/4419596.html)和[CSS水平垂直居中总结](http://www.cnblogs.com/dojo-lzz/p/4419596.html)
    
16. base64的原理及优缺点
    
17. CSS reset和normalize的区别
    
    答案：reset是将浏览器所有的默认样式进行重置、覆盖，normalize是保留原来浏览器的样式并且尽量在不同浏览器里保持一致。
    
18. link和@import的区别

    答案：本质上他们都是为了引入外部css样式的，但是有区别如下：①link属于XHTML标签，而@import完全是CSS提供的一种方式。②加载顺序的差别。当一个页面被加载的时候（就是被浏览者浏览的时候），link引用的CSS会同时被加载，而@import引用的CSS 会等到页面全部被下载完再被加载。所以有时候浏览@import加载CSS的页面时开始会没有样式（就是闪烁），网速慢的时候还挺明显。③兼容性的差别。由于@import是CSS2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题。④使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的。



## 综合

1. HTTP状态码
   
   答案：http://baike.baidu.com/link?url=2Mur0ZYizeDAwgnKKu_REjgsbW-vVbKNA44xxHuz7C4sn6YBtIn2KCMPSSIqQJC0BQsQNxMKQE-EcS8iZPEWg_
   
2. Cach-Control
   
   答案：http://baike.baidu.com/link?url=I2l51auZpAcJ8F0-ozRZUWRcCatmQz7PCZ8vdbEzHvCz_yJKcSSeDmn2cDWfOhrUIqL3KRa7wueujDcEZ9QBN_
   
   | 方法     | 描述                                       | 
   | ------ | :--------------------------------------- | 
   | 打开新窗口  | 如果指定cache-control的值为private、no-cache、must-revalidate,那么打开新窗口访问时都会重新访问服务器。而如果指定了max-age值,那么在此值内的时间里就不会重新访问服务器,例如：Cache-control: max-age=5 表示当访问此网页后的5秒内再次访问不会去服务器. | 
   | 在地址栏回车 | 如果值为private或must-revalidate,则只有第一次访问时会访问服务器,以后就不再访问。如果值为no-cache,那么每次都会访问。如果值为max-age,则在过期之前不会重复访问。 | 
   | 按后退按扭  | 如果值为private、must-revalidate、max-age,则不会重访问,而如果为no-cache,则每次都重复访问 | 
   | 按刷新按扭  | 无论为何值,都会重复访问.                            | 
   
3. 项目经历及作用和用到的技术等
   
4. SEO
   
5. 一个页面从输入 URL 到页面加载完的过程中都发生了什么事情？
   
   答案：［从输入url到页面加载完成发生了什么］（http://fex.baidu.com/blog/2014/05/what-happen/）
   
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
    
    答案：[渐进增强和优雅降级的区别](http://www.cnblogs.com/mofish/p/3822879.html)
    
16. 什么是“FOUC”及如何避免（http://blog.csdn.net/kongtoubudui/article/details/12975401）
    
17. 页面性能优化方法及其原理
    
    答案：[web前端页面性能优化小结](http://www.cnblogs.com/mofish/archive/2010/10/12/1849041.html)
    
18. POST和GET的异同
    
    答案：1. get是从服务器上获取数据，post是向服务器传送数据。2. get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。 3. 对于get方式，服务器端用Request.QueryString获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据。 4. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。<br> 建议： 1、get方式的安全性较Post方式要差些，包含机密信息的话，建议用Post数据提交方式； 2、在做数据查询时，建议用Get方式；而在做数据添加、修改或删除时，建议用Post方式。
    
19. 你是如何了解到并且学习一门技术的
    
20. 讲一下你读过的关于前端技术的书
    
21. 你未来三年的计划
    
22. 响应式布局
    
23. 文件上传的实现
    
24. 雅虎性能优化的15条规则
    
25. 浏览器加载原理和过程
    
    答案：[浏览器加载过程和原理](http://kb.cnblogs.com/page/129756/)。
    
26. HTTP如何实现缓存的。

    答案：[HTTP协议：缓存](http://kb.cnblogs.com/page/166267/)