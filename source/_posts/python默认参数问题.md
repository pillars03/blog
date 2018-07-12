---
title: python默认参数问题
date: 2018-07-12 20:54:28
tags:
---
用Python写了一个默认目录的函数，用了个默认参数，发现了个高级特性
```py
import os

def getAllFile(dir,file_arr=[]):
    all_file = os.listdir(dir)
    for file in all_file:
        path = os.path.join(dir, file)
        if os.path.isdir(path):
            getAllFile(path,file_arr)
        else:
            file_arr.append(path)
    return file_arr
cc = getAllFile('20180703')
print len(cc)
```

```py
import os

def getAllFile(dir,file_arr=[]):
    all_file = os.listdir(dir)
    for file in all_file:
        path = os.path.join(dir, file)
        if os.path.isdir(path):
            getAllFile(path)
        else:
            file_arr.append(path)
    return file_arr
cc = getAllFile('20180703')
print len(cc)
```
上面那两个脚本，我在递归的函数里不管传不传fill_arr，得到的结果是一样的。。。。。真是个高级特性

默认参数是[]，但是函数似乎每次都“记住了”上次的fill_arr,感觉是个全局变量。

原因解释如下：

Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。

这也太危险了。。。。脚本语言各种陷阱啊

所以，定义默认参数要牢记一点：默认参数必须指向不变对象！

要修改上面的例子，我们可以用None这个不变对象来实现：
```py
import os

def getAllFile(dir,file_arr=None):
    if file_arr is None:
        file_arr = []
    all_file = os.listdir(dir)
    for file in all_file:
        path = os.path.join(dir, file)
        if os.path.isdir(path):
            getAllFile(path,file_arr)
        else:
            file_arr.append(path)
    return file_arr
cc = getAllFile('20180703')
print len(cc)
```