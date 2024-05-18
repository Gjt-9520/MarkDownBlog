---
title: "JavaScript"
date: 2024-05-28
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage30.jpg?raw=true
tags: ["Web","前端","JavaScript"]
category: "学习笔记"
updated: 2024-05-29
swiper_index: 
top_group_index: 
---

# JavaScript

JavaScript(简称:JS)是一门跨平台、面向对象的脚本语言,是用来控制网页行为的,它能使网页可交互

# 引入方式

1. 内部脚本:将JS代码定义在HTML页面中
2. 外部脚本:将JS代码定义在外部JS文件中,然后引入到HTML页面中

## 内部脚本

内部脚本:将JS代码定义在HTML页面中

- JavaScript代码必须位于`<script></script>`标签之间
- 在HTML文档中,可以在任意地方,放置任意数量的`<script>`
- 一般会把脚本放置于`<body>`元素的底部,以改善显示速度

## 外部脚本

外部脚本:将JS代码定义在外部JS文件中,然后引入到HTML页面中

- 外部JS文件中,只包含JS代码,不包含`<script>`标签
- `<script>`标签不能自闭合

## 范例

```js
alert("Hello,JavaScript");
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript</title>
</head>

<body>

</body>

<!--内部脚本-->
<!-- <script>
    alert("Hello,JavaScript");
</script> -->

<!--外部脚本-->
<script src="js\JS.js"></script>

</html>
```

# 基础语法

## 书写语法

- 区分大小写:与Java一样,变量名、函数名以及其他一切东西都是区分大小写的
- 每行结尾的分号可有可无
- 注释              
单行注释:`// 注释内容`              
多行注释:`/*注释内容*/`
- 大括号表示代码块         
               
### 输出语句

- 使用`window.alert()`写入警告框
- 使用`document.write()`写入HTML输出,在浏览器显示
- 使用`console.log()`写入浏览器控制台

范例:

```js
window.alert("Hello,JavaScript");

document.write("Hello,JavaScript");

console.log("Hello,JavaScript");
```

## 变量

JavaScript中可以使用`var`关键字(variable的缩写)来声明变量                                                              
JavaScript是一门弱类型语言,变量**可以存放不同类型的值**                       

变量名的命名规则:
- 组成字符可以是任何字母、数字、下划线(_)、美元符号($)
- 数字不能开头
- 建议使用驼峰命名

特点:
- 变量作用域比较大,属于全局变量
- 可以重复定义

细节:
- ES6新增了`let`关键字来定义变量,其用法与`var`类似,但是所声明的变量,只在let关键字所在的代码块内有效,且不允许重复声明                  
- ES6新增了`const`关键字,用来声明一个只读的常量,一旦声明,常量的值就不能改变              

```js
{
    var x = 10;
    var x = 'A';
    // 输出A
    window.alert(x);
}

{
    let x = 11;
    // 输出11
    window.alert(x);

    const pi = 3.14;
    // 输出3.14
    window.alert(pi);
}

// 输出A
window.alert(x);
```

## 数据类型

JavaScript中分为:原始类型和引用类型

原始类型:
- number:数字(整数、小数、NaN(Not a Number))
- string:字符串,单双引皆可
- boolean:布尔(true/false)
- null:对象为空
- undefined:当声明的变量未初始化时,该变量的默认值是undefined

使用`typeof`运算符可以获取数据类型

范例:

```js
{// 输出number
    alert(typeof 3);
    // 输出number
    alert(typeof 3.14);

    // 输出string
    alert(typeof "A");
    // 输出string
    alert(typeof 'A');

    // 输出boolean
    alert(typeof true);
    // 输出boolean
    alert(typeof false);

    var a;
    // 输出undefined
    alert(typeof a);

    // 输出object
    alert(typeof null);
}
```

## 运算符

- 算数运算符:`+`,`-`,`*`,`/`,`%`,`++`,`--`
- 赋值运算符:`=`,`+=`,`-=`,`*=`,`/=`,`%=`
- 比较运算符:`>`,`<`,`>=`,`<=`,`!=`,`==`,`===`
- 逻辑运算符:`&&`,`||`,`!`
- 三元运算符:`条件表达式?true_value:false_value`

细节:`==`会进行类型转换,`===`不会进行类型转换

范例:

```js
{
    let a = 10;
    // 输出true
    alert(a == "10");
    
    // 输出false
    alert(a === "10");
    
    // 输出true
    alert(a === 10);
}
```

### 类型转换

- 字符串类型转为数字:将字符串面值转为数字,如果字面值不是数字,则转为NaN
- 其他类型转为boolean:               
number:0和NaN为false,其他均为true                   
string:空字符串为false,其他均为true                       
null和undefined:均为false                 

范例:

```js
{
    // 输出12
    alert(parseInt("12"));

    // 输出12
    alert(parseInt("12A45"));

    // 输出NaN
    alert(parseInt("A45"));

    if(0){ 
        // 输出false
        alert("0转换为false");
    }

    if(NaN){ 
        // 输出false
        alert("NaN转换为false");
    }

    if(-1){ 
        // 输出true
        alert("除0和NaN其他数字都转为true");
    }

    if(""){ 
        // 输出false
        alert("空字符串为false,其他都是true");
    }
        
    if(null){ 
        // 输出false
        alert("null转化为false");
    }

    if(undefined){ 
        // 输出false
        alert("undefined转化为false");
    }
}
```

## 流程控制语句

- `if...else if...else`
- `switch`
- `for`
- `while`
- `do...while`

# 函数

定义方式一:

![函数](../images/JS函数.png)

定义方式二:

![函数](../images/JS函数2.png)

细节:**函数可以传递任意个数的参数,但是根据函数的定义来接收指定个数的参数**

范例:

```js
// 函数1
function add1(a, b) {
    return a + b;
}

// 函数1调用
var result1 = add1(19, 23);
alert(result1);

// 函数2
var add2 = function(a, b) {
    return a + b;
}

// 函数2调用
var result2 = add2(11, 23);
alert(result2);
```

# 对象

## Array

Array对象用于定义数组

定义:
1. `var 变量名 = new Array(元素列表);`
2. `var 变量名 = [元素列表];`

访问:`arr[索引] = 值;`

特点:**JavaScript中的数组相当于Java中的集合,数组的长度是可变的,而JavaScript是弱类型,所以可以存储任意类型的数据**

![Array](../images/JS_Array.png)

范例:

```js
{
    // 定义数组
    var arr1 = new Array(1, 2, 3, 4, 5);
    var arr2 = [1, 2, 3];

    // 输出3
    console.log(arr1[2]);
    // 输出1
    console.log(arr2[0]);

    // 输出undefined
    console.log(arr2[10]);

    arr2[11] = 100;
    // 输出100
    console.log(arr2[11]);

    arr2[9] = true;
    arr2[8] = "A";
    // 输出(12) [1, 2, 3, empty × 5, 'A', true, empty, 100]
    console.log(arr2);

    // 遍历数组中的所有元素
    for (let i = 0; i < arr2.length; i++) {
        const element = arr2[i];
        console.log(element);
    }

    // 遍历数组中有值的元素
    arr2.forEach(function (e) {
        console.log(e);
    });
    // 或者ES6的箭头函数
    arr2.forEach((e) => {
        console.log(e);
    });

    // 将新元素添加到数组末尾
    arr2.push(23, 24, 25);
    console.log(arr2);

    // 删除数组中的元素
    // 删除0索引开始之后的2个元素
    arr2.splice(0, 2);
    console.log(arr2);
}
```

补充:箭头函数(ES6),用来简化函数定义语法

具体形式:`(...) => {...}`,如果需要给箭头函数起名字:`var XXX = (...) => {...}`


## String

## JSON

## BOM

## DOM

# 事件