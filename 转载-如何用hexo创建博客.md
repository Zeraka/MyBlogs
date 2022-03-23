作者：程序员吴师兄
链接：https://www.zhihu.com/question/23934523/answer/1882886859
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

这个提问那我回忆起入门程序员的那些年，技术没增加多少，博客倒是搭建了不少，动态静态的都用过，GitHub 用的尤为最多。

今天我就手把手教你**如何利用 GitHub 搭建博客，文章较长，细节较多，记得先收藏，如果有帮助记得点赞，如果搭建成功，可以留言一下你的博客。**

## **准备条件**

在这里先跟大家说一些准备条件，有些同学可能一听到搭建博客就望而却步。弄个博客网站，不得有台服务器吗？不得搞数据库吗？不得注册域名吗？没事，如果都没有，那照样是能搭建一个博客的。

GitHub 是个好东西啊，它提供了 GitHub Pages 帮助我们来架设一个[静态网站](https://www.zhihu.com/search?q=%E9%9D%99%E6%80%81%E7%BD%91%E7%AB%99&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D)，这就解决了服务器的问题。

Hexo 这个博客框架没有那么重量级，它是 MarkDown 直接写文章的，然后 Hexo 可以直接将文章编译成静态网页文件并发布，所以这样文章的内容、标题、标签等信息就没必要存数据库里面了，是直接纯静态页面了，这就解决了数据库的问题。

GitHub Pages 允许每个账户创建一个名为 {username}.[http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io) 的仓库，另外它还会自动为这个仓库分配一个 [http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io) 的[二级域名](https://www.zhihu.com/search?q=%E4%BA%8C%E7%BA%A7%E5%9F%9F%E5%90%8D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D)，这就解决了域名的问题，当然如果想要自定义域名的话，也可以支持。

所以说，基本上，先注册个 GitHub 账号就能搞了，下面我们来正式开始吧。

关于如何使用 GitHub 请阅读这篇文章：

[还不会使用 GitHub ？ GitHub 教程来了！万字图文详解](https://zhuanlan.zhihu.com/p/369486197)

额外整理了一些电子书，应该对你很有帮助：

[iCSToCS/CSBook**github.com/iCSToCS/CSBook**![](https://pic3.zhimg.com/v2-a81f0c86c6000418eb85ab1d3dbbe28e_180x120.jpg)](https://link.zhihu.com/?target=https%3A//github.com/iCSToCS/CSBook)

## **新建项目**

首先在 GitHub 新建一个仓库（Repository），名称为 {username}.[http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io)，注意这个名比较特殊，必须要是 [http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io) 为后缀结尾的。比如 NightTeam 的 GitHub 用户名就叫 NightTeam，那我就新建一个 [http://**nightteam.github.io**](https://link.zhihu.com/?target=http%3A//nightteam.github.io)，新建完成之后就可以进行后续操作了。

另外如果 GitHub 没有配置 SSH 连接的建议配置一下，这样后面在部署博客的时候会更方便。

## **安装环境**

### **安装 Node.js**

首先在自己的电脑上安装 Node.js，下载地址：[https://**nodejs.org/zh-cn/downlo**ad/](https://link.zhihu.com/?target=https%3A//nodejs.org/zh-cn/download/)，可以安装 Stable 版本。

安装完毕之后，确保环境变量配置好，能正常使用 npm 命令。

### **安装 Hexo**

接下来就需要安装 Hexo 了，这是一个博客框架，Hexo 官方还提供了一个命令行工具，用于快速创建项目、页面、编译、部署 Hexo 博客，所以在这之前我们需要先安装 Hexo 的命令行工具。

命令如下：

```text
npm install -g hexo-cli
```

安装完毕之后，确保环境变量配置好，能正常使用 hexo 命令。

## **初始化项目**

接下来我们使用 Hexo 的命令行创建一个项目，并将其在本地跑起来，整体跑通看看。

首先使用如下命令创建项目：

```text
hexo init {name}
```

这里的 name 就是项目名，我这里要创建 NightTeam 的博客，我就把项目取名为 nightteam 了，用了纯小写，命令如下：

```text
hexo init nightteam
```

这样 nightteam 文件夹下就会出现 Hexo 的初始化文件，包括 themes、scaffolds、source 等文件夹，这些内容暂且先不用管是做什么的，我们先知道有什么，然后一步步走下去看看都发生了什么变化。

接下来我们首先进入新生成的文件夹里面，然后调用 Hexo 的 generate 命令，将 Hexo 编译生成 HTML 代码，命令如下：

```text
hexo generate
```

可以看到输出结果里面包含了 js、css、font 等内容，并发现他们都处在了项目根目录下的 public 文件夹下面了。

然后我们利用 Hexo 提供的 serve 命令把博客在本地运行起来，命令如下：

```text
hexo serve
```

运行之后命令行输出如下：

```text
INFO  Start processing
INFO  Hexois running at http://localhost:4000 . Press Ctrl+C to stop 
```

它告诉我们在本地 4000 端口上就可以查看博客站点了，如图所示：

![](https://pic1.zhimg.com/80/v2-f7a0d722b670429e6e23ed8d86823b4c_720w.jpg?source=1940ef5c)

这样一个博客的架子就出来了，我们只用了三个命令就完成了。

## **部署**

接下来我们来将这个初始化的博客进行一下部署，放到 GitHub Pages 上面验证一下其可用性。成功之后我们可以再进行后续的修改，比如修改主题、修改页面配置等等。

那么怎么把这个页面部署到 GitHub Pages 上面呢，其实 Hexo 已经给我们提供一个命令，利用它我们可以直接将博客一键部署，不需要手动去配置服务器或进行其他的各项配置。

部署命令如下：

```text
hexo deploy
```

在部署之前，我们需要先知道博客的部署地址，它需要对应 GitHub 的一个 Repository 的地址，这个信息需要我们来配置一下。

打开根目录下的 _config.yml 文件，找到 Deployment 这个地方，把刚才新建的 Repository 的地址贴过来，然后指定分支为 master 分支，最终修改为如下内容：

![](https://pic1.zhimg.com/80/v2-58f606f59ffea1edbbe2e4eccbb8e707_720w.jpg?source=1940ef5c)

我的就修改为如下内容：

![](https://pic3.zhimg.com/80/v2-c4b116a6a82b7914e20faf193838c63d_720w.jpg?source=1940ef5c)

另外我们还需要额外安装一个支持 Git 的部署插件，名字叫做 hexo-deployer-git，有了它我们才可以顺利将其部署到 GitHub 上面，如果不安装的话，在执行部署命令时会报如下错误：

```text
Deployer not found: git
```

好，那就让我们安装下这个插件，在项目目录下执行安装命令如下：

```text
npm install hexo-deployer-git --save
```

安装成功之后，执行部署命令：

```text
hexo deploy
```

运行结果类似如下：

![](https://pic1.zhimg.com/80/v2-207ea40880e68fd5aa37261af20b5807_720w.jpg?source=1940ef5c)

如果出现类似上面的内容，就证明我们的博客已经成功部署到 GitHub Pages 上面了，这时候我们访问一下 GitHub Repository 同名的链接，比如我的 NightTeam 博客的 Repository 名称取的是 [http://**nightteam.github.io**](https://link.zhihu.com/?target=http%3A//nightteam.github.io)，那我就访问 [http://**nightteam.github.io**](https://link.zhihu.com/?target=http%3A//nightteam.github.io)，这时候我们就可以看到跟本地一模一样的博客内容了。

![](https://pic3.zhimg.com/80/v2-2e202bf18935a7c12eedb8633d254a11_720w.jpg?source=1940ef5c)

这时候我们去 GitHub 上面看看 Hexo 上传了什么内容，打开之后可以看到 master 分支有了这样的内容：

![](https://pic3.zhimg.com/80/v2-3c851d9321c74ff931712d839e2eb73e_720w.jpg?source=1940ef5c)

仔细看看，这实际上是博客文件夹下面的 public 文件夹下的所有内容，Hexo 把编译之后的静态页面内容上传到 GitHub 的 master 分支上面去了。

这时候可能就有人有疑问了，那我博客的源码也想放到 GitHub 上面怎么办呢？其实很简单，新建一个其他的分支就好了，比如我这边就新建了一个 source 分支，代表[博客源码](https://www.zhihu.com/search?q=%E5%8D%9A%E5%AE%A2%E6%BA%90%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D)的意思。

具体的添加过程就很简单了，参加如下命令：

```text
git init
git checkout -b source
git add -A
git commit -m "init blog"
git remote add origin git@github.com:{username}/{username}.github.io.git
git push origin source
```

成功之后，可以到 GitHub 上再切换下默认分支，比如我就把默认的分支设置为了 source，当然不换也可以。

**补充一句，多看书总是没有坏处的，把看书时的心得笔记放到博客上，也是一种学习方式。**

[程序员必备的书籍有哪些？（含下载方式）](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMzU1ODg0OTkxNQ%3D%3D%26mid%3D100005447%26idx%3D1%26sn%3D21a43fc9ebe2028d9ee7d3f8374783bf%26chksm%3D7c211c534b5695452062c17f6cdf8ef1604d29a49171cc50a91e8fc04a19342bda847366581a%23rd)

## **配置站点信息**

完成如上内容之后，实际上我们只完成了博客搭建的一小步，因为我们仅仅是把初始化的页面部署成功了，博客里面还没有设置任何有效的信息。下面就让我们来进行一下博客的基本配置，另外换一个好看的主题，配置一些其他的内容，让博客真正变成属于我们自己的博客吧。

下面我就以自己的站点 NightTeam 为例，修改一些基本的配置，比如站点名、站点描述等等。

修改根目录下的 _config.yml 文件，找到 Site 区域，这里面可以配置站点标题 title、副标题 subtitle 等内容、关键字 keywords 等内容，比如我的就修改为如下内容：

```text
# Site
title: NightTeam
subtitle: 一个专注技术的组织
description: 涉猎的主要编程语言为 Python、Rust、C++、Go，领域涵盖爬虫、深度学习、服务研发和对象存储等。
keywords: "Python, Rust, C++, Go, 爬虫, 深度学习, 服务研发, 对象存储"
author: NightTeam
```

这里大家可以参照格式把内容改成自己的。

另外还可以设置一下语言，如果要设置为汉语的话可以将 language 的字段设置为 zh-CN，修改如下：

```text
language: zh-CN
```

这样就完成了站点基本信息的配置，完成之后可以看到一些基本信息就修改过来了，页面效果如下：

![](https://pic3.zhimg.com/80/v2-69f1bbf60b1483446875a51eea033ef9_720w.jpg?source=1940ef5c)

## **修改主题**

目前来看，整个页面的样式个人感觉并不是那么好看，想换一个风格，这就涉及到主题的配置了。目前 Hexo 里面应用最多的主题基本就是 Next 主题了，个人感觉这个主题还是挺好看的，另外它支持的插件和功能也极为丰富，配置了这个主题，我们的博客可以支持更多的扩展功能，比如阅览进度条、中英文空格排版、图片懒加载等等。

那么首先就让我们来安装下 Next 这个主题吧，目前 Next 主题已经更新到 7.x 版本了，我们可以直接到 Next 主题的 GitHub Repository 上把这个主题下载下来。

主题的 GitHub 地址是：[https://**github.com/theme-next/h**exo-theme-next](https://link.zhihu.com/?target=https%3A//github.com/theme-next/hexo-theme-next)，我们可以直接把 master 分支 Clone 下来。

首先命令行进入到项目的根目录，执行如下命令即可：

```text
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

执行完毕之后 Next 主题的源码就会出现在项目的 themes/next 文件夹下。

然后我们需要修改下博客所用的主题名称，修改项目根目录下的 _config.yml 文件，找到 theme 字段，修改为 next 即可，修改如下：

```text
theme: next
```

然后本地重新开启服务，访问刷新下页面，就可以看到 next 主题就切换成功了，预览效果如下：

![](https://pica.zhimg.com/80/v2-4ab7d9bfd0036e92f63203eeafdddf5d_720w.jpg?source=1940ef5c)

## **主题配置**

现在我们已经成功切换到 next 主题上面了，接下来我们就对主题进行进一步地详细配置吧，比如修改样式、增加其他各项功能的支持，下面逐项道来。

Next 主题内部也提供了一个配置文件，名字同样叫做 _config.yml，只不过位置不一样，它在 themes/next 文件夹下，Next 主题里面所有的功能都可以通过这个配置文件来控制，下文所述的内容都是修改的 themes/next/_config.yml 文件。

### **样式**

Next 主题还提供了多种样式，风格都是类似黑白的搭配，但整个布局位置不太一样，通过修改配置文件的 scheme 字段即可，我选了 Pisces 样式，修改 _config.yml （注意是 themes/next/_config.yml 文件）如下：

```text
scheme: Pisces
```

刷新页面之后就会变成这种样式，如图所示：

![](https://pica.zhimg.com/80/v2-20e8c6bb647a02441ccfd1930d1484f9_720w.jpg?source=1940ef5c)

另外还有几个可选项，比如：

```text
# scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

大家可以自行根据喜好选择。

### **favicon**

favicon 就是站点标签栏的小图标，默认是用的 Hexo 的小图标，如果我们有站点 Logo 的图片的话，我们可以自己定制小图标。

但这并不意味着我们需要自己用 PS 自己来设计，已经有一个网站可以直接将图片转化为站点小图标，站点链接为：[https://**realfavicongenerator.net**](https://link.zhihu.com/?target=https%3A//realfavicongenerator.net)[1]，到这里上传一张图，便可以直接打包下载各种尺寸和适配不同设备的小图标。

图标下载下来之后把它放在 themes/next/source/images 目录下面。

然后在配置文件里面找到 favicon 配置项，把一些相关路径配置进去即可，示例如下：

![](https://pica.zhimg.com/80/v2-d63b377fb895d4064ee8c72d46a45a2d_720w.jpg?source=1940ef5c)

配置完成之后刷新页面，整个页面的标签图标就被更新了。

### **avatar**

avatar 这个就类似站点的头像，如果设置了这个，会在站点的作者信息旁边额外显示一个头像，比如我这边有一张 [avatar.png](https://www.zhihu.com/search?q=avatar.png&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D) 图片：

![](https://pica.zhimg.com/80/v2-5d168143fb53a125b6be49fccff5a46d_720w.jpg?source=1940ef5c)

将其放置到 themes/next/source/images/avatar.png 路径，然后在主题 _config.yml 文件下编辑 avatar 的配置，修改为正确的路径即可。

![](https://pic1.zhimg.com/80/v2-5d149a49184f4b06871e4037f580b563_720w.jpg?source=1940ef5c)

这里有 rounded 选项是是否显示圆形，rotated 是是否带有旋转效果，大家可以根据喜好选择是否开启。

效果如下：

![](https://pic3.zhimg.com/80/v2-d7c0cf3ae7ac500a5d9c1eb6dc580edd_720w.jpg?source=1940ef5c)

配置完成之后就会显示头像。

### **rss**

博客一般是需要 RSS 订阅的，如果要开启 RSS 订阅，这里需要安装一个插件，叫做 hexo-generator-feed，安装完成之后，站点会自动生成 RSS Feed 文件，安装命令如下：

```text
npm install hexo-generator-feed --save
```

在项目根目录下运行这个命令，安装完成之后不需要其他的配置，以后每次编译生成站点的时候就会自动生成 RSS Feed 文件了。

### **code**

作为程序猿，代码块的显示还是需要很讲究的，默认的代码块我个人不是特别喜欢，因此我把代码的颜色修改为黑色，并把复制按钮的样式修改为类似 Mac 的样式，修改 _config.yml 文件的 codeblock 区块如下：

![](https://pic3.zhimg.com/80/v2-7941a7561406404708ca63a6a71be76b_720w.jpg?source=1940ef5c)

修改前的代码样式：

![](https://pic2.zhimg.com/80/v2-21004bfd97961e586a2576ca2a5f68fa_720w.jpg?source=1940ef5c)

修改后的代码样式：

![](https://pic2.zhimg.com/80/v2-d9a11592b4758413b6a399ee8df3c821_720w.jpg?source=1940ef5c)

嗯，个人觉得整体看起来逼格高了不少。

### **top**

我们在浏览网页的时候，如果已经看完了想快速返回到网站的上端，一般都是有一个按钮来辅助的，这里也支持它的配置，修改 _config.yml 的 back2top 字段即可，我的设置如下：

![](https://pic1.zhimg.com/80/v2-013b9a741ad6e13b45cefa5eb04158e2_720w.jpg?source=1940ef5c)

enable 默认为 true，即默认显示。sidebar 如果设置为 true，按钮会出现在侧栏下方，个人觉得并不是很好看，就取消了，scrollpercent 就是显示阅读百分比，个人觉得还不错，就将其设置为 true。

具体的效果大家可以设置后根据喜好选择。

### **reading_process**

reading_process，阅读进度。大家可能注意到有些站点的最上侧会出现一个细细的进度条，代表页面加载进度和阅读进度，如果大家想设置的话也可以试试，我将其打开了，修改 _config.yml 如下：

![](https://pic1.zhimg.com/80/v2-56c6165e6ce714126ed976b802b31cf2_720w.jpg?source=1940ef5c)

设置之后显示效果如下：

![](https://pica.zhimg.com/80/v2-d8754aaa53d04f9fbb4506aa6262f2cf_720w.jpg?source=1940ef5c)

### **bookmark**

书签，可以根据阅读历史记录，在下次打开页面的时候快速帮助我们定位到上次的位置，大家可以根据喜好开启和关闭，我的配置如下：

![](https://pic1.zhimg.com/80/v2-734de19aeaaf2a0f52afe5525ab6483d_720w.jpg?source=1940ef5c)

### **github_banner**

在一些技术博客上，大家可能注意到在页面的右上角有个 GitHub 图标，点击之后可以跳转到其源码页面，可以为 GitHub Repository 引流，大家如果想显示的话可以自行选择打开，我的配置如下：

![](https://pica.zhimg.com/80/v2-8ca24abd3879a853c1e85fb0de4c6846_720w.jpg?source=1940ef5c)

记得修改下链接 permalink 和标题 title，显示效果如下：

![](https://pica.zhimg.com/80/v2-54f7770e6f9f5da44ddb1e31156a52d9_720w.jpg?source=1940ef5c)

可以看到在页面右上角显示了 GitHub 的图标，点击可以进去到 Repository 页面。

### **gitalk**

由于 Hexo 的博客是[静态博客](https://www.zhihu.com/search?q=%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D)，而且也没有连接数据库的功能，所以它的评论功能是不能自行集成的，但可以集成第三方的服务。

Next 主题里面提供了多种评论插件的集成，有 changyan | disqus | disqusjs | facebook_comments_plugin | gitalk | livere | valine | vkontakte 这些。

作为一名程序员，我个人比较喜欢 gitalk，它是利用 GitHub 的 Issue 来当评论，样式也比较不错。

首先需要在 GitHub 上面注册一个 OAuth Application，链接为：[https://**github.com/settings/app**lications/new](https://link.zhihu.com/?target=https%3A//github.com/settings/applications/new)，注册完毕之后拿到 Client ID、Client Secret 就可以了。

首先需要在 _config.yml 文件的 comments 区域配置使用 gitalk：

![](https://pica.zhimg.com/80/v2-3069040981826d816a1a7f25ec010a98_720w.jpg?source=1940ef5c)

主要是 comments.active 字段选择对应的名称即可。

然后找打 gitalk 配置，添加它的各项配置：

![](https://pic3.zhimg.com/80/v2-7c83e977b325a79a14d47f050864681f_720w.jpg?source=1940ef5c)

配置完成之后 gitalk 就可以使用了，点击进入文章页面，就会出现如下页面：

![](https://pica.zhimg.com/80/v2-559e8819d4853ea0129a7391bb9c67b2_720w.jpg?source=1940ef5c)

GitHub 授权登录之后就可以使用了，评论的内容会自动出现在 Issue 里面。

### **pangu**

我个人有个强迫症，那就是写中文和英文的时候中间必须要留有间距，一个简单直接的方法就是中间加个空格，但某些情况下可能习惯性不加或者忘记加了，这就导致中英文混排并不是那么美观。

pangu 就是来解决这个问题的，我们只需要在主题里面开启这个选项，在编译生成页面的时候，中英文之间就会自动添加空格，看起来更加美观。

具体的修改如下：

```text
pangu: true
```

### **math**

可能在一些情况下我们需要写一个公式，比如演示一个算法推导过程，MarkDown 是支持公式显示的，Hexo 的 Next 主题同样是支持的。

Next 主题提供了两个渲染引擎，分别是 mathjax 和 katex，后者相对前者来说渲染速度更快，而且不需要 JavaScript 的额外支持，但后者支持的功能现在还不如前者丰富，具体的对比可以看官方文档：[https://**theme-next.org/docs/thi**rd-party-services/math-equations](https://link.zhihu.com/?target=https%3A//theme-next.org/docs/third-party-services/math-equations)。

所以我这里选择了 mathjax，通过修改配置即可启用：

![](https://pic1.zhimg.com/80/v2-a62dd177301c9a10db561efb844a47fa_720w.jpg?source=1940ef5c)

mathjax 的使用需要我们额外安装一个插件，叫做 hexo-renderer-kramed，另外也可以安装 hexo-renderer-pandoc，命令如下：

```text
npm un hexo-renderer-marked --save
npm i hexo-renderer-kramed --save
```

另外还有其他的插件支持，大家可以到官方文档查看。

### **pjax**

可能大家听说过 Ajax，没听说过 pjax，这个技术实际上就是利用 Ajax 技术实现了局部页面刷新，既可以实现 URL 的更换，有可以做到无刷新加载。

要开启这个功能需要先将 pjax 功能开启，然后安装对应的 pjax 依赖库，首先修改 _config.yml 修改如下：

```text
pjax: true
```

然后安装依赖库，切换到 next 主题下，然后安装依赖库：

```text
$ cd themes/next
$ git clone theme-next/theme-next-pjax source/lib/pjax
```

## **文章**

现在整个站点只有一篇文章，那么我们怎样来增加其他的文章呢？

这个很简单，只需要调用 Hexo 提供的命令即可，比如我们要新建一篇「HelloWorld」的文章，命令如下：

```text
hexo new hello-world
```

创建的文章会出现在 source/_posts 文件夹下，是 MarkDown 格式。

在文章开头通过如下格式添加必要信息：

![](https://pic1.zhimg.com/80/v2-32a7128a11514dfc685da1886481181c_720w.jpg?source=1940ef5c)

开头下方撰写正文，MarkDown 格式书写即可。

这样在下次编译的时候就会自动识别标题、时间、类别等等，另外还有其他的一些参数设置，可以参考文档：[https://**hexo.io/zh-cn/docs/writ**ing.html](https://link.zhihu.com/?target=https%3A//hexo.io/zh-cn/docs/writing.html)。

## **标签页**

现在我们的博客只有首页、文章页，如果我们想要增加标签页，可以自行添加，这里 Hexo 也给我们提供了这个功能，在根目录执行命令如下：

```text
hexo new page tags
```

执行这个命令之后会自动帮我们生成一个 source/tags/index.md 文件，内容就只有这样子的：

![](https://pic3.zhimg.com/80/v2-08aa303d94d97f9ae54661868cc00d65_720w.jpg?source=1940ef5c)

我们可以自行添加一个 type 字段来指定页面的类型：

```text
type: tags
comments:false 
```

然后再在主题的 _config.yml 文件将这个页面的链接添加到主菜单里面，修改 menu 字段如下：

![](https://pic1.zhimg.com/80/v2-baa662c5f96094ff93f1b117c16ad3f6_720w.jpg?source=1940ef5c)

这样重新本地启动看下页面状态，效果如下：

![](https://pic1.zhimg.com/80/v2-6fd01415fd544fd6671d4f9d718bbc67_720w.jpg?source=1940ef5c)

可以看到左侧导航也出现了标签，点击之后右侧会显示标签的列表。

### **分类页**

分类功能和标签类似，一个文章可以对应某个分类，如果要增加分类页面可以使用如下命令创建：

```text
hexo new page categories
```

然后同样地，会生成一个 source/categories/index.md 文件。

我们可以自行添加一个 type 字段来指定页面的类型：

![](https://pic1.zhimg.com/80/v2-b1814bd2fdd5329fd345b8b5f58edc25_720w.jpg?source=1940ef5c)

然后再在主题的 _config.yml 文件将这个页面的链接添加到主菜单里面，修改 menu 字段如下：

![](https://pic3.zhimg.com/80/v2-5c101a2b21a2c7f678990fb03ffe87b8_720w.jpg?source=1940ef5c)

这样页面就会增加分类的支持，效果如下：

![](https://pic1.zhimg.com/80/v2-2d0de859f6ccd6bfec4cc301ce6dee91_720w.jpg?source=1940ef5c)

## **搜索页**

很多情况下我们需要搜索全站的内容，所以一个搜索功能的支持也是很有必要的。

如果要添加搜索的支持，需要先安装一个插件，叫做 [hexo-generator-searchdb](https://www.zhihu.com/search?q=hexo-generator-searchdb&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D)，命令如下：

```text
npm install hexo-generator-searchdb --save
```

然后在项目的 _config.yml 里面添加搜索设置如下：

![](https://pic2.zhimg.com/80/v2-02dfede3f1e48b6d84346223bde6d74b_720w.jpg?source=1940ef5c)

然后在主题的 _config.yml 里面修改如下：

![](https://pica.zhimg.com/80/v2-011b4a2d503960b05d9c542a2f18624e_720w.jpg?source=1940ef5c)

## **部署脚本**

最后我这边还增加了一个简易版的部署脚本，其实就是重新 gererate 下文件，然后重新部署。在根目录下新建一个 deploy.sh 的脚本文件，内容如下：

hexo clean

hexo generate

hexo deploy

这样我们在部署发布的时候只需要执行：

```text
sh deploy.sh
```

就可以完成博客的更新了，非常简单。

## **自定义域名**

将页面修改之后可以用上面的脚本重新部署下博客，其内容便会跟着更新。

另外我们也可以在 GitHub 的 Repository 里面设置域名，找到 Settings，拉到下面，可以看到有个 GitHub Pages 的配置项，如图所示：

![](https://pic1.zhimg.com/80/v2-2074f86ef4d52c22c0f8782d301f11aa_720w.jpg?source=1940ef5c)

下面有个 [custom domain](https://www.zhihu.com/search?q=custom+domain&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D) 的选项，输入你想自定义的域名地址，然后添加 CNAME 解析就好了。

另外下面还有一个 Enforce HTTPS 的选项，GitHub Pages 会在我们配置自定义域名之后自动帮我们配置 HTTPS 服务。刚配置完自定义域名的时候可能这个选项是不可用的，一段时间后等到其可以勾选了，直接勾选即可，这样整个博客就会变成 HTTPS 的协议的了。

另外有一个值得注意的地方，如果配置了自定义域名，在目前的情况下，每次部署的时候这个自定义域名的设置是会被自动清除的。所以为了避免这个情况，我们需要在项目目录下面新建一个 CNAME 文件，路径为 source/CNAME，内容就是自定义域名。

比如我就在 source 目录下新建了一个 CNAME 文件，内容为：

```text
blog.nightteam.cn
```

这样避免了每次部署的时候自定义域名被清除的情况了。

以上就是从零搭建一个 Hexo 博客的流程，希望对大家有帮助。

也欢迎大家访问小吴的个人博客网站：[http://www.**cxyxiaowu.com**](https://link.zhihu.com/?target=http%3A//www.cxyxiaowu.com) 目前里面更新了 400 多篇算法文章。

> 来源：NightTeam 作者：[崔庆才](https://www.zhihu.com/search?q=%E5%B4%94%E5%BA%86%E6%89%8D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1882886859%7D) 链接：[超全面！如何用 GitHub 从零开始搭建一个博客 ？](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/3li0n8REcU1DviwWiEYw_A)

[发布于 2021-05-12 22:56](//www.zhihu.com/question/23934523/answer/1882886859)

赞同 839 条评论分享

收藏喜欢**收起**

![知乎用户](https://pic3.zhimg.com/v2-abed1a8c04700ba7d72b45195223e0ff_xs.jpg?source=1940ef5c)**知乎用户**

98 人赞同了该回答

我之前谢了一个完整的教程，希望对你有所帮助：

[在github pages网站下用jekyll制作博客教程](https://link.zhihu.com/?target=http%3A//kresnikwang.github.io/works/tech/2016/06/07/%25E5%259C%25A8github%2520pages%25E7%25BD%2591%25E7%25AB%2599%25E4%25B8%258B%25E7%2594%25A8jekyll%25E5%2588%25B6%25E4%25BD%259C%25E5%258D%259A%25E5%25AE%25A2%25E6%2595%2599%25E7%25A8%258B.html)在我动手用jekyll部署我的博客之前，一直使用godaddy上面的wordpress主页来部署我的博客[kresnik.co](https://link.zhihu.com/?target=http%3A//kresnikwang.github.io/works/tech/2016/06/07/kresnik.co)。[WordPress](https://link.zhihu.com/?target=http%3A//kresnikwang.github.io/works/tech/2016/06/07/wordpress.com)当然有很多的优点，在我看来我用WordPress主要是为了

* 方便清晰的文件结构
* 可以随意选用的各种模板和插件
* 相对便宜的部署价格

因为这些优点，所以我想我还会在WordPress官网上继续保留我的免费博客。

既然这样，看官想必想问为什么要换成jekyll来重新部署博客？我简单的总结了一下：

* 流行又简洁的MarkDown写作语法
* 轻量级的网站结构，不再有动态网站的沉重
* 方便的和github pages结合，不仅免费，而且方便

所以对比与WordPress的沉重，jekyll让你回归到创作本身，当然如果你喜欢折腾，jekyll也绝对不会让你失望。推荐下面几个站点亮一下。

* [rusty shutter](https://link.zhihu.com/?target=http%3A//foto.lhzhang.com/)
* [Rasmus Andersson](https://link.zhihu.com/?target=http%3A//rsms.me/)

安装流程

1. 要用github pages，首先要在github中建立一个基于你的用户名的repository: 比如说我，就要建立名为[kresnikwang.github.io](https://link.zhihu.com/?target=https%3A//github.com/kresnikwang/kresnikwang.github.io)的repo。在以前的github版本中还需要在后台开启pages的功能，现在系统检测到这样的repo名称之后，会在setting中自动开启GitHub Pages的功能，如下图： 这样之后你就可以把这个repo克隆到本地随意进行修改了，在这个里面上传的网页就是你的网站的内容了，可以上传一个index.html试一试，这就是你的网站主页了。 关于GiuHub的使用，可以看几个比较好的入门教程：[GitHub](http://www.zhihu.com/question/20070065)
2. 之后我们就要在本地部署jekyll，jekyll的原理很简单。这是一个已经合成好的静态html网站结构，你用这个结构在username,[http://**github.io**](https://link.zhihu.com/?target=http%3A//github.io)文件夹里面粘帖好所有文件。再把更新完的本地repo推送到GitHub的master branch里面，你的网站就更新建设完毕了。 首先你需要[ruby](https://link.zhihu.com/?target=https%3A//www.ruby-lang.org/en/)来使用本地jekyll。Mac和Linux可以用Terminal配合[yum](https://link.zhihu.com/?target=http%3A//yum.baseurl.org/)或者[brew](https://link.zhihu.com/?target=http%3A//brew.sh/)这样的包管理器很方便的安装ruby。Windows下更是方便，可以直接中集成好的[Ruby installer](https://link.zhihu.com/?target=http%3A//rubyinstaller.org/)来进行安装，文章里的就是传送门。
   安装完ruby，之后就是要安装[RubyGems](https://link.zhihu.com/?target=https%3A//rubygems.org/pages/download)，gem是一个ruby的包管理系统，可以用gem很方便的在本地安装ruby应用。
   安装方法

   ```text
   //在RubyGems官网上下载压缩包，解压到你的本地任意位置
   //在Terminal中
   cd yourpath to RubyGems //你解压的位置
   ruby setup.rb
   ```
3. 有了gem之后安装jekyll就很容易了，其实用过nodejs和npm的同学应该很熟悉这样的包安装，真是这个世界手残脑残们的救星。。。。。（楼主不自觉的摸了摸自己快残了的手） 安装jekyll，有了gem，直接在Terminal里面输入以下代码：

   ```text
   $ gem install jekyll 
   ```
4. 好了，现在你的电脑已经准备完毕了。如果你是想自己捣鼓，可以根据这样的目录结构在你的[http://**username.github.io**](https://link.zhihu.com/?target=http%3A//username.github.io)文件夹下建立以下目录结构：
   ├── _config.yml
   ├── _drafts

   ```text
   |   ├── begin-with-the-crazy-ideas.textile
   |   └── on-simplicity-in-technology.markdown
   ```
   ├── _includes

   ```text
   |   ├── footer.html
   |   └── header.html
   ```
   ├── _layouts

   ```text
   |   ├── default.html
   |   └── post.html
   ```
   ├── _posts

   ```text
   |   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
   |   └── 2009-04-26-barcamp-boston-4-roundup.textile
   ```
   ├── _site
   └── index.html
   你可以一个个依次建立起来，然后在自己编写一个你想要的博客。
5. 如果你只是个普通用户，只是想要一个模板然后开始写自己的博客。那就很容易了，有几个可以简单开始的模板。

   * [poole/poole · GitHub](https://link.zhihu.com/?target=https%3A//github.com/poole/poole)极简风格的模板
   * [Jekyll Themes](https://link.zhihu.com/?target=http%3A//jekyllthemes.org/) jekyll的模板网站，可以找到各式各样你喜欢的模板。
6. 下载完了模板，可以吧里面的内容解压到你自己的网站目录底下。这时候你可以测试一下：

   ```text
   $ cd you website path //cd到你的网站目录下
   $ jekyll serve
   //一个开发服务器将会运行在 http://localhost:4000/
   //你就能在本地服务器看到你用模板搭建的网站了
   ```
7. 这时候可以看一下jekyll的设置，让你把模板变成你自己个性化的内容。在网站根目录下面找到 **_config.yml** ,这里会有几个比较关键的设置： 里面的permalink 就是你博客文章的目录结构，可以用pretty来简单的设置成日期+文章标题.html，也可以用自己喜欢的结构来设置。 记得把encoding 设置成utf-8，这样有利于中英文双语的写作和阅读。
8. 到这里你就可以开始写博客了，所有的文章直接放在**_posts**文件夹下面，格式就是我们之前提到的markdown文件，默认的格式是.md和.markdown文件。每篇文章的开始处需要使用yml格式来写明这篇文章的简单介绍，格式如下：

   ```text
       ---
       author: kresnikwang
       comments: true
       date: 2015-04-28 17:42:32+00:00
       layout: post
       title: PHP, Angular JS Development|My Export Quote|农产品出口工具开发
       categories:
       - Works
       - Tech
       tags:
       - bootstrap
       - javascript
       - php
       - AngularJS
       ---
   ```
   layout就是post，让jekyll知道你这是一篇post，很直观。需要注意的是里面的 **date** ，必须按照yml的语法来写，否则就会出现编译错误。可以只用**YYYY-MM-DD**来显示日期，也可以像我一样在后面加上 **HH:MM:SS+00:00** 来表示更具体的时间。
9. 到此为止可以开始尽情的写博客了，用GitHub软件同步到你的repository里面，网站上面就可以进行正常的显示了。如果说要添加一下有用的extra功能的话，评论和相关文章这两个功能比较多人会关注。 评论我们可以用[Disqus](https://link.zhihu.com/?target=https%3A//disqus.com/)国内应该也有类似的网站，到Disqus注册一个账号，选择添加评论区域到自己的网页，你将会的得到类似的代码：

   ```html
   <!-- Add Disqus comments. -->
   <div id="disqus_thread"></div>
   <script type="text/javascript">
     /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
     var disqus_shortname = '<USERNAME>'; // required: replace example with your forum shortname
     var disqus_identifier = "/works/tech/2016/06/07/%E5%9C%A8github%20pages%E7%BD%91%E7%AB%99%E4%B8%8B%E7%94%A8jekyll%E5%88%B6%E4%BD%9C%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B.html";

     /* * * DON'T EDIT BELOW THIS LINE * * */
     (function() {
       var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
       dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
       (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
         })();
   </script>
   <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
   <a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
   ```
   根据不同的模板，把代码添加到**_post/post.html**或者**_include/post.html**里你的文章底下，那当这篇文章被访问时，下方就会有评论区了
10. 相关文章的功能也比较好做，jekyll本来就集成了**site.related_posts**的功能，自动会寻找相关内容的文章，在你的post代码下面融入以下代码：

    ```html
    <aside class="related">
      <h2>Related Posts</h2>
      <ul class="related-posts">

        <li>
          <h3>
            <a href="http://kresnikwang.github.io///journey/2015/06/05/kresnik.co-%E5%8D%9A%E5%AE%A2%E6%90%AC%E5%AE%B6%E5%91%8A%E7%A4%BA.html">
              kresnik.co博客搬家告示
              <small><time datetime="2015-06-05T00:00:00+00:00">05 Jun 2015</time></small>
            </a>
          </h3>
        </li>

        <li>
          <h3>
            <a href="http://kresnikwang.github.io///tech/2015/06/02/javascript-include-html-page-by-jquery.html">
              Javascript Include Html Page By Jquery
              <small><time datetime="2015-06-02T18:45:42+00:00">02 Jun 2015</time></small>
            </a>
          </h3>
        </li>

        <li>
          <h3>
            <a href="http://kresnikwang.github.io///tech/2015/05/31/Github-use-http-instead-of-git.html">
              Github设置，强制使用"https://" 来代替 "git://"
              <small><time datetime="2015-05-31T05:03:36+00:00">31 May 2015</time></small>
            </a>
          </h3>
        </li>

      </ul>
    </aside>
    ```
    你每篇文章下面就会有三个相关文章的链接了。
