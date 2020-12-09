---
title: Node.js实现github第三方登录
date: 2020-12-09 13:50:52
tags: node.js github
cover:
categories: node.js
---

### 一、什么是第三方登录


##### 第三方登录是基于用户在第三方平台上已有的账号和密码来快速完成己方应用的登录或者权注册的功能。而这里的第三方平台，一般是已经拥有大量用户的平台，国外的比如Facebook，Twitter等，国内的比如微博、微信、QQ等。

##### 第三方登录的目的是使用用户在其他平台上频繁使用的账号，来快速登录己方产品，也可以实现不注册就能登录，好处就是登录比较快捷，不用注册。

### 二、第三方账号登录流程
![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110142954.png)


#### 获取授权 ----> 绑定账号 ----> 登陆成功 ----> 解绑/换绑



### 三、获取第三方账号登录授权



以Github 为例

流程：

![这里写图片描述](https://img-blog.csdn.net/20170828161704516?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1bWluZzM4MzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 1、 在github创建应用

***Settings ---->  Developer setting ----> OAuth Apps ----> New OAuth Apps*** 

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110143108.png)

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110143216.png)

至此  应用就创建成功，接下来就可以使用！

***其他微博、腾讯QQ 、微信等都大同小异，只是申请流程会有不同，但是大体流程不会有出入，目的都是为了得到 `Client ID`和`Clinet Secret`(或者是APPID 和 APP Secret)***

#### 2、Nodejs实现后台登录处理

- ##### 前端页面导航到我们的接口  （a标签）

`https://github.com/login/oauth/authorize?client_id=`+刚创建的client_id+`&redirect_url=`+回调方法接口的地址


Eg: `https://github.com/login/oauth/authorize?client_id=e2a1eaa9cf8f58c79077&redirect_url=http://www.netbugs.cn:3002/api/authorizeLogin`


- ##### 得到授权的`redirect_url` 此回调地址会携带一个`code` 


  Eg：` http://www.netbugs.cn:3002/api/authorizeLogin?code=xxxxx`


- ##### `authorizeLogin`  解析code并请求 github的`access_token`


  请求地址：`https://github.com/login/oauth/access_token`


  请求body : ```{ client_id,    client_secret, code}```


- ##### 根据得到的`access_token` 获取用户信息


  请求地址 ： `https://api.github.com/user?access_token=`+刚刚获取到的access_token


#### 3、得到用户信息

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110142712.png)

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110140652.png)

##### 测试链接 ： http://www.netbugs.cn:3002/api/loginGithub

##### 注意事项： 各个不同的第三方登录基本上流程与此方法没有太大出入，只是在不同的平台，和自己此github应用的业务会有点不一样,只要阅读官方文档，都可以很好解决 ：）~