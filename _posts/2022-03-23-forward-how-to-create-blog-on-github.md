---
layout: post
title: 转载——三分钟在GitHub上搭建个人博客
subtitle: Each post also has a subtitle
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---

来自：https://zhuanlan.zhihu.com/p/28321740

# 三分钟在GitHub上搭建个人博客

[![会飞的猪](https://pica.zhimg.com/v2-29e36ac62525ddd884635b3093f5505b_xs.jpg?source=172ae18b)](https://www.zhihu.com/people/luhongquan)

[会飞的猪](https://www.zhihu.com/people/luhongquan)

731 人赞同了该文章

关注公众平台:【资料分享大师】获取更多免费资料

GitHub作为全球最大的程序员交友网站，是一个代码托管平台和开发者社区，开发者可以在Github上创建自己的开源项目并与其他开发者协作编码。

最近想在GitHub上搭建一个自己的博客，可是搜寻了网上的好多教程都不成功。有的教程偏老，有的教程不适合小白过于复杂。没办法，自己刚入门，捣鼓了半天，总算摸出了一条适合小白搭建博客的方法，下面进入正题：（注意：此方法适合小白）

1.首先，不用多说，得先有一个GitHub账号吧：

![](https://pic3.zhimg.com/80/v2-be8a3a6ad036c32b51caa3d78af4f146_720w.jpg)

填好自己得个人信息（用户名好像不能用汉字）：

![](https://pic3.zhimg.com/80/v2-37aac3db6e2094f0e2da6fb41196d6d6_720w.jpg)

然后点击Create an account.

![](https://pic2.zhimg.com/80/v2-7872012db459db8c8bab2793f7f79101_720w.jpg)

这里选择免费的啦，然后Continue

![](https://pic3.zhimg.com/80/v2-6b2a6d5aee07e8ab650c09e45c18d5d6_720w.jpg)

这里选择自己的一些基本情况，然后submit。

![](https://pic3.zhimg.com/80/v2-f96b47e3b9b37bbcebf8ae83d26fec2e_720w.jpg)

最后来到这个界面就对了，点击右上角那个加号旁边那里，可以来到这个页面。

2.开始搭建博客：

以前GitHub上的项目之类的都是一大堆源代码，对于一个新手来说，看到一大堆源码，只会让人头晕脑涨，不知何处入手。他希望看到的是，一个简明易懂的网页，说明每一步应该怎么做。因此，github就设计了Pages功能，允许用户自定义项目首页，用来替代默认的源码列表。所以，github Pages可以被认为是用户编写的、托管在github上的静态网页。

首先，我们需要新建一个Repositories,Repositories就相当于一个库，存放我们的项目文件。

![](https://pic4.zhimg.com/80/v2-bb9610cdfb32da5e5d029356666ac8b3_720w.jpg)

然后给自己的Repositories取一个名字，注意：名称格式最好为：[用户名.github.io](https://link.zhihu.com/?target=http%3A//xn--eqr924avxo.github.io/)

因为我搭建过，所以会提示错误。

![](https://pic2.zhimg.com/80/v2-ae40c07cde4cfe27b93c902850160891_720w.jpg)

然后点击create repositories.

随后跳转到该库界面,由于我是搭建好的，所以会有项目文件，然后选择Settings

![](https://pic4.zhimg.com/80/v2-3d639cfbaff027254b6e899a1642644b_720w.jpg)

进入settings后，往下拉，找到GitHub pages设置界面

![](https://pic1.zhimg.com/80/v2-90469a0aba1ab48ac8ddb7d9f39c1230_720w.jpg)

按如图所示选择，注意，选择source之后记得Save，然后点击Choose a theme选择一个博客主题。

![](https://pic2.zhimg.com/80/v2-be796f462202406cb2cd42fd511d6a85_720w.jpg)

然后点击Select theme

到这一步呢，正常来说你就可以看到自己的博客了。在地址栏输入：用户名.[http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io)你就可以访问页面了。

3.接下来，就该去下载GitHub桌面版了，为什么使用桌面版呢，因为简单，适合小白，没那么多命令行。官网下载可能有一些慢，这里是我下好的：链接：[http://**pan.baidu.com/s/1bpkzaK**B](https://link.zhihu.com/?target=http%3A//pan.baidu.com/s/1bpkzaKB) 密码：mwgm。

下载下来后，打开用自己的用户名登录。然后选择File下的Clone repositories.

![](https://pic4.zhimg.com/80/v2-c5f165c7056bf8a1e81c98880e6bc3b7_720w.jpg)

然后将你刚刚建好的repositories的url复制过来，点击Clone。一般存放的默认文件夹为

C:\Users\计算机名\Documents\GitHub。

![](https://pic1.zhimg.com/80/v2-faf4513657fd78aad02687c02ba2e1c0_720w.jpg)

clone完之后你会发现C:\Users\计算机名\Documents\GitHub路径下多了一个文件夹，如图：

![](https://pic2.zhimg.com/80/v2-c73a4bec66a84957dd8b069b43f177d1_720w.jpg)

然后将该文件夹里的文件都删掉，如果有.git文件，需要保留.git文件

![](https://pic2.zhimg.com/80/v2-9e80d042f9b5c2cd0f4326890cdaa331_720w.jpg)

接下来可以去找一个博客模板，如果自己喜欢折腾，也可以自己写。推荐一个模板地址：

[https://**html5up.net/**](https://link.zhihu.com/?target=https%3A//html5up.net/)

模板下载下来之后，将所有文件都复制到本地的那个库文件夹中，如图：

![](https://pic2.zhimg.com/80/v2-8f81e8870436ae635ebba5af137990f5_720w.jpg)

4.接下来改同步到网络上了，打开GitHub桌面版。你会发现有非常多的changes，如图

在summary中随意填一些内容，然后带年纪commit to master。

![](https://pic4.zhimg.com/80/v2-3b08ca2dfb6215d86595c4fdc9fdb7f7_720w.jpg)

然后，同步，点击如图所示区域。

注意：可能由于墙或者什么网络问题，上传有可能会失败或较久，需要多试几次。

![](https://pic4.zhimg.com/80/v2-dcf13d09dfd1b34fb474ecbdcd404567_720w.jpg)

然后回到GitHub网站上，你会发现该库文件下多了很多文件

![](https://pic4.zhimg.com/80/v2-00a8904053bb937ecf5498a1db241ae3_720w.jpg)

到这里也就差不多了，可以输入地址访问自己的博客了，剩下的就该自己优化博客了。
