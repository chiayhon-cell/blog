---

title: 【Hexo+GitHub搭建个人博客】一、框架建立

date: 2020-12-31 10:50:00

tags: Hexo

---
# 前言
　　当今市场上有着众多创作平台如知乎、微信公众好号、简书，为啥还要这么麻烦的去搭建一个个人博客？我想自己的答案就是想有一个安静的地方记录生活吧。不用注册登录账号、不用注意内容审核、不用担心文章消失，即使没有读者，我个人还是沉浸其中。

　　对于博客的搭建，静态站点对于动态站点而言，有着相对较快的访问速度，能托管在 github 上简单又快捷，于是选择了目前流行且免费的 Hexo 框架

<!-- more -->

# 搭建准备
　　在搭建前需要准备一下搭建环境，接下来的操作是在**命令行**中进行
　　
## Github 的注册
　　注册一个 [Github](https://github.com/join?source=login) 账号
　　
## Git 的下载与安装
　　[下载Git](https://git-scm.com/downloads) 并进行安装，最后一步添加路径时选择`Use Git from the Windows Command Prompt`，安装完成后在命令提示符中输入`git --version`验证是否安装成功

```
git config --global user.email "你的GitHub邮箱"

git config --global user.name "你的GitHub用户名"
```

　　Git 是版本控制系统，通俗的说就是提交更改项目等操作的工具。Git 在提交更改的时候，会需要提交者的邮箱和用户名，这可以在命令行窗口通过以下命令来设置：，--global 代表全局配置，若以后想为单个项目配置，则在项目文件夹输入去掉--global 后的命令
　　
## Node.js下载
　　[下载 Node.js](https://nodejs.org/en/download/) 并安装
　　　　
# 开始搭建
　　在**命令行**中输入下面命令来安装 Hexo，安装完成后输入`hexo -v`验证是否安装成功。
　　
```
npm install -g hexo-cli
```

　　-g表示全局安装，会将 Hexo 命令加入环境变量中，以使其在 cmd 下有效。


　　**新建**存放博客的目录，然后在**该路径下执行**初始化命令：
　　
```
hexo init
```

　　执行完毕后，输入`dir`将会生成以下文件结构：
　　
```
.
├── node_modules       //依赖安装目录
├── scaffolds          //模板文件夹，新建的文章将会从此目录下的文件中继承格式
|   ├── draft.md         //草稿模板
|   ├── page.md          //页面模板
|   └── post.md          //文章模板
├── source             //资源文件夹，用于放置图片、数据、文章等资源
|   └── _posts           //文章目录
├── themes             //主题文件夹
|   └── landscape        //默认主题
├── .gitignore         //指定不纳入git版本控制的文件
├── _config.yml        //站点配置文件
├── db.json            
├── package.json
└── package-lock.json
```

　　在根目录下执行如下命令或 `hexo s` 启动 hexo 的内置 Web 服务器 
　　
```
hexo server
```

　　该命令将会调用Markdown引擎解析项目中的博客内容生成网页资源，资源将会存于内存中，所以用户执行完命令之后在项目文件夹中是找不到相关的Web资源目录的。该命令还会启动一个简易的 Web服务器用于提供对内存中网页资源的访问（工作机制类似于webpack-dev-server），Web 服务器默认监听 4000 端口，用户可在浏览器中通过地址 `localhost:4000` 访问博客。

![Hexo默认首页](https://z4a.net/images/2020/12/31/image539b863fea85a0b8.png)

　　当 4000 端口已被其他应用占用时，可以添加 -p / --port 参数来设置 Web 服务监听的端口号，如 `hexo s -p 8000`




# 更换主题
　　当然现在这个页面太丑了，换一个比较入眼的主题，这里选择`Next`主题示例，在博客根目录右键打开`Git Bash here`，输入如下命令

```
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

　　等待下载完成，打开站点配置文件`_config.yml` 文件，将 `theme` 字段的值修改为 `next`，即：

``` yml /_config.yml
theme: next
```





　　然后重启内在服务器 `hexo s` 即可看到新主题：

![Next主题](https://z4a.net/images/2020/12/31/image4285b977a8308096.png)

``` yml /themes/next/config.yml
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini      #选择Gemini风格
```



# 博客配置

　　框架建立起来了，这时候则需要配置站点的信息，首先先介绍框架目录下比较重要的文件和文件夹：

- _config.yml :配置文件，配置了**博客的各项设置**
- source :资源文件夹，里面有所有的**博文或是独立页面**
- themes :主题文件夹，包含了一个或多个**主题文件夹**
- scaffolds :模版文件夹,新建文章时，Hexo 会根据文件夹里的**模板**来建立文件。



## 配置站点信息

　　打开 `_config.yml 文件`，可以对站点进行配置，`#`后面表示注释
　　

``` yml /_config.yml
# Site
title: chiayhon的小站       # 站点主题，标签页主题
subtitle: '欢迎光临'         # 副标题
description: '渴望早睡晚起'     # 网站描述
keywords: chiayhon's blog       # 网站关键字
author: chiayhon                # 作者名字
language: zh-CN                 # 语言设置为中文
timezone: 'Asia/Shanghai'       # 时区设为上海
```



## 博文字数统计

　　先安装字数统计插件:

```
npm install hexo-word-counter
```

　　然后在博客配置文件末尾添加：

``` yml /config.yml
symbols_count_time:
  symbols: true    # 帖子字数
  time: true    # 预计阅读时间
  total_symbols: false    # 在页脚显示所有帖子总字数
  total_time: false    # 在页脚显示所有帖子的阅读时间
```

　　配置完博客如下：

![信息配置完成](https://z4a.net/images/2020/12/31/3.png)

　　

{% note success%}

至此，一个最基本的框架已经被搭建起来了，框架建立篇到此结束，本框架版本是基于Hexo 5.3.0 + Node.js 12.13.0 + Git 2.23.0 + Next 8.1.0搭建的，不同版本可能会不兼容，若是安装出错需核对版本。

{% endnote %}

