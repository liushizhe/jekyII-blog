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
1. JavaScript 有5种基本数据类型，还有一种 Object 类型。  
基本类型：Undefined、Null、Boolean、Number、String。  
Object类型：通过 Object 类型可创建对象，对象包含数据和功能的集合。  
Object 类型应用实例：  
对象由键值对组成，key 是唯一的，在实现数组去重功能中，可将数组元素作为对象的 key，根据 key 唯一性，即可将数组去重。








