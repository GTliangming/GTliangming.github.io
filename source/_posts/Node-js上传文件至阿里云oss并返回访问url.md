---
title: Node.js上传文件至阿里云oss并返回访问url
date: 2020-12-09 13:47:44
tags: node.js OSS
cover:
categories: node.js
---

## Node.js上传文件至阿里云oss并返回访问url

### 一、什么是阿里云oss

[阿里云官网 ：https://www.aliyun.com](https://www.aliyun.com/)

阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。它具有与平台无关的RESTful API接口，能够提供99.999999999%（11个9）的数据可靠性和99.99%的服务可用性。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。

您可以使用阿里云提供的API/SDK接口或者OSS迁移工具轻松地将海量数据移入或移出阿里云OSS。数据存储到阿里云OSS以后，您可以选择标准类型（Standard）的阿里云OSS服务作为移动应用、大型网站、图片分享或热点音视频的主要存储方式，也可以选择成本更低、存储期限更长的低频访问类型（Infrequent Access）和归档类型（Archive）的阿里云OSS服务作为不经常访问数据的备份和归档。

[阿里云OSS官方介绍地址：https://help.aliyun.com/document_detail/31947.html](https://help.aliyun.com/document_detail/31947.html)



具体的怎么开通阿里云oss、新建Bucket，创建accessKeyId和accessKeySecret等，百度有很多

这里提供一个参考链接 ：https://blog.csdn.net/qq_15980721/article/details/102490019

### 二、前端表单提交+后端node请求

需要安装node的阿里云oss插件`ali-oss`  与上传所需的`multer`、`co`

`npm install ali-oss --save`

` npm install --save multer`

`npm install co`

> Multer 是一个 node.js 中间件，用于处理 `multipart/form-data` 类型的表单数据，它主要用于上传文件。它是写在 [busboy](https://github.com/mscdex/busboy) 之上非常高效。
>
> **注意**: Multer 不会处理任何非 `multipart/form-data` 类型的表单数据。
>
> [传送门](https://www.npmjs.com/package/multer)

```js

var express = require("express")
var fs = require("fs")
var router = express.Router();

// 引入插件
var OSS = require('ali-oss');
const multer=require("multer");
const co = require("co");

// 设置阿里云连接参数
var client = new OSS({
  // 你的oss的源地址、可以在阿里云的oss对象存储内的bucket概览内看到
  region:'oss-cn-beijing', 
	// 这里填入刚生成的id和key
  accessKeyId:'****************',
  accessKeySecret:'*************8'
})
// 定义访问的bucket
var ali_oss_bucket ='bucket-lm-demo';

// 设置multer
const upload = multer({ 
  dest: "public/image/", //此路径为本地存储的路径，以后会删除
}).single("avatar");


router.post('/getImg',upload, async (req, res) => {
    console.log(1111111,req.file)
    // 将文件名改为了 例如 2020-09-15-文件名前六位+文件后缀
    var fileName = setFileName(req.file.originalname)
    console.log(2222,fileName)
		// 文件重命名 
    fs.rename('public/image/'+req.file.filename,'public/image/'+fileName,(err)=>{
        if(err){
          res.json({
            code:400,
            msg:"文件上传失败",
            error:JSON.stringify(err)
          })
        }else{
          
          // 此部分为上传至阿里云
          
          // 本地图片的路径
          var localFile = 'public/image/'+fileName;
          // 上传后保存的路径
          var key = 'image/'+fileName;
          
          co(async function(){
            client.useBucket(ali_oss.bucket);
            var result = await client.put(key,localFile);
            console.log(33333,result)
            // 上传完成后删除本地文件
            fs.unlinkSync(localFile);
            res.json({
              code:200,
              msg:"success",
              // 将网址返回
              resultUrl:result.res.requestUrls
            })

          }).catch(function(err){
            fs.unlinkSync(localFile);
            console.log(444,err)
            res.json({
              code:401,
              msg:"文件上传失败",
              error:JSON.stringify(err)
            })
          })  
        }
    })

   
})
router.get("/",(req,res,next)=>{
  res.render("index")
})

// 设置文件名的方法
function setFileName (val){
  const index = val.indexOf(".");
  const actureName =val.slice(0,index)
  const actureType =val.slice(index,val.length)
  const Time = setTime();
  if(actureName.length>=6){
    return Time + actureName.slice(0,6)+actureType;
  }else{
    return Time+actureName+actureType;
  }
}
function setTime(){
  const dateStr = new Date();
  var year = dateStr.getFullYear().toString().padStart(4, "0");
  var month = (dateStr.getMonth() + 1).toString().padStart(2, "0");
  var day = dateStr.getDate().toString().padStart(2, "0");
  return `${year}-${month}-${day}-`;
}
module.exports = router;
```

前端请求页面

```js
<div id="Box">
	<form action="http://127.0.0.1:3001/uploads/getImg" method="post" enctype="multipart/form-data">
      <div>
          <input type="file" name="avatar" accept="image/*">
        </div>
        <input type="submit" value="上传">
      </form>
    <h2>以下为预览效果：</h2>
</div>
```

### 三、 前端antd的upload上传组件实现上传

​	antd是在react内很好用的一个组件库，不仅好看还好用，这部分就来使用它来上传至阿里云OSS

**前端**

```js

import React from 'react'
// antd的所用的组件
import { Upload, message, Input, Button } from 'antd';
// 引入阿里云插件
import oss from 'ali-oss';
// import moment from 'moment';
import { setFileName } from '../utils';
import './tooss.css';
// 点击复制到粘贴板的一个react插件
import CopyToClipboard from 'react-copy-to-clipboard';
import axios from "axios"

function getBase64(img:any, callback:any) {
  const reader = new FileReader();
  reader.addEventListener('load', () => callback(reader.result));
  reader.readAsDataURL(img);
}

const client = (self:any) => {
  const {token} = self
  // 当时使用的插件版本为5.2
  /*
  return new oss.Wrapper({
    accessKeyId: token.access_key_id,
    accessKeySecret: token.access_key_secret,
    region: '', //
    bucket: '',//
  });
  */
  // 2018-12-29更新
  // ali-oss v6.x版本的写法
  return new oss({
    accessKeyId: token.access_key_id,
    accessKeySecret: token.access_key_secret,
    region: token.OSS_ENDPOINT, //
    bucket: token.OSS_BUCKET,//
  });
}

// 上传文件的路径，使用日期命名文件目录 文件的名字可以自定义这个部分随意
const uploadPath = (path:any, file:any) => {
  const name = setFileName(file.name)
  // return `${moment().format('YYYYMMDD')}/${file.name.split(".")[0]}.${file.type.split("/")[1]}`
  return  `image/${name}`
}

// 上传方法
const UploadToOss = (self:any, path:any, file:any) => {
  const url = uploadPath(path, file)
  return new Promise((resolve, reject) => {
    client(self).multipartUpload(url, file,{}).then(data => {
      resolve(data);
    }).catch(error => {
      reject(error)
    })
  })
}

interface ExampleStatus {
  loading:boolean;
  fileList: any[];
  token:any;
  UrlList:any;
  copied:boolean;
}
class UploadToOSS extends React.Component<{},ExampleStatus>{
  constructor(props:any){
    super(props);
    this.state = {
      loading: false,
      token: {
        access_key_id: '***************', // oss的key_id
        access_key_secret: '****************', // oss的secret
        OSS_ENDPOINT: 'oss-cn-beijing',  // 自己oss服务器的源地址// 
        OSS_BUCKET: '*********', // 自己要上传的oss服务器的bucket的名字
      },
      UrlList:[],// 获取阿里云oss内所有图片的链接的一个数组
      fileList:[
        {
          uid: '-1',
          name: 'image.png',
          status: 'done',
          url: '',
        }
      ],
      copied:false
    };
  }
  async componentDidMount(){
   	await this.getUploadInfo();
    console.log(11111,this.state.UrlList)
  }
	// 这个方法是一个我自己node写的一个获取阿里云oss内指定文件夹下的所有的文件，目的就是能让我一次性获取我存的所有图片的链接，实现一个展示的作用
  getUploadInfo= async ()=>{
    // const HOST = "http://localhost:3003/uploads2/getImg";
    const HOST = "http://xxx.xxx.xxx.xxx:3003/toOSS/getImg"
   await axios.get(HOST).then((result:any)=>{
        if(result.data.code===400){
          const data = result.data.result.objects;
          this.setState({
            UrlList:data
          })
        }
    })
  }
  handleChange = (info:any) => {
    this.setState({
      fileList:info.fileList
    })
    if (info.file.status === 'uploading') {
      this.setState({ loading: true });
      return;
    }
    if (info.file.status === 'done') {
      // Get this url from response in real world.
      getBase64(info.file.originFileObj, (imageUrl:any) => this.setState({
        loading: false,
      }));
    }
  }

  beforeUpload = (file:any) => {
    const isJPG = file.type === 'image/jpeg';
    if (!isJPG) {
      message.error('You can only upload JPG file!');
      return false
    }
    const isLt2M = file.size / 1024 / 1024 < 2;
    if (!isLt2M) {
      message.error('Image must smaller than 2MB!');
      return false
    }
    let reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onloadend = () => {

      // 使用ossupload覆盖默认的上传方法
      UploadToOss(this.state, '上传路径oss配置信息', file).then((data:any) => {
        console.log(2222,data.res)
        if(data.res.status===200){
          this.getUploadInfo()
        }else{
          message.error("上传失败!",1)
        }
        
      })
    }
    return false; // 不调用默认的上传方法
  }
  render() {
    const {fileList,UrlList} = this.state
    return (
      <div className="uploadBox">
        <h2>小梁图片上传 ^_^</h2>
        <Upload
          name="avatar"
          listType="picture-card"
          className="avatar-uploader"
          beforeUpload={this.beforeUpload}
          onChange={this.handleChange}
          fileList={fileList}
        >
          {fileList.length < 5 && '+ 点击上传'}
        </Upload>
        <ul className="urlList">
          {UrlList.map((item:any,index:number)=>{
              return(
                <li key={index} className="liItem">
                  <span className="itemspan">{item.name}:</span>
                  <Input type="text"
                    id={`item${index}`}
                    value={item.url} 
                    className="item" 
                    onChange={()=>this.setState({copied:false})}/>
                  <CopyToClipboard text={item.url}  // 这里实现一个点击将文本复制到粘贴板的作用
                    onCopy={()=>{
                      message.success("复制成功!",1)
                      this.setState({copied:true}
                      )}}>
                    <Button type="primary">点击复制</Button>
                  </CopyToClipboard> 
                </li>
              )
          })}
        </ul>
      </div>
    );
  }
}
export default UploadToOSS;

```

后端node （实现获取oss指定文件夹下的所有文件）

```js
var express = require('express');
var router = express.Router();
const OSS = require('ali-oss');
const client = new OSS({
  accessKeyId: '****************', 
  accessKeySecret: '***************',
  region: 'oss-cn-beijing', 
  bucket: '***********',
})

// 这里的参数有4个 我只做了数量的限制 具体查看 
// https://help.aliyun.com/document_detail/111389.html?spm=a2c4g.11186623.2.10.644e898eFaa3Cr#concept-jjz-wxh-dhb 
router.get('/getImg',async (req, res, next)=>{
  const result = await client.list({
    "max-keys":20
  })
  console.log(22222,result)
  if(result.res.status===200){
    res.json({
      code:400,
      msg:"20条获取成功！",
      result:result
    })
  }else{
    res.json({
      code:401,
      msg:"获取失败！",
    })
  }
})
router.get("/",(req,res,next)=>{
    res.render("index2")
  })
module.exports = router;

```

​	

### 四、问题解决

#### 1、上传后访问图片链接直接下载而非访问图片

当我们把图片上传到阿里云oss后，会得到此图片的链接，但是呢，当我们在浏览器页面打开这个链接，竟然是直接下载了这个图片，What ？。是不是很难受！所以我们就要解决这个问题（在标签内使用是可以滴）

————————>我找了很多 配置域名，请求头，都么有解决我的问题，然后我就发现了这个--`PicGo`真香

[安装教程](https://www.jianshu.com/p/9d91355e8418)，

#### 2、上传报错: You have no right to access this object because of bucket acl

https://blog.csdn.net/iteye_19045/article/details/106721450

### 四、后续可以改进的地方

我们这样上传，可以实现文件上传，但是如果再加上上传的进度条，以及获取上传了的所有的图片以及文件呢？

文件上传进度条可参考：https://github.com/berwin/aliyun-oss-upload-stream

获取已经上传的参考：**参考SDK [阿里云ossNode.js](https://help.aliyun.com/document_detail/32068.html?spm=a2c4g.11186623.6.1262.6fdf64edKkYqlU)**

