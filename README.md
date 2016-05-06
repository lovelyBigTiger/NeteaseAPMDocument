# NeteaseAPM iOS SDK技术实现分享

一.	[NeteaseAPM是什么](#一)  
二. [NeteaseAPM IOS SDK的目标](#二)  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;1. 最小侵入  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;2. 最大化自由配置  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;3. 更多功能  
三. [现状](#三)  
四. [整体设计](#四)  
五. [一些关键实现](#五)  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;1. [开始使用面向切面编程](#五1)  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;2. [使用代理模式采集回调消息的数据](#五2)  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;3. [借助桥接模式，从面向过程到面向对象](#五3)  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;4. [借助NSURLProtocol的UIWebView监控](#五4)  
六. [后续工作](#六)  

##一. <span id="一">NeteaseAPM是什么</span>

Application Performance Management(APM)，应用程序性能管理，
主要指对企业的关键业务应用进行监测、优化，提高企业应用的可靠性和质量，保证用户得到良好的服务。  
一个企业的关键业务应用的性能强大，可以提高竞争力，并取得商业成功。

NetsaseAPM是网易性能数据分析平台，一个用户数据分析平台，支持非侵入式获取应用性能数据，实时展示多个维度分析结果。  
目前支持移动端和浏览器端，此次分享只介绍IOS SDK端。

NetsaseAPM移动端支持的功能：

1. 应用性能分析  
对当前应用请求的各项性能指标进行分析，如响应时间，吞吐量，下载速率等，帮助用户全面了解应用性能表现。
2. 错误分析  
分析应用每个域名的网络错误率及响应码错误率，快速定位应用问题。
3. 多维分析  
可以组合域名，地理位置，运营商，网络环境等参数，精确定位应用的性能问题。

下面是从移动端收集的数据在NeteaseAPM Web平台的展示

<div align=center>
<img src="NAPMRequestSearch.png" width="800" height="160"/>
</div>
<div align=center>
<img src="NAPMRequest.png" width="800" height="300"/>
</div>
<div align=center>
<img src="NAPMUITrace.png" width="800" height="360"/>
</div>


##二. <span id="二">NeteaseAPM IOS SDK的目标</span>

1.	最小侵入：  
只需要启动一次，就可以持续收集网络和交互数据，不需要手动收集数据。  
启动方式：
<div align=center>
<img src="NAPMUseSample.png" width="800" height="116"/>
</div>
启动之后，NeteaseAPM会插入收集数据的代码到系统调用中，却不会影响用户的使用，在用户的网络消息中的位置如图：
<div align=center>
<img src="NAPMStruct.png" width="300" height="287"/>
</div>
2.	最大化自由配置：  
	*	用户可以看到收集到的数据，选择是否上传到NeteaseAPM服务器；  
	*	用户可以自己上传数据到NeteaseAPM服务器；  
3. 更多功能：  
首创监控底层网络库CFNetwork，和竞品监控第三方网络库ASIHttpRequest的方案相比，更加干净，能监控更多数据；

##三 <span id="三">现状</span>

目前实现的功能： 
 
*	网络请求的响应时间，下载速率，状态码，错误码，网络状态等数据的收集；
*	页面加载时间的收集，检查出慢交互页面；

已经接入的应用： 二次元，秀品，网易新闻，考拉。

##四 <span id="四">整体设计</span>

<div align=center>
<img src="NAPMStructDesign.png" width="400" height="272"/>
</div>

NeteaseAPM iOS SDK分为四个部分：  

+ Hooker：  
Hooker负责在用户感知不到的情况下替换程序原实现，转发消息回调，完成对系统消息的hook和数据的采集。  
Hooker能否监控到更多更准确的数据，是衡量一个APM产品是否优秀的最重要的标准。  
一个APM产品监控的数据面越广，收集的数据越细，就越能准确地取得用户的性能数据，帮助产品优化性能。

+ DataBuilder：     
收集监控数据；
+ Persistence：  
缓存监控数据；
+ Poster：         
上传监控数据到NeteaseAPM；  

线程模型： 监控数据的保存和发送都在后台队列中执行，不会影响用户线程。  

数据上传规则：  

*	可设置允许数据上传的网络环境；
*	数据支持批量发送，可自定义发送批量和等待间隔；

================================

##五. <span id="五">一些关键实现</span>

APM SDK使用方式和一般的UI库有很大不同，APM只需要启动一次即可生效，不需要修改代码。  
例如：启动APM之后，用户使用NSURLConnection执行的网络请求就会在开始连接，得到响应，获取数据等时机被APM监控到，
而不需要使用者手动添加任何监控代码。  
要实现这样的目标，Hooker需要使用一些比较特殊的的方式来实现，下面介绍Hooker使用到的解决方案：

###1.	<span id="五1">开始使用面向切面编程</span>  
APM需要将监控代码插入到系统实现中，完成系统消息的拦截和数据收集。  

比如我们需要知道NSURLConnection在什么时候开始发送请求，就会监控`-[NSURLConnection start]`方法的调用时机，
这就需要在这个方法中插入APM的逻辑，要做到这一点，需要理解面向切面编程的思想。

Aspect Oriented Programming(AOP)，面向切面编程，面对的是处理过程中的某个步骤或阶段。
在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面编程。
借助AOP，甚至不用修改一行代码，就可以修改现有程序的行为，非常高效。

AOP基本原理：将一个函数替换为一个新函数，新的函数中插入代码片段，然后执行原函数。
<div align=center>
<img src="NAPMAOPTheory.png" width="400" height="277"/>
</div>
iOS开发中用到的库按照实现语言可以分为Objective-C和C两种，下面介绍这两种语言能够支持AOP的原理。  

Objective-C对AOP的支持非常容易：  
runtime支持方法名和方法实现的分离，Objective-C的方法名类型是**SEL**，方法实现类型是**IMP**。  
通过一个例子了解SEL和IMP的关系：  
一个Objective-C方法 `[self setFilled:YES]`完全可以用下面的代码代替
<div align=left>
<img src="Objective-CAOP.png" width="580" height="97"/>
</div>
所以通过修改SEL对应的IMP，可以方便地hook原方法。  
Objective-C上的AOP有一个好听的名字：
[Method Swizzling](http://blog.csdn.net/yiyaaixuexi/article/details/9374411) 被灌醉的方法 ~~！  

C的AOP涉及到程序较底层，比较少见，简单了解一下：  
C函数指针的地址可以通过`dlsym`函数取得，如：
<div align=left>
<img src="NAPMDlsymClose.png" width="600" height="50"/>
</div>
C的AOP需要查找到这个函数指针的地址，再使用新的函数指针替换原函数指针。  
基于iOS动态链接器的符号绑定，查找函数指针地址的过程见下图：
<div align=center>
<img src="NAPMFindSymbolIMPLocation.png" width="600" height="460"/>
</div>
这里借助了一个开源库：[fishhook](https://github.com/facebook/fishhook) 。
###2.	<span id="五2">使用代理模式采集回调消息的数据</span>  
通过AOP，可以监控指定类的指定方法了，我们可以取得方法调用的时机了，
但是程序中除了方法调用还存在方法回调，这是一种不适合用AOP监控的情况。  

例如NSURLConnection的构造方法和start方法可以通过Method Swizzling监控到，
但是回调消息的接收者delegate的类名不固定，可能是任意一个页面实例，
如果还要使用Method Swizzling的方法来监控，会面对未知个数的页面的delegate方法，不是一个好办法。  

解决方法是构造一个回调消息的转发者作为代理，在转发者中收集数据，再转发给用户。  
下图演示对NSURLConnection的监控，MAM IMP就是被替换过的新的start方法的实现，
ProxyDelegate就是消息转发者，负责将回调消息转发给delegate对象：
<div align=center>
<img src="NAPMURLConnectionHook.png" width="600" height="347"/>
</div>  
要实现一个Objective-C的代理，一定要注意以下问题： 
 
* 系统在向delegate发送消息时会调用`-[NSObject respondsToSelector:]`方法，所以ProxyDelegate需要重写此方法，才能正确地获取到回调消息；
* ProxyDelegate没必要实现的用户delegate方法，如鉴权请求，需要借助Objective-C的动态特性，使用`-[NSObject forwardInvocation:]`方法将delegate能够响应的方法直接转发给用户的delegate；  

ProxyDelegate中的`forwardInvocation:`方法实现：
<div align=left>
<img src="ProxyDelegateInvocation.png" width="647" height="180"/>
</div>  

不止NSURLConnection，CFNetwork的监控也使用了代理模式：
<div align=center>
<img src="NAPMCFHookConnectionFlow.png" width="600" height="429"/>
</div>
Proxy Stream 拦截read方法，记录stream读取成功的数据长度，再转发给Original Stream。

###3.	<span id="五3">借助桥接模式，从面向过程到面向对象的数据</span>

CFNetwork是一个C语言实现的网络系统框架，虽然使用起来比较麻烦，但是可配置的功能更多，仍然被一些产品看好。  
但是由于面向过程编程难以扩展的缺点，没有办法选择性地监控和http有关的CFReadStream，而不影响到来自文件或内存的CFReadStream；

虽然面向过程编程难以扩展，但是Objective-C支持从面向过程到面向对象的桥接：  
Toll-Free Bridging，
它允许某些CoreFoundation类与其对应的Objective-C类互换使用，使我们在面向过程编程和面向对象编程两种编程思想中自由切换。

NeteaseAPM的策略：在系统构造http stream时，将一个NSInputStream的子类ProxyStream桥接为CFReadStream，返回给用户，
达到单独监控http stream的效果。
<div align=center>
<img src="NAPMCFHookCreate.png" width="360" height="178"/>
</div>
使用Toll-Free Bridging时需要重点关注两种编程思想切换时内存管理机制的不同引起的内存问题；  
例如`-[NSInputStream propertyForKey]`方法和`CFReadStreamCopyProperty`函数是桥接的，但是它们内存管理方法不同，前者是自动管理，后者是手动管理，后者在调用之后需要使用者在随后手动调用`CFRelease`。

以ProxyStream举例，ProxyStream是CFReadStream的代理，负责管理http stream的数据收集。  
如果用户对一个http stream执行如下调用：
<div align=left>
<img src="NAPMCopyProperty.png" width="563" height="54"/>
</div>
ProxyStream的`propertyForKey:`方法会因为桥接而被调用，ProxyStream需要从original stream中获取正确的property，如果ProxyStream中这么写
<div align=left>
<img src="NAPMErrorPropertyForKey.png" width="300" height="72"/>
</div>
那么这个方法返回的结果会被`CFReadStreamCopyProperty`随后的`CFRelease`函数错误地释放，引起内存异常。

正确的书写方式应该是
<div align=left>
<img src="NAPMRightPropertyForKey.png" width="600" height="75"/>
</div>
ProxyStream只需要”转发”`CFReadStreamCopyProperty`函数给original stream就可以了。

###4. <span id="五4">借助NSURLProtocol的UIWebView监控</span>
NSURLProtocol是监控UIWebView请求最普遍的解决方案。

这里用户会产生多个URLProtocol会不会冲突的疑问？  
结论是不会，因为即使用户注册了NSURLProtocol，拦截了NeteaseAPM的URLProtocol的请求，
但用户的URLProtocol发送请求时的NSURLConnection仍然会被NeteaseAPM监控到的；

UIWebView的请求比较复杂，下面收集几个容易出错的问题：

*	某些网页的验证码的请求可能是一个阻塞主线程的请求，在URLProtocol的网络请求需要放在后台执行，否则会被阻塞住而无法得到响应。
*	重定向的网页请求需要转发给网页，APM不能监控。

================================

##六. <span id="六">后续工作</span>
*	支持更多的性能数据的收集，如内存，CPU，帧率等；
*	支持性能问题的自动诊断，如卡顿监控，Out of Memory问题诊断等，同时提高性能问题诊断的能力；