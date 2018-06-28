---
title: 闭包
date: 2017-03-17 11:05:19
tags:
---
闭包（closure），把变量'包在函数中';

<!--more-->
```js

function createFunction() {
    var result = [];

    for (var i = 0; i < 10; i++) {
        result[i] = (function (val) {
            return function () {
                return val;
            }
        })(i)
    }
    return result;
}
var foo = createFunction();
for (var index = 0; index < foo.length; index++) {
    console.log(foo[index]());
}
```



直接返回变量

```js

function createFunction() {
    var result = [];

    for (var i = 0; i < 10; i++) {
        result[i] = function () {
                return val;
            }
    }
    return result;
}
var foo = createFunction();
for (var index = 0; index < foo.length; index++) {
    console.log(foo[index]());
}
//打印出来的都是10，不是我们想要的结果
```
教科书解释： 因为每个函数都保存着createFunction中的活动对象，所以它们引用的都是同一个变量 i 。而循环结束后 i 的值为10，所以每个函数的输出都是10.
简单来说，上面的代码工作细节如下
```js
result[0] = function() { return i;};
result[1] = function() { return i;};
result[2] = function() { return i;};
//而当我们调用result[0]函数时， 这个函数执行到 return 语句，发现并没有 i 这个变量，于是顺着作用链去找，在createFunctions里找到了已经变成10的 i ，于是输出 10. 这个过程才是闭包的寻找变量的过程。
```
