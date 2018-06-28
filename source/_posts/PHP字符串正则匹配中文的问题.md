---
title: PHP字符串正则匹配中文的问题
date: 2017-08-09 05:05:19
tags:
---
今天有个需求要匹配"【标签a】",这样的字符（方括号中有文字）。下面记录一下我遇到的坑

<!--more-->
### js正则匹配
```js
"【fjkldjsfkl号安徽】".match(/【[\u4e00-\u9fa5_a-zA-Z0-9]*】/g);
```

![](http://ouepvhejb.bkt.clouddn.com/a6462ae4-d3be-4bf9-9f7a-b98d1f409c25.png)  

这段代码是有效的，能匹配到目标字符，但是这段正则放到php的结果就不一样的，看下面的代码;

### php
```php
$content= '【嘿嘿ddddd】';
var_dump($content);
preg_match('/【[\u4e00-\u9fa5_a-zA-Z0-9]*】/u',$content, $matches, PREG_OFFSET_CAPTURE);
var_dump($matches);
exit();
```
输出如图：
![](http://ouepvhejb.bkt.clouddn.com/7013ef36-2512-4cdf-8f71-f625c11743b8.png)

### 分析
首先要注意一个问题，坑了很长时间，php的在匹配中文的时候切记要加上加上UTF8修饰符，具体的在这篇文章中有解释
[PHP字符串中用正则表达式匹配中文出现乱码](https://segmentfault.com/q/1010000004153832)  
php的正则应该怎么写呢，在一篇博客中找到了解释，原文在这  

[中文的简单正则匹配](http://www.cnblogs.com/devcjq/articles/5059017.html)


我摘录一下主要内容：  

* 在PHP中使用正则匹配中文，很多时候会出现问题，在不同的编码情况下，正则表达式不太一样，所以希望大家注意，在使用正则匹配中文的时候，多多注意编码问题。
* 在JS下能够使用的在PHP中不一定可以使用，比如：/^[a-zA-Z0-9\_\.\＿\．\u4E00-\u9FA5\uF900-\uFA2D]+$/;
* 如果在PHP中使用 ：\u4E00-\u9FA5\uF900-\uFA2D 来匹配，那么会在运行PHP脚本的时候出现错误，Warning: preg_match(): Compilation failed: PCRE does not support \L, \l, \N, \P, \p, \U, \u, or \X at offset 6 那是因为：PHP正则表达式中不支持下列 Perl 转义序列：\L, \l, \N, \P, \p, \U, \u, or \X  

* 但是在在 UTF-8 模式下，允许用“\x{...}”。  
所以我们的代码应该这么写：  
```php
$content= '【嘿嘿ddddd】';
var_dump($content);
preg_match('/【[\x{4e00}-\x{9fa5}A-Za-z0-9_]*】/u',$content, $matches, PREG_OFFSET_CAPTURE);
var_dump($matches);
exit();
```