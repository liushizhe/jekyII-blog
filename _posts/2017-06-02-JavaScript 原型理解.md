---
layout: post
title: JavaScript 原型理解
date: 2017-06-02 15:10:35
category: "JavaScript"
keywords: javascript,原型
---

网上有太多解读 JavaScript 原型相关文章，只写自己目前状态下的理解，或有谬误，以后再改。

#### 原型定义
根据 JavaScript 语法规格，每个函数都有一个 prototype 属性，该属性指向一个对象，即原型对象。新建一个函数时，可以往原型对象添加属性或方法，如  
{% highlight html %}
function Person(name) {
	this.name = name;
	this.sayName = function(){
		console.log(this.name);
	};
}
Person.prototype.name = 'li';
Person.prototype.sayName = function () {
	console.log(this.name);
};
Person.prototype.age = 20;
Person.prototype.sayAge = function () {
	console.log(this.age);
};
{% endhighlight %}
或者用字面量格式，如  
{% highlight html %}
Person.prototype = {
	constructor: Person, //必须重新指定构造函数
	name: 'li',
	age: 20,
	sayName: function(){
		console.log(this.name);
	},
	sayAge: function(){
		console.log(this.age);
	}
};
{% endhighlight %}
字面量格式将原型对象全部改了，由于原型对象默认有一个 construtor 属性，该属性指向构造函数，所以用字面量设定原型对象值时需重新设定 constructor 属性。  
#### 原型对象使用  
上面构造函数 Person 包含有函数自身的属性和方法（name/sayName），原型属性指向的对象也包含了属性和方法（name/sayName），通过该构造函数创建多个对象实例，获取对象属性或方法时，先从对象本身属性方法开始查找，若该对象本身没有需要的属性或方法，再到原型对象包含的属性方法里查找。如  
var person = new Person('lin');  
person.sayName();  
此时直接调用对象内部方法 sayName，若调用 person.sayAge()，由于对象没有 sayAge 方法，就查找原型对象内部方法 sayAge。  
#### property 与 \_\_proto\_\_ 区别
每个对象包含一个 \_\_proto\_\_ 属性（非标准），该属性指向原型对象，\_\_proto\_\_ 非通用标准，不建议使用。  
property  -> 函数属性  
\_\_proto\_\_ -> 对象属性  
最终都指向原型对象。  
#### 原型共享特性  
每创建一个对象，内存就多一份对象的拷贝，使用原型对象，可多个对象共享原型对象里面的属性或方法。如  
var person1 = new Person('li1');  
var person2 = new Person('li2');  
console.log(person1.sayName === person2.sayName, person1.sayAge === person2.sayAge);  
上面输出结果分别为 false, true，sayName 对象本身方法，每个对象都有一份拷贝，sayAge 原型对象方法，多个对象共用一份。   
#### 原型继承  
前面提到，创建一个对象实例后，若需要操作对象属性或方法，先在对象本身查找，再到原型对象内查找，若将原型对象指向其它对象，该对象实例就包含有新原型对象的属性和方法。因此，可通过这种原型链的形式实现继承。



