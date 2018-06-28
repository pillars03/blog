---
title: Node.js里Cannot find moudle（针对第三方模块）
date: 2017-05-17 11:05:19
tags:
---
Node.js里Cannot find moudle（针对第三方模块

<!--more-->

全局安装（坑货）：例如npm install cheerio -g，这个安装之后会在你本地的电脑下安装一个模块，如果是window的话，目测是在c-用户-admin...下，如果你要用这个模块的话，需要在环境变量里配置，我起先也是这么玩的，感觉有点坑，坑点就是这个项目只能在你本地的电脑上运行，因为只有你电脑上有模块的依赖环境，现在我很负责的告诉你，这个全局安装的做法其实是很愚蠢的，完全没必要用，我们要达到的效过肯定是只要有node.js的环境到处都能运行，这个就需要在项目里安装模块了。

项目里安装模块（正解）：例如npm install cheeio,相对于全局安装，这个少了一个-g,也很容易理解，g就是global的意思嘛。这个的安装的路径需要定位到你的项目路径下，我这里有一个小技巧，就是鼠标选中你的项目，然后按住shift键不放，点击右键，选择在此处打开命令窗口，然后在cmd下输入按住命令。

完成以上步骤之后，你会发现还是会报错，是不是缺少package.json这个文件？这个文件的作用是来管理你项目的一些配置信息的，例如node.js的版本号，作者啊以及项目相关联的第三方模块依赖，你自己打开这个文件看看就知道了。接下来就是生成这个文件，npm init看名字就知道这个命令就是在初始化，会生产 package.json文件（同时还要一些其他的文件），有了这个文件之后你就可以执行安装第三方模块的的命令了。

值得注意的是，安装第三方模块通常是执行npm install cheeio --save这个命令，这个与上面那个就多了一个--save，--save的作用的把你安装的这个第三方模块的依赖写入到package.json里面，你可以同时试一下这两种安装的区别，然后打开package.json查看。

基本上要注意的就这些了，还有一个就是模块可以一次安装多个，例如npm install cheerio formidable --save这样就同时安装了cheerio和formidable两个模块。这里附上一个nodejs框架express的安装教程：http://www.expressjs.com.cn/starter/installing.html
那里有对express这个第三方模块的安装教程，相信对你有用。
