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

获取当前节点指向下一节点的地址，对该节点进行解引用就是下一节点，以此实现链表的迭代并查找待删除节点，并且链表删除中，将当前节点（也就是前一节点next指向节点）指向删除节点的下一个节点，就可直接删掉待删除的节点（只需一条语句），无需判断是否为头节点。

[测试源码github地址](https://github.com/liushizhe/good-taste-code){:target="_blank"}

