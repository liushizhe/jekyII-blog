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
4. DOM 生成  
浏览器通过渲染引擎将 HTML、CSS 解析成 DOM 树。（不同浏览器有区别）
5. DOM ready  
前面说过，在构建 DOM 节点树时，为提升浏览器渲染性能，需将 js 文件放置在『/body』之前（浏览器从上到下、从左到右进行渲染），防止影响页面渲染。  
实际上，为提高性能，需要一个 DOM ready 方法用于加载 js 文件，方法里面不管引用多少个 js 文件，js 文件如何相互调用，这样也能保证在浏览器渲染后执行 js 行为。  
使用 window.onload 方法可以实现 DOM ready 功能，但是若 js 里面请求资源过多，比如在页面中操作图片，但是图片可能尚未加载成功，这样就会影响用户体验。
为了解决 window.onload 短板问题，引进了 DOMContentLoaded 事件。






