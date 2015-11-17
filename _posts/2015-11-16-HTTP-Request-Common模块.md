---
layout: post
tag: perl
date: 2015-11-16 23:36:00 +0800
---

#HTTP::Request::Common模块

远比HTTP::Request要简单的构造Request的方法。

```perl
$req=GET $url, Header=>$header;

$req=POST $url, $form_ref, Header=>$header;

$req=POST 'http://www.perl.org/survey.cgi',
    [
      a  => 1,
      b  => 2
    ];
```
将构造为：

```perl
POST http://www.perl.org/survey.cgi
Content-Length: 66
Content-Type: application/x-www-form-urlencoded
 
a=1%20&b=2
```
