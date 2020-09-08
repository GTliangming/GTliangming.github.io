---
title: WebPack的配置(一)
date: 2020-09-08 15:00:00
tags: WebPack
cover: 
# 版权信息
# copyright_author: xxxx
# copyright_author_href: https://xxxxxx.com
# copyright_url: https://xxxxxx.com
# copyright_info: 此文章版權歸xxxxx所有，如有轉載，請註明來自原作者 
---

### 一、webpack.config.js

#### 1、打包模式 mode

在打包时配置mode是为了告知 webpack 使用相应模式的内置优化

mode的模式分别有三种：we

- Production   生产环境打包 默认值
- Development  开发环境打包
- None  不使用任何默认优化选项

Production与Development的区别：

- 生产模式会把打包好后的代码进行压缩，可阅读性不好，但是代码体积小
- 开发模式不会压缩代码，可阅读性好，但是代码体积大
- 开发模式一些没有依赖的方法 变量 文件会保留，生产模式会移除

#### 2、入口  entry

入口起点告诉 webpack 从哪里开始，并根据依赖关系图确定需要打包的文件内容

`entry`接受三种形式的值：字符串，数组和对象

- 对象

  基本形式为：

  ```js
  entyr:{
  	<key>:<value>
  }
  // 每一个属性对都对应一个入口文件，适合多页面打包配置
  ```

    

   **key的值可以为比较简单的字符串，此时key对应出口文件output.filename配置中的[name]变量**

  ```js
  //例如 
  entry: {
      'app-entry': './app.js'
  },
  output: {
      path: './dist',
      filename: '[name].js'
  }
  // 则对./app.js打包完成后生成的文件是在dist文件夹下的app-entry.js文件
  ```

  

  **key的值也可以为路径字符串，此时webpack会自动生成路径目录，并将路径的最后作为[name]**

  ```js
  //例如
  entry: {
      '/example/entry': './deep-app.js',
  },
  output: {
      path: './dist',
      filename: '[name].js'
  }
  // 则对./app.js打包完成后生成的文件是在dist文件夹下的example文件夹下的entry.js文件
  ```

  **value如果是字符串，那就必须是合理的node require函数参数字符串。**

  **比如文件路径：'./app.js'       (require('./app.js'))；**

  **比如安装的npm模块：'lodash'       (require('lodash'))**

  ```js
  entry: {
      'my-lodash': 'lodash'
  },
  output: {
      path: './output',
      filename: '[name].js'
  }
  ```

  **value如果是数组，则数组中元素需要是上面描述的合理字符串值。数组中的文件一般是没有相互依赖关系的，但是又处于某些原因需要将它们打包在一起**

  ```js
  entry: {
      vendor: ['jquery', 'lodash']
  }
  ```

- 字符串

  `entry: './app.js'`  等价于下面的对象形式：

  ```js
  entry: {
      main: './app.js'
  }
  ```

- 数组

  `entry: ['./index.js', 'lodash']` 等价于下面的对象形式：

  ```js
  entry: {
      main: ['./app.js', 'lodash']
  }
  ```

#### 3、出口 output

告诉webpack怎样存储输出结果以及存储到哪里,指定每个输出文件的名称。在这里不能指定为绝对路径！

基本形式：

```js
output:{
	path:'',
  publicPath:'',
  filename:''
}
// path:所有输出文件的目标路径;打包后文件在硬盘中的存储位置。
// publicPath:输出解析文件的目录，指定资源文件引用的目录 ，打包后浏览器访问服务时的 url 路径中通用的一部分。
```

- Path 仅仅告诉Webpack结果存储在哪里

- publicPath 用于在生产模式下更新内嵌到css、html文件里的url值

  **output.publicPath 是很重要的选项。如果指定了一个错误的值，则在加载这些资源时会收到 404 错误**

注 ：在使用html-webpack-plugin 生成index.html时，publicPath是可以不用配置的。

​	**output的输出hash值**

- [hash] 

  每个文件的hash都一样，文件的hash为打包的hash。

- [chunkhash] 

  使用文件的hash都不一样，也与打包hash不一样

  

#### 4、 模块

在模块化编程中，开发者将程序分解成离散功能块，并称之为模块。每个模块具有比完整程序更小的接触面，使得校验、调试、测试轻而易举。 精心编写的模块提供了可靠的抽象和封装界限，使得应用程序中每个模块都具有条理清楚的设计和明确的目的。

- 配置 Loader

  rules 配置模块的读取和解析规则，通常用来配置 Loader。其类型是一个数组，数组里每一项都描述了如何去处理部分文件。 配置一项 `rules` 时大致通过以下方式：

  1. 条件匹配：通过 test 、 include 、 exclude 三个配置项来命中 Loader 要应用规则的文件。
  2. 应用规则：对选中后的文件通过 `use` 配置项来应用 Loader，可以只应用一个 Loader 或者按照从后往前的顺序应用一组 Loader，同时还可以分别给 Loader 传入参数。
  3. 重置顺序：一组 Loader 的执行顺序默认是从右到左执行，通过 `enforce` 选项可以让其中一个 Loader 的执行顺序放到最前或者最后

```js
module: {
  rules: [
    {
      // 命中 css 文件
      test: /\.css$/,
      // 用 babel-loader 转换 css 文件
      // ?cacheDirectory 表示传给 babel-loader 的参数，用于缓存 babel 编译结果加快重新编译速度
      use: ['babel-loader?cacheDirectory'],
      // 只命中src目录里的js文件，加快 Webpack 搜索速度
      include: path.resolve(__dirname, 'src')
    },
    {
      // 命中 SCSS 文件
      test: /\.scss$/,
      // 使用一组 Loader 去处理 SCSS 文件。
      // 处理顺序为从后到前，即先交给 sass-loader 处理，再把结果交给 css-loader 最后再给 style-loader。
      use: ['style-loader', 'css-loader', 'sass-loader'],
      // 排除 node_modules 目录下的文件
      exclude: path.resolve(__dirname, 'node_modules'),
    },
    {
      // 对GIF、PNG等的文件采用 file-loader 加载
      test: /\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/,
      use: ['file-loader'],
    },
  ]
}
```

当 Loader 需要传入很多参数时，你还可以通过一个 对象 来描述

```js
use: [
  {
    loader:'babel-loader',
    options:{
      cacheDirectory:true,
    },
    // enforce:'post' 的含义是把该 Loader 的执行顺序放到最后
    // enforce 的值还可以是 pre，代表把 Loader 的执行顺序放到最前面
    enforce:'post'
  },
]
```

其中的test、include、exclude都可以传入数组，数组里的每项之间是或的关系，即文件路径符合数组中的任何一个条件就会被命中

- 配置 noParse

`noParse` 配置项可以让 Webpack 忽略对部分没采用模块化的文件的递归解析和处理，这样做的好处是能提高构建性能。 原因是一些库例如 jQuery 、ChartJS 它们庞大又没有采用模块化标准，让 Webpack 去解析这些文件耗时又没有意义。

`noParse` 是可选配置项，类型需要是 `RegExp`、`[RegExp]`、`function` 其中一个。

```js
// 使用正则表达式
noParse: /jquery|chartjs/

// 使用函数，从 Webpack 3.0.0 开始支持
noParse: (content)=> {
  // content 代表一个模块的文件路径
  // 返回 true or false
  return /jquery|chartjs/.test(content);
}
//注意被忽略掉的文件里不应该包含 import 、 require 、 define 等模块化语句，不然会导致构建出的代码中包含无法在浏览器环境下执行的模块化语句。
```

- 配置 parser

因为 Webpack 是以模块化的 JavaScript 文件为入口，所以内置了对模块化 JavaScript 的解析功能，支持 AMD、CommonJS、SystemJS、ES6。 `parser` 属性可以更细粒度的配置哪些模块语法要解析哪些不解析，和 `noParse` 配置项的区别在于 `parser` 可以精确到语法层面， 而 noParse 只能控制哪些文件不被解析。

```js
module: {
  rules: [
    {
      test: /\.css$/,
      use: ["style", "css", "postcss"],
      parser: {
        amd: false, // 禁用 AMD
        commonjs: false, // 禁用 CommonJS
        system: false, // 禁用 SystemJS
        harmony: false, // 禁用 ES6 import/export
        requireInclude: false, // 禁用 require.include
        requireEnsure: false, // 禁用 require.ensure
        requireContext: false, // 禁用 require.context
        browserify: false, // 禁用 browserify
        requireJs: false, // 禁用 requirejs
      }
    },
  ]
}
```


