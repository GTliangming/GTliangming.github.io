---
title: Hexo+TravisCI自动部署项目至github
date: 2020-12-09 13:46:51
tags:
cover:
categories:
---
> 什么是 Hexo 这里就不做过多的介绍了，百度一下有很多介绍的
> 这里来介绍一下什么是 Travis CI

### 一、什么是 Travis CI

作为一个专(diao)业(si)研(cheng)发(xu)人员，开发代码、与产品和 bug 斗志斗勇已经都让我们的发量不保了，很多的项目构建与发布还要让头发遭罪，这不能忍。为了提高开发的效率，自动构建和测试的自动化工具就宛如一个利器，其中的`Travis CI`  就是其中免费的、简单的、而且市场占比最高的一个。

**Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。**
\*\*

> 什么是持续集成服务 ？
> 持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码"集成"到主干。
> 持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。

### 二、创建项目的 github 仓库

#### 1、github

至于什么 github 是什么、github 怎么用、github 怎么创建仓库，这里就不做赘述
相关介绍可以点击了解一下：
[https://www.leiue.com/what-is-github](https://www.leiue.com/what-is-github)
[https://www.leiue.com/what-is-github](https://www.leiue.com/what-is-github)

#### 2、登录 github 创建一个用来放置 hexo 博客项目的仓库

创建 hexo 博客可查看我另外一篇文章[hexo 博客搭建与部署 github](https://www.yuque.com/jijiangyongyoubakuaifujilm/kb/al1tyx)

### 三、使用 Travis CI 托管博客项目

登录`Travis CI`官网，官网地址[https://travis-ci.org/](https://travis-ci.org/) 选择 Github 登录
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598517318983-7261f671-9820-4ddb-8fd3-ab128bb7fa12.png#align=left&display=inline&height=529&margin=%5Bobject%20Object%5D&name=image.png&originHeight=529&originWidth=1130&size=62588&status=done&style=none&width=1130)
登录自己的 github 账号后 就可看到登陆成功的界面，点击头像右边的下拉标签，选择 Setting,即可看到下图的样子，主要分为三部分。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598517445333-6810df05-c003-444c-813f-1b724f566443.png#align=left&display=inline&height=762&margin=%5Bobject%20Object%5D&name=image.png&originHeight=762&originWidth=1097&size=120720&status=done&style=none&width=1097)
1、 左边的部分是我们的 github 账户信息。可以点击`Sync account` 实时同步我们的账户信息
2、右上角是我们登陆 Travis CI 的账户，包括设置和退出登陆，设置以后会用到
3、重要部分！！！！
在这可以看到我们 github 账户下的所有的仓库。我们需要托管那个仓库，就在右边将它的开关打开，此时的 Travis CI 就会帮我们监管这个仓库的活动。当然，仓库也不是说管就能管的，我们要给他一定的权限

### 四、赋予 Travis CI 管理仓库的权限

赋予权限，我们就需要使用到 github 的`Personal access tokens` 接下来我们来说怎么生成这个 Token 并且使用，

#### 1、生成 Token

步骤：
登录 github---> 点击右上角头像，选择 setting 并点击--->
在左侧导航栏最底下选择`Developer setting`--->然后选择`Personal access tokens`
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598518197562-1f67e48c-0483-4b0d-9c39-f8cfffe62f34.png#align=left&display=inline&height=252&margin=%5Bobject%20Object%5D&name=image.png&originHeight=252&originWidth=1041&size=43472&status=done&style=none&width=1041)
然后选择 Generate new token
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598518325096-c117920a-bdff-4075-95d0-30e4ecc2cf9c.png#align=left&display=inline&height=605&margin=%5Bobject%20Object%5D&name=image.png&originHeight=605&originWidth=1066&size=90394&status=done&style=none&width=1066)
给 token 起个名字，底下选取一下权限，前三项建议都选上，如果不知道选择那个，项目自用也不考虑安全性的问题的话，可以全选。选择完权限后，点击 Generate token ，token 就生成了
！！！！划重点，划重点，划重点，重要的事情说三遍！
因为 token 只会在生成后显示一次，之后就不会显示，如果忘记，就只能删除后重新创建了。所以此时需要把这个 token 保存起来，安全的地方。因为后面会用到。

#### 2、将权限赋予给 Travis Ci

所谓的权限赋予，就是把刚才生成的 token 赋予给 Travis CI 托管的仓库。
登录 Travis ci 点击需要托管的仓库后面的 setting 选项
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598518780158-b3f0e7f5-4115-487b-920d-b6cac119b354.png#align=left&display=inline&height=59&margin=%5Bobject%20Object%5D&name=image.png&originHeight=59&originWidth=698&size=5505&status=done&style=none&width=698)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598518846941-bab61896-e0de-40db-bf9f-1760c034e526.png#align=left&display=inline&height=676&margin=%5Bobject%20Object%5D&name=image.png&originHeight=676&originWidth=1121&size=79574&status=done&style=none&width=1121)
首先打开最上面的 General 内的开关

> 依据版本，有些会有 Build only if .travis.yml is present 开关，有的话一同打开

这样，当向项目 push 代码的时候 travis CI 就会据  `.travis.yml`  的内容去部署我们的项目了。

### 五、告诉 Travis CI 怎么部署---创建.travis.yml

建议在开始配置之前，先去官网了解一下.travis.yml 内各个钩子方法的作用以及执行顺序
传送门：[https://docs.travis-ci.com/](https://docs.travis-ci.com/)

**以下两种方法，都要提前在博客项目的根目录下创建.travis.yml 文件**

#### 1、方案二：普通方法

直接配置.yml 文件

```
sudo: false
# 配置语言与node版本
language: node_js
node_js:
  - 10 # use nodejs v10 LTS

# 缓存
cache:
  directories:
    - node_modules
before_install:
  - npm install -g hexo-cli
  - npm install -g hexo
  - npm install hexo-deployer-git@2.1.0 --save

install:
  - npm i

script:
  - hexo clean

  # generate static files
  - hexo generate
after_success:
	# 这里的命令都可以自行添加（例如我加了ls，想要查看编译后文件都有那些）
  - cd ./public
  - ls
  - git init
  - git config --global user.name "yourname"
  - git config --global user.email "youremail"
  - git add  .
  - git commit -m "Travis CI Auto Builder"
  - git push --quiet --force https://$GH_TOKEN@${GH_REF} master:master
  # 如果不习惯这种变量引用，可直接复制粘贴到这，

# 只有指定的分支提交时才会运行脚本,也就是你要监控的分支
branches:
  only:
    - blog

 #全局可引用变量
 env:
 global:
   - GH_REF ：你的项目github仓库的ssh
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598600138240-ae7cf4c6-6b3f-4b3e-ad09-209632c9dac1.png#align=left&display=inline&height=248&margin=%5Bobject%20Object%5D&name=image.png&originHeight=248&originWidth=355&size=21803&status=done&style=none&width=355)

#### 2、加密方法

此方法需要安装 ruby 环境以及很多东西，可以尝试，但不建议使用，其实就是我觉得麻烦，还不一定配得好。
所以这里只列出了大概的.travis.yml 的配置信息，有兴趣的可以百度搜一下这种方法

```ruby
# 配置语言及相应版本
language: node_js

node_js:
  - "4"

# 配置环境
before_install:
# 替换为刚才生成的解密信息
- openssl aes-256-cbc -K $encrypted_xxxxxxxxxxxx_key -iv $encrypted_xxxxxxxxxxxx_iv -in .travis/id_rsa.enc -out ~/.ssh/id_rsa -d
# 改变文件权限
- chmod 600 ~/.ssh/id_rsa
# 配置 ssh
- eval $(ssh-agent)
- ssh-add ~/.ssh/id_rsa
- cp .travis/ssh_config ~/.ssh/config
# 配置 git 替换为自己的信息
- git config --global user.name 'yuorname'
- git config --global user.email youremail@xx.com

# 安装依赖
install:
- npm install hexo-cli -g
- npm install

# 部署的命令
script:
- npm run deploy  # hexo clean && hexo g -d

# 项目所在分支
branches:
  only:
  - master
```

#### 无论是以上哪种方法，配置完成后，只要我们在修改的分支提交了代码后，就可以在 Travis Ci 内看到编译以及部署的过程

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2401896/1598600662273-33958ad7-644d-41ab-91fe-b3cd03108ad8.png#align=left&display=inline&height=570&margin=%5Bobject%20Object%5D&name=image.png&originHeight=570&originWidth=1426&size=81088&status=done&style=none&width=1426)
黄色是正在部署中，绿色是部署成功，红色当然就是失败了！！

部署完成后，再去 github 的 master 分支，就可以看到 travis ci 它已经自动帮我们将编译后的代码提交到 master 分支了（有一定延迟），然后去我们的博客，刷新一下就可以看到修改了 😸😺😊

### 六、问题总结

#### 1、编译完成后，发现 public 文件夹内就没有生成 index.html 文件

出现这种问题后，在终端运行`npm ls --depth 0` 查看 hexo 的各个插件是都安装完成，没有安装完成的安装一下 `npm install 插件名 --save`

#### 2、编译完成后，生成了 index.html 文件，但是文件为空

出现这种问题，去检查一下 theme 主题文件夹下你所使用的主题文件内是否还有东西，如果有的话还出现这种问题，建议删除主题后重新 clone 一下（小贴士：刚 clone 的主题记得要去删除里面的.git 文件夹哟）
