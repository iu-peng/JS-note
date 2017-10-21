# ES6



---
[TOC]

## 严格模式
> 'use strict'-----ES6 自动开启严格模式。
用法：
1. 在全局开启：在script标签开头写上以上代码
2. 在局部开启：在需要的地方写

#### 严格模式下的规则：
1. 函数中的this不再是window，而是undefined；

    ```js
    function fn(){
		console.log(this)
	}
	fn();
	//ES5中：window   ES6中：undefined
    ```

2. 变量未定义调用会报错，必须用var定义

    ```js
    a = 8;
	console.log(a)
	//ES5中：8   ES6中：报错
	```
		
3. 使用call，apply调用函数,第一个参数如果为null或undefined，函数中的this指向会指向null或undefined，ES5中会指向window。

    ```js
    function fn(){
		console.log(this)
	}
	fn.call(undefined)
	//ES5中：iwindow  ES6中：undefined
	```

4. 函数形参不能重复定义，否则报错

    ```js
    function f(a,a){
		console.log(a)
	}
	f(1,3)
	//ES5中：3  ES6中：报错
    ```

5. ES5下如果形参把实参改变，arguments会追踪变化。但是ES6严格模式下，不会追踪形参变化

    ```js
    function fn(a){
    	a = 99;
    	console.log(arguments)
    }
    fn(1,2,3,4);
    //ES5中：99,2,3,4  ES6中：1,2,3,4
    ```
    
6. ES5下可以对arguments进行赋值，但是在ES6严格模式下是不能赋值的，否则报错。

    ```js
    function fn(a){
		arguments = 0
		console.log(arguments)
	}
	fn(1,2,3,4);
	//ES5中：0  ES6中：报错
    ```

7. 不能delete删除变量，否则报错。
8. 不能再for，if，while中声明函数。



## ES6 的6种声明变量的方式
1. var---(ES5)
2. function---(ES5)
3. let
4. const
5. import
6. class
## let 的使用

    let声明的变量只在它所在的代码块有效
    在它所在的代码块之外调用会报错

### let应用于for循环

```js
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6

<!------------------------------------------->

for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
> 由于第一个for循环是由let声明的，因此当前的i只在当前本轮循环中起作用，每次循环都会声明let，因此每次的let都是全新的变量，js会有引擎记住上次执行完后的i.

> 另外for循环还有一个特别之处，那就是小括号里的i和大括号里的i不是一个作用域，小括号中的是父作用域，大括号里的是子作用域。


### 不存在变量提升

```js
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
> var 存在变量提升的现象，但是let不存在，它所声明的变量只能在声明后才能使用，否则报错

### 暂时性死区

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
//-------------------------------------

if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
//----------------------------------

// 不报错
var x = x;
//----------------------------------

// 报错
let x = x;
// ReferenceError: x is not defined
```
* 上面代码中，在let声明变量前，对tmp赋值会报错

> 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

> ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。这在语法上，称为“**暂时性死区**”

#### 暂时性死区对typeOf的影响

    在let所在区域中，在let声明变量之前调用typeOf都会报错，因为在声明之前的区域都是let声明变量的死区，就会报错。
    
    但是typeOf 一个不存在的变量反而不会报错。
    
    所以，在let没出现之前，typeof运算符是百分之百安全的，永远不会报错。现在这一点不成立了
    
#### 暂时性死区的本质
> 总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
    
### 不允许重复声明

#### let不允许在相同作用域内，重复声明同一个变量
> let定义的变量不管之前由var定义还是let定义，都会报错

```js
// 报错
function () {
  let a = 10;
  var a = 1;
}

// 报错
function () {
  let a = 10;
  let a = 1;
}
```

#### 不能在函数内部重新声明参数

```js
function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```

## 块级作用域

    ES5 只有全局作用域和函数作用域，没有块级作用域
    
    块级作用域：用一对大括号形成的作用域

ES5中的问题：

* 函数可以修改全局变量
* 例如：for循环中的var会泄露为全局变量

```js
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
> if语句中的let声明的变量不受外界的影响，即使在同一个函数中

ES6块级作用域：

> 1. 允许块级作用域的任意嵌套。
> 2. 外层作用域无法读取内层作用域的变量。
> 3. 内层作用域可以定义外层作用域的同名变量。

## const声明常量

> 一旦定义了值，就不能改变了。如果再次赋值，会报错

```js
const a = 4;
a = 5;
//会报错
```

> 但是只是值不能改，址还是可以改的

```js
const a = { b:4 }
a.b = 10;
//不会报错  可以修改

//--------------------

const a = [1,2,3,4];
a[0] = 100;
console.log(a)//[100, 2, 3, 4]
```

> 让对象的属性值不可修改

```js
const obj = {}
Object.defineProperty( obj,'abc',{
    value:123,//属性值
    writable:false,//是否可以被改写，默认为false
    
    ebumerable:false,//是否可以通过for in被枚举，默认为false
    configurable:false,//是否可以被删除，默认为false
})
```

> 为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性
> 另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性
> 也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩

## 变量的解构赋值

> **定义**：ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

### 默认值

```js
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

    使用默认值的条件：后面的数组成员必须严格等于undefined，变量的默认值才会生效
    
    例子1中后面数组中第一个为undefined，所以默认值才会生效
    
    例子2中后面数组中第一个虽然为null，但不是undefined，默认值不会生效
    
```js
function f() {
  return 100;
}

let [x = f()] = [1];
```
    如果默认值为一个函数表达式，它只会在后面数组第一个undefined的时候才会调用函数，是惰性的，并不会主动求值


### 包含数组
```js
let [a,b,c,d] = [1,2,3,4]
console.log(a)//1
console.log(b)//2
console.log(c)//3
console.log(d)//4

//----------------------------

let [a,b,c,d,e='abc'] = [1,2,3,4]
//e在后面的数组中找不到值时，取默认的abc

//-----------------------------

let [foo] = [];//foo:undefined
let [bar, foo] = [1];//foo:undefined
//如果解构不成功，变量的值为undefined

```
### 包含在字符串

```js
let str = 'worldpeace';
let [a,b,c,d] = str;//字符串也可以拆解进行循环取值
console.log(a,b,c,d)//w o r l
```
#### 字符串属性的解构赋值

```js
let {length : len} = 'hello';
len // 5
//length是匹配模式，找到字符串对应的length属性匹配，成功后，把5赋给变量len
```

### JS方法的解构赋值

```js
let { log, sin, cos } = Math;
```

### 包含对象
    变量必须与属性同名，才能取到正确的值
对象key值为函数
```js
let t = (function(){
	function on(){
		console.log('on')
	}
	function off(){
		console.log('off')
	}
	function down(){
		console.log('down')
	}
	return {
		on:on,
		off:off,
		down:down
	}
})()
t.on();//函数return出来的是一个包含三个函数的对象，可以通过调用t.on()来达到调用的目的

let {on,off,down} = t;
//含有对象的时候，变量名必须和对象的key址相同，否则报错。
//定义了三个变量分别对应t的三个key值，
on();//---on
```
对象key值不为函数
```js
let obj = {
	a:1,
	b:2,
	c:3
}

let {a,b,c,d} = obj;
console.log(a,b,c,d)//1,2,3,undefined
//声明变量后，会在obj对象中找key名为变量的key值赋给变量，如果没有，就是undefined
---------------------

let {a,b,c,d=100} = obj;
console.log(a,b,c,d) //1,2,3,100
//声明的变量如果在obj中找不到，就会使用候补值，如果有则优先使用obj里的值

---------------------

let {floor} = Math;
//解构赋值也可以用于调用方法
console.log(floor(0.9))//0
```

#### 对象解构赋值的本质

```js
let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
//first是匹配的模式，f才是变量。真正被赋值的是变量f，而不是模式first

//-----------------------------------

var {x: y = 3} = {x: 5};
y // 5
//匹配模式是x，真正的变量是y，如果后面的对象未定义，变量y会使用默认值3
```
`let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };`

    对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

### 解构不成功情况

1. 解构失败，变量值为undefined

    ```js
    let {a} = {b: '123'};
    a // undefined
    ```
2. 解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。

    ```js
    let {a: {c}} = {b: '123'};//报错
    //因为a这时等于undefined，再取子属性就会报错
    ```
3. 将一个已经声明的变量用于解构赋值，这样写容易报错

    ```js
    let x;
    {x} = {x: 1};
    //--------------------
    //正确写法：
    let x;
    ({x} = {x: 1});
    ```

        JavaScript 引擎会将{x}理解成一个代码块，从而发生语法错误

### 函数传参的应用

```js
function fn([a=1,b=1]){
//a,b相当于定义的变量，他会从fn的实参中去取，取到的话就用，如果取不到，就用形参规定的默认值1
	return a*b
}
console.log( fn([3,5]) )//15
console.log( fn([5]) )//5

-------------------------

//map传入参数
let arr = [[1,2],[3,4],[5,6]]
let sumarr = arr.map(function([a,b]){
	return a+b;
})
console.log(sumarr)//[3, 7, 11]

---------------------------

[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

### 数值和布尔值的解构赋值
    解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
    
    解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
    
```js
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

### 不能使用圆括号的情况
1. 变量声明语句

    ```js
    // 全部报错
    let [(a)] = [1];
    
    let {x: (c)} = {};
    let ({x: c}) = {};
    let {(x: c)} = {};
    let {(x): c} = {};
    
    let { o: ({ p: p }) } = { o: { p: 2 } };
    ```
        它们都是变量声明语句，模式不能使用圆括号

2. 函数参数

    ```js
    // 报错
    function f([(z)]) { return z; }
    // 报错
    function f([z,(x)]) { return x; }
    ```
        函数参数也属于变量声明，因此不能带有圆括号。
3. 赋值语句的模式

    ```js
    // 全部报错
    ({ p: a }) = { p: 42 };
    ([a]) = [5];
    //整个模式放在圆括号之中，导致报错。
    
    ----------------------
    
    [({ p: a }), { x: c }] = [{}, {}];
    //一部分模式放在圆括号之中，导致报错。
    ```
### 能使用圆括号的情况

    可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号
    
```js
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```
    因为首先它们都是赋值语句，而不是声明语句；其次它们的圆括号都不属于模式的一部分

## 函数的扩展

### 箭头函数
> 箭头函数基本都是表达式的形式，没有函数声明

> 小括号中的是函数参数，箭头后面的是函数体，省略大括号，函数体是return，省略return，如果复杂的话，就要写大括号，大括号中没有return的话，函数返回值是undefined。

```js
let fn = (a,b) => a+b;
console.log(fn(3,9))//12
```
```js
let fn = a => a;
//一个参数可以省略小括号
console.log(fn(5))//5
```
```js
let fn = () => ({a:2})
//return return一个对象的话，对象要用小括号包起来，形参小括号不能省略
```
```js
var fn = (a,b=1) => {
	return a+ a*b
}
console.log( fn(2) )//4
console.log( fn(2,2) )//6
```
### 箭头函数的this指向
> 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象

> 不可以使用arguments对象，该对象在函数体内不存在，会报错

```js
document.onclick = function(){
	setTimeout(function(){
		console.log(this)//window
	},500)
	
	setTimeout(() => {
		console.log(this)//document
	},500)
}

---------------------

document.onclick = () => {
	setTimeout(function(){
		console.log(this)//window
	},500)
	
	setTimeout(() => {
		console.log(this)//window
	},500)
}
```
    以上代码在执行时，第一个点击事件发生后，里面的第一个定时器里的this是传统定时器指代的window，但是第二个定时器里的函数是在document对象下定义的，所以this指的是document。
    
    但是第二个点击事件发生时调用的函数是在全局window下定义的箭头函数，其this指向的是window，里面的this都是指向window。

### rest形参（类似arguments）

> 用于获取函数的多余实参，这样就不需要使用arguments对象了。

> 参数搭配的变量是一个**数组**，该变量将多余的参数放入数组中。arguments是类数组

> 不仅仅可以用在箭头函数中，平常的函数也可以使用

全部为多余参数
```js
let fn = (...a) => {
	console.log(a)//此处全部实参都为多余参数
}
fn(1,2,3,4);//[1, 2, 3, 4]
```
部分为多余参数
```js
let fn = (a,b,...c) => {
	console.log(a,b,c)//此处c为多余参数
}
fn(1,2,3,4);//1,2,[3, 4]
```
#### 展开运算符
* 将类数组转为数组的方法
        1. for循环
        2. 方法Array.from()
        3. 扩展运算符
```js
var inputs = document.getElementsByTagName('input');

[...inputs].forEach(function(item,index){
	item.onclick = function(){
		alert(index)
	}
})
```
    将inputs这个类数组转为了数组
        
## 字符串的扩展

### for of
> 循环遍历字符串，除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点

```js
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```
### includes()
> 返回布尔值，表示是否找到了参数字符串。

> 支持第二个参数，表示开始搜索的位置。

```js
var s = 'Hello world!';

s.includes('o') // true

s.includes('Hello', 6) // false
```

### startsWith()
> 返回布尔值，表示参数字符串是否在原字符串的头部。

> 支持第二个参数，表示开始搜索的位置。

```js
var s = 'Hello world!';

s.startsWith('Hello') // true

s.startsWith('world', 6) // true
```

### endsWith()
> 返回布尔值，表示参数字符串是否在原字符串的尾部。

> 支持第二个参数，表示从0位到n位的搜索范围。

```js
var s = 'Hello world!';

s.endsWith('!') // true

s.endsWith('Hello', 5) // true -- 搜索前五位
```

### repeat()
> 方法返回一个新字符串，表示将原字符串重复n次

> 参数如果是小数，会被取整,向下取整。

> 小于-1或是Infinity，会报错
> 参数如果为-1~0之间，会取整为0---''
> 参数为NaN也会取为0----''

> 参数为字符串，会先转为数字,转不了的视为NaN----''

```js
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```


















## Object.defineProperty(obj, prop, descriptor)

* obj
需要被操作的目标对象
* prop
目标对象需要定义或修改的属性的名称。
* descriptor
将被定义或修改的属性的描述符。

* value:123,//属性值
* writable:false,//是否可以被改写，默认为false

* ebumerable:false,//是否可以通过for in被枚举，默认为false
* configurable:false,//是否可以被删除，默认为false

## 















