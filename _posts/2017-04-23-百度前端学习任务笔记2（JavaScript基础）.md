---
layout: post
title: 百度前端学习任务笔记2（JavaScript）
date: 2017-04-23 21:07:35
category: "JavaScript"
---

掌握JavaScript基础功能，使用JavaScript编写简单网页交互功能。

#### 了解 JavaScirpt
JavaScript 是一种轻量级解释语言，作为Web开发脚本语言而出名，但也用于非浏览器环境中，如node.js。  
JavaScript 是一种基于原型、多范式的动态脚本语言，支持面向对象、命令式、函数式编程风格。  
[MDN JavaScript 概述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/JavaScript_Overview#What_is_JavaScript.3F){:target="_blank"}

#### JavaScript 性能优化：加载和执行
- script 标签放在 body 闭合标签之前  
若 script 放置 head 标签中，则浏览器在解析script标签时，会停止页面解析渲染，转而下载JavaScript脚本并执行，导致页面显示延迟。  
- 尽可能地合并脚本  
页面中的 script 标签越少，加载也就越快，响应也越迅速。无论是外链脚本还是内嵌脚本都是如此。  
- 使用 script 标签的 defer 属性（仅适用于 IE 和 Firefox 3.5 以上版本）  
若 script 带 defer 属性，浏览器执行到script标签时，会下载 js 文件，但不会执行，直至 DOM 加载完成，即 onload 事件触发前才会被执行。  
HTML 5 中，使用新扩展属性 async 异步加载执行脚本，不会阻塞页面加载。js 脚本一下载完就执行，所以脚本执行顺序可能会变，若 js 脚本前后有依赖，使用 async 可能出现错误。  
- 使用动态创建的 script 元素来下载并执行代码  
DOM 中动态创建 script 元素
{% highlight html %}
function loadScript(url, callback){
    var script = document.createElement ("script")
    script.type = "text/javascript";
    if (script.readyState){ //IE
        script.onreadystatechange = function(){
            if (script.readyState == "loaded" || script.readyState == "complete"){
                script.onreadystatechange = null;
                callback();
            }
        };
    } else { //Others
        script.onload = function(){
            callback();
        };
    }
    script.src = url;
    document.getElementsByTagName("head")[0].appendChild(script);
}
{% endhighlight %}

动态脚本加载是非阻塞 JavaScript 下载中最常用的模式，因为它可以跨浏览器，而且简单易用。  
- 使用 XHR 对象下载 JavaScript 代码并注入页面中

[JavaScript 的性能优化：加载和执行](https://www.ibm.com/developerworks/cn/web/1308_caiys_jsload/index.html){:target="_blank"}

#### JavaScript 基础
1. JavaScript 有6种基本数据类型，还有一种 Object 类型。  
基本类型：Undefined、Null、Boolean、Number、String、Symbol（ES6添加）。  
Object类型：通过 Object 类型可创建对象，对象包含数据和功能的集合。  
Object 类型应用实例：  
对象由键值对组成，key 是唯一的，在实现数组去重功能中，可将数组元素作为对象的 key，根据 key 唯一性，即可将数组去重。
2. 实例、构造函数、原型对象  
实例：包含 constructor 属性（指向构造函数）和 _proto_ 属性（指向原型对象）  
构造函数：包含 prototype 属性，指向它的原型对象  
原型对象：包含 constructor 属性，指向构造函数，如Person.prototype.constructor = Person    
实例可以访问原型对象上定义的属性和方法。  
![实例、构造函数与原型对象的关系]({{site.baseurl}}/images/posts/2017-04-23-百度前端学习任务笔记2（JavaScirpt基础）/task0002_constructor_proto.png)

#### DOM
1. 什么是 DOM  
DOM，文档对象模型（Document Object Model）。DOM是 W3C（万维网联盟）的标准，DOM定义了访问HTML和XML文档的标准。在W3C的标准中，DOM是独于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。
2. 认识 DOM  
DOM可以将任何HTML描绘成一个由多层节点构成的结构。节点分为12种不同类型，每种类型分别表示文档中不同的信息及标记。每个节点都拥有各自的特点、数据和方法，也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点为根节点的树形结构。
3. DOM 节点类型  
DOM1级定义了一个Node接口，这个Node接口在javascript中是作为Node类型来实现的。除了IE以外，其他所有浏览器都可以访问这个类型。每个节点都有一个nodeType属性，用于表明节点的类型。节点类型通过定义数值常量和字符常量两种方式来表示，IE只支持数值常量。节点类型一共有12种，这里介绍常用的7种类型。如下图：  
![DOM 常用节点类型]({{site.baseurl}}/images/posts/2017-04-23-百度前端学习任务笔记2（JavaScirpt基础）/task0002_dom_node_type.jpeg)
节点内容可详看一下文档 [DOM模型概述](http://javascript.ruanyifeng.com/dom/node.html#toc3){:target="_blank"}
4. DOM 节点树  
节点（Node）是 DOM 最小组成单位，不同类型的节点组成 DOM 树。各个节点存在3种关系，父节点关系、子节点关系、同级别节点关系。  
DOM 树最顶层节点是 document，表示整个文档。  
5. document 节点  
document 节点是网页的根节点，每个网页都有自己的根节点，只要浏览器载入 HTML 文档，document 节点对象就存在并可直接调用。  
   1. document 节点获取方式  
   - 对于正常的网页，直接使用document或window.document。  
   - 对于iframe载入的网页，使用iframe节点的contentDocument属性。  
   - 对Ajax操作返回的文档，使用XMLHttpRequest对象的responseXML属性。  
   - 对于包含某个节点的文档，使用该节点的ownerDocument属性。  
   2. document 节点属性  
   - document.doctype 文档类型  
   - document.defaultView 浏览器下为 window 对象  
   - document.head head 节点，等同于 document.querySelector('head') 
   - document.body body 节点，等同于 document.querySelector('body') 
   3. document 节点集合属性  
   - document.links 返回文档中所有设定 href 属性的 a 或 area 元素  
   - document.forms 返回文档中所有表单元素  
   - document.images 返回文档中所有图片元素（即 img 标签）  
   - document.embeds 返回文档中所有嵌入对象（即 embed 标签）  
   - 以上属性都是 HTMLCollection 实例（HTMLCollection实例可以用HTML元素的id或name属性引用）  
   4. document 文档信息属性  
   - document.readyState 文档当前状态  
    loading：加载HTML代码阶段（尚未完成解析）  
    interactive：加载外部资源阶段时  
    complete：加载完成时  
   这个属性变化的过程如下:  
     1.浏览器开始解析HTML文档，document.readyState属性等于loading。  
     2.浏览器遇到HTML文档中的 script 元素，并且没有async或defer属性，就暂停解析，开始执行脚本，这时document.readyState属性还是等于loading。  
     3.HTML文档解析完成，document.readyState属性变成interactive。  
     4.浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，document. readyState属性变成complete。  
    5. 查找节点  
    - document.querySelector() 查找节点，参数为 CSS 选择器，返回第一个匹配的节点，不支持 CSS 伪类或伪元素选择器，下同  
    - document.querySelectorAll() 查找节点，参数为 CSS 选择器，返回所有匹配的节点  
    - document.getElementsByTagName() 返回指定标签的所有元素（返回值 HTMLCollection 实例，元素节点集合）  
    - document.getElementsByClassName 返回指定类的所有元素（返回值 HTMLCollection 实例，元素节点集合），如果参数多个类，则该节点必须包含所有指定的类  
    - document.getElementsByName 返回拥有 name 元素（如\<input\>、\<form\>、\<radio\>、\<img\>、\<frame\>、\<embed\>和\<object\>等）的 HTML 元素（返回值 NodeList 实例，节点集合）  
    - getElementById 返回匹配指定 id 属性的元素节点，参数大小写敏感，功能与 document.querySelector 一样，不过 document.querySelector 参数为 CSS 选择器
    6. 生成节点  
    - document.createElement 生成网页元素节点，参数为标签名  
    - document.createTextNode 生成文本节点，参数为所要生成的文本节点的内容  
    - document.createAttribute 生成一个新的属性对象节点，并返回它。  
    - document.createDocumentFragment 生成一个DocumentFragment对象  
    7. 选择器引擎
    学习 document 节点时可配合一个简易选择器引擎进行学习。  
    [简易选择器源码](https://github.com/liushizhe/mini-query-selector){:target="_blank"}  
    Sizzle 选择器引擎参考资料：  
    [慕课 Sizzle设计思路](http://www.imooc.com/code/4523){:target="_blank"}   
    [Sizzle引擎--原理与实践](http://www.cnblogs.com/xesam/archive/2012/02/15/2352466.html){:target="_blank"})  
    [Sizzle是怎样工作的](http://www.cnblogs.com/rubylouvre/archive/2011/01/24/1942818.html){:target="_blank"}  
6. DOM ready  
前面说过，在构建 DOM 节点树时，为提升浏览器渲染性能，需将 js 文件放置在『/body』之前（浏览器从上到下、从左到右进行渲染），防止影响页面渲染。  
实际上，为提高性能，需要一个 DOM ready 方法用于加载 js 文件，方法里面不管引用多少个 js 文件，js 文件如何相互调用，这样也能保证在浏览器渲染后执行 js 行为。  
使用 window.onload 方法可以实现 DOM ready 功能，但是若 js 里面请求资源过多，比如在页面中操作图片，但是图片可能尚未加载成功，这样就会影响用户体验。
为了解决 window.onload 短板问题，引进了 DOMContentLoaded 事件。  
7. 事件模型  
   1. 事件接口  
    事件是一种异步编程的实现方式，本质上是程序各个部分之间的通信。  
    DOM 事件操作包含监听和触发，都定义在 EventTarget 接口。Element 节点、document 节点和 window 对象，XMLHttpRequest、AudioNode、AudioContext等浏览器内置对象，都部署了这个接口。  
    该接口有三个方法：  
       - addEventListener：绑定事件的监听函数  
       - removeEventListener：移除事件的监听函数  
       - dispatchEvent：触发事件  
    target.addEventListener(type, listener[, useCapture]);  
    type：事件名称，大小写敏感。  
    listener：监听函数。事件发生时，会调用该监听函数。  
    useCapture：布尔值，表示监听函数是否在捕获阶段（capture）触发，默认为false（监听函数只在冒泡阶段被触发）。  
    removeEventListener 参数一样  
    target.dispatchEvent(event) 触发事件，event 由 Event 对象创建  
    使用实例：  
    function debug() {  
      console.log('hello world');  
    }  
    var button = $('#test1');  
    button[0].addEventListener('click', debug, false);  
    var event = new Event('click');  
    button[0].dispatchEvent(event);  
    button[0].removeEventListener('click', debug, false);        
   2. 事件传播  
   当一个事件发生以后，它会在不同的DOM节点之间传播（propagation）。这种传播分成三个阶段：  
   - 第一阶段：从window对象传导到目标节点，称为“捕获阶段”（capture phase）。
   - 第二阶段：在目标节点上触发，称为“目标阶段”（target phase）。
   - 第三阶段：从目标节点传导回window对象，称为“冒泡阶段”（bubbling phase）。  
   详情参考[事件模型](http://javascript.ruanyifeng.com/dom/event.html#toc0){:target="_blank"}  
8. BOM  
浏览器对象模型，现代浏览器几乎实现了 JavaScript 交互性的属性和方法。
   1. window  
   所有浏览器都支持 window 对象。它表示浏览器窗口。  
   所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。  
   全局变量是 window 对象的属性，全局函数是 window 对象的方法。  
   document 也是 window 属性。  
   2. navigator  
   包含访问者浏览器信息  
   3. cookie  
   cookie 是存储于访问者的计算机中的变量。每当同一台计算机通过浏览器请求某个页面时，就会发送这个 cookie。你可以使用 JavaScript 来创建和取回 cookie 的值。  
   cookie 包含有名字、密码、日期。
9. AJAX  
AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。    
具体来说，AJAX包括以下几个步骤。  
- 创建AJAX对象  
- 发出HTTP请求  
- 接收服务器传回的数据  
- 更新网页数据  
概括起来，就是一句话，AJAX通过原生的XMLHttpRequest对象发出HTTP请求，得到服务器返回的数据后，再进行处理。  
AJAX可以是同步请求，也可以是异步请求。但是，大多数情况下，特指异步请求。因为同步的Ajax请求，对浏览器有“堵塞效应”。  
详情参考[AJAX](http://javascript.ruanyifeng.com/bom/ajax.html#toc27){:target="_blank"}  

#### 练习  
1. 练习1  
涉及知识点：字符串、数组操作，正则表达式，DOM 基本操作  
- HTML 设计相关标签： 
textarea：文本编辑框，可设定行列宽度等属性  
input：输入元素，type 可设置为text/password/summit/button/checkbox/radio等  
2. 练习2  
涉及知识点：正则表达式，定时操作，Date 对象  
- 定时操作：  
clock = setInterval(handler, timeout)  设定定时查询函数，handler 到时后执行动作，timeout 定时时间，单位 ms  
clearInterVal(clock) 清除定时器  
setTimeout 定时执行动作，不过只执行一次，用法同 setInterval  
clearTimeout 清除定时器  
- Date 对象  
new Date(YYYY/MM/DD)  由参数字符串设定的时间创建一个 Date 实例  
new Date()  由当前时间创建一个 Date 实例  
GMT 格林威治时间，UTC 世界标准时间，时间值跟 GMT 差不多，计算方式更准确  
注：2个 Date 实例相减即可获得时间差（ms）  
3. 练习3  
涉及知识点：HTML、CSS 相关知识，定时操作  
HTML：可以自定义元素属性，如属性rel='1'，js 操作时可通过 getAttribute/setAttribute 操作  
CSS：positon 属性无法继承，多个选择器注意同级、父子等关系  
4. 练习4  
涉及知识点：HTML、CSS 相关知识，事件处理  
CSS：  
涉及到弹出框可显示可不显示，display 设置是否显示  
DOM 操作 CSS 选择器时，CSS 设置元素与 DOM 操作元素双方要一致  
事件处理：  
按键事件：keydown，不同按键对应不同 keycode（event.keyCode）  
鼠标事件：mouseover 放置在某元素上，mouseout 移开某元素，click 点击  
5. 练习5  
涉及知识点：HTML、CSS 相关知识，事件处理  
CSS:  
两个方框并排，需要用到浮动布局方式  
小方框移动，需要手动重排，定位方式采用绝对定位  
事件处理：  
鼠标事件：mousedown 鼠标按下，mousemove 拖动鼠标，mouseup 点击鼠标后松开  
鼠标位置可由事件中属性 event.clientX、event.clientY 设定  
元素位置大小可由属性 ele.offsetLeft/ele.offsetTop/ele.offsetWidth/ele.offsetHeight 设定  

