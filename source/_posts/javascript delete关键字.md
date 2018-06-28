---
title: javascript delete关键字
date: 2017-02-13 05:05:19
tags:
---
javascript delete关键字

<!--more-->
2. js 的内部属性
在上面的例子中，有的变量或属性能够删除成功，而有的变量或属性则无法进行删除，那是什么决定这个变量或属性能不能被删除呢。
ECMA-262第5版定义了JS对象属性中特征（用于JS引擎，外部无法直接访问）。ECMAScript中有两种属性：数据属性和访问器属性。
2.1 数据属性
数据属性指包含一个数据值的位置，可在该位置读取或写入值，该属性有4个供述其行为的特性：
  ● [[configurable]]:表示能否使用delete操作符删除从而重新定义，或能否修改为访问器属性。默认为true;
  ● [[Enumberable]]:表示是否可通过for-in循环返回属性。默认true;
  ● [[Writable]]:表示是否可修改属性的值。默认true;
  ● [[Value]]:包含该属性的数据值。读取/写入都是该值。默认为undefined；如上面实例对象Person中定义了name属性，其值为’wenzi’,对该值的修改都反正在这个位置
要修改对象属性的默认特征（默认都为true)，可调用Object.defineProperty()方法，它接收三个参数：属性所在对象，属性名和一个描述符对象（必须是：configurable、enumberable、writable和value，可设置一个或多个值）。
如下：
```js
var person = {};
Object.defineProperty(person, 'name', {
    configurable: false,    // 不可删除，且不能修改为访问器属性
    writable: false,        // 不可修改
    value: 'wenzi'          // name的值为wenzi
});
console.log( person.name);          // wenzi
console.log( delete person.name );  // false，无法删除
person.name = 'lily';
console.log( person.name );         // wenzi
```

可以看出，delete及重置person.name的值都没有生效，这就是因为调用defineProperty函数修改了对象属性的特征；值得注意的是一旦将configurable设置为false，则无法再使用defineProperty将其修改为true（执行会报错：Uncaught TypeError: Cannot redefine property: name）;
2.2 访问器属性
它主要包括一对getter和setter函数，在读取访问器属性时，会调用getter返回有效值；写入访问器属性时，调用setter，写入新值；该属性有以下4个特征：
  ● [[Configurable]]:是否可通过delete操作符删除重新定义属性；
  ● [[Numberable]]:是否可通过for-in循环查找该属性；
  ● [[Get]]:读取属性时自动调用，默认：undefined;
  ● [[Set]]:写入属性时自动调用，默认：undefined;
访问器属性不能直接定义，必须使用defineProperty()来定义，如下：
```js
var person = {
    _age: 18
};
Object.defineProperty(person, 'isAdult', {
    Configurable : false,
    get: function () {
        if (this._age >= 18) {
            return true;
        } else {
            return false;
        }
    }
});
console.log( person.isAdult );  // true
```

不过还是有一点需要额外注意一下，Object.defineProperty()方法设置属性时，不能同时声明访问器属性（set和get）和数据属性（writable或者value）。意思就是，某个属性设置了writable或者value属性，那么这个属性就不能声明get和set了，反之亦然。
如若像下面的方式进行定义，访问器属性和数据属性同时存在：
```js
var o = {};
Object.defineProperty(o, 'name', {
    value: 'wenzi',
    set: function(name) {
        myName = name;
    },
    get: function() {
        return myName;
    }
});
```

上面的代码看起来貌似是没有什么问题，但是真正执行时会报错，报错如下：
Uncaught TypeError: Invalid property. A property cannot both have accessors and be writable or have a value
对于数据属性，可以取得：configurable,enumberable,writable和value；
对于访问器属性，可以取得：configurable,enumberable,get和set。
由此我们可知：一个变量或属性是否可以被删除，是由其内部属性Configurable进行控制的，若Configurable为true，则该变量或属性可以被删除，否则不能被删除。
可是我们应该怎么获取这个Configurable值呢，总不能用delete试试能不能删除吧。有办法滴！！
2.3 获取内部属性
ES5为我们提供了Object.getOwnPropertyDescriptor(object, property)来获取内部属性。
如：
```js
var person = {name:'wenzi'};
var desp = Object.getOwnPropertyDescriptor(person, 'name'); // person中的name属性
console.log( desp );    // {value: "wenzi", writable: true, enumerable: true, configurable: true}
```

通过Object.getOwnPropertyDescriptor(object, property)我们能够获取到4个内部属性，configurable控制着变量或属性是否可被删除。这个例子中，person.name的configurable是true，则说明是可以被删除的：
console.log( person.name );         // wenzi
console.log( delete person.name );  // true，删除成功
console.log( person.name );         // undefined

我们再回到最开始的那个面试题：
```js
a = 1;
var desp = Object.getOwnPropertyDescriptor(window, 'a');
console.log( desp.configurable );   // true，可以删除

var b = 2;
var desp = Object.getOwnPropertyDescriptor(window, 'b');
console.log( desp.configurable );   // false，不能删除
```

跟我们使用delete操作删除变量时产生的结果是一样的。