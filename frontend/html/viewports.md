【A tale of two viewports 】
【两个viewports的故事】

http://www.w3cplus.com/css/viewports.html  参照译文1，w3cplus转载  
http://weizhifeng.net/viewports.html 参照译文2，官方原文中文版 
http://www.quirksmode.org/mobile/viewports.html  原文1  
http://www.quirksmode.org/mobile/viewports2.html 原文2  

In this mini-series I will explain how viewports and the widths of various important elements work, such as the <html> element, as well as the window and the screen.

在这个迷你系列的文章里我将解释viewports以及许多重要的HTML标签元素的宽度是如何工作的，例如<html>元素，也包括窗口（window）和屏幕（screen）。

This page is about the desktop browsers, and its sole purpose is to set the stage for a similar discussion of the mobile browsers. Most web developers will already intuitively understand most desktop concepts. On mobile we’ll find the same concepts, but more complicated, and a prior discussion on terms everybody already knows will greatly help your understanding of the mobile browsers.

这篇文章（第一部分）主要关于桌面浏览器的，其唯一的目的就是为移动浏览器中相似的讨论做个铺垫。大部分开发者凭直觉已经明白了大部分桌面浏览器中的概念。在移动端我们将会接触到相同的概念，但是会更加复杂，所以对大家已经知道的术语做个提前的讨论将会对你理解移动浏览器产生巨大的帮助（友好的预热）。

【Concept: device pixels and CSS pixels】
【概念：设备像素和CSS像素】

The first concept you need to understand is CSS pixels, and the difference with device pixels.

你需要明白的第一个概念是CSS像素，以及它和设备像素的区别。

Device pixels are the kind of pixels we intuitively assume to be “right.” These pixels give the formal resolution of whichever device you’re working on, and can (in general) be read out from screen.width/height.

设备像素是我们直觉上觉得「靠谱」的像素。这些像素为你所使用的各种设备都提供了正规的分辨率，并且其值可以（通常情况下）从screen.width/height属性中读出。

If you give a certain element a width: 128px, and your monitor is 1024px wide, and you maximise your browser screen, the element would fit on your monitor eight times (roughly; let’s ignore the tricky bits for now).

如果你给一个元素设置了width: 128px的属性，并且你的显示器是1024px宽，当你最大化你的浏览器屏幕，这个元素将会在你的显示器上重复显示8次（大概是这样；我们先忽略那些微妙的地方）。

If the user zooms, however, this calculation is going to change. If the user zooms to 200%, your element with width: 128px will fit only four times on his 1024px wide monitor.

如果用户进行缩放（zoom），那么计算方式将会发生变化。如果用户放大到200%，那么你的那个拥有width:128px属性的元素在1024px宽的显示器上只会重复显示4次。

Zooming as implemented in modern browsers consists of nothing more than “stretching up” pixels. That is, the width of the element is not changed from 128 to 256 pixels; instead the actual pixels are doubled in size. Formally, the element still has a width of 128 CSS pixels, even though it happens to take the space of 256 device pixels.

现代浏览器中实现缩放的方式无怪乎都是「拉伸」像素。所以，元素的宽度并没有从128个像素被修改为256个像素；相反是实际像素被放大了两倍。形式上，元素仍然是128个CSS像素宽，即使它占据了256个设备像素的空间。

In other words, zooming to 200% makes one CSS pixel grow to four times the size of one device pixels. (Two times the width, two times the height, yields four times in total).

换句话说，放大到200%使一个CSS像素变成为一个设备像素的四倍。（宽度2倍，高度2倍，总共4倍）

A few images will clarify the concept. Here are four pixels on 100% zoom level. Nothing much to see here; CSS pixels fully overlap with device pixels.

一些配图可以解释清楚这个概念。这儿有四个100%缩放比的元素。这儿没有什么值得看的；CSS像素与设备像素完全重叠。

![](/img/viewports/csspixels_100.gif)

当我们缩小浏览器时，CSS的pixels开始收缩，导致1单位的设备的pixels上重叠了多个CSS的pixels，如图1-2
![](/img/viewports/csspixels_out.gif)
同理，放大浏览器时，相反的事情发生了，CSS的pixels开始扩大，导致1单位的CSS的pixels上重叠了多个设备的pixels，如图1-3

![](/img/viewports/csspixels_in.gif)

总体而言，你只需要关注CSS的pixels，这些pixels指定你的样式被如何渲染。

设备的pixels几乎对你毫无用处。但对用户而言却不是这样。用户会缩放页面，直到他能舒服的阅读内容。但是你不需关心这些缩放级别。浏览器会自动的保证你的CSS的pixels会被伸展还是收缩。

【100% 缩放】
本例设定缩放级别为100%。现在我们更严谨的定义，如下：

    在缩放级别为100%时，1单位的CSS的pixel是严格相等于1单位的设备pixel

100%缩放的概念非常有利于表述接下来的内容，但你不必在日常工作中过度担忧这个问题。在桌面系统上，你通常会在100%缩放级别下测试你的网站，但即便用户缩放，CSS的pixels的魔法依然能保证你网站外观保存相同的比例。

【屏幕尺寸 Screen size】

screen.width/height

* 含义：用户的屏幕的完整大小。
* 度量：设备的pixels。
* 兼容性问题：IE8里，不管使用IE7模式还是IE8模式，都以CSS的pixels来度量

我们先了解一些特殊的尺寸：screen.width 和 screen.height。
这两个属性包含了用户屏幕的完整宽度高度。这些尺寸使用设备的pixels来定义，他们的值不会因为缩放而改变：他们是显示器的特征，而不是浏览器。如图1-4所示。
[]()
很有趣吧？但是我们拿来何用呢？
简单的说，木有用！用户的显示器宽度对我们而言不重要 – 除非你想要用他们做网络统计数据.

【浏览器尺寸 Window size】

window.innerWidth/Height

* 含义：包含滚动条尺寸的浏览器完整尺寸
* 度量：CSS的pixels
* 兼容性问题：IE不支持，Opera用设备pixels来度量

相反的，你想要知道的浏览器的内部尺寸。它定义了当前用户有多大区域，可供你的CSS布局占用。你可以通过window.innerWidth 和 window.innerHeight来获取。如图1-5
[]()
显然，窗口的内部宽度使用CSS的pixels.你需要知道多少你自己定义的元素能塞入浏览器窗口，而这些数量会随着用户放大浏览器而减少（如图1-6）。所以当用户放大显示时，你能获取的浏览器窗口可用空间会减少，window.innerWidth/Height就是缩小的比例。

    Opera浏览器在这个问题上是一朵奇葩，当用户放大浏览器显示时不少。所以当用户放大显示时，你能获取的浏览器窗口可用空间会减少。window.innerWidth/Height 却并不会减小。在桌面浏览器上，这个特性很烦人，但在移动设备浏览器上简直是致命的，后面我们会讨论
    
[]()

注意，窗口内部宽度和高度的尺寸，包含了滚动条的尺寸。（这主要是来至于历史原因）

【滚动移位 Scrolling offset】

window.pageX/YOffset

* 含义：页面的移位
* 度量：CSS的pixels
* 兼容性问题：pageXOffset 和 pageYOffset 在 IE 8 及之前版本的IE不支持, 使用”document.body.scrollLeft” and “document.body.scrollTop” 来取代

window.pageXOffset 和 window.pageYOffset，定义了页面(document)的相对于窗口原点的水平、垂直位移。因此你能够定位用户滚动了多少的滚动条距离。如图1-7
[]()

该属性也以CSS的pixels来度量.同上面的问题，你想要知道在用户放大窗口的情况下，用户向上滚动了多少的滚动条。

原理上来说，在用户放大浏览器时，向上滚动了页面，window.pageX/YOffset会改变。但当用户放大页面时，浏览器会尝试着保存用户当前可见的页面的元素依然在可见位置。虽然该特性表现得不如预期，但它意味着：在理论上 该情况下 window.pageX/YOffset并没有改变，被用户滚出屏幕的CSS的pixels几乎保存不变。如图1-8。

【概念：视窗 viewport】

在我们继续讨论更多的JavaScript的特性properties之前，先介绍另外一个概念:viewport。

viewport的功能在于控制你网站的最高块状（block）容器：<html>元素。

听起来有点玄乎，举个例子~假设你定义了一个可变尺寸的布局（liquid layout），且你定义一个侧边栏的宽度为width: 10%。当你改变浏览器窗口大小时，该侧边栏会自动扩张和收缩。这是什么原理呢？

Technically, what happens is that the sidebar gets 10% of the width of its parent. Let’s say that’s the <body> (and that you haven’t given it a width). So the question becomes which width the <body> has.

技术上讲，原理是侧边栏的宽度为它父元素宽度的10%，我们设它的父元素是body，且你未指定宽度。那么问题就变为了<body>的宽度到底是多少？

Normally, all block-level elements take 100% of the width of their parent (there are exceptions, but let’s ignore them for now). So the <body> is as wide as its parent, the <html> element.

通常，一个块级元素占有起父元素的100%的宽度（这里有异常情况，暂时忽略）。所以<body>的宽度就是其父元素<html>的宽度。

And how wide is the <html> element? Why, it’s as wide as the browser window. That’s why your sidebar with width: 10% will span 10% of the entire browser window. All web developers intuitively know and use this fact.

那么<html>元素到底有多宽？因为它的宽度恰好为浏览器的宽度。所以你的侧边栏宽度width:10%会占用10%的浏览器宽度。所以的web开发人员都直观的知道和使用该特性了。

What you may not know is how this works in theory. In theory, the width of the <html> element is restricted by the width of the viewport. The <html> element takes 100% of the width of that viewport.

但是你也许不知道原理。在原理上，<html>的宽度受viewport所限制，<html>元素为viewport宽度的100%。

The viewport, in turn, is exactly equal to the browser window: it’s been defined as such. The viewport is not an HTML construct, so you cannot influence it by CSS. It just has the width and height of the browser window — on desktop. On mobile it’s quite a bit more complicated.

反过来，viewport是严格的等于浏览器的窗口：定义就是如此。viewport不是一个HTML的概念，所以你不能通过CSS修改它。它就是为浏览器窗口的宽度高度 – 在桌面浏览器上如此，移动设备浏览器上有点复杂。

【影响 Consequences】

This state of affairs has some curious consequences. You can see one of them right here on this site. Scroll all the way up to the top, and zoom in two or three steps so that the content of this site spills out of the browser window.

Now scroll to the right, and you’ll see that the blue bar at the top of the site doesn’t line up properly any more.

缩放事件有一些奇怪的影响，你可以在本站上实验。页面滚动到最上面，放大浏览器2-3倍，网站的宽度会超过浏览器窗口。再将页面滚动到最右边，你会发现网站最上面的蓝色栏目不再对齐了。如图2-1

This behaviour is a consequence of how the viewport is defined. I gave the blue bar at the top a width: 100%. 100% of what? Of the <html> element, which is as wide as the viewport, which is as wide as the browser window.

这个效果反应了viewport是如何被定义的。我定义了最上面蓝色栏目的宽度为width: 100%。什么的100%？当然是html的宽度，同样是viewport的宽度，同样是浏览器窗口的宽度。

Point is: while this works fine at 100% zoom, now that we’ve zoomed in the viewport has become smaller than the total width of my site. In itself that doesn’t matter, the content now spills out of the <html> element, but that element has overflow: visible, which means that the spilled-out content will be shown in any case.

重点：缩放比例100%的情况下很正常，现在我们放大浏览器，viewport变得比网站的总宽度更小。对viewport无影响，但页面的内容溢出了<html>元素，但它却有属性overflow: visible。意味着溢出的部分依然会被显示。

But the blue bar doesn’t spill out. I gave it a width: 100%, after all, and the browsers obey by giving it the width of the viewport. They don’t care that that width is now too narrow.

但蓝色栏目却不会溢出。我定义了它宽度为width:100%，结果浏览器为他赋值宽度为viewport的宽度。浏览器不会在乎这个栏目的宽度是不是过窄了。如图2-2

【页面宽度 document width ?】

What I really need to know is how wide the total content of the page is, including the bits that “stick out.” As far as I know it’s not possible to find that value (well, unless you calculate the individual widths and margins of all elements on the page, but that’s error-prone, to put it mildly).

我真正想要知道的是页面内容的总大小，包括超出浏览器窗口的部分。到目前为止，据我所知并没有办法找到这个值（当然，除非你计算页面所有部分的宽度包括所有元素的margin，但是这种计算很容易出错）。

I am starting to believe that we need a JavaScript property pair that gives what I’ll call the “document width” (in CSS pixels, obviously).

我开始相信我们需要一个JavaScript特性对(property pair)来标示我所谓的页面宽度 document width(当然，以CSS的pixels来度量)。如图2-3

And if we’re really feeling funky, why not also expose this value to CSS? I’d love to be able to make the width: 100% of my blue bar dependent on the document width, and not the <html> element’s width. (This is bound to be tricky, though, and I wouldn’t be surprised if it’s impossible to implement.)

如果我们真的感觉对这事儿很烦躁：为什么不在CSS中揭露这些值？我期望定义width:100%来控制页面蓝色栏目的宽度，它基于页面的宽度而不是元素的宽度。这似乎很棘手(This is bound to be tricky）,如果这成功的被实现我也不会感倒惊讶。

【度量viewport Measuring the view port】
document. documentElement. clientWidth/Height
* 含义：viewport的尺寸
* 度量：CSS的pixels
* 兼容性问题：无
 
You might want to know the dimensions of the viewport. They can be found in document.documentElement.clientWidth and -Height.
你也许想要知道viewport的尺寸，他们可以通过document. documentElement. clientWidth/Height来获取。如图2-4

If you know your DOM, you know that document.documentElement is in fact the <html> element: the root element of any HTML document. However, the viewport is one level higher, so to speak; it’s the element that contains the <html> element. That might matter if you give the <html> element a width. (I don’t recommend that, by the way, but it’s possible.)

如果你熟悉DOM，你会知道document.documentElement实际上就是<html>元素：HTML文档的根元素。然而viewport是比<html>更高级别的元素，打个比喻，它是容纳<html>元素的元素。那会和你是否给元素赋值width相关（我不建议这么做，但是却是可行的）

In that situation document.documentElement.clientWidth and -Height still gives the dimensions of the viewport, and not of the <html> element. (This is a special rule that goes only for this element only for this property pair. In all other cases the actual width of the element is used.)

在那种情况下document. documentElement. clientWidth/Height依然给出了viewport的尺寸，而不是<html>元素。（这是特殊的规则只针对这个特殊的元素针对这个特性对。在其余任何情况下元素使用实际的宽度）如图2-5。为<html>元素赋值25%。但document. documentElement. clientWidth/Height的值不变。它虽然貌似从<html>元素取值，但实际描述的确是viewport的尺寸。

So document.documentElement.clientWidth and -Height always gives the viewport dimensions, regardless of the dimensions of the <html> element.

所以document. documentElement. clientWidth/Height只会给出viewport的尺寸，而不管<html>元素尺寸如何改变。

【两个特性对 two property pairs】

But aren’t the dimensions of the viewport width also given by window.innerWidth/Height? Well, yes and no.
但是viewport的尺寸不是也通过window.innerWidth/Height来描述的么？嗯，是，也不是。

There’s a formal difference between the two property pairs: document.documentElement.clientWidth and -Height doesn’t include the scrollbar, while window.innerWidth/Height does. That’s mostly a nitpick, though.

这两个特性对有严格的区别，几乎算是吹毛求疵了
* window.innerWidth/Height包含滚动条
* document. documentElement. clientWidth/Height不包含

The fact that we have two property pairs is a holdover from the Browser Wars. Back then Netscape only supported window.innerWidth/Height and IE only document.documentElement.clientWidth and -Height. Since then all other browsers started to support clientWidth/Height, but IE didn’t pick up window.innerWidth/Height.

我们能获取这两个特性对是因为他们是浏览器大战的残留。过去Netscape只支持window.innerWidth/Height，IE 只支持document. documentElement. clientWidth/Height。从那时候开始所有其余浏览器都支持这两个特性。但IE一直未支持window.innerWidth/Height。

Having two property pairs available is a minor nuisance on desktop — but it turns out to be a blessing on mobile, as we’ll see.

在桌面系统中，拥有这两个特性对只是一点小麻烦，但在移动设备中却变成了一种祝福。后面我们会看到 。

【度量<html>元素 measuring the <html>element】

So clientWidth/Height gives the viewport dimensions in all cases. But where can we find the dimensions of the <html> element itself? They’re stored in document.documentElement.offsetWidth and -Height.

document. documentElement. offsetWidth/Height
* 含义：<html>的尺寸
* 度量：CSS的pixels
* 兼容性问题：IE用这个值标示viewport的尺寸而非<html>

如果clientWidth/Height一直用以标示viewport的尺寸，我们该如何去获取<html>元素的尺寸呢？— document.documentElement.offsetWidth/Height。如图2-6

These properties truly give you access to the <html> element as a block-level element; if you set a width, offsetWidth will reflect it.
  
这个特性对真实的让你访问块级元素<html>元素，如果你为<html>元素赋值了宽度，offsetWidth会真实的反应出来。如图2-7


【事件坐标 Event coordinates】

pageX/Y, clientX/Y, screenX/Y

* 含义：见下文
* 度量：见下文
* 兼容性问题：IE不支持pageX/Y,IE使用CSSpixels来度量screanX/Y
* 详细描述:
* pageX/Y：从<html>原点到事件触发点的CSS的 pixels
* clientX/Y：从viewport原点（浏览器窗口）到事件触发点的CSS的 pixels
* screenX/Y：从用户显示器窗口原点到事件触发点的设备 的 pixels。

如图2-8、2-9和2-10


9成可能你会用到pageX/Y而1成左右会使用clientX/Y,screenX/Y基本没啥用。


【Media查询 media queries】
mediaqueries
* 含义：见下文
* 度量：见下文
* 兼容性问题：IE不支持.

最后一点文字关于@media的css属性。出发点很简单：你可以根据页面的特定宽度来定义特殊的CSS规则。举个例子。

如果宽度大于400px，那么sidebar宽度为300px。反之，sidebar宽度为100px

有两个相关的media查询：width/height 和 device-width/device-height。如图2-11

* device-width/height使用screen.width/height来做为的判定值。该值以设备的pixels来度量
* width/height使用documentElement.clientWidth/Height即viewport的值。该值以CSS的pixels来度量

到底该使用那个呢？一个很无脑的结果：width。web开发中不需要对设备的宽度感兴趣，而width却使按照浏览器窗口的大小计算的

所以在桌面浏览器中使用width而忘记device-width。接下来我们会看到在移动设备中有点凌乱。

【总结】

在此结束对桌面浏览器的特性的简短讨论，第二部分主要涉及移动设备和其与桌面浏览器的重要区别。

【接下来是第二部分内容，来源于A tale of two viewports — part two一文。】




