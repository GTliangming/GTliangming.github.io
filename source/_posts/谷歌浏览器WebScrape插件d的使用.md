---
title: 谷歌浏览器WebScrape插件d的使用
date: 2020-12-09 13:49:04
tags: chrome 
cover:
categories: chrome
---


#  Web Scraper

官方文档 https://www.webscraper.io/documentation?utm_source=extension&utm_medium=popup

## 一、什么是Web Scraper

web scraper是一款网站数据提取工具，类似于爬虫，但不需要像python爬虫那样编写代码，使用门槛较低，适用于轻度的数据爬取。web scraper主要以谷歌扩展插件的形式存在。

##  二、如何安装Web Scraper

打开谷歌应用商店，搜索栏搜索web scraper,点击添加

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135614.png)

## 三、使用介绍

##### 1、确定谷歌浏览器安装了Web Scraper后，打开开发者工具界面，并使其悬浮在浏览器底部，可在工具栏发现web scraper一项

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135607.png)

##### 2、功能栏介绍

- create new sitemap  创建新的网站查询

**Create Sitemap**

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135603.png)



> sitemap name :名称 随便起 (非中文)
>
> Start URL ：需要抓取数据的网站链接
>
> 以百度为例创建查询

**Import Sitemap**

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135556.png)

> Sitemap Json : 导入json数据进行抓取
>
> Rename Sitemap : 起名字

- Sitemap baidu

  ![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135550.png)

> Selectors :查看创建的筛选器
>
> Selectors graph :筛选的层级结构
>
> Edit metadata : 编辑之前创建的网站
>
> Scrape: 开始根据创建的筛选器开始抓取
>
> Browser : 在线查看筛选的数据 （需在抓取完成后查看）
>
> Export Sitemap : 导出创建的查询网站信息
>
> Export data as CSV ：导出抓取的数据以CSV文件格式



## 四、实例

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135542.png)

抓取顶部导航的名称及链接

### 1、第一步 创建sitemap

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135536.png)

### 2、创建筛选器

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135530.png)

单击创建的sietmap 然后点左下角 Add new Selector（可添加多个）

​	**筛选标题筛选器**

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135525.png)

> id：名字随便起
>
> Type: 类型 可选  （具有较多类型，建议查看官网）
>
> Selector ： 先点击selector ，然后点击网站上要抓取的东西，等被红框框住，就可选择下一项（会自动帮你多选），选择完成后点击Done selecting
>
> Element Preview :预览选择的元素
>
> Data Preview :预览选择数据
>
> Multiple:是否选择多个元素
>
> Regex :正则筛选规则
>
> Parent Selectors :选择父级筛选规则

​	**筛选链接筛选器**

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135519.png)

​	**选择器创建完成**

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135515.png)

可点击Sitemaps baidu ---->Selectors graph 查看创建的筛选器层级

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135503.png)

### 3 、开始筛选

点击Sitemaps baidu ---> Scrape

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135456.png)

> 请求时间与页面滚动延迟，可自定义 完成后点击 Start Scraping 会跳出一个新的页面，待整个页面自动关闭后（关闭时间与筛选的复杂程度与数据量大小有关），表示已经筛选完成

### 4、查看结果

点击Sitemaps baidu ---- >Browser

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135442.png)

点击refresh  就可查看到抓取的数据

![](https://lm-imgstore.oss-cn-shanghai.aliyuncs.com/img/20201110135354.png)

