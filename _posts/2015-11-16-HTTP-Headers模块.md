---
layout: post
date: 2015-11-16 13:15:00 +0800
tag: perl
---

# HTTP::Headers模块

该模块用来组装HTTP访问的消息头。

* 创建对象，然后赋值

```perl
use HTTP::Headers;
$h=HTTP::Headers->new;
$h->header('Content-Type'=>'text/plain');
$h->header('User-Agent'=>'Perl/1.0');
```

* 也可以直接复制

```perl
$h=HTTP::Headers->new(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);
```

* 也可以先赋值给一个数组

```perl
@header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);
$h=HTTP::Headers->new(@header);
```

* 赋值给一个哈希，也可以

```perl
%header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);
$h=HTTP::Headers->new(%header);
```

* 但你不可以这样，这样会搞出来一个怪东西：

```perl
$h=HTTP::Headers->new(\@header);
$h=HTTP::Headers->new(\%header);

$VAR1 = bless( {
                 '::std_case' => {
                                   'hash(0x7fd33b801d20)' => 'HASH(0x7fd33b801d20)'
                                 }
               }, 'HTTP::Headers' );
```

* 打印出来看看

```perl
say Dumper($h);
$VAR1 = bless( {
                 'user-agent' => 'Perl/1.0',
                 'content-type' => 'text/plain'
               }, 'HTTP::Headers' );
```

* 常用操作

```perl
$h->clone; #拷贝一个变量
$h->content_type('text/plain'); #快捷函数
```

* 一些提示
  * 不区分大小写
  * 支持content-type用作content_type
	
	