---
title: JS进阶知识
date: 2019-10-30 14:39:05
tags: 
    - ES6
    - JS
categories: 技术文章
---
> 作者：前端小智
> 来源：掘金

**本文介绍 JS 比较重要的12个概念，但绝对不是说 JS开发人员只需要知道这些就可以了。**

### 1.变量赋值（值 vs 引用）

******
理解 JS 如何给变量赋值可以帮助我们减少一些不必要的 bug。相反，如果，不理解这一点，可能很容易地编写被无意中更改值的代码

JS 总是按照值来给变量赋值。 **这一部分非常重要**：当指定的值是 JavaScript 的五种基本类型之一（即 `Boolean`，`null`，`undefined`，`String` 和 `Number`）时，分配是实际值。 但是，当指定的值是 `Array`，`Function`或`Object`时，将内存中对象的引用地址赋值给变量。
在以下代码段中，使用 `var1` 对 `var2` 进行赋值。 由于`var1`是基本类型（String），因此 `var2` 的值等于 `var1` 字符中值，但 `var1` 和 `var2` 之间是独立的变量，不会影响彼此。 因此，重新赋值`var2`对`var1`没有影响。

```
    let var1 = '小智;
    let var2 = var1;
    var2 = '王大冶';
    console.log(var1); // 小智
    console.log(var2); // 王大冶
```

接着，与对象赋值进行比较。
```
let var1 = { name: '小智' }
let var2 = var1;
var2.name = '王大冶';
console.log(var1); // {name: "王大冶"}
console.log(var2); // {name: "王大冶"}
```

### 2.闭包

https://juejin.im/post/5db7826df265da4d26043291#heading-1
