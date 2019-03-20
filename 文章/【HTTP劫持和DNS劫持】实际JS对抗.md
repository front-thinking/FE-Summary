【HTTP劫持和DNS劫持】实际JS对抗
1、对于DIV注入的，可以初始化时检查全部html代码。
检测是否被劫持比较简单，但对抗就略麻烦，这个在说完第2点之后再解释。



2、对于js注入，可以在window监听DOMNodeInserted事件。
事件有srcElement，可以获取到刚插入的dom节点。
这里开始简单粗暴的做正则匹配，匹配所有url。
再逐个比较是否白名单域名，如果不是，则判定为劫持。可以上报，同时可以移除dom.parentNode.removeChild(dom);
但这样容易造成误伤，因为正常页面中可能有外部链接，或者一些纯文本url。

    function checkDivHijack(e) {
        var html = e ? (e.srcElement.outerHTML || e.srcElement.wholeText) : $('html').html();
        var reg = /http:\/\/([\w.:]+\/)[^'"\s]+/g;
        var urlList = html.match(reg);
        if (!urlList || urlList.length == 0) {
            return;
        }
        reg = /^http:\/\/(.*\.qq\.com|.*\.gtimg\.cn|.*\.qlogo\.cn|.*\.qpic\.cn|.*\.wanggou\.com|.*\.jd\.com)\/$/;
        var hijack = false;
        for (var i = 0; i < urlList.length; i++) {
            if (!reg.test(urlList[i])) {
                hijack = true;
                break;
            }
        }
     }
后来改为

    function checkDivHijack(e) {
        var dom = e ? e.srcElement : document.documentElement;
        if (!dom.outerHTML) {
            return;     //e不是一个dom，只是插入一段文本
        }

        var imgList = (dom.nodeName.toUpperCase() == 'IMG') ? [dom] : dom.getElementsByTagName('img');
        if (!imgList || imgList.length == 0) {
            return;
        }

        var httpReg = /^http:\/\/(.*\.qq\.com|.*\.gtimg\.cn|.*\.qlogo\.cn|.*\.qpic\.cn)\//;
        var base64Reg = /^data:image/;
        var src;
        var hijack = false;
        for (var i = 0; i < imgList.length; i++) {
            src = imgList[i].src;
            if (!httpReg.test(src) && !base64Reg.test(src)) {
                hijack = true;
                break;
            }
        }
     }
但这样也有漏洞，如果运营商通过div+style设置背景的方式显示广告图，上述代码就无法检查出来。

那么，就还需要检查style的情况，但style情况就更复杂了。可能是<style>，也可能是inline样式，最终还是要回到url识别上。
那么做个折衷，我们继续用最初的纯文本正则匹配url的方式，但跳过纯文本的情况（例如修改div的内容，替换为一段文本），只检查插入dom的情况。
具体方法是

        if (!dom.outerHTML) {
            return;     //e不是一个dom，只是插入一段文本
        }
回到刚才第一点的问题，监测第一点的情况，可以用一样的做法。但是，对抗就麻烦很多，因为广告dom节点可以插在body第一层，也可以插在某个内容div中。如果简单粗暴的把广告dom节点到body的全部div都移除，可能会造成大面积的误伤。
所以，针对这个情况，我们还在做进一步的监测统计。



3、对于iframe的情况，要检测非常简单，只需要比较self和top是否相同。
不过，要完整解决这个嵌套劫持，就要知道运营商的小把戏。
试想一下，iframe前，请求http://www.host.com/xxx.html ，就被劫持，302重定向到一个iframe的页面，这个页面使用iframe重新加载我们原来要请求的html。
那么，此时在iframe中的html为什么能够顺利加载回来呢？而不是又被劫持？
我们猜想，运营商应该在url中加了一个参数，标记是否已经劫持过。
而实际监测发现，我们的猜想也是正确的。

呃，我们仔细看，还可以发现运营商做这个劫持也非常粗暴，如果页面依赖hash，就会引起错误了。
Image

见招拆招，这个比较好办，我们只需要把top的地址修改为self地址即可。一来冲掉iframe，二来绕过劫持。

    function checkIframeHijack() {
        var flag = 'iframe_hijack_redirected';
        if (getURLParam(flag)) {
            sendHijackReport('jiankang.hijack.iframe_ad', 'iframe hijack: ' + location.href);
        } else {
            if (self != top) {
                var url = location.href;
                var parts = url.split('#');
                if (location.search) {
                    parts[0] += '&' + flag + '=1';
                } else {
                    parts[0] += '?' + flag + '=1';
                }
                try {
                    top.location = parts.join('#');
                } catch (e) {
                }
            }
        }
    }
为了安全起见，防止运营商有新招数，所以这里只尝试一次，用iframe_hijack_redirected参数标记，已经尝试过。

按照统计情况来看，运营商还是挺猖狂的，平均大约有6~10个劫持上报，大概占整个QQ健康用户的3%到5%。
