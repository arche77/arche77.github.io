---
layout: post
date: 2015-11-16 11:10:00 +0800
tag: perl
---

# HTTP::Request模块

* 创建一个请求

```perl
$req=HTTP::Request->new;
say Dumper($req);

$VAR1 = bless( {
                 '_content' => '',
                 '_uri' => undef,
                 '_headers' => bless( {}, 'HTTP::Headers' ),
                 '_method' => undef
               }, 'HTTP::Request' );
```

* 完整的语法是：

```perl
$req = HTTP::Request->new($method, $uri, $header, $content);

$req->method('GET');	#必须是大写
$req->method(GET);	#可以不加引号
$req->method('get');	#但不可以是小写

$req->uri('http://www.baidu.com'); #一般是个网址

$req->header('Content-Type'=>'text/plain'); #预备header信息

$req->header(
	'Content-Type'=>'text/plain',
	'User-Agent'=>'Perl/1.0'
); #可以一次多个值

@header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);
$req->header(@header); #可以先赋值给1个数组

%header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);
$req->header(%header); #可以先赋值给1个哈希

$req->header(\@header); #不可以
$req->header(\%header); #不可以


@header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);

$req = HTTP::Request->new(	#使用连写语法时候可以赋值给一个数组
	GET,
	"http://www.baidu.com",
	\@header,	#但是你必须写成引用
	@header,	#这样会引起混乱而报错
	$content
);

$header=HTTP::Headers->new(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);

$req = HTTP::Request->new(
	GET,
	"http://www.baidu.com",
	$header,	#先创建对象，再传入也比较好
	$content
);

$header=(
'Content-Type'=>'text/plain',
'User-Agent'=>'Perl/1.0'
);

$req = HTTP::Request->new(
	GET,
	"http://www.baidu.com",
	$header,	#这样想蒙混过关是不可以的
	$content
);

@header=(
'Content-Type'=>'text/plain',
);
$content="a=1&b=2";
$req = HTTP::Request->new(
	POST,
	"http://101.200.173.64/cgi/para.pl",
	\@header,	#这样得不到正确的结果，根源不在于content，而这样content-type错误
	$content
);

@header=();	#头部为空是可以的
@header=("Content-Type"=>"application/x-www-form-urlencoded");	#其实默认值是这个，配套a=1&b=2使用
@header=("Content-Type"=>"multipart/form-data");	#这个仅仅配合form方式使用

# 参数型标准用法
@header=(
"Content-Type"=>"application/x-www-form-urlencoded"
);
$content="a=1&b=2";
$req = HTTP::Request->new(
	POST,
	"http://101.200.173.64/cgi/para.pl",
	\@header,
	$content
);

# 没有找到表单型标准用法，可以直接用ua的post方法调过去
%content=(
	"a"=>1,
	"b"=>2
);
my $res=$ua->post(
	"http://101.200.173.64/cgi/para.pl",
	'Content_Type' => 'multipart/form-data',
	Content=>\%content
);
say $res->content;

# 或者用HTTP::Request::Common的POST方法，这样可以看清楚
%content=(
	"a"=>1,
	"b"=>2
);
my $req=POST "http://101.200.173.64/cgi/para.pl", 'Content_Type' => 'multipart/form-data', Content=>\%content;
say Dumper($req);

$res=$ua->request($req);
say $res->content;

# 内容如下

$VAR1 = bless( {
                 '_content' => '--xYzZY
Content-Disposition: form-data; name="a"

1
--xYzZY
Content-Disposition: form-data; name="b"

2
--xYzZY--
',
                 '_uri' => bless( do{\(my $o = 'http://101.200.173.64/cgi/para.pl')}, 'URI::http' ),
                 '_headers' => bless( {
                                        'content-type' => 'multipart/form-data; boundary=xYzZY',
                                        'content-length' => 123
                                      }, 'HTTP::Headers' ),
                 '_method' => 'POST'
               }, 'HTTP::Request' );

```