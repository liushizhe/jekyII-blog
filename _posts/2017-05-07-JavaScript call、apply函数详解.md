---
layout: post
title: JavaScript call apply 函数详解
date: 2017-05-07 22:34:35
category: "JavaScript"
---

初学 JavaScript 这门脚本语言，未深入使用，对一些操作方式用得比较别扭，包括call、apply函数使用，现记录下使用心得。  

#### 功能
call、apply 功能类似，都是为了改变函数中 this 对象值，该值由 call 或 apply 函数第一个参数指定，call 与 apply 区别是 call 传递一个个独立参数，apply传递参数是数组。

#### 应用举例
一个简单例子分析下函数内部 this 指向。
{% highlight html %}
function apple() {}
apple.prototype = {
    product: 'mac',
    say: function() {
        console.log(this.product);
    }
}

var product = new apple();
product.say(); //mac
var phone = {
    product: 'iphone'
}
product.say.call(phone); //iphone
{% endhighlight %}

第一次直接调用 say 函数，this 指向函数内部默认对象，此时 product 为默认值 「mac」；   
第二次使用 call 方式调用 say 函数，此时 say 函数内部 this 值由 call 第一个参数 phone 决定，故输出 product 值为 phone 对象 product 值 「iphone」。  

apply 使用方式类似，区别在于参数格式。

{% highlight html %}
function fun(arg1, arg2) {
    console.log(arg1, arg2);
}
var arg1 = 1, arg2 = 2;
fun.call(this, arg1, arg2);
fun.apply(this, [arg1, arg2]);
{% endhighlight %}
示例中，call apply执行结果一样，call 传递独立参数列表，apply 传递数组参数。

注意：  
arguments 参数无法直接调用 slice 方法分离参数，用 call 函数配合使用。  
例如，获取参数数组 arguments 第2个开始参数列表   
[].slice.call(arguments, 2);  
而无法直接 arguments.slice(2);  
报类型错误。

apply 函数可以将数组参数转换称单个参数进行处理，如下  
func.apply(null, arguments);  
function func(arg1, arg2){}


[参考文档：MDN JavaScript 标准库 Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function){:target="_blank"}
