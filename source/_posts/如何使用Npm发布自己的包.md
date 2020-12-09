---
title: 如何使用Npm发布自己的包
date: 2020-12-09 13:50:04
tags: npm git 
cover:
categories: npm
---


## 一、在npm发布可安装的代码

##### 前言	

​	我们前端程序员，必不可少的经常使用`npm install xxx` 来安装各种各样的包。最近，我就很疑惑也很好奇，就为什么一条命令就可以将代码下载下来。那我自己的代码也能这么便捷的被下载吗？当然，结果是肯定的，本着好奇心害(cu)死(shi)头(jin)发(bu)，学习与了解后，接下来就来实现 : )

#### 1、关于npm

**简单介绍一下npm：**`npm`大家应该都了解，它是node.js官方的包管理工具。我们只需要在控制台输入一些简单的命令就能使用它来更新，下载，上传node包（https://www.npmjs.com ）。 

#### 2、发布准备

- 验证安装node和npm 已安装跳过 ^__^

  基本的安装node在这里就不做过多赘述，只来验证一下是否安装了node,因为这是我们实现npm上发布代码的基础

  打开命令行，输入`npm -v` 如果出现版本号，则说明已经安装

- 新建package.json

  使用`npm init` 创建一个package.json文件（在项目根目录下新建一个`package.json`，这是一个用来描述你的包的json文件）

  配置package.json

  ```json
  {
    "name": "lmtestpack",   											// name是你发布出去的npm包名
    "version": "1.0.1",															  // 发布版本
    "description": "My NPM test pack",                // 对此npm包的描述
    "main": "index.js",															  // 入口文件
    "scripts": {                                      // 脚本命令
      "start": "node index.js",                      
      "dev": "nodemon",
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "devDependencies": {                              // 仅在开发环境的包
      "eslint": "^5.16.0",
      "nodemon": "^1.18.11"
    },
    "dependencies": {                                 // 生产环境依赖包     
      "react": "16.13.1",
      "cli-color": "^2.0.0",
      "console-png": "^1.2.1"
    },
    "repository": {                                   // 包所在仓库信息                                   
      "type": "git",
      "url": "https://github.com/GTliangming/demo-object.git"
    },
    "author": "Peter Leung",                          // 作者
    "license": "ISC",                                 // 描述代码的许可证 MIT、ISC或UNLICENSED
    "keywords": [                                     // 此包的搜索关键词
      "node",
      "node.js",
      "express",
      "react.js"
    ],
  }
  
  ```

- 编写代码

  虽然npm是允许发布一个空包（只含有`package.json`的包），但是这样的一个包是没有意义的。 我们如果没有代码，可以先加一个`README.md`来说明一下自己的包。或者直接代码(js,html,css等)放进来。

#### 3、开始发布

- 创建npm账号

  如果你没有创建过npm账号，可输入如下命令添加一个npm账号，并跟着提示填写要注册的账号和密码

  `npm adduser`

  依次输入用户名、密码和邮箱 （也可以进入 https://www.npmjs.com/ 这个网址注册）

- 开始发布

  输入命令`npm publish` 或`npm publish --access public` 进行发布

- 发布成功

  这样你就可以在[www.npmjs.com](https://www.npmjs.com/)搜索并找到你刚才发布的npm包名（如果需要登录，请使用刚才你在控制台注册的账号）

  这样就可以通过`npm install xxx`来安装你的包了(例如：npm install lmtestpack)

#### 4、常见问题

- 1.如果需要更新包，在修改完代码后请记得修改`package.json`包的`version`字段，然后 `npm publish`。否则会无法发布；

- 2.如果在发布中显示类似'请确认你是否有权限更新xxx包'的英文提示，这就说明你的包名有人使用了。换个名字就好啦。

- 3.如果你想删除一个自己发布过的包，请使用命令 `npm unpublish --force xxx` （xxx为包名），一些没有意义的包还是建议删掉。

