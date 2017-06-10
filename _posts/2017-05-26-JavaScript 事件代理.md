---
layout: post
title: JavaScript 事件代理
date: 2017-05-26 15:00:35
category: "JavaScript"
---

学习 DOM 事件模型，通过具体实例，对 JavaScript 事件代理有更深的理解，同时也了解到 call/apply 函数妙用。

下面以具体例子说明。  
html  
{% highlight html %}
<ul id="list">
	<li id="item1">Simon</li>
	<li id="item2">Kenner</li>
	<li id="item3">Erik</li>
</ul>
{% endhighlight %}
js  
{% highlight html %}
function renderList(event) {
    console.log(event);
    $('#list').innerHTML = '<li>new item</li>';
}
function init() {
    each($('#list li'), function(item) {
        $.click(item, renderList);
    });
    $.click($('#addbtn'), renderList);
}
init();
{% endhighlight %}
上面实例将事件绑定在 li 节点中，事件触发时，li 节点被修改，因此下次如果要重新触发 li 事件必须要重新绑定，这种操作方式太不方便了。
使用代理处理方式如下。  
{% highlight html %}
function delegateEvent(element, tag, eventName, listener) {
    addEvent(element, eventName, function(event){
        //target Firefox下属性，srcElement IE，chrome两者皆有
        var target = event.target || event.srcElement;
        if(target.tagName.toLowerCase() == tag.toLowerCase()){
            listener.call(target, event);
        }
    });
}
{% endhighlight %}
定义事件代理函数 delegateEvent，此时不将事件绑定到具体触发的节点 li 中，而是将事件绑定到父节点 ul，事件触发时，根据事件传递机制（冒泡或捕获），从事件流中获取触发事件的源节点，用代理方式，传递源节点执行监听动作。
因为绑定的是固定节点 ul，所以修改子节点无法改变到事件监听行为。同时，使用代理机制，最终执行的是源节点对应行为。而且，使用事件代理，代码简洁、占用资源少，更新 DOM 时无需重新绑定事件。比如，li 列表元素一大堆，如果一个个绑定事件，效率岂不是很低。  
代理中使用 call 函数，call 函数改变 this 指针指向具体对象，若传递触发事件源节点，即可执行该节点动作，算是 call 函数一处妙用。



