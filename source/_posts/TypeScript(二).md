---
title: TypeScript(一)
date: 2020-09-08 16:30:00
tags: TypeScript
cover: 
categories: TypeScript
---

# TypeScript



## 1、TypeScript的函数



- **TypeScript的可选参数**

  在 TypeScript 函数里，如果我们定义了参数，则我们必须传入这些参数，除非将这些参数设置为可选，可选参数使用问号标识 ？。

  ```typescript
  function setName1(bigTitle1:string, smallTitle1:string){//不设置可选参数
      return "大标题："+bigTitle1+"  小标题："+smallTitle1
    }
    function setName2(bigTitle2:string, smallTitle2?:string){//设置可选参数
      return "大标题："+bigTitle2+"  小标题："+smallTitle2
    }
    console.log(setName1("哈哈1","嘿嘿1"))
    console.log(setName1("哈哈2"))//typescript编译时就会报错
    console.log(setName2("哈哈1","嘿嘿1"))
    console.log(setName2("哈哈2"))//不会报错，可以正常编译通过，但是缺少的参数会被赋值为undefined
  ```

- **TypeScript默认参数**

  可以设置参数的默认值，这样在调用函数的时候，如果不传入该参数的值，则使用默认参数

  ```typescript
  function function_name(param1[:type],param2[:type] = default_value) { 
    
  }
  //例如
  function addNum(num1:number,num2:number=0.5){
    console.log(num1+num2)
  }
  addNum(1,2);//输出3
  addNum(2);//输出2.5
  ```

- **TypeScript的剩余参数**

  有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。

  剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入

  ```typescript
  function setName(firstName: string, ...restOfName: string[]) {
      return firstName + " " + restOfName.join(" ");
  }
  let setName1 = setName("Joseph", "Samuel", "Lucas", "MacKinzie");
  
  //例如
  function addNumbers(...nums:number[]) {  
      var i;   
      var sum:number = 0; 
      
      for(i = 0;i<nums.length;i++) { 
         sum = sum + nums[i]; 
      } 
      console.log("和为：",sum) 
   } 
   addNumbers(1,2,3) 
   addNumbers(10,10,10,10,10)
  ```



## 2、TypeScript的Number对象

- **Number 对象属性**

|       属性        | 属性描述                                                     |
| :---------------: | ------------------------------------------------------------ |
|     MAX_VALUE     | 可表示的最大的数，MAX_VALUE 属性值接近于 1.79E+308。大于 MAX_VALUE 的值代表 "Infinity" |
|     MIN_VALUE     | 可表示的最小的数，即最接近 0 的正数 (实际上不会变成 0)。最大的负数是 -MIN_VALUE，MIN_VALUE 的值约为 5e-324。小于 MIN_VALUE ("underflow values") 的值将会转换为 0 |
|        NAN        | 非数字值（Not-A-Number）                                     |
| NEGATIVE_INFINITY | 负无穷大，溢出时返回该值。该值小于 MIN_VALUE。               |
| POSITIVE_INFINITY | 正无穷大，溢出时返回该值。该值大于 MAX_VALUE。               |
|     prototype     | Number 对象的静态属性。使您有能力向对象添加属性和方法。      |
|    constructor    | 返回对创建此对象的 Number 函数的引用。                       |

- **Number对象方法**

| 方法             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| toExponential()  | 把对象的值转换为指数计数法。                                 |
| toFixed()        | 把数字转换为字符串，并对小数点指定位数                       |
| toLocaleString() | 把数字转换为字符串，使用本地数字格式顺序。                   |
| toPrecision()    | 把数字格式化为指定的长度。                                   |
| toString()       | 把数字转换为字符串，使用指定的基数。数字的基数是 2 ~ 36 之间的整数。若省略该参数，则使用基数 10。 |
| valueOf()        | 返回一个 Number 对象的原始数字值。                           |

```typescript
//toExponential()
var num1:number = 12.375;
var cNum = num1.toExponential();
console.log(cNum);
//toFixed()
var num2:number = 18.656;
console.log(num2.toFixed());
console.log(num2.toFixed(1));
console.log(num2.toFixed(2));
//toLocaleString()
var num3:number = 19.33;
console.log(num3.toLocaleString());
//toPrecision()
var num4 = new Number(7.123456); 
console.log(num4.toPrecision());  // 输出：7.123456 
console.log(num4.toPrecision(1)); // 输出：7
console.log(num4.toPrecision(2)); // 输出：7.1
//toString()
var num5 = new Number(10); 
console.log(num5.toString());  // 输出10进制：10
console.log(num5.toString(2)); // 输出2进制：1010
console.log(num5.toString(8)); // 输出8进制：12
//valueOf()
var num6 = new Number(12); 
console.log(num6.valueOf()); // 输出：10
```


