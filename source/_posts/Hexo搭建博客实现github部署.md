---
title: Hexo搭建博客实现github部署
date: 2020-12-09 13:45:39
tags: hexo github
cover:
categories: hexo
---



### 一、本地创建 Hexo 博客框架

\*\*

#### 1、搭建所需环境

**Hexo 官网地址：[https://hexo.io/](https://hexo.io/)上都对所需环境、版本信息都做了介绍，这里不过多介绍。**
\*\*

#### 2、配置本地仓库与远程仓库的 ssh key

打开终端，输入`cd ~/.ssh` 查看是否有`id_rsa` 以及`id_rsa.pub` 文件，如果有或者自己已经配过。此项可以跳过。如果没有，现在开始创建新的

- 打开终端，输入`cd ~/.ssh`

- 在 ssh 文件目录下执行 `ssh-keygen -t rsa -C “yourname@xx.com`   此处的邮箱地址换成你自己的

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598520503078-fefabe3c-1a79-4642-8181-ed53257b0d7d.png#align=left&display=inline&height=68&margin=%5Bobject%20Object%5D&name=image.png&originHeight=68&originWidth=607&size=83506&status=done&style=none&width=607)

- 点击后，箭头处填写生成的公钥以及私钥的文件名，如果只配置一个 ssh key ,就直接点击回车选择默认，
- 接下来的两项也直接回车选择默认
- ![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598520679281-49a1b535-8359-4882-b504-c6cdc70d46ea.png#align=left&display=inline&height=422&margin=%5Bobject%20Object%5D&name=image.png&originHeight=422&originWidth=509&size=500198&status=done&style=none&width=509)

这样，SSH key 就生成成功了，现在我们需要使用这个 ssh key

- 使用 ssh-agent 代理管理 git 私钥

添加本地私钥：`ssh-add ~/.ssh/刚才生成的公钥的名字`

- 继续在终端输入`cat ~/.ssh/刚才生成的公钥的名字`（默认是 id_rsa.pub）如果你创建的时候起了别的名字，这里就输入刚才的名字。然后点击回车，终端内就会显示出公钥的内容（很长一段,以 ssh-rsa 开头）
- 复制这段信息，打开 github，点击头像选择 setting
- 在左边的导航选项内选择`SSH and GPG keys`选项，选择上面的`New SSH key`

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598521006842-eb0f9a45-2e6f-4d87-a944-fbb893abb3d0.png#align=left&display=inline&height=115&margin=%5Bobject%20Object%5D&name=image.png&originHeight=115&originWidth=795&size=14106&status=done&style=none&width=795)

- 给这个 SSH key 起个名字，以便区分，因为不同的电脑需要配置不同 ssh key，然后将刚才复制的公钥信息粘贴在 key 内，点击 Add ssh key. 这样就配置成功了
- ![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598521113622-875772c9-e7dc-46c8-b84b-e4966e976042.png#align=left&display=inline&height=469&margin=%5Bobject%20Object%5D&name=image.png&originHeight=469&originWidth=824&size=29895&status=done&style=none&width=824)
- 在终端输入` ssh -T git@github.com` 检测是否配置成功，如下就表示配置成功

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598521331141-0c765591-ce89-472c-8d76-d8019952440c.png#align=left&display=inline&height=37&margin=%5Bobject%20Object%5D&name=image.png&originHeight=37&originWidth=655&size=47981&status=done&style=none&width=655)

> 如果中途哪一步出现了问题，可以在.ssh 文件夹下干掉生成的文件，重新开始

#### 3、创建博客仓库

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598516924636-5ca5fb75-87f1-4f5b-a160-2f36280d11aa.png#align=left&display=inline&height=568&margin=%5Bobject%20Object%5D&name=image.png&originHeight=756&originWidth=710&size=101157&status=done&style=none&width=533)
点击 Create respository 就创建成功了 😃

- 使用 SSH 将项目克隆到本地，并初始化 git 仓库`git init ` 此时默认在`master` 分支

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598521616753-518b911a-39c0-4c98-8b2f-e239295f9e04.png#align=left&display=inline&height=320&margin=%5Bobject%20Object%5D&name=image.png&originHeight=320&originWidth=542&size=41348&status=done&style=none&width=542)

- 因为 github 部署博客的编译后的代码默认生成到 master 分支，所以我们将项目的源码放置在一个新的`blog`分支，执行`git checkeout -b blog` 创建并切换到 blog 分支

### 二、生成博客项目

#### 1、项目生成

1>、安装生成项目的脚手架工具：`npm install hexo-cli -g`
2>、生成项目`hexo init 项目名`

#### 2、运行项目

1>、进入到刚才创建的项目下，使用`hexo s `(hexo server 的缩写)就可启动项目，本地浏览器打开`localhost:4000`就可查看到我们新创建的 hexo 项目

> 如果默认的 4000 端口被占用，可使用 hexo s -port 端口号 来启动项目，并在浏览器使用对应的端口来访问

2>、安装完成后，指定文件夹的目录如下：

```
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

其中\_config.yml 文件用于存放网站的配置信息，你可以在此配置大部分的参数；scaffolds 是存放模板的文件夹，当新建文章时，Hexo 会根据 scaffold 来建立文件；source 是资源文件夹，用于存放用户资源，themes 是主题文件夹，存放博客主题，Hexo 会根据主题来生成静态页面

### 三、部署到 gtihub

**安装部署到 github 的工具插件：**`npm install hexo-deployer-git --save`

\_config.yml 是重要的网站的配置信息，具体配置详见官网[https://hexo.io/docs/configuration](https://hexo.io/docs/configuration)

因为我们要将编译后的代码放置在 master 分支下，所以我们需要修改\_config.yml 文件下的 deploy

```
deploy:
  type: 'git'
  repo: 存放博客项目的github仓库地址，建议使用ssh地址
  branch: master  需要将博客的代码编译到那个分支，此处就写对应分支的分支名

```

修改完成后，使用 hexo 的命令

1. `hexo clean` 清除之前的缓存
1. `hexo generate` 编译项目代码
1. `hexo deploy` 将编译后的代码根据\_congin.yml 内的 deploy 推送到指定仓库的制定分支
   > 第二步和第三部可以一块 执行 hexo g -d

此时就可在`https://你的博客仓库名` (例如我的[https://gtliangming.github.io/](https://gtliangming.github.io/))查看到你新建的的博客了
可能会有一定的延迟，稍等下刷新就有了\*\*

💐 至此，我们的博客就搭建完成了，至于后面的主题以及美化等的细节问题，期待下一篇吧！！！

\*\*
**不知道细心的大家发现了问题了吗？我们博客是更新了，但是我们的本地 blog 分支的代码更新该没有提交，这就表示我们每次执行完 hexo 的命令部署完博客后，还要 Git 三部曲提交我们 blog 分支的代码。好奇的同学就有问题了，能不能一步解决呢，宾果！，关注下一篇哦（Tracis cI 实现自动部署 hexo 博客至 github）**
