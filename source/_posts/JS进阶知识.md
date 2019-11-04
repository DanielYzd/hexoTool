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

******
闭包是一个重要的JS模式，可以访问私有变量。在本例中，`createGreeter`返回一个匿名函数，这个函数可以访问参数`greeting`。在
后续的调用中，`sayHello`有权访问这个greeting参数。

```
function createGreeter(greeting) {
    return function(name) {
        console.log(greeting + ',' + name);
    }
}
const sayHello = createGreeter('你好');
sayHello('Daniel'); //你好，Daniel
```

**在更真实的场景中，咱们可以设想一个初始函数`apiConnect(apiKey)`，它返回一些使用`API key`的方法。在这种情况下，`apiKey` 只需要提供一次即可。**
```
function apiConnect(apiKey) {
    function get(route) {
        return fetch(`${route}?key=${apiKey}`);
    }
    function post(route,params) {
        return fetch(route, {
            method: 'POST',
            body: JSON.stringigy(params),
            headers:{
                'Authorization': `Bearer ${apiKey}`
            }
        })
    }
    return { get, post}
}
const api = apiConnect('my-secret-key');
//不再需要包含apiKey
api.get('http://www.example.com/get-endpoint');
api.post('http://www.example.com/post-endpoint', { name: 'Joe'});
```

### 3.解构

******
JS参数解构可以从对象中提取所需属性的常用方法
```
const obj = {
    name: 'Daniel',
    food: '食物',
}
const { name, food } = obj;
console.log(name, food); //Daniel 食物
```

如果需要取别名，可以使用如下方式：
```
const obj = {
    name: 'Daniel',
    food: '食物'
}
const { name: myName, food: myFood } = obj;
console.log(myName,myFood); // Daniel 食物
```

**解构经常用于直接用于提取传给函数的参数。如果你熟悉React，可能已经见过这个:**
```
const person = {
    name: 'Daniel',
    age: 28
}
function introduce({ name, age}) {
    console.log(`我是${name} , 今天 ${age}岁了！`);
}
introduce(person);
```

### 4.展开运算符

******
ES6的常用之一的特性就是展开(``...``),在下面的例子中, ``Math.max``不能应用于``arr``数组，因为它不将数组作为参数,但它可以将各个元素作为参数传入。展开运算符``...``可用于提取数组的各个元素。

```
const arr = [2, 4, 5, 10, -1];
console.log(...arr);
const max = Math.max(...arr);
console.log(max); // 10
```

### 5.剩余参数

******
剩余参数语法和展开语法看起来的一样的，不同的是展开语法是为了解构数组和对象；而剩余参数和展开运算符是相反的，剩余参数手机多个
参数合成一个数组。

```
function myFunc(...args) {
    console.log(args[0] + args[1]);
}
myFunc(1, 2, 3, 4); // 3
```

**rest parameters** 和 **arguments** 的区别

arguments 是伪数组，包含所有的实参
rest(剩余)参数 是标准的数组，可以使用数组的方法

### 6.数组方法

******

#### map filter reduce

JS数组方法``map`` ``filter``和``reduce``容易混淆，这些都是转换数组或返回聚合值的有用方法

**map:返回一个数组，其中每个元素都使用指定函数进行过转换。**

```
const arr = [1, 2, 3, 4, 5, 6];
const mapped = arr.map(el => el + 20);
console.log(mapped);
//[21, 22, 23, 24, 25, 26]
```

**filter:返回一个数组，只有当指定函数返回true时，相应的元素才会被包含在这个数组中。**

```
const arr = [1, 2, 3, 4, 5, 6];
const filtered = arr.filter(el => el ===2 || el ===4);
console.log(filtered);
```

**reduce:按函数中指定的值累加.**

```
const arr = [1, 2, 3, 4, 5, 6];
const reduced = arr.reduce((total, current) => total + current);
console.log(reduced);
```
>recude的用法
**语法**
```
arr.reduce(function(prev,cur,index,arr){
    ....
},init)
```

其中：
`arr` 表示原数组
`prev` 表示上一次调用回调的返回值，或者初始值`init`；
`cur` 表示当前正在处理的数组元素；
`index` 表示当前正在处理的数组元素的索引，若提供`init`值，则索引为**0**，否在索引为**1**；
`init` 表示初始值

看上去是不是感觉很复杂？没关系，只是看起来而已，其实常用的参数只有两个:`prev`和`cur`。
接下来我们跟着实例来看看具体用法吧~

先通过一个原始数组：

```
var arr = [3, 8, 4, 5, 3, 0, 9];
```
通过reduce实现以下需求，实现起来比较简洁

1. 求数组项之和

```
var sum = arr.reduce(function(prev, cur){
    return prev + cur;
},0);
```

2. 求数组的最大值

```
var max = arr.reduce((prev, cur) => Math.max(prev, cur))
```

由于未传入初始值，所以开始时prev的值为数组第一项3，cur的值为数组第二项9，取两值最大值后继续进入下一轮回调。

3. 数组去重

```
var newArr = arr.reduce(function (prev, cur) {
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[])
```

```
var newArr = arr.reduce((prev, cur)=>{
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[])

```

实现的基本原理如下：
>① 初始化一个空数组
>② 将需要去重处理的数组中的第1项在初始化数组中查找，如果找不到（空数组中肯定找不到），就将该项添加到初始化数组中
>③ 将需要去重处理的数组中的第2项在初始化数组中查找，如果找不到，就将该项继续添加到初始化数组中
>④ ……
>⑤ 将需要去重处理的数组中的第n项在初始化数组中查找，如果找不到，就将该项继续添加到初始化数组中
>⑥ 将这个初始化数组返回

其他reduce用法

* 计算对象每个值的总和

```
var result = [
    {
        subject: 'math',
        score: 88
    },
    {
        subject: 'chinese',
        score: 95
    },
    {
        subject: 'english',
        score: 80
    },
];
var sum = result.reduce((prev,{score}) => {
    return prev + score;
},0)
console.log(sum); //263
```

* 替代map(filter,find)类似

```
var arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((prev, cur, index ,arr) => {
    prev.push(cur + 1);
    return prev;
},[])
console.log(arr,sum)
```

#### find, findIndex, indexOf
  
  **find:返回与指定条件匹配的第一个实例，如果查到不会继续查找其他匹配的实例。**

  ```
  const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  const found = arr.find(el => el > 5);
  console.log(found);   //6
  ```

  **注意**，虽然5之后的所有元素都满足条件，但是 `find` 只返回第一个匹配的元素。当咱们发现匹配项并想中断`for`循环，在这种情况下，`find` 就可以派上用场了。

  **findIndex：这与find几乎完全相同，但不是返回第一个匹配元素，而是返回第一个匹配元素的索引.**
  
  ```
  const arr = ['Nick', 'Frank', 'Joe', 'Frank'];
  const foundIndex = arr.findIndex(el => el === 'Frank');
  console.log(foundIndex) //1
  ```

  **indexOf：与`findIndex`几乎完全相同，但它不是将函数作为参数，而是采用一个简单的值。 当你需要更简单的逻辑并且不需要使用函数来检查是否存在匹配时，可以使用此方法。**

  ```
    const arr = ['Nick', 'Frank', 'Joe', 'Frank'];
    const foundIndex = arr.indexOf('Frank');
    console.log(foundIndex); // 1
   ```

#### push pop shift unshift

**push:** 这是一个相对简单的方法，它将一个项添加到数组的末尾。它就地修改数组，函数本身会返回数组长度

```
let arr = [1, 2, 3, 4]
const pushed = arr.push(6)
console.log(arr); // [1, 2, 3, 4, 6]
console.log(pushed); // 5 数组长度
```

**pop:** 这将从数组中删除最后一项，同样，它在适当的位置修改数组，函数本身返回从数组中删除的项

```
let arr = [1, 2, 3, 4]
const popped = arr.pop();
console.log(arr); // [1, 2, 3]
console.log(popped); // 4
```

**shift:** 从数组中删除第一项，同样它在适当的位置修改数组。函数本身返回从数组中删除的项。

```
let arr = [1, 2, 3, 4]
const shifted = arr.shift()
console.log(arr); // [2, 3, 4]
console.log(shifted); //1
```

**unshift:** 将一个或多个元素添加到数组的开头。同样，它在适当的位置修改数组。与许多其他方法不同，函数本身返回数组的长度。

```
let arr = [1, 2, 3, 4];
const unshifted = arr.unshift(5, 6, 7);
console.log(arr); // [5, 6, 7, 1, 2, 3, 4]
console.log(unshifted); // 7
```

#### splice,slice

**splice:** 通过删除或替换现有元素和/或添加新元素来更改数组的内容，此方法会修改了数组本身。

**语法:** 

```
arrObject.splice(index,howmany,item1,....,itemX)
```

参数|描述
--|:--:
index|必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
howmany|必需。需要删除的项目数量。如果设置为0，则不会删除项目。
item1,...,itemX|可选。向数组添加新项目

**返回值：**

类型|描述
--|:--:
Array|包含被删除项目的新数组，如果有的话。

下面的代码示例的意思是：在数组的位置`1`上删除`0`各元素，并插入`b`。

```
let arr = ['a', 'c', 'd', 'e'];
arr.splice(1, 0, 'b')
console.log(arr);
```

**slice:**从指定的起始位置和指定的结束位置之前返回数组的浅拷贝。如果未指定结束位置，则返回数组的其余部分。重要的是，此方法
不会修改数组，而是返回所需的子集。

```
 let arr = ['a', 'b', 'c', 'd', 'e'];
 const sliced = arr.slice(2, 4);
 console.log(sliced); // ['c', 'd']
 console.log(arr) //  ['a', 'b', 'c', 'd', 'e']
```

#### sort

**sort:**根据提供的函数对数组进行排序。这个方法就地修改数组。如果函数返回负数或0，则顺序保持不变。如果返回正数，则交换元素顺序。

```
let arr = [1, 7, 3, -1, 5, 7, 2];
const sorter = (firstEl, secondEl) => firstEl - secondEl;
arr.sort(sorter);
cosnole.log(arr); // [-1, 1, 2, 3, 5, 7, 7]
```

### Generators(生成器)

******

生成器是一种特殊的行为,实际上是一种设计模式，咱们通过`next()`方法来遍历一组有序的值。想象一下，例如使用遍历器对数组`[1, 2, 3, 4, 5]`进行遍历。
第一次调用`next()`方法返回`1`，第二次调用`next()`返回`2`，以此类推。当数组的所有值都返回后,调用`next()`方法将返回`nul`l或`false`，或其他可能的值用来
表示数组中的所有元素都已遍历完毕。

```
function* greeter() {
    yield 'Hi';
    yield 'How are you?';
    yield 'Bye';
}
const greet = greeter();
console.log(greet.next().value);
// 'Hi'
console.log(greet.next().value);
// 'How are you?'
console.log(greet.next().value);
// 'Bye'
console.log(greet.next().value);
// undefined
```

使用生成器生成无限个值：

```
function* idCreator(){
    let i = 0;
    while (true)
      yield i++;
}
const ids = idCreator();
console.log(ids.next().value);
// 0
console.log(ids.next().value);
// 1
console.log(ids.next().value);
// 2
console.log(ids.next().value);
// ...
```

### 严格相等运算符(===)与相等运算符(===)

******
大家一定要知道JS中的严格相等运算符(`===`)和相等运算符(`==`)之间的区别。`==`运算符在比较值之前会进行类型转换，而`===`运算符在比较之前不会进行任何类型转换。

```
console.log(0 == '0');
// true
console.log(0 === '0');
// false
```

### 对象比较

******
JS新手经常所犯的错误是直接比较对象。变量指向内存中对象的引用，而不是对象本身!实际比较它们的一种方法是将对象转换为JSON字符串。这有一个缺点：对象属性顺序不能
保证，比较对象的一种更安全的方法是引入专门进行深度对象比较的库(例如,[lodash中isEqual](https://lodash.com/docs/4.17.15#isEqual))

下面的对象看起来是相等的，但实际上它们指向不同的引用。

```
const joe1 = { name: 'Daniel' };
const joe2 = { name: 'Daniel' };
console.log(joe1 === joe2); // false
```

相反，下面的计算结果为`true`，因为一个对象被设置为与另一个对象相等，因此指向相同的引用(内存中只有一个对象)。

```
const joe1 = { name: 'Daniel' };
const joe2 = joe1;
console.log(joe1 === joe2); //true
```

### 回调函数

******
很多人都被JS回调函数吓倒了。他们很简单，举个例子，`console.log`函数作为回调函数传递给`myFunc`,它在`setTimeout`完成时进行。

```
function myFunc(text, callback){
    setTimeout(function() {
        console.log(text);
    }, 2000);
}
myFunc('Hello world!',console.log);
```

### Promises

******
一旦你理解了JS回调，很快就会发现自己陷入了"回调地狱"中。这个时候可以使用`promise`,将一部逻辑包装在`promise`中，成功时`resolve`或在失败时`reject`
使用`then`来处理成功的情况，使用`catch`来处理失败异常

```
const myPromise = new Promise(function(res, rej) {
    setTimeout(function(){
        if(Math.random() < 0.9) {
            return res('Hooray!');
        } else {
            return rej('Oh no!');
        }
    },1000);
})
myPromise.then(function(data) {
    console.log('Success' + data);
})
.catch(function(err) {
    console.log('Error' + err);
});
```

### Async/Await

******
在掌握了promise的用法后，你可能也会喜欢`async await`,它是一种基于`promise`的"语法糖"。在下面的示例中，咱们创建了一个`async`函数，并`awit greeter`。

```
const greeter = new Promise((res,rej) => {
    setTimeout(() => res('Hello world!'),2000);
})
async function myFunc() {
    const greeting = await greeter;
    console.log(greeting);
}
myFunc();
// 'Hello world!'
```
