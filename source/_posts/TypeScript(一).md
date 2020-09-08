---
title: TypeScript(一)
date: 2020-09-08 16:30:00
tags: TypeScript
cover: http://www.lmwebs.top/img/nanan.jpg
# 版权信息
# copyright_author: xxxx
# copyright_author_href: https://xxxxxx.com
# copyright_url: https://xxxxxx.com
# copyright_info: 此文章版權歸xxxxx所有，如有轉載，請註明來自原作者 
---

# TypeScript



## 1、什么是TypeScript

typescript是Javascript的一个超集，它也支持ECMAScript 6的标准。

TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

> typescript与javascript的区别
>
> typescript是Javascript的超集，扩展了Javascript的语法，因此现有的js代码可以与ts一起工作而无需任何修改，ts通过类型注解提供编译时的静态类型检查。
>
> ts可处理已有的js代码，并且只对其中的ts代码进行编译

**TypeScript 转换为 JavaScript 过程如下图：**

<img src="https://www.runoob.com/wp-content/uploads/2019/01/typescript_compiler.png" />



##   2、TypeScript的安装

***一般使用.ts作为TypeScript文件的后缀名***

### npm安装

使用``npm install -g typescript`` 来进行安装

安装完成后使用``tsc -v ``来进行查看安装是否成功和查看版本



##  3、TypeScript的基础语法

#### 空白和换行
TypeScript 会忽略程序中出现的空格、制表符和换行符。

空格、制表符通常用来缩进代码，使代码易于阅读和理解。

#### TypeScript 区分大小写
TypeScript 区分大写和小写字符。

#### TypeScript 与面向对象

面向对象是一种对现实世界理解和抽象的方法。

TypeScript 是一种面向对象的编程语言。

> 1、先编写一段ts代码，定义一个luckboy类，它具有一个私有属性方法name

>2、创建一个这个类的实例newboy来继承这个类的属性和方法
>
>3、用这个实例调用类里面的name方法

```typescript
class  lackBoy {
    name():void{
        console.log("luckBoy");
    }
}  
var newboy = new lackBoy;
newboy.name();
```

> 4、使用tsc来对这个ts代码进行转换，转换为js代码(同级目录下出现同名的js文件)

>5、查看这个js文件,发现创建实例与调用方法没有区别，只是在类的部分有了变化

```js
var lackBoy = /** @class */ (function () {
    function lackBoy() {
    }
    lackBoy.prototype.name = function () {
        console.log("luckBoy");
    };
    return lackBoy;
}());
var newboy = new lackBoy;
newboy.name();
```

> 6、使用node运行这个js文件，会发现控制台输出luckboy

## 4、TypeScript的基本类型

**TypeScript包含的所有数据类型如下**

|   数据类型    | 关键字    | 描述                                                         |
| :-----------: | :-------- | :----------------------------------------------------------- |
|   任意类型    | any       | 声明为 any 的变量可以赋予任意类型的值。                      |
|   数字类型    | number    | 双精度 64 位浮点值。它可以用来表示整数和分数。               |
|  字符串类型   | string    | 一个字符系列，使用单引号（**'**）或双引号（**"**）来表示字符串类型。反引号（**`**）来定义多行文本和内嵌表达式 |
|   布尔类型    | boolean   | 表示逻辑值：true 和 false。                                  |
|   数组类型    | 无        | 声明变量为数组。                                                                                                       1、在元素类型后面加上[] let arr: number[] = [1, 2];                                          2、或者使用数组泛型 let arr: Array<number> = [1, 2]; |
|   元组类型    | 无        | 元组类型用来表示已知元素数量和类型的数组，<span style="color:red">各元素的类型不必相同，对应位置的类型需要相同</span>                                                                                                      let x: [string, number];                                                                                                x = ['Runoob', 1];    // 运行正常                                                                                        x = [1, 'Runoob'];    // 报错                                                                       console.log(x[0]);    // 输出 Runoob |
|   枚举类型    | enum      | 枚举类型用于定义数值集合。                                                                                             enum Color {Red, Green, Blue};                                                                                   let c: Color = Color.Blue;                                                                            console.log(c);    // 输出 2 |
|   void类型    | void      | 用于标识方法返回值的类型，表示该方法没有返回值。             |
|   null类型    | null      | 表示对象值缺失                                               |
| undefined类型 | undefined | 用于初始化变量为一个未定义的值                               |
|   never类型   | never     | never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。 |

#### Any类型

 任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型，它常用于以下三种情况。

1. 变量的值会动态改变时，比如来自用户的输入，任意值类型可以让这些变量跳过编译阶段的类型检查

2. 改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查

   ```js
   //运行时并不会检查所调用的方法是否存在
   let x: any = 4;
   x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
   x.toFixed();    // 正确
   ```

3. 定义存储各种类型数据的数组时

   ```js
   let arrayList: any[] = [1, false, 'fine'];
   arrayList[1] = 100;
   ```



##  5 、TypeScript的变量声明

**1.TypeScript 变量的命名规则：**

- 变量名称可以包含数字和字母。
- 除了下划线 **_** 和美元 **$** 符号外，不能包含其他特殊字符，包括空格。
- 变量名不能以数字开头。

变量使用前必须先声明，我们可以使用 var 来声明变量。

**2.声明TypeScript的变量一般有四种方式**

1. 声明变量的类型及初始值：var [变量名] : [类型] = 值;

   ```typescript
   var typeName:string ="typescript"
   ```

2. 声明变量的类型，但没有初始值，变量值会设置为 undefined

   ```typescript
   var userNum : number; 
   ```

3. 声明变量并初始值，但不设置类型类型，该变量可以是任意类型

   ``` typescript
   var userID = 888;
   ```

4. 声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined

   ```typescript
   var userSize;
   ```

<p style="color:red">TypeScript 遵循强类型，如果将不同的类型赋值给变量会编译错误</p>

**3.TypeScript变量作用域**

- **全局作用域** − 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用。
- **类作用域** − 这个变量也可以称为 **字段**。类变量声明在一个类里头，但在类的方法外面。 该变量可以通过类的对象来访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
- **局部作用域** − 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。