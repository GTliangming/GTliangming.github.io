---
title: Jenkins的安装与配置
date: 2020-12-09 13:52:52
tags: jenkins 项目部署
cover:
categories: jenkins
---


#  一、什么是jenkins

​	Jenkins是一个开源软件项目，是`基于java开发`的一种[持续集成](https://aws.amazon.com/cn/devops/continuous-integration/)工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

> 为什么要使用jenkins这类持续集成工具（优势）
>
> 1、提高开发人员的工作效率
>
> ​	持续集成可将开发人员从手动任务中解放出来，并且鼓励有助于减少发布到客户环境中的错误和缺陷数量的行为，从而提高团队的工作效率。	
>
> 2、更快发现并解决缺陷
>
> ​	通过更频繁的测试，您的团队可以在缺陷稍后变成大问题前发现并解决这些缺陷。
>
> 3、更快交付更新
>
> ​	持续集成有助于您的团队更快、更频繁地向客户交付更新。

常见的持续集成工具还有： **TeamCity** 、**Travis CI**、**Go CD** 等

# 二、jenkins部署流程

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201204175311.png)

# 三、安装jenkins

#### **1、安装java环境**

​	主要是因为jenkins是基于java环境的 

- 准备工作。需要为 Jenkins 安装一个 Java 运行环境 `yum search openjdk`
- 安装合适的jdk 例如 `yum install java-1.8.0-openjdk`
- 配置环境变量
- 命令查看 `java -version` 是否安装成功

#### **2、jenkins的安装**

​	官方网站：[https://www.jenkins.io/zh/](https://www.jenkins.io/zh/)

​	安装文档：[https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos](https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos)(此次安装的是基于Centos)

	> sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo 
	>
	> sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key  
	>
	> sudo yum install jenkins 
	>
	> :(  可能会有报错(很大可能会有报错^__^)  百度都可以解决

#### **3、 启动/关闭 Jenkins**

`sudo service jenkins start/stop/restart  //显然，最后的参数分别对应启动、关闭、重启操作`

 `sudo chkconfig jenkins on`

上面的操作创建了一个叫做 jenkins 的用户来运行这个服务（也就是 Jenkins 服务）、将Jenkins 将在开机时作为守护进程启动，避免了每次都需要自行启动jenkins

#### **4、配置防火墙**

- 查看jenkins状态 `systemctl status jenkins`

- 查看防火墙状态 `systemctl status firewalld`

- 查看端口情况 `firewall-cmd --list-ports`

  >Jenkins 默认端口8080 需要在安全组配置中打开 

#### **5、查看jenkins**

​	本地：浏览器打开`localhost:8080`

​	自定义服务器：浏览器打开 `服务器ip:8080`

首次打开需要密码：密码在` /var/lib/jenkins/secrets/initialAdminPassword `内可以查看到

输入密码后稍等片刻就可看到jenkins的界面了

# 四、自定义配置

#### **1、自定义jenkins端口** 

- 终端打开`vim /etc/sysconfig/jenkins` 修改`JENKINS_PORT="xxxx"`

- jenkins管理界面修改

  **系统管理--->系统设置--->管理监控配置--->JenKins Location** 修改和刚才一致的端口号，否则会报错反向代理设置有误

- 然后重启jenkins:`systemctl restart jenkins`

#### **2、安装插件**

-  1、打开登陆Jenkins，点击Manage Jenkins
-  2、找到Plugin Manager，点击进入
-  3、打开Available点击，输入需要的插件名称，
-  4、勾选后 点击`install without restart ` 下载并重启jenkins

#### **3、jenkins汉化 **   🤷‍♂️ 🤷‍♂️ 🤷‍♂️ 🤷‍♂️ 🤷‍♂️

​	对于一个程序员，虽然说英文是必须的，但是有中文还是更方便的，话不多说，来安装插件

-  1、打开登陆Jenkins，点击Manage Jenkins
-  2、找到Plugin Manager，点击进入
-  3、打开Available点击，输入需要的插件名称，查找插件 **Localization: Chinese (Simplified)** 、**locale**
- 4、勾选后 点击`install without restart ` 下载并重启jenkins
- 5 、找到configure Sytem
- 6、找到locale 并设置默认语言为 `zh_CN` 并勾选下面的Ignore browser preference and force this language to all users
- 7、点击 Apply应用 
- 8、重启jenkins就可以看到汉化的界面（遗憾的是  部分汉化）

# 五、使用jenkins

### 1、创建任务  

打开jenkins控制台-->选择New Item 开始创建新的任务

基本操作：先起个名   然后选择 `Freestyle project` （具体的选择那个要看自己的项目情况）

### 2、开始配置

- General 通用配置

  描述：描述一下此任务的内容（可省略）

  选项：

  > -  Discard old builds    丢弃旧的构建
  >
  >   选择保持构建的天数（Days to keep builds）和保持构建的最大个数（Max # of builds to keep）
  >
  > - GitHub project     GitHub项目构建
  >
  > - This build requires lockable resources   此构建需要可锁定的资源
  >
  > - This project is parameterized   参数化构建
  >
  > - Throttle builds    节流阀构建
  >
  > - Disable this project    禁用此项目
  >
  > - Execute concurrent builds if necessary  在必要的时候并发构建

- 源码管理

  可以选择源码的来源（github、gitlab、svn都可以，但是都需要安装对应的插件）

  > 这里以github为例 gitbub插件 `github`、`Git Parameter`（支持选择分支和 Tag ）
  >
  > 注意：未安装插件前此处是不会有git的选项的 

  1、选择git后 配置仓库地址Repository URL

  2、配置证书 Credentials（可选配置，私有仓库需要）

  3、配置构建分支

  4、可选构建的Tag

- 构建触发器（在什么情况下触发构建）

  > - Trigger builds remotely (e.g., from scripts)     远程触发构建（例如，从脚本）
  > - Build after other projects are built    其他项目建成后再进行构建
  > - Build periodically   定期构建
  > - GitHub hook trigger for GITScm polling   通过Github hook进行轮训构建
  > - Poll SCM 轮询构建   

  主要来说轮询构建（在符合时间表的规定时间来进行自动构建）

  添加日程表 格式：五位 ****

  1、第一个*代表分钟 取值0-59  第几分钟执行

  2、第二个*代表小时 取值0-23  第几小时执行

  3、第三个*代表日 取值1-31 第几日执行

  4、第四个*代表月 取值1-12 第几个月执行

  5、第五个*代表星期 取值0-6 每周第几天执行（周天是第一天）

  例：

  `* * * * *` 每分钟 、 `H * * * *`每小时 、`/` 代表步长

  `H/30 * * * *` : 每半小时检查一次代码是否有更新，有的话进行构建

  `H H/2 * * *` :  每两小时检查一次代码是否有更新，有的话进行构建

  `H 2 * * *` :  每天凌晨两点定时检查一次代码是否有更新，有的话进行构建

  `H H 15 * *` :  每月15号定时检查一次代码是否有更新，有的话进行构建

  `H 9 * * 1-5` :  周一至周五上午9点定时检查一次代码是否有更新，有的话进行构建

  还有很多具体的配置，想了解的话配置的后面有个小问号，去哪里可以了解：）

- 构建环境

  > Delete workspace before build starts  在生成开始之前删除工作区
  >
  > Use secret text(s) or file(s) 使用机密文本或文件
  >
  > Provide Configuration files  提供配置文件
  >
  > Send files or execute commands over SSH before the build starts 在构建开始之前通过SSH发送文件或执行命令
  >
  > Send files or execute commands over SSH after the build runs  在构建运行后通过SSH发送文件或执行命令
  >
  > Abort the build if it's stuck  如果生成被卡住，请中止它
  >
  > Add timestamps to the Console Output  向控制台输出添加时间戳
  >
  > Inspect build log for published Gradle build scans  检查生成日志中已发布的分级生成扫描
  >
  > Provide Node & npm bin/ folder to PATH  提供节点和npm bin/文件夹到路径 (此次选择node)

  但是初始是没有node & npm这个选项的 需要做几步操作

  1、运行服务器需要安装node环境

  2、安装node插件`NodeJS Plugin` （安装方法上面说到过）

  3、jenkins控制台选择 Manage jenkins  ---> Global Tool Configuration--->最底下Node.js选择

  4、 选择node.js安装  起别名、选择版本  记得勾选`Install automatically` 应用后保存退出

  5、再去选择就会有对应的版本以供选择了

-  构建

选择构建的方式（选项很多），这里选择Execute shell 

然后就可以填写自定义的shell命令了

```shell
pwd
ls
node -v 
npm -v 
npm install 
npm run build
echo "打包构建成功！"
```

- 构建后操作 

> 上一步我们已经成功执行到将我们的项目自动克隆下来进行了打包，生成了打包好的文件build,接下来就要执行发布，将打包好的文件放到我们的服务器，使其可以通过域名或者公网IP进行访问

选择增加构建后操作步骤（选项依旧很多） 这里选择 Send build artifacts SSH 通过SSH发送我们构建的结果

此选项也需要安装插件 步骤：

1、 安装插件`Publish Over SSH`

2、jenkins控制台选择 Manage jenkins  --->Configure System系统配置--->找到Publish over SSH 选项

3、选择SSH servers 并点击新增Server

4、填写name、发布服务器主机IP地址、用户名、发布目录

5、选择高级配置 进行连接密码和端口号的配置

6、配置完成后点击`Test configuration`进行测试连接   显示success 表示连接成功

7、点击应用并保存退出

然后回到我们刚才的配置，选择Send build artifacts SSH

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201205172521.png)

Source files :都上传那些文件 build/**  表示build文件夹下的所有文件

Remove prefix:去掉前缀 这里选择build/  表示只需要build文件夹里的文件

Remote directory :放置到远程服务器的那个文件夹下 

Exec command:执行的命令

至此就配置完成 ，就可以点击build now进行构建了






