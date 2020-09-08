---
title: Mac下Mongodb的安装
date: 2020-09-08 16:00:00
tags: mongodb
cover: http://www.lmwebs.top/img/nanan.jpg
---

## Mongodb的安装

> `MongoDB` 是一个跨平台的，面向文档的数据库，提供高性能，高可用性和可扩展性方便。 `MongoDB` 工作在收集和文件的概念。

### 一、下载方式

下载MongoDB有三种方式：(具体版本查看官网)

（1）手动命令安装

终端运行：`curl http://downloads.mongodb.org/osx/mongodb-osx-x86_64-2.4.6.tgz > mongodb.tgz`

（2）采用Homebrew

更新homebrew ：`brew update`

安装mongodb ：` brew install mongodb `

> 想安装最新开发版命令 ：`brew install mongodb --devel`

（3）官网下载安装包。

官网地址 ：`https://www.mongodb.com/try?jmp=nav#community`

### 二、安装配置

#### 1、创建MongoDB数据存放的默认文件夹

MongoDB数据存放的默认路径为/data/db(即根目录下/data/db)，但该目录的文件夹在你电脑不一定存在，执行以下命令创建(你未必有权限创建，所以加上sudo)

`sudo mkdir -p /data/db`

> 数据库需要读写数据，如果你没root权限，在根目录下创建的文件夹默认是没有写入权限的，直接启动MongoDB会报错

为此文件夹添加读写权限 : `sudo chown -R 用户名 /data/db`

#### 2、配置环境变量

打开终端，输入 `open -e .bash_profile`

在其中添加 `export PATH=${PATH}:/usr/local/MongoDB/bin`

用Command+S保存配置，关闭上面的.bash_profile编辑窗口 

打开终端 输入`source .bash_profile` 使刚才的配置生效

然后在终端输入 `mongod -version` 可以看到版本信息  说明已经安装成功

#### 3、使用mongodb

运行mongodb :终端输入 `mongod` 启动mongob 

显示等候客户端连接的界面就代表启动成功了，如果不成功就检查下 /data/db 文件夹位置对不对，不行就重新删掉建一个， 打开浏览器，输入localhost:27017 ，会出现这样一行文字。

> It looks like you are trying to access MongoDB over HTTP on the native driver port

然后再次在终端输入 `mongo`   开始进入数据库操作

#### 4、 退出mongodb

要停止MongoDB的时候一定要正确的退出，不然下次再次连接数据库会出现问题，使用下面的两行代码可以完成这一操作。

```
use admin;

db.shutdownServer();
```

### 三、可视化数据库工具

​	Robo 3t

RoboMongo是一个跨平台的MongoDB GUI客户端管理工具，支持Windows、MacOS、Linux。其特点是支持到MongoDB服务器的SSL连接，还支持使用SSH隧道连接。RoboMongo的查询界面同时支持树视图、表格视图、文本视图三种，也可以保存查询结果供以后使用。

下载地址 ：https://robomongo.org/


