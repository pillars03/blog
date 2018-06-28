---
title: REST
date: 2017-08-16 05:05:19
tags:
---
首先要明确一点：REST 实际上只是一种设计风格，它并不是标准。（所以你可以看到网上一大堆的各种最佳实践，设计指南，但是没有人说设计标准）。
[aisuhua/restful-api-design-references · GitHub](https://github.com/aisuhua/restful-api-design-references)

URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。

<!--more-->
### 说说几个重要的概念：
1. REST 是面向资源的，这个概念非常重要，而资源是通过 URI 进行暴露。URI 的设计只要负责把资源通过合理方式暴露出来就可以了。对资源的操作与它无关，操作是通过 HTTP动词来体现，所以REST 通过 URI 暴露资源时，会强调不要在 URI 中出现动词。  

* 比如：左边是错误的设计，而右边是正确的  

```
GET /rest/api/getDogs --> GET /rest/api/dogs 获取所有小狗狗 
GET /rest/api/addDogs --> POST /rest/api/dogs 添加一个小狗狗 
GET /rest/api/editDogs/:dog_id --> PUT /rest/api/dogs/:dog_id 修改一个小狗狗 
GET /rest/api/deleteDogs/:dog_id --> DELETE /rest/api/dogs/:dog_id 删除一个小狗狗
```  

左边的这种设计，很明显不符合REST风格，上面已经说了，URI 只负责准确无误的暴露资源，而 getDogs/addDogs...已经包含了对资源的操作，这是不对的。相反右边却满足了，它的操作是使用标准的HTTP动词来体现。  

2. REST很好地利用了HTTP本身就有的一些特征，如HTTP动词、HTTP状态码、HTTP报头等等REST API 是基于 HTTP的，所以你的API应该去使用 HTTP的一些标准。这样所有的HTTP客户端（如浏览器）才能够直接理解你的API（当然还有其他好处，如利于缓存等等）。REST 实际上也非常强调应该利用好 HTTP本来就有的特征，而不是只把 HTTP当成一个传输层这么简单了。  

* HTTP动词    

```
GET     获取一个资源 
POST    添加一个资源 
PUT     修改一个资源 
DELETE  删除一个资源 
```    

实际上，这四个动词实际上就对应着增删改查四个操作，这就利用了HTTP动词来表示对资源的操作。  
* HTTP状态码  

```
200 OK 
400 Bad Request 
500 Internal Server Error
```    

在 APP 与 API 的交互当中，其结果无非就三种状态：
* 所有事情都按预期正确执行完毕 - 成功
* APP 发生了一些错误 – 客户端错误
* API 发生了一些错误 – 服务器端错误  

这三种状态与上面的状态码是一一对应的。  

* HTTP报头  

```
Authorization 认证报头 
Cache-Control 缓存报头 
Cnotent-Type  消息体类型报头 
......
```   

报头还有很多，不一一列举。HTTP报头是描述HTTP请求或响应的元数据，它的作用是客户端 与 服务器端进行相互通信时，告诉对方应该如何处理本次请求。  

3. 超媒体  
老实说，这个词汇我到目前还有没搞得全懂。那也说说自己的理解吧，不一定准确哦，有错误希望指出。”超媒体“ 你没听说过没关系，”超链接“ 你一定不会陌生。简单来说，”超链接“ 是实现超媒体中的一种方式。”超媒体“希望达到一种就是说在 REST API 中把所有资源给链接起来。它就犹如你打开一个网站的首页，你难道看到的只有首页吗？NO !, 不是的，你可以通过首页查看商品、查看文章、查看论坛。”超媒体“ 就是做这个事情，它利用 API 把所有资源的关系给链接起来了，你看到不会只是一个独立的资源，而是关系网中的一个资源。”超媒体“ 有点高大上了，老实说，就算你够牛X，写出了一个非常棒的符合”超媒体“的REST API，你的用户即开发者，也不一定能够接受你这种高大上的设计。当然，我相信未来也许可以普及了。  

作者：suhua su
链接：https://www.zhihu.com/question/28557115/answer/47846156
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。