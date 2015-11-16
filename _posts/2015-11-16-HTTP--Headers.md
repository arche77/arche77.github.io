---
layout: post
date: 2015-11-16 13:15:00 +0800
category: perl
tag: perl
---

# HTTP::Headers

该模块用来组装HTTP访问的消息头。

* 创建对象，直接赋值

``` perl
use HTTP::Headers;
$h=HTTP::Headers->new;
$h->header('Content-Type'=>'text/plain');
$h->header('User-Agent'=>'Perl/1.0');
```