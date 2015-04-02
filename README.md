# 收集各种前端面试中被问到的问题，以方便大家找工作的时候做准备，参考答案在Q&A.md文件中，欢迎补充、完善！
------
> * 圣杯布局
> * ie的某些兼容性问题
> * CORS跨域请求的问题
> * JavaScript数据类型
> * 字符串转化
> * JSONP原理
> * CSS合并方法
> * 盒子模型
> * CSS定位
> * XMLHttpRequest
> * HTTP状态码
> * Cach-Control
> * CSS动画原理
> * 事件委托
> * 前端模块化（AMD和CommonJS的原理及异同，requirejs的用法）
> * session
> * Cookie
> * ajax跨域(跨域的方法及原理，HTML5的跨域问题和方案)
> * this问题
> * 模块化原理（作用域）
> * JavaScript动画算法
> * 拖拽的实现
> * JavaScript原型链
> * 闭包及闭包的用处
> * 常见算法的JS实现（例如：实现将两个不同长度的数组组合，顺序越乱越好，以及其的复杂度）
> * 项目经历及作用和用到的技术等
> * CSS3动画（简单动画的实现，如旋转等）
> * SEO
> * 页面加载
> * HTML5的新特性
> * 常见组件的实现（如让你实现图片轮播、时间计时等）
> * 事件冒泡
> * 浏览器检测
> * canvas画图
> * doctype的作用
> * JavaScript代码测试
> * 匿名函数的作用（一般用在什么情况下）
> * call与apply的不同及bind的作用
> * 变量名提升
> * == 与 ===
> * "use strict"作用
> * AJAX请求的细节
> * 函数柯里化（Currying）
> * 同源策略（比较JSONP和document.domain的不同及优劣）
> * CSS不同选择器的权重
> * HTTP头部包含的信息及作用
> * HTML\CSS\JS在处理浏览器兼容性方面的实践
> * seaJS的用法及原理，依赖加载的原理、初始化、实现等
> * flexbox布局
> * 块级元素和行内元素的异同
> * CSS在性能优化方面的实践（比方说选择器的效率等）
> * NodeJS健壮性方面的实践（子进程等）
> * NodeJS能否用利用多核实现在计算性能上的劣势等
> * jQuery链式调用的原理
> * CSS打包压缩的方法
> * 前端发展的方向及你的了解和尝试（例如：组件化、工程化、前后端分离、前端质量体系、数据可视化、前端工具及生态圈、前端安全、下一代类库框架等）
> * ES6及jQuery新引进的Promise有什么用处
> * NodeJS的优缺点及使用场景
> * 使用CSS预处理的优缺点
> * 前端工作需要注重的哪些点儿及你在这方面的理解和实践（如：用户体验、性能优化等）
> * JS中random的概率问题
> * 客户端存储及他们的异同（例如：cookie, sessionStorage和localStorage等）
> * AngularJS的文件管理及打包（包括模板打包及请求、JS的打包和请求等）
> * AngularJS的JS模块管理及实践
> * 在你的Angular App页面里随意加一个JS文件，会有什么影响
> * AngularJS directive及自己如何定义directive
> * AngularJS双向绑定的原理及实现
> * 前端MVC与后端MVC的异同及你对前端MVC的理解（个人在实践方面的理解）
> * 什么是面向对象编程及面向过程编程，它们的异同和优缺点
> * 从你自己的理解来看，你是如何理解面向对象编程的，它解决了什么问题，有什么作用
> * 你对前端的理解？你为什么学前端？
> * “渐进增强”和“优雅降级”
> * 什么是“FOUC”及如何避免（http://blog.csdn.net/kongtoubudui/article/details/12975401）
> * HTML5中引进的data-有什么作用
> * HTML标准模式和怪异模式有什么不同
> * * { box-sizing: border-box; }这条CSS规则是干嘛的，有什么优点
> * 你如何测试你的JS代码
> * POST和GET的异同
> * 你是如何了解到并且学习一门技术的
> * 讲一下你读过的关于前端技术的书

--- 答案（仅作参考，欢迎补充完善）

### 1. doctype的作用
doctype告诉w3c验证器它使用了什么文档类型。它指出阅读程序应该用什么规则集来解释文档中的标记。XHTML中有三种，包括过度型、严格型、框架型。HTML4严格。随着XML的流行，HTML推出了XHTML标准，其中严格模式严格遵守了XML的规范，例如属性必须有值、标签必须闭合等，同时也抛弃了一些不推荐的标签。而XHTML过度版本，则稍微比严格模式松散些，一些不推荐的标签依然可用外。当页面有框架时，则应该使用框架型。再就是HTML5的版本。使用HTML5的Doctype会默认触发标准模式。

### 2. 同源策略（比较JSONP和document.domain的不同及优劣）
概念:同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。这里的同源指的是：同协议，同域名和同端口。
JSONP：一句话就是利用script标签绕过同源策略，获得一个类似这样的数据，jsonpcallback是页面存在的回调方法，参数就是想得到的json。
document.domain:浏览器都的同源策略，其限制之一就是第一种方法中我们说的不能通过ajax的方法去请求不同源中的文档。 它的第二个限制是浏览器中不同域的框架之间是不能进行js的交互操作的。有一点需要说明，不同的框架之间（父子或同辈），是能够获取到彼此的window对象的，但头疼的是你却不能使用获取到的window对象的属性和方法(html5中的postMessage方法是一个例外，还有些浏览器比如ie6也可以使用top、parent等少数几个属性)，总之，你可以当做是只能获取到一个几乎无用的window对象。document.domain开始派上用场，只要将同一域下不同子域的document.domain设置为共同的父域，则这个时候我们就可以访问对应window的各种属性和方法了。例如：www.example.com父域下的www.lib.example.com和www.hr.example.com两个子域，将对应页面的document.domain设为example.com即可。
（参考：http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html和http://www.cnblogs.com/2050/p/3191744.html及http://adam.kahtava.com/journal/2010/03/18/the-same-origin-policy-jsonp-vs-the-documentdomain-property/）

### 3. CORS跨域请求
这里说的js跨域是指通过js在不同的域之间进行数据传输或通信，比如用ajax向一个不同的域请求数据，或者通过js获取页面中不同域的框架中(iframe)的数据。只要协议、域名、端口有任何一个不同，都被当作是不同的域。跨域的方法参考上一问题答案。

### 4. 圣杯布局
详细描述：http://www.elonglau.com/33.html

### 5. JavaScript数据类型
JavaScript中有5种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number和String。还有1种复杂数据类型——Object，Object本质上是由一组无序的名值对组成的。

### 6. JSONP原理
答案参考2和3

### 7. CSS合并方法
避免使用@import引入多个css文件，可以使用CSS工具将CSS合并为一个CSS文件，例如使用Sass\Compass等。

### 8. 盒子模型
参考：http://www.w3school.com.cn/css/css_boxmodel.asp

### 9. CSS定位
参考：http://www.cnblogs.com/fsjohnhuang/p/3967350.html

### 10. XMLHttpRequest
参考：http://www.cnblogs.com/beniao/archive/2008/03/29/1128914.html

### 11. HTTP状态码
参考：http://baike.baidu.com/link?url=2Mur0ZYizeDAwgnKKu_REjgsbW-vVbKNA44xxHuz7C4sn6YBtIn2KCMPSSIqQJC0BQsQNxMKQE-EcS8iZPEWg_
大致来说就是：1开头的是消息，用的较少 2开头，代表成功 3开头代表重定向 4开头代表请求错误 5开头代表服务器错误

### 12. Cach-Control
http://baike.baidu.com/link?url=I2l51auZpAcJ8F0-ozRZUWRcCatmQz7PCZ8vdbEzHvCz_yJKcSSeDmn2cDWfOhrUIqL3KRa7wueujDcEZ9QBN_

| 打开新窗口	| 如果指定cache-control的值为private、no-cache、must-revalidate,那么打开新窗口访问时都会重新访问服务器。而如果指定了max-age值,那么在此值内的时间里就不会重新访问服务器,例如：Cache-control: max-age=5 表示当访问此网页后的5秒内再次访问不会去服务器. |
| 在地址栏回车	| 如果值为private或must-revalidate,则只有第一次访问时会访问服务器,以后就不再访问。如果值为no-cache,那么每次都会访问。如果值为max-age,则在过期之前不会重复访问。 |
| 按后退按扭	| 如果值为private、must-revalidate、max-age,则不会重访问,而如果为no-cache,则每次都重复访问. |
| 按刷新按扭	| 无论为何值,都会重复访问.

### 13. CSS动画原理
参考：http://blog.csdn.net/dojotoolkit/article/details/6859754

### 14. 事件委托
参考：http://www.cnblogs.com/owenChen/archive/2013/02/18/2915521.html

### 15. 前端模块化（AMD和CommonJS）
参考：阮一峰的博客http://www.ruanyifeng.com/blog/2012/10/javascript_module.html

### 16. session

### 17. cookie
参考：http://www.cnblogs.com/fish-li/archive/2011/07/03/2096903.html

### 18. ajax跨域
参考问题2和3的答案

### 19. this问题
参考：我之前写过的一篇博客（http://www.cnblogs.com/front-Thinking/p/4364337.html）

### 20. 模块化原理（作用域）
参考： 同问题15

### 21. JavaScript动画算法

### 22. 拖拽的实现
参考：http://www.cnblogs.com/cloudgamer/archive/2008/11/17/drag.html

### 23. JavaScript原型链

### 24. 闭包
参考：我之前写的一篇博客（http://www.cnblogs.com/front-Thinking/p/4317020.html）

### 25. 事件冒泡和事件捕获（及绑定同一元素多个事件的执行顺序等）
参考：http://www.quirksmode.org/js/events_order.html#link4