---
layout: post
title: click onclick 区别
date: 2017-06-10 20:00:35
category: "JavaScript"
---

文章对 DOM 中常用点击操作涉及 click onclik 方法或对象进行详细说明。

click 是 DOM 元素方法（继承自 EventTarget），onclick 是 DOM 元素对象，用于绑定点击事件。例如，
{% highlight html %}
function clickTest(e) {
    console.log('clickTest', e);
}
p.onclick = clickTest; //绑定点击事件函数 clickTest
p.click(); //执行点击操作
{% endhighlight %}
p 是 DOM 元素，绑定点击事件函数为 clickTest，调用 click 方法执行点击动作，此时触发 onclick 事件，onclick 事件绑定的方法 clickTest 被调用。   
上面是第 1 种绑定事件方式。
第 2 种可在 html 中设定，如 \<p onclick="clickTest"\>  
第 3 种 可通过方法 addEventListener（或attachEvent）绑定多个不同方法用于监听点击事件。  
前面两种用 onclick 只能绑定一个函数，若设定多个则前面覆盖前面，用第 3 种则可绑定多个不同的事件。    
若调用 click 方法，则所有绑定事件对应函数依次执行。

总之，click 是 DOM 元素方法，调用 click 方法可执行点击操作，onclick 用于绑定执行 click 操作行为的一种方式。

