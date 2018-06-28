---
title: call&apply
date: 2017-04-21 11:05:19
tags:
---
js中的call及apply;

<!--more-->

作者：赵望野
链接：https://www.zhihu.com/question/20289071/answer/14745394
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

这是我见过的比较简练的说明，不需要太多的术语。

call 和 apply 都是为了改变某个函数运行时的 context 即上下文而存在的，换句话说，就是为了改变函数体内部 **this** 的指向。
因为 JavaScript 的函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。二者的作用完全一样，只是接受参数的方式不太一样。
例如，有一个函数 func1 定义如下：

```js 
var func1 = function(arg1, arg2) {}; 
```
就可以通过 
```js
func1.call(this, arg1, arg2); 
或者 
func1.apply(this, [arg1, arg2]); 
```
来调用。其中 **this** 是你想指定的上下文，他可以任何一个 JavaScript 对象(JavaScript 中一切皆对象)，call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时，用 call，而不确定的时候，用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个数组来遍历所有的参数。

```js
function cat(){}
cat.prototype={
    food:"fish",     
    say: function(){           
        alert("I love "+this.food);     
    }
}
var blackCat = new cat;blackCat.say(); 
//但是如果我们有一个对象
whiteDog = {food:"bone"},
//我们不想对它重新定义say方法，那么我们可以通过call或apply用blackCat的say方法：
blackCat.say.call(whiteDog);
```
***
这是从MDN文档中摘录的，用apply可以很巧妙的解决需要遍历数组的问题
```js
/* min/max number in an array */
var numbers = [5, 6, 2, 3, 7];

/* using Math.min/Math.max apply */
var max = Math.max.apply(null, numbers); /* This about equal to Math.max(numbers[0], ...) or Math.max(5, 6, ..) */
var min = Math.min.apply(null, numbers);
```
***
