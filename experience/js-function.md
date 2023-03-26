
## 前言

每个 JavaScript 函数实际上都是一个 `Function` 对象。运行 `(function(){}).constructor === Function // true` 便可以得到这个结论。


## [一]、函数
**函数的声明**

声明函数的存在，包括函数的名字、函数类型以及形参类型。并把这些信息通知编译系统，以便在调用该函数时进行对照检查（例如，函数名是否正确，实参与形参的类型和个数是否一致），它不包括函数体。这种情况下函数和函数定义分开。

JS中不存在函数声明。 因为JS是弱类型语言，声明变量的时候并不能确定变量的类型。

### 1.1 函数定义

对函数功能的确立，包括指定函数名，函数类型(返回值)、形参类型(参数)、函数体等，它是一个完整的、独立的函数单位。

#### 1.1.1 声明式

使用关键字`function`定义函数。

```
function func() {
  // body
}
```

#### 1.1.2 函数表达式
在一个表达式中定义一个函数，可以指定函数名也可以不指定。可以理解为定义一个变量，这个变量是函数对象。函数表达式定义的函数在代码执行的时候才会真正创建。

```
// # 语法
const func = function [name]([param1[, param2[, ..., paramN]]]) { statements };

const func = function() {
  // code
};


const func = function func() {
  // code
}
```

#### 1.1.3 构造函数 Function
JS支持使用`Function / GeneratorFunction`动态创建一个新的Function对象，也就是函数对象。

```
// # 语法
new Function ([arg1[, arg2[, ...argN]],] functionBody)

let str = 'return ' + '`Hello ${name}!`';
let func = new Function('name', str);

func('Jack') // "Hello Jack!"
```

使用Function构造器生成的Function对象是在函数创建时解析的。这比你使用函数声明或者函数表达式(function)并在你的代码中调用更为低效，因为使用后者创建的函数是跟其他代码一起解析的。所以一般情况下不推荐使用这种方式

### 1.2 函数类型

```
-   Function：普通函数，使用function定义的函数。
-   Arrow Function：箭头函数，使用=>定义的函数。
-   Generator Function：使用function*定义的函数。
-   Async Function：使用async function定义的函数。
```

#### 1.2.1 Function

普通函数，也就是我们常说的函数。

#### 1.2.2 Arrow Function
箭头函数表达式的语法比函数表达式更短，简洁。箭头函数更适用于那些本来需要匿名函数的地方。

箭头函数的特点
-   函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
-   不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
-   不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。


##### 适用场景
```
// 1、简化回调函数
const arr = ['a', ' b', ' c '];


// 正常函数写法
arr.map(function (str) {
  return str.trim();
});


// 箭头函数写法
arr.map(str => str.trim())
```
##### 不适场景

```
// 1. 定义函数方法，且方法内部包含this
const cat = {
  lives: 9,
  jumps: () => { this.lives--; }
};


// 2. 需要动态的this
const button = document.getElementById('press'); 
button.addEventListener('click', () => { 
    this.classList.toggle('on'); 
});
```

- 1. cat.jumps()方法是一个箭头函数，这是错误的。调用cat.jumps()时，如果是普通函数，该方法内部的this指向cat；如果写成上面那样的箭头函数，使得this指向全局对象，因此不会得到预期结果。

- 2. 点击按钮会报错，因为button的监听函数是一个箭头函数，导致里面的this就是全局对象。如果改成普通函数，this就会动态指向被点击的按钮对象。


#### 1.2.3 Generator Function

generator函数用来返回generator对象，并且它符合可迭代协议和迭代器协议。

在没有async函数之前，generator函数主要用来异步编程。阮一峰老师的ES6入门对generator有详细的有详细的介绍，这里就不再啰嗦。

co是一个generator流程控制器。在有async函数之前，co和generator是绝佳搭配。co代码小巧精悍，值得学习其中的JS技巧。

co.wrap用来封装generator成Promise对象，使得generator可以像Promise对象一样使用，同时也依然保持generator要完成的功能，也就是generator会依然正常执行。

#### 1.2.4 Async Function

Async函数用来处理异步操作，避免了回调黑洞，让异步代码看起来更human readable。

**Async函数的特点**

-   调用async函数的时候会异步执行。
-   在async函数中使用await的时候，会执行完当前代码才会往下继续执行其他代码，实现按照指定顺序执行异步操作。
-   async函数会返回一个Promise对象。


##### 异常处理方案
```
// 1.  **try catch**
async function fun() {
  await Promise.reject('error test')
}

async function run() {
  try {
    await fun();
  } catch(e) {
    console.log(e); // catch error test
  }
}

run()

// 2. Promise.catch()
async function fun() {
  await Promise.reject('error test')
}

fun()
.then(v => console.log('success'))
.catch(e => console.log('catch', e)); // catch error test
```

### 1.3 函数的参数(arguments)

#### 1.3.1 参数类型
##### 形参

形式参数，是在声明或定义函数的时候在()中使用的参数，目的是用来接收调用该函数时传递的参数。可以把形参理解为一种变量。

形参只有在函数被调用时才分配内存单元，在调用结束时，即刻释放所分配的内存单元。因此，形参只在函数内部有效。函数调用结束返回主调用函数后则不能再使用该形参变量。

##### 实参

实际参数，是在调用函数时传递的参数，即传递给被调用函数的值。

#### 1.3.2 参数传递

-   值传递：对于基本数据类型来说，形参会拷贝实参的值(参考[JS Data & Type](https://link.zhihu.com/?target=https%3A//shimo.im/docs/MKnt1hIpFK4rp9ml)基本类型)。
-   引用传递：对于引用类型来说，形参拷贝实参的内存地址(参考[JS Data & Type](https://link.zhihu.com/?target=https%3A//shimo.im/docs/MKnt1hIpFK4rp9ml)引用类型)。

### 1.4 函数调用

JS有三种函数调用方式：直接调用、apply调用和call调用。

#### 1.4.1 直接调用
函数名 + 参数
```
// 语法
funcName(arg1, arg2, ..) 
```

#### 1.4.2 call
```
// 语法
funcName.call(thisArg, arg1, arg2, ..) 
```

#### 1.4.3 apply
通过apply方法可以调用它所属的函数(对象)。可以给函数提供运行时的this值，以及使用一个数组（或类似数组对象）作为参数。

```
// 语法
func.apply(thisArg, [argsArray])
```

#### 1.4.4 bind

bind方法创建并返回一个新的函数，当这个绑定函数被调用时this值为其提供的值，其参数列表前几项值为创建绑定函数时指定的参数序列。

```
// 语法:
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```
-   thisArg：调用绑定函数时作为this参数传递给目标函数的值。 如果使用`new运算符`构造绑定函数，则忽略该值。
-   arg1, arg2, ...：当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法


#### 1.4.5 apply vs call vs bind 区别
- apply和call都是函数对象的方法，两者都可以改变函数运行时的this，这个是apply和call的主要使用的功能。

- apply和call不同在于，提供的参数格式不一样：apply需要的是一个参数数组；call需要的是参数列表。

- bind与apply和call不同的是：apply和call是在每次调用的时候动态指定被调用函数的this和实参，apply和call自动帮我们对目标函数进行调用；而bind是创建一个新的封装绑定函数，这个绑定函数固定了目标函数的this值，和部分实参。


## [二]、使用场景

### 2.1 函数作为参数

### 2.2 函数作为返回值
函数作为返回值一个典型的情况就是闭包。比如之前提到的co.wrap使用了闭包。


```
co.wrap = function (fn) {
  createPromise.__generatorFunction__ = fn;
  return createPromise;
  function createPromise() {
    return co.call(this, fn.apply(this, arguments));
  }
};
```


### 2.3 立即执行表达式(IIFE)

简单来说：立即调用函数表达式，是一个在定义时就会立即执行的JS函数。

```
// 支持所有类型的函数
(function () {
  // body
})();
```
`分析`：
- 第一部分是包围在圆括号运算符`()` 里的一个匿名函数，这个匿名函数拥有独立的词法作用域。这不仅避免了外界访问此`IIFE`中的变量，而且又不会污染全局作用域。 
- 第二部分再一次使用`()`创建了一个立即执行函数表达式，JavaScript 引擎到此将直接执行函数


### 2.4 函数原型链Prototype
[JS Prototype 原型&原型链](https://shimo.im/docs/sWPctYHDkToZaoIt/read)

### 2.5 函数上下文
[JS Excuse Context](https://shimo.im/docs/yzSqYk3YWd45YQgQ/read)

### 2.6 函数式编程
函数式编程是编程范式中的一种，是一种典型的编程思想和方法。其他的编程范式还包括面向`[对象编程]`，逻辑编程等。

编程范式的意义在于它提供了模块化代码的各种思想和方法。
-   模块化使得开发更快、维护更容易
-   模块可以复用
-   模块化便于单元测试和debug

由此可以理解为：函数式编程是以函数为核心来组织模块的一套编程方法。(不是写了函数就是函数式编程)

#### 2.6.1 高阶函数

#### 2.6.2 纯函数
符合以下要求的函数就是纯函数：
-   相同输入总是会返回相同的输出。返回的结果只依赖于输入的参数且与外部系统状态无关。
-   没有副作用。不会影响该函数作用域以外的外部状态(比如全局变量、参数)。

**优点：**

-   更加容易被测试，因为它们唯一的职责就是根据输入计算输出。
-   结果可以被缓存，因为相同的输入总会获得相同的输出。
-   自我文档化，因为函数的依赖关系很清晰。
-   更容易被调用，因为你不用担心函数会有什么副作用。

#### 2.6.3 柯里化
把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回一个新的函数的技术，新函数接受余下参数并返回运算结果。


## 拓展

- [# JS Function 函数](https://zhuanlan.zhihu.com/p/267403845)
- [JS data type](https://shimo.im/docs/MKnt1hIpFK4rp9ml/read)