### H5与Native交互之JSBridge技术

转载出处：https://tech.youzan.com/jsbridge/

做过混合开发的很多人都知道Ionic和PhoneGap之类的框架，这些框架在web基础上包了一层Native，然后通过Bridge技术使得js可以调用视频、位置、音频等功能。本文就是介绍这层Bridge的交互原理，通过阅读本文你可以了解到js与ios及android底层的通讯原理及JSBridge的封装技术及调试方法。
一、原理篇下面分别介绍IOS和Android与Javascript的底层交互原理
IOS在讲解原理之前，首先来了解下iOS的UIWebView组件，先来看一下苹果官方的介绍：
You can use the UIWebView class to embed web content in your application. To do so, you simply create a UIWebView object, attach it to a window, and send it a request to load web content. You can also use this class to move back and forward in the history of webpages, and you can even set some web content properties programmatically.
上面的意思是说UIWebView是一个可加载网页的对象，它有浏览记录功能，且对加载的网页内容是可编程的。说白了UIWebView有类似浏览器的功能，我们使用可以它来打开页面，并做一些定制化的功能，如可以让js调某个方法可以取到手机的GPS信息。
但需要注意的是，Safari浏览器使用的浏览器控件和UIwebView组件并不是同一个，两者在性能上有很大的差距。幸运的是，苹果发布iOS8的时候，新增了一个WKWebView组件，如果你的APP只考虑支持iOS8及以上版本，那么你就可以使用这个新的浏览器控件了。
原生的UIWebView类提供了下面一些属性和方法
属性：

	* loading：是否处于加载中
	* canGoBack：A Boolean value indicating whether the receiver can move backward. (只读)
	* canGoForward：A Boolean value indicating whether the receiver can move forward. (只读)
	* request：The URL request identifying the location of the content to load. (read-only)

方法：

	* loadData：Sets the main page contents, MIME type, content encoding, and base URL.
	* loadRequest：加载网络内容
	* loadHTMLString：加载本地HTML文件
	* stopLoading：停止加载
	* goBack：后退
	* goForward：前进
	* reload：重新加载
	* stringByEvaluatingJavaScriptFromString：执行一段js脚本，并且返回执行结果

Native（Objective-C或Swift）调用Javascript方法Native调用Javascript语言，是通过UIWebView组件的stringByEvaluatingJavaScriptFromString方法来实现的，该方法返回js脚本的执行结果。
// Swift
webview.stringByEvaluatingJavaScriptFromString("Math.random()")
// OC
[webView stringByEvaluatingJavaScriptFromString:@"Math.random();"];

从上面代码可以看出它其实就是调用了window下的一个对象，如果我们要让native来调用我们js写的方法，那这个方法就要在window下能访问到。但从全局考虑，我们只要暴露一个对象如JSBridge对native调用就好了，所以在这里可以对native的代码做一个简单的封装：
//下面为伪代码
webview.setDataToJs(somedata);
webview.setDataToJs = function(data) {
 webview.stringByEvaluatingJavaScriptFromString("JSBridge.trigger(event, data)")
}

Javascript调用Native（Objective-C或Swift）方法反过来，Javascript调用Native，并没有现成的API可以直接拿来用，而是需要间接地通过一些方法来实现。UIWebView有个特性：在UIWebView内发起的所有网络请求，都可以通过delegate函数在Native层得到通知。这样，我们就可以在UIWebView内发起一个自定义的网络请求，通常是这样的格式：jsbridge://methodName?param1=value1&param2=value2
于是在UIWebView的delegate函数中，我们只要发现是jsbridge://开头的地址，就不进行内容的加载，转而执行相应的调用逻辑。
发起这样一个网络请求有两种方式：1. 通过localtion.href；2. 通过iframe方式； 通过location.href有个问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。
使用iframe方式，以唤起Native APP的分享组件为例，简单的封闭如下：
var url = 'jsbridge://doAction?title=分享标题&desc=分享描述&link=http%3A%2F%2Fwww.baidu.com';
var iframe = document.createElement('iframe');
iframe.style.width = '1px';
iframe.style.height = '1px';
iframe.style.display = 'none';
iframe.src = url;
document.body.appendChild(iframe);
setTimeout(function() {
    iframe.remove();}, 100);
然后Webview就可以拦截这个请求，并且解析出相应的方法和参数。如下代码所示：
func webView(webView: UIWebView, shouldStartLoadWithRequest request: NSURLRequest, navigationType: UIWebViewNavigationType) -> Bool {
        print("shouldStartLoadWithRequest")
        let url = request.URL
        let scheme = url?.scheme
        let method = url?.host
        let query = url?.query

        if url != nil && scheme == "jsbridge" {
            print("scheme == \(scheme)")
            print("method == \(method)")
            print("query == \(query)")

            switch method! {
                case "getData":
                    self.getData()
                case "putData":
                    self.putData()
                case "gotoWebview":
                    self.gotoWebview()
                case "gotoNative":
                    self.gotoNative()
                case "doAction":
                    self.doAction()
                case "configNative":
                    self.configNative()
                default:
                    print("default")
            }

            return false
        } else {
            return true
        }
    }

Android在android中，native与js的通讯方式与ios类似，ios中的通过schema方式在android中也是支持的。
javascript调用native方式目前在android中有三种调用native的方式：
1.通过schema方式，使用shouldOverrideUrlLoading方法对url协议进行解析。这种js的调用方式与ios的一样，使用iframe来调用native代码。
2.通过在webview页面里直接注入原生js代码方式，使用addJavascriptInterface方法来实现。
在android里实现如下：
class JSInterface {
    @JavascriptInterface //注意这个代码一定要加上
    public String getUserData() {
        return "UserData";
    }
}
webView.addJavascriptInterface(new JSInterface(), "AndroidJS");

上面的代码就是在页面的window对象里注入了AndroidJS对象。在js里可以直接调用
alert(AndroidJS.getUserData()) //UserDate
3.使用prompt,console.log,alert方式，这三个方法对js里是属性原生的，在android webview这一层是可以重写这三个方法的。一般我们使用prompt，因为这个在js里使用的不多，用来和native通讯副作用比较少。
class YouzanWebChromeClient extends WebChromeClient {
    @Override
    public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {
        // 这里就可以对js的prompt进行处理，通过result返回结果
    }
    @Override
    public boolean onConsoleMessage(ConsoleMessage consoleMessage) {

    }
    @Override
    public boolean onJsAlert(WebView view, String url, String message, JsResult result) {

    }

}

Native调用javascript方式在android里是使用webview的loadUrl进行调用的，如：
// 调用js中的JSBridge.trigger方法
webView.loadUrl("javascript:JSBridge.trigger('webviewReady')");

二、库的封装js调用native的封装上面我们了解了js与native通讯的底层原理，所以我们可以封装一个基础的通讯方法doCall来屏蔽android与ios的差异。
YouzanJsBridge = {
    doCall: function(functionName, data, callback) {
        var _this = this;
        // 解决连续调用问题
        if (this.lastCallTime && (Date.now() - this.lastCallTime) < 100) {
            setTimeout(function() {
                _this.doCall(functionName, data, callback);
            }, 100);
            return;
        }
        this.lastCallTime = Date.now();

        data = data || {};
        if (callback) {
            $.extend(data, { callback: callback });
        }

        if (UA.isIOS()) {
            $.each(data, function(key, value) {
                if ($.isPlainObject(value) || $.isArray(value)) {
                    data[key] = JSON.stringify(value);
                }
            });
            var url = Args.addParameter('youzanjs://' + functionName, data);
            var iframe = document.createElement('iframe');
            iframe.style.width = '1px';
            iframe.style.height = '1px';
            iframe.style.display = 'none';
            iframe.src = url;
            document.body.appendChild(iframe);
            setTimeout(function() {
                iframe.remove();
            }, 100);
        } else if (UA.isAndroid()) {
            window.androidJS && window.androidJS[functionName] && window.androidJS[functionName](JSON.stringify(data));
        } else {
            console.error('未获取platform信息，调取api失败');
        }
    }}
上面android端我们使用了addJavascriptInterface方法来注入一个AndroidJS对象。
项目通用方法抽象在项目的实践中，我们逐渐抽象出一些通用的方法，这些方法基本上都是可以满足项目的需求。如下所示：
1.getData(datatype, callback, extra) H5从Native APP获取数据使用场景：H5需要从Native APP获取某些数据的时候，可以调用这个方法。
参数类型是否必须示例值说明datatypeString是userInfo数据类型callbackFunction是
回调函数extraObject否
传递给Native APP的数据对象示例代码：
JSBridge.getData('userInfo',function(data) {
    console.log(data);});
2.putData(datatype, data) H5告诉Native APP一些数据使用场景：H5告诉Native APP一些数据，可以调用这个方法。
参数类型是否必须示例值说明datatypeString是userInfo数据类型dataObject是{ username: 'zhangsan', age: 20 }传递给Native APP的数据对象示例代码：
JSBridge.putData('userInfo', {
    username: 'zhangsan',
    age: 20});
3.gotoWebview(url, page, data) Native APP新开一个Webview窗口，并打开相应网页参数类型是否必须示例值说明urlString是http://www.youzan.com网页链接地址，一般都只要传递URL参数就可以了pageString否web网页page类型，默认为webdataObject否
额外参数对象示例代码：
// 示例1：打开一个网页
JSBridge.gotoWebview('http://www.youzan.com');

// 示例2：打开一个网页，并且传递额外的参数给Native APP
JSBridge.gotoWebview('http://www.youzan.com', 'goodsDetail', {
    goods_id: 10000,
    title: '这是商品的标题',
    desc: '这是商品的描述'});
4.gotoNative(page, data) 从H5页面跳转到Native APP的某个原生界面参数类型是否必须示例值说明pageString是loginPageNative页面标示符，例如loginPagedataObject否{ username: 'zhangsan', age: 20 }额外参数对象示例代码：
// 示例1：打开Native APP登录页面
JSBridge.gotoNative('loginPage');

// 示例2：打开Native APP登录页面，并且传递用户名给Native APP
JSBridge.gotoNative('loginPage', {
    username: '张三'});
5.doAction(action, data) 功能上的一些操作参数类型是否必须示例值说明actionString是copy操作功能类型，例如分享、复制dataObject否{ content: '这是要复制的内容' }额外参数示例代码：
// 示例1：调用Native APP复制一段文本到剪切板
JSBridge.doAction('copy', {
    content: '这是要复制的内容'});

// 示例2：调用Native APP的分享组件，分享当前网页到微信
JSBridge.doAction('share', {
    title: '分享标题',
    desc: '分享描述',
    link: 'http://www.youzan.com',
    imgs_url: 'http://wap.koudaitong.com/v2/common/url/create?type=homepage&index%2Findex=&kdt_id=63077&alias=63077'});
三、调试篇使用Safari进行UIWebView的调试（1）首先需要打开Safari的调试模式，在Safari的菜单中，选择“Safari”→“Preference”→“Advanced”，勾选上“Show Develop menu in menu bar”选项，如下图所示。（2）打开真机或iPhone模拟器的调试模式，在真机或iPhone模拟器中打开设置界面，选择“Safari”→“高级”→“Web检查器”，选择开启即可，如下图所示。（3）将真机通过USB连上电脑，或者开启模拟器，Safari的“Develop”菜单下便会多出相应的菜单项，如图所示。

（4）Safari连接上UIWebView之后，我们就可以直接在Safari中直接修改HTML、CSS，以及调试Javascript。

四、参考链接
	* UIWebView Class Reference
	* WKWebView Class Reference
	* https://github.com/marcuswestin/WebViewJavascriptBridge


本文由 @kk @劲风 共同创作
