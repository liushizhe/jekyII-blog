---
layout: post
title: good taste 代码
date: 2017-05-17 00:27:35
category: "C"
---

刷小密圈看到圈友分享一篇linus关于采访，采访中提及代码的good taste 或者 poor taste，并用删除单链表节点进行举例。[采访链接](http://blog.jobbole.com/111159/){:target="_blank"}

删除单链表节点很常见，如下所示，
{% highlight html %}
//poor taste code, need to test head node
node_t *remove_node_poor(node_t *head, node_t *node)
{
	node_t *prv = NULL;
	node_t *cur = head;

	while(head != NULL && cur != node){
		prv = cur;
		cur = cur->next;
	}
	//delete node and test whether head node or not
	if(!prv){
		head = node->next;
	} else {
		prv->next = node->next;
	}
	free(node);

	return head;
}
{% endhighlight %}

删除过程中要判断删除的节点是否为头节点，此时代码中需要一个 if 语句进行判断，但是，这个判断做法在Linus看来不是 good taste 代码，并且整段代码量有点多。  
底下是good taste 代码，
{% highlight html %}
//good taste code, no need to test head node
node_t *remove_node(node_t *head, node_t *node)
{
	node_t **indirect = NULL;
	if(head == NULL)return head;
	indirect = &head;
	while((*indirect) != node){
		indirect = &((*indirect)->next);
	}
	//delet node directly, not need to judge head node
	*indirect = node->next; 
	free(node);

	return head;
}
{% endhighlight %}

本例中，节点（node）结构体属性 next 存放在节点首地址，也就是说，存放 next 指针变量的地址就是本节点的地址（&node->next），又 next 指针指向下一节点地址，所以对 next 指针取地址和获取 next 指针值就分别获取当前节点和下一节点地址。链表结构如下图所示。
![链表结构]({{site.baseurl}}/images/posts/good-taste-c-node.png)

[测试源码github地址](https://github.com/liushizhe/good-taste-code){:target="_blank"}

