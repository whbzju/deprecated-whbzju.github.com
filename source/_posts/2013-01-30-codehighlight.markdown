---
layout: post
title: "codeHighlight"
date: 2013-01-30 15:47
comments: true
categories: 
---

#代码高亮

```
$sudo give me a hug for test

```
``` c test
printf("Hello world");
```

``` ruby Discover if a number is prime http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/ Source Article
 class Fixnum
   def prime?
     ('1' * self) !~ /^1?$|^(11+?)\1+$/
   end
 end
```


##Code Block
{% codeblock testTitle lang:c %}
int main()
{
	printf("Hello octopress");
	return 0;
}
{% endcodeblock %}


{% codeblock Javascript Array Syntax lang:js http://j.mp/pPUUmW MDN Documentation %}
var arr1 = new Array(arrayLength);
var arr2 = new Array(element0, element1, ..., elementN);
{% endcodeblock %}
