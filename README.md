# ES6+新特性学习指南

## 概述

ES全称ECMAScript，ECMAScript是ECMA制定的标准化脚本语言。目前JavaScript使用的ECMAScript版本为[ECMAScript-262](https://www.ecma-international.org/ecma-262/)。

ECMAScript 标准建立在一些原有的技术上，最为著名的是 JavaScript (网景) 和 JScript (微软)。它最初由网景的 Brendan Eich 发明，第一次出现是在网景的 Navigator 2.0 浏览器上。Netscape 2.0 以及微软 Internet Explorer 3.0 后序的所有浏览器上都有它的身影。

ECMAScript|发布时间|新增特性
---|:--:|---:
ECMAScript 2009(ES5)|2009年11月|扩展了Object、Array、Function等功能
ECMAScript 2015(ES6)|2015年6月|class、module、箭头函数、默认参数、解构赋值等
ECMAScript 2016(ES7)|2016年3月|includes、指数操作符
ECMAScript 2017(ES8)|2017年6月|async/await、Object.values、Object.entries等
ECMAScript 2018(ES9)|2018年|for-await-of、Promise.prototype.finally、正则表达式优化等
ECMAScript 2019(ES10)|2019年|flat、flatMap、trimStart、trimEnd、fromEntries等
ECMAScript 2020(ES11)|2020年|BigInt、Promise.AllSettled、matchAll等

> 希望通过这些特性，能够提升你代码的开发效率。

## ES6特性

ES6的特性比较多，在 ES5 发布近 6 年（2009-11 至 2015-6）之后才将其标准化。两个发布版本之间时间跨度很大，所以ES6中的特性比较多。

ES6中常用特性如下：
- 类 class
- 模块化 module
- 箭头函数 () => {}
- 函数默认参数
- 模板字符串
- 解构赋值
- 延展操作符 ...
- 对象属性简写
- Promise
- let、const

### 1、类 class

对熟悉Java，object-c，c#等纯面向对象语言的开发者来说，都会对class有一种特殊的情怀。ES6 引入了class（类），让JavaScript的面向对象编程变得更加简单和易于理解。

```javascript
class Animal {
    // 构造函数，实例化的时候将会被调用，如果不指定，那么会有一个不带参数的默认构造函数.
    constructor(name,color) {
      this.name = name
      this.color = color
    }
    // toString 是原型对象上的属性
    toString() {
      console.log('name:' + this.name + ',color:' + this.color)

    }
  }

 var animal = new Animal('dog','white')//实例化Animal
 animal.toString()

 console.log(animal.hasOwnProperty('name')) //true
 console.log(animal.hasOwnProperty('toString')) // false
 console.log(animal.__proto__.hasOwnProperty('toString')) // true

 class Cat extends Animal {
  constructor(action) {
    // 子类必须要在constructor中指定super 函数，否则在新建实例的时候会报错.
    // 如果没有置顶consructor,默认带super函数的constructor将会被添加、
    super('cat','white')
    this.action = action
  }
  toString() {
    console.log(super.toString())
  }
 }

 var cat = new Cat('catch')
 cat.toString()

 // 实例cat 是 Cat 和 Animal 的实例，和Es5完全一致。
 console.log(cat instanceof Cat) // true
 console.log(cat instanceof Animal) // true

```

### 2、模块化

ES5不支持原生的模块化，在ES6中模块作为重要的组成部分被添加进来。模块的功能主要由 export 和 import 组成。每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过 export 来规定模块对外暴露的接口，通过import来引用其它模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突。

> 导出 export、export default

ES6允许在一个模块中使用export来导出多个变量、常量、类、函数等

```javascript
  // 导出常量
  export const name = 'xiaoming'
  // 导出异步函数
  export const axios = params => new Promise((resolve, reject) => {})
  // 导出类
  export class Student {
    constructor(name, age) {
      this.name = name
      this.age = age
    }
    
    sayname() {
      console.log(this.name, this.age)
    }
  }
  // 合并导出
  export default { name, axios }
```

> 引入 import

```javascript
  // 引入方法
  import { name, Student } from 'utils'

  // 或者命名引入
  import * as Utils from 'utils'
```

### 3、箭头函数
这是ES6中最令人激动的特性之一。=>不只是关键字function的简写，它还带来了其它好处。箭头函数与包围它的代码共享同一个this,能帮你很好的解决this的指向问题。有经验的JavaScript开发者都熟悉诸如var self = this或var that = this这种引用外围this的模式。但借助=>，就不需要这种模式了。

> 箭头函数没有作用域，他的作用域属于调用方，没有prototype，所以箭头函数不能new

> 箭头函数结构
箭头函数的箭头=>之前是一个空括号、单个的参数名、或用括号括起的多个参数名，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）。


```javascript
  () => 1
  v => v+1
  (a,b) => a+b
  () => {
      alert("foo")
  }
  e => {
    if (e == 0){
        return 0
    }
    return 1000/e
  }

  // 箭头函数写在同一行不需要写return，例如
  const fetch = params => new Promise((resolve, reject) => {})
  // or
  const fetch = params => {
    return new Promise((resolve, reject) => {})
  }
```

### 4、函数默认参数

> ES6支持默认参数
```javascript
  const match = (arr = [], type = 'map', name = '小明', age = 19) => {
    // 函数处理
  }
```

> 不使用默认参数的话，则会造成默认值错误

```javascript
  function foo (height, color) {
    document.body.style.height = (height || 50) + 'px'
    document.body.style.color = color || 'green'
    // ...
  }
```
这样的写法一般来说没问题，但是当入参布尔值为false就会有问题，例如 0, ''

```javascript
  foo(0, '')
```

这个时候使用||的转换结构如下

```javascript
  (height || 50) // 50 因为height入参是0
  color || 'green' // green 因为入参是 ''
```

### 5、模板字符串

> 字符串可作为变量拼接、可以换行、

```javascript
  // 变量拼接
  const name = '汤师爷之子'
  const age = '8岁'

  console.log(`${name}说他${age}`)

  // 添加运算符
  document.body.style.color = `${color || 'green'}`

  // 换行
  `
    第一行
    第二行
    第三行
  `


```


### 6、解构赋值

```javascript
  // 对象解构赋值
  const data = {
    code: 0,
    list: [{
      name: '陈永仁',
      code: '27149'
    }, {
      name: '周润发',
      code: '9527'
    }]
  }

  const { code, list } = data

  // 数组解构赋值
  const name = ['周润发', '周星驰', '陈永仁', ['泰哥', '龙五']]

  const [ name1, name2, name3, [ name4, name5 ] ] = name
  console.log(name1, name2, name3, name4, name5)
  // 打印： 周润发, 周星驰, 陈永仁, 泰哥, 龙五


  // ！！！！！注意如果是这样的话打印出来的就有问题
  const [ name1, name2, [ name4 ] ] = name
  console.log(name1, name2, name4)
  // 打印： 周润发, 周星驰, 陈
  

  // 如果你想忽略某些值
  const [ name1, name2, ,[ name4 ] ] = name
  console.log(name1, name2, name4)
  // 打印： 周润发, 周星驰, 泰哥

```

> 使用解构赋值交换两个变量的值
```javascript
  let a=1
  let b=2
  [a,b]=[b,a]
  console.log(a,b)
  // 打印： 2, 1
```

> 如果解构赋值没有相关参数，可以设置默认值
```javascript
  let a = null
  let b = null
  [a = 5, b = 7] = [1]
  console.log(a) // 1
  console.log(b) // 7

```

### 7、延展操作符（spread operator）
延展操作符 ... 可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造对象时, 将对象表达式按key-value的方式展开。

语法示例如下：
```javascript
  const iterableObj = {
    name: '渣渣',
    age: 18,
    role: '学生',
    rank: '一年级'
  }

  // 函数调用
  myFunction(...iterableObj)

  // 等价于
  myFunction(iterbaleObj.name, iterbaleObj.age, iterbaleObj.roel, iterbaleObj.rank)

```

> ！！！延展功能不能数组、对象混用，例如将[...iterableObj, 1, 3] 或者 {
  name: '22', 
  ...[1,2,3]
}
> 这种方式会因为类型错误报错，Uncaught TypeError: iterableObj is not iterable at <anonymous>xxx

> 数组延展
```javascript
  const arrs = [1,2,4]
  [...arrs, 'key', 23]
```

> 对象延展
```javascript
  const iterableObj = {
    name: '渣渣',
    age: 18,
    role: '学生',
    rank: '一年级'
  }

  { ...iterableObj, value: '123' }
```

> 应用场景示例：
- 函数调用
```javascript
  function sum(x, y, z) {
    return x + y + z
  }
  const numbers = [1, 2, 3]

  //不使用延展操作符
  console.log(sum.apply(null, numbers))

  //使用延展操作符
  console.log(sum(...numbers))// 6

```
- 构造数组
```javascript
  // 没有展开语法的时候，只能组合使用 push，splice，concat 等方法，来将已有数组元素变成新数组的一部分。有了展开语法, 构造新数组会变得更简单、更优雅：
  const stuendts = ['Jine','Tom'] 
  const persons = ['Tony',...stuendts,'Aaron','Anna']
  conslog.log(persions)// ["Tony", "Jine", "Tom", "Aaron", "Anna"]

```
- 数组拷贝
```javascript
  var arr = [1, 2, 3]
  var arr2 = [...arr] // 等同于 arr.slice()
  arr2.push(4) 
  console.log(arr2) //[1, 2, 3, 4]
    
```
- 连接多数组 
```javascript
  var arr1 = [0, 1, 2]
  var arr2 = [3, 4, 5]
  var arr3 = [...arr1, ...arr2]// 将 arr2 中所有元素附加到 arr1 后面并返回
  //等同于
  var arr4 = arr1.concat(arr2)

```
- 对象合并克隆
```javascript
  var obj1 = { foo: 'bar', x: 42 }
  var obj2 = { foo: 'baz', y: 13 }

  var clonedObj = { ...obj1 }
  // 克隆后的对象: { foo: "bar", x: 42 }

  var mergedObj = { ...obj1, ...obj2 }
  // 合并后的对象: { foo: "baz", x: 42, y: 13 }

```
### 8、对象属性简写

> 不使用es6

```javascript
  const name='Ming',age='18',city='Shanghai'
        
  const student = {
      name:name,
      age:age,
      city:city
  }
  console.log(student)//{name: "Ming", age: "18", city: "Shanghai"}

```

> 使用es6

```javascript
  const name='Ming',age='18',city='Shanghai'
        
  const student = {
      name,
      age,
      city
  }
  console.log(student)//{name: "Ming", age: "18", city: "Shanghai"}

```

### 9、Promise

Promise 是异步编程的一种解决方案，比传统的解决方案callback更加的优雅。它最早由社区提出和实现的，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。

优点：
- 链式调用

> 异步请求封装
```javascript
  const fetch = (params = {}) => new Promise((resolve, reject) => {
    if (Object.keys(params).length) {
      resolve({
        status: 200
      })
    } else {
      reject({
        status: 500
      })
    }
  })

  fetch({}).then(res => {

  }).catch(e => {

  })
  
```

### 10、let const

在之前JS是没有块级作用域的，const与let填补了这方便的空白，const与let都是块级作用域。
防止变量提升

> 使用var定义的变量为函数级作用域
```javascript
  {
    var a = 10
  }

  console.log(a) // 输出10

```

> 使用let与const定义的变量为块级作用域：

```javascript
  {
    let a = 10
  }

  console.log(a) //-1 or Error“ReferenceError: a is not defined”

```

## ES7 新特性
在ES6之后，ES的发布频率更加频繁，基本每年一次，所以自ES6之后，每个新版本的特性的数量就比较少。

- includes()
- 指数运算符

### 1、includes
includes() 函数用来判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回false。

includes 函数与 indexOf 函数很相似，下面两个表达式是等价的：

```javascript
  arr.includes(x)
  arr.indexOf(x) >= 0

```
> 使用includes验证数组中是否存在某个参数
```javascript
  let arr = ['react', 'angular', 'vue']

  if (arr.includes('react'))
  {
      console.log('react存在')
  }

```

### 2、指数运算符

在ES7中引入了指数运算符**，**具有与Math.pow(..)等效的计算结果。

> 不使用运算符, 使用自定义的递归函数calculateExponent或者Math.pow()进行指数运算：
```javascript
  function calculateExponent(base, exponent)
  {
      if (exponent === 1)
      {
          return base
      }
      else
      {
          return base * calculateExponent(base, exponent - 1)
      }
  }

  console.log(calculateExponent(2, 10)) // 输出1024
  console.log(Math.pow(2, 10)) // 输出1024

```
> 使用指数运算符**，就像+、-等操作符一样：
```javascript
  console.log(2**10)// 输出1024

```


## ES8特性
- async/await
- Object.values()
- Object.entries()
- String padding  padStart()和padEnd()，填充字符串达到当前长度
- 函数参数列表结尾允许逗号
- Object.getOwnPropertyDescriptors()
- ShareArrayBuffer和Atomics对象，用于从共享内存位置读取和写入

### 1、async await

在ES8中加入了对async/await的支持，也就我们所说的异步函数，这是一个很实用的功能。 async/await将我们从头痛的回调地狱中解脱出来了，使整个代码看起来很简洁。

> 使用async/await与不使用async/await的差别：

```javascript
  login(userName) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('1001')
      }, 600)
    })
  }

  getData(userId) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (userId === '1001') {
          resolve('Success')
        } else {
          reject('Fail')
        }
      }, 600)
    })
  }

  // 不使用async/await ES7
  doLogin(userName) {
    this.login(userName)
      .then(this.getData)
      .then(result => {
          console.log(result)
      })
  }

  // 使用async/await ES8
  async doLogin2(userName) {
    const userId = await this.login(userName)
    const result = await this.getData(userId)
  }

  this.doLogin()// Success
  this.doLogin2()// Success

```

> async/await的几种应用场景
- 获取异步函数的返回值, 异步函数本身会返回一个Promise，所以我们可以通过then来获取异步函数的返回值。
```javascript
  async function charCountAdd(data1, data2) {
    const d1 = await charCount(data1)
    const d2 = await charCount(data2)
    return d1 + d2
  }
  charCountAdd('Hello','Hi').then(console.log)//通过then获取异步函数的返回值。
  function charCount(data) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(data.length)
      }, 1000)
    })
  }

```

- async/await在并发场景中的应用， 对于上述的例子，我们调用await两次，每次都是等待1秒一共是2秒，效率比较低，而且两次await的调用并没有依赖关系，那能不能让其并发执行呢，答案是可以的，接下来我们通过Promise.all来实现await的并发调用。
```javascript
  async function charCountAdd(data1, data2) {
    const [d1,d2] = await Promise.all([charCount(data1),charCount(data2)])
    return d1 + d2
  }
  charCountAdd('Hello','Hi').then(console.log)
  function charCount(data) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(data.length)
      }, 1000)
    })
  }

```

- async/await的几种错误处理方式
  - 第一种：捕捉整个async/await函数的错误
  ```javascript
  async function charCountAdd(data1, data2) {
    const d1 = await charCount(data1)
    const d2 = await charCount(data2)
    return d1 + d2
  }
  charCountAdd('Hello','Hi')
    .then(console.log)
    .catch(console.log)//捕捉整个async/await函数的错误
  // 这种方式可以捕捉整个charCountAdd运行过程中出现的错误，错误可能是由charCountAdd本身产生的，也可能是由对data1的计算中或data2的计算中产生的。
  ```
  - 第二种：捕捉单个的await表达式的错误
  ```javascript
  async function charCountAdd(data1, data2) {
    const d1 = await charCount(data1)
        .catch(e => console.log('d1 is null'))
    const d2 = await charCount(data2)
        .catch(e => console.log('d2 is null'))
    return d1 + d2
  }
  charCountAdd('Hello','Hi').then(console.log)
    .catch(console.log)//捕捉整个async/await函数的错误

  // 通过这种方式可以捕捉每一个await表达式的错误，既可以捕捉每一个await表达式的错误，又可以捕捉整个charCountAdd函数的错误
  ```

  - 同时捕捉多个的await表达式的错误
  ```javascript
  async function charCountAdd(data1, data2) {
    let d1,d2
    try {
      d1 = await charCount(data1)
      d2 = await charCount(data2)
    }catch (e){
      console.log('d1 is null')
    }
    return d1 + d2
  }
  charCountAdd('Hello','Hi')
    .then(console.log)

  function charCount(data) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(data.length)
      }, 1000)
    })
  }

  ``` 


### 2、Object.values
Object.values()是一个与Object.keys()类似的新函数，但返回的是Object自身属性的所有值，不包括继承的值。

假设我们要遍历如下对象obj的所有值：
```javascript
  const obj = {a: 1, b: 2, c: 3}

  const values=Object.values(obj)
  console.log(values) //[1, 2, 3]

```

### 3、Object.entries 

Object.entries()函数返回一个给定对象自身可枚举属性的键值对的数组。

接下来我们来遍历上文中的obj对象的所有属性的key和value：

```javascript
  const obj = {a: 1, b: 2, c: 3}
  for(let [key,value] of Object.entries(obj1)){
    console.log(`key: ${key} value:${value}`)
  }
  //key:a value:1
  //key:b value:2
  //key:c value:3

```

### 4、String padding

在ES8中String新增了两个实例函数String.prototype.padStart和String.prototype.padEnd，允许将空字符串或其他字符串添加到原始字符串的开头或结尾。

> String.padStart(targetLength,[padString])

- targetLength:当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
- padString:(可选)填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断，此参数的缺省值为 " "。

```javascript
  console.log('0.0'.padStart(4,'10')) //10.0
  console.log('0.0'.padStart(20))//                0.00    

```

> String.padEnd(targetLength,padString])
- targetLength:当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
- padString:(可选) 填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断，此参数的缺省值为 " "

```javascript
  console.log('0.0'.padEnd(4,'0')) //0.00    
  console.log('0.0'.padEnd(10,'0'))//0.00000000

```

### 5、函数参数列表结尾允许逗号
这是一个不痛不痒的更新，主要作用是方便使用git进行多人协作开发时修改同一个函数减少不必要的行变更。

```javascript
  //程序员A
  var f = function(a,
    b,
    ) { 
    ...
    }

  //程序员B
  var f = function(a,
    b,
    c,   //变更行
    ) { 
    ...
    }

  //程序员C
  var f = function(a,
    b,
    c,
    d,   //变更行
    ) { 
    ...
    }

```

### 6、Object.getOwnPropertyDescriptors()

Object.getOwnPropertyDescriptors()函数用来获取一个对象的所有自身属性的描述符,如果没有任何自身属性，则返回空对象。

函数原型如下：Object.getOwnPropertyDescriptors(obj)

```javascript
  const obj2 = {
    name: 'Jine',
    getage() { return '18' }
  };
  Object.getOwnPropertyDescriptors(obj2)

  // 返回如下
  // {
  //   age: {
  //     configurable: true,
  //     enumerable: true,
  //     get: function age(){}, //the getter function
  //     set: undefined
  //   },
  //   name: {
  //     configurable: true,
  //     enumerable: true,
  //		value:"Jine",
  //		writable:true
  //   }
  // }

```

## ES9特性

- 迭代函数 for-await-of
- Promise.finnally
- Rest / Spread 属性
- 正则表达式命名捕获组（Regular Expression Named Capture Groups）
- 正则表达式反向断言（lookbehind）
- 正则表达式dotAll模式
- 正则表达式 Unicode 转义
- 非转义序列的模板字符串

### 1、迭代函数

在async/await的某些时刻，你可能尝试在同步循环中调用异步函数。例如：

```javascript

  async function process(array) {
    for (let i of array) {
      await doSomething(i);
    }
  }

  // or

  async function process(array) {
    array.forEach(async i => {
      await doSomething(i);
    });
  }


```

这段代码中，循环本身依旧保持同步，并在在内部异步函数之前全部调用完成。
ES2018引入异步迭代器（asynchronous iterators），这就像常规迭代器，除了next()方法返回一个Promise。因此await可以和for...of循环一起使用，以串行的方式运行异步操作。例如：

```javascript
  async function process(array) {
    for await (let i of array) {
      doSomething(i);
    }
  }

```

### 2、Promise.finnally

一个Promise调用链要么成功到达最后一个.then()，要么失败触发.catch()。在某些情况下，你想要在无论Promise运行成功还是失败，运行相同的代码，例如清除，删除对话，关闭数据库连接等。

.finally()允许你指定最终的逻辑：

```javascript
  function doSomething() {
    doSomething1()
    .then(doSomething2)
    .then(doSomething3)
    .catch(err => {
      console.log(err);
    })
    .finally(() => {
      // finish here!
    });
  }

```

### 3、Rest/Spread 属性

ES2015引入了[Rest参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)和[扩展运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)。三个点（...）仅用于数组。Rest参数语法允许我们将一个布丁数量的参数表示为一个数组。

```javascript
  restParam(1, 2, 3, 4, 5);
  function restParam(p1, p2, ...p3) {
    // p1 = 1
    // p2 = 2
    // p3 = [3, 4, 5]
  }

```

展开操作符以相反的方式工作，将数组转换成可传递给函数的单独参数。例如Math.max()返回给定数字中的最大值：

```javascript
  const values = [99, 100, -1, 48, 16];
  console.log( Math.max(...values) ); // 100

```

ES2018为对象解构提供了和数组一样的Rest参数（）和展开操作符，一个简单的例子：

```javascript
  const myObject = {
    a: 1,
    b: 2,
    c: 3
  };

  const { a, ...x } = myObject;
  // a = 1
  // x = { b: 2, c: 3 }

```

### 4、正则表达式命名捕获组（Regular Expression Named Capture Groups）
JavaScript正则表达式可以返回一个匹配的对象——一个包含匹配字符串的类数组，例如：以YYYY-MM-DD的格式解析日期：
```javascript
  const
  reDate = /([0-9]{4})-([0-9]{2})-([0-9]{2})/,
  match  = reDate.exec('2018-04-30'),
  year   = match[1], // 2018
  month  = match[2], // 04
  day    = match[3]; // 30

```

这样的代码很难读懂，并且改变正则表达式的结构有可能改变匹配对象的索引。

ES2018允许命名捕获组使用符号?<name>，在打开捕获括号(后立即命名，示例如下：

```javascript
  const
  reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  match  = reDate.exec('2018-04-30'),
  year   = match.groups.year,  // 2018
  month  = match.groups.month, // 04
  day    = match.groups.day;   // 30

```
任何匹配失败的命名组都将返回undefined。

命名捕获也可以使用在replace()方法中。例如将日期转换为美国的 MM-DD-YYYY 格式：
```javascript
  const
  reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  d      = '2018-04-30',
  usDate = d.replace(reDate, '$<month>-$<day>-$<year>');

```

### 5、正则表达式反向断言（lookbehind）

目前JavaScript在正则表达式中支持先行断言（lookahead）。这意味着匹配会发生，但不会有任何捕获，并且断言没有包含在整个匹配字段中。例如从价格中捕获货币符号：

```javascript

const
  reLookahead = /\D(?=\d+)/,
  match       = reLookahead.exec('$123.89');

  console.log( match[0] ); // $

```

ES2018引入以相同方式工作但是匹配前面的反向断言（lookbehind），这样我就可以忽略货币符号，单纯的捕获价格的数字：
```javascript
const
  reLookbehind = /(?<=\D)\d+/,
  match        = reLookbehind.exec('$123.89');

 console.log( match[0] ); // 123.89

```
以上是 肯定反向断言，非数字\D必须存在。同样的，还存在 否定反向断言，表示一个值必须不存在，例如：

```javascript
const
  reLookbehindNeg = /(?<!\D)\d+/,
  match           = reLookbehind.exec('$123.89');

console.log( match[0] ); // null

```

### 6、正则表达式dotAll模式

正则表达式中点.匹配除回车外的任何单字符，标记s改变这种行为，允许行终止符的出现，例如：

```javascript
/hello.world/.test('hello\nworld');  // false
/hello.world/s.test('hello\nworld'); // true

```

### 7、正则表达式 Unicode 转义
到目前为止，在正则表达式中本地访问 Unicode 字符属性是不被允许的。ES2018添加了 Unicode 属性转义——形式为\p{...}和\P{...}，在正则表达式中使用标记 u (unicode) 设置，在\p块儿内，可以以键值对的方式设置需要匹配的属性而非具体内容。例如：

```javascript
const reGreekSymbol = /\p{Script=Greek}/u;
reGreekSymbol.test('π'); // true

```

### 8、非转义序列的模板字符串
之前，\u开始一个 unicode 转义，\x开始一个十六进制转义，\后跟一个数字开始一个八进制转义。这使得创建特定的字符串变得不可能，例如Windows文件路径 C:\uuu\xxx\111。更多细节参考[模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)。


## ES10特性
- 行分隔符（U + 2028）和段分隔符（U + 2029）符号现在允许在字符串文字中，与JSON匹配
- 更加友好的 JSON.stringify
- 新增了Array的flat()方法和flatMap()方法
- 新增了String的trimStart()方法和trimEnd()方法
- Object.fromEntries()
- Symbol.prototype.description
- String.prototype.matchAll
- Function.prototype.toString()现在返回精确字符，包括空格和注释
- 简化try {} catch {},修改 catch 绑定
- 新的基本数据类型BigInt
- globalThis
- 动态引入模块 import()
- Class: private, static & public 成员变量，函数 私有的实例方法和访问器

### 1、行分隔符（U + 2028）和段分隔符（U + 2029）符号现在允许在字符串文字中，与JSON匹配
以前，这些符号在字符串文字中被视为行终止符，因此使用它们会导致SyntaxError异常。

```javascript
  const ls = ' '
  const ps = eval("'\u2029'")
```

### 2、更加友好的 JSON.stringify

如果输入 Unicode 格式但是超出范围的字符，在原先JSON.stringify返回格式错误的Unicode字符串。现在实现了一个改变JSON.stringify的第3阶段提案，因此它为其输出转义序列，使其成为有效Unicode（并以UTF-8表示）

### 3、新增了Array的flat()方法和flatMap()方法

flat()和 flatMap()本质上就是是归纳（reduce） 与 合并（concat）的操作。

> Array.prototype.flat()

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```javascript
  const arr1 = [1, 2, [3, 5, [6]]]
  arr1.flat() // [1, 2, 3, 5, [6]]

  arr1.flat(2) // [1, 2, 3, 5, 6]

  //使用 Infinity 作为深度，展开任意深度的嵌套数组

  arr1.flat(Infinity)
```

> flat 可以移除数组空项
```javascript
  const arr = [1, 2, , 3]
  arr.flat() // [1, 2, 3]
```


> Array.prototype.flatMap()
flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 和 深度值1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。 这里我们拿map方法与flatMap方法做一个比较。

```javascript
  var arr1 = [1, 2, 3, 4];
  arr1.map(x => [x * 2]); // [[2], [4], [6], [8]] 
  arr1.flatMap(x => [x * 2]); // [2, 4, 6, 8] 
  // 只会将 flatMap 中的函数返回的数组 “压平” 一层
  arr1.flatMap(x => [[x * 2]]); // [[2], [4], [6], [8]] 
```

### 4、新增了String的trimStart()方法和trimEnd()方法

新增的这两个方法很好理解，分别去除字符串首尾空白字符

```javascript
  ' 有一个空格 '.trimEnd() // ' 有一个空格'
  ' 有一个空格 '.trimStart() // '有一个空格 '
```

### 5、Object.fromEntries()
Object.entries()方法的作用是返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。

而 Object.fromEntries() 则是 Object.entries() 的反转。

Object.fromEntries() 函数传入一个键值对的列表，并返回一个带有这些键值对的新对象。这个迭代参数应该是一个能够实现@iterator方法的的对象，返回一个迭代器对象。它生成一个具有两个元素的类似数组的对象，第一个元素是将用作属性键的值，第二个元素是与该属性键关联的值。

- 通过 Object.fromEntries， 可以将 Map 转化为 Object:

```javascript
  const map = new Map([ ['foo', 'bar'], ['baz', 42] ]); // Map(2) {"foo" => "bar", "baz" => 42}
  const obj = Object.fromEntries(map);
  console.log(obj); // {foo: "bar", baz: 42}
  
  Object.entries({foo: "bar", baz: 42}) // [ ['foo', 'bar'], ['baz', 42] ]
```

> 通过 Object.fromEntries， 可以将 Array 转化为 Object:

```javascript
  const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
  const obj = Object.fromEntries(arr);
  console.log(obj); // {0: "a", 1: "b", 2: "c"}
```

### 6、Symbol.prototype.description

通过工厂函数Symbol（）创建符号时，您可以选择通过参数提供字符串作为描述：

```javascript
  const sym = Symbol('The description');
```
以前，访问描述的唯一方法是将符号转换为字符串：

```javascript
  assert.equal(String(sym), 'Symbol(The description)');
```
现在引入了getter Symbol.prototype.description以直接访问描述：
```javascript
  assert.equal(sym.description, 'The description');
```

### 7、String.prototype.matchAll
matchAll() 方法返回一个包含所有匹配正则表达式及分组捕获结果的迭代器。 在 matchAll 出现之前，通过在循环中调用regexp.exec来获取所有匹配项信息（regexp需使用/g标志：

```javascript
  const regexp = RegExp('foo*','g');
  const str = 'table football, foosball';
  while ((matches = regexp.exec(str)) !== null) {
    console.log(`Found ${matches[0]}. Next starts at ${regexp.lastIndex}.`);
    // expected output: "Found foo. Next starts at 19."
  }
```
如果使用matchAll ，就可以不必使用while循环加exec方式（且正则表达式需使用／g标志）。使用matchAll 会得到一个迭代器的返回值，配合 for...of, array spread, or Array.from() 可以更方便实现功能：

```javascript
  const regexp = RegExp('foo*','g'); 
  const str = 'table football, foosball';
  let matches = str.matchAll(regexp);
  for (const match of matches) {
    console.log(match);
  }
  // Array [ "foo" ]
  // Call matchAll again to create a new iterator
  matches = str.matchAll(regexp);
  Array.from(matches, m => m[0]);
```
> matchAll可以更好的用于分组

```javascript
  var regexp = /t(e)(st(\d?))/g;
  var str = 'test1test2';
  str.match(regexp); 

  let array = [...str.matchAll(regexp)];
  array[0];
  array[1];
```
### 8、Function.prototype.toString()现在返回精确字符，包括空格和注释

```javascript
  function /* comment */ foo /* another comment */() {}
  console.log(foo.toString()); 
  // ES2019 会把注释一同打印
  console.log(foo.toString()); 
  // 箭头函数
  const bar  = /* another comment */ () => {};
  console.log(bar.toString());
```

### 9、修改 catch 绑定

在 ES10 之前，我们必须通过语法为 catch 子句绑定异常变量，无论是否有必要。很多时候 catch 块是多余的。ES10 提案使我们能够简单的把变量省略掉。

不算大的改动。

> 之前
```javascript
  try {} catch(e) {}
```

> 现在
```javascript
  try {} catch {}
```
### 10、新的基本数据类型 BigInt

现在的基本数据类型（值类型）不止5种（ES6之后是六种）了哦！加上BigInt一共有七种基本数据类型，分别是：String、Number、Boolean、Null、Undefined、Symbol、BigInt

<p>在此标准下，无法精确表示的非常大的整数将自动四舍五入。确切地说，JS 中的<code>Number</code>类型只能安全地表示<code>-9007199254740991 (-(2^53-1))</code> 和<code>9007199254740991(2^53-1)</code>之间的整数，任何超出此范围的整数值都可能失去精度。</p>

```javascript
  console.log(9999999999999999) // → 10000000000000000
```
该整数大于JS Number 类型所能表示的最大整数，因此，它被四舍五入的。意外四舍五入会损害程序的可靠性和安全性。这是另一个例子：

```javascript
  // 注意最后一位的数字
  9007199254740992 === 9007199254740993 // true
```
JS 提供Number.MAX_SAFE_INTEGER常量来表示 最大安全整数，Number.MIN_SAFE_INTEGER常量表示最小安全整数：

```javascript
  const minInt = Number.MIN_SAFE_INTEGER

  console.log(minInt) // → -9007199254740991
  console.log(minInt - 5) // → -9007199254740996
  console.log(minInt - 4) // → -9007199254740996
  console.log(minInt - 8) // -9007199254741000


```
使用BigInt，应用程序不再需要变通方法或库来安全地表示Number.MAX_SAFE_INTEGER和Number.Min_SAFE_INTEGER之外的整数。 现在可以在标准JS中执行对大整数的算术运算，而不会有精度损失的风险。

```javascript
  BigInt(9007199254740993) // 9007199254740992n
  BigInt('9007199254740993') // 9007199254740993n
  // 所以记得用字符串

  BigInt('9007199254740993') + 8n // 9007199254741001n
  BigInt('9007199254740993') + 8 // VM3851:1 Uncaught TypeError: Cannot mix BigInt and other types, use explicit conversions


```

BigInt 文字也可以用二进制、八进制或十六进制表示

```javascript
  // binary
  console.log(b100000000000000000000000000000000000000000000000000011n) // → 9007199254740995n

  // hex
  console.log(x20000000000003n) // → 9007199254740995n

  // octal
  console.log(o400000000000000003n) // → 9007199254740995n

  // note that legacy octal syntax is not supported
  console.log(0400000000000000003n) // SyntaxError
```

### 11、globalThis

> 全局 this 在ES10之前尚未标准化。在生产代码中，您可以通过编写下边代码来“标准化”它：

```javascript
  // 之前获取全局对象
  const getGlobal = function () {
    if (typeof self !== 'undefined') return self
    if (typeof window !== 'undefined') return window
    if (typeof global !== 'undefined') return global
    throw new Error('找不到全局对象')
  }

  // ES10
  // 定义全局参数, 等价于 window.v self.v global.v
  globalThis.v = { bol: true }

```
### 12、动态引入

动态import()返回所请求模块的Promise。因此，可以使用async/await 将导入的模块分配给变量。

```javascript
  const component = await import('xxx.js')
```
### 13、Class: private, static & public 成员变量，函数 私有的实例方法和访问器

现在，新的语法字符＃（哈希标签）用于直接在类中定义变量，函数，getter和setter，以及构造函数和类方法。

```javascript

  class Point {
    #x;
    #y;

    constructor(x, y) {
      this.#x = x
      this.#y = y
    }
    
    equals(point) {
      return this.#x === point.#x && this.#y === point.#y
    }
  }

  // 静态方法访问
  class Foo {
    #privateValue = 24
    static getPrivateValue (foo) {
      return foo.#privateValue
    }
  }

  Foo.getPrivateValue(new Foo()) // 24


  // 私有成员变量
  class Foo {
    publicFieldName = 1
    #privateFieldName = 2
    add () {
      return this.publicFieldName + this.#privateFieldName
    }
  }
  new Foo().add() // 3



  // 私有方法
  class Foo {
    constructor() {
      this.#method()
    }
    #method () {}
  }
```

## ES11特性

- 可选链(Optional Chaining)
- null或操作符
- Promise.AllSettled

### 1、可选链(Optional Chaining)

```javascript
  const user = {
    "name": "Aryclenio Barros",
    "age": 22,
    "alive": true,
    "address": {
      "street": "Hyrule street",
      "number": 24,
    }
  }

  // Without optional chaining
  const number = user.address && user.address.number

  // With optional chaining
  const number = user?.address?.number

  // 不存在返回undefined
```

### 2、null或操作符

新增加了 ?? 运算符, 与 || 不同的是，??只允许变量与undefined和null进行或运算

|| 和 三目运算符会出现布尔类型的错误验证

>不使用??运算符
```javascript
  '' || 10 // => 10
  
  const a = ''
  a ? a : 10 // 10

  0 || 10 // 10
  const b = 0
  b ? b : 10 // 10

```

> 使用??运算符
```javascript
  '' || 10 // ''
  
  const a = ''
  a ? a : 10 // ''

  0 || 10 // 0
  const b = 0
  b ? b : 10 // 0
```

### 3、Promise.AllSettled
Promise对象新增了AllSettled属性，允许传入条件语句来监听数组中所有的promise是否已经都已被resolve

> Promise.AllSettled

```javascript
  // 不管什么状态 都会收集起来 
  const p1 = new Promise((resolve, reject) => {
    reject("第一个错");
  });

  const p2 = new Promise((resolve, reject) => {
    resolve('第二个对----');
  });

  Promise.allSettled([p1, p2]).then(results => {

    console.log(results);
    // [{status: "rejected", reason: "第一个错"}, {status: "fulfilled", value: "第二个对----"}]
  });
```

> Promise.all
```javascript
  // 只要有一个报错就进入catch
  const p1 = new Promise((resolve, reject) => {
    reject("第一个错");
  });

  const p2 = new Promise((resolve, reject) => {
    resolve('第二个对----');
  });

  Promise.all([p1, p2]).then(results => {
    console.log(results);
  }).catch(error => {
    console.log(error)
    // Uncaught (in promise) 第一个错
  });
```
