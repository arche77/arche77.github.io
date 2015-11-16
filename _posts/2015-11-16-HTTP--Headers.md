---
layout: post
date: 2015-11-16 13:15:00 +0800
tag: perl
---

# HTTP::Headers

该模块用来组装HTTP访问的消息头。

* 创建对象，直接赋值

{% highlight perl linenos %}
use HTTP::Headers;
$h=HTTP::Headers->new;
$h->header('Content-Type'=>'text/plain');
$h->header('User-Agent'=>'Perl/1.0');
{% endhighlight %}