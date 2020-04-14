# let命令
## 基本用法
### ES6新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
### 例子1

  ```
{
  let a = 10;
  var b = 1;
}

console.loh('11,,',a) // ReferenceError: a is not defined.
console.loh('22,,',b) // 1
```
### 解释：上面代码在代码块之中，分别用let和var声明了两个变量。然后在代码块之外调用这两个变量，结果let声明的变量报错，var声明的变量返回了正确的值。这表明，let声明的变量只在它所在的代码块有效。

###  例子2: for循环的计数器

  ```
for (let i = 0; i < 10; i++) {}

console.log(i);
//ReferenceError: i is not defined

```
### 解释：上面代码中，计数器i只在for循环体内有效，在循环体外引用就会报错。

### 常见面试题：（使用var和let进行for循环）

#### 使用var进行for
  ```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
console.log(a[6]())//10

```
### 解释：变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。

#### 使用let进行for
  ```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
console.log(a[6]())//6
```

### 解释：由于上面代码中，变量i是let声明的, （let声明的变量仅在块级作用域内有效），当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。


## 不存在变量提升
### let不像var那样会发生“变量提升”现象。所以，变量一定要在声明后使用，否则报错。

#### 使用let进行for
  ```
console.log(foo); // 输出undefined
console.log(bar); //Cannot access 'bar' before initialization：初始化前无法访问“bar”
var foo = 2;
let bar = 2;

```

### 解释：变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

## 暂时性死区

### 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

```
var tmp = 123;

if (true) {
  
  tmp = 'abc'; 
  let tmp;
  console.log(tmp)// ReferenceError

}
```
### 解释：上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

### 总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”

### “暂时性死区”也意味着typeof不再是一个百分之百安全的操作。
```
console.log(typeof x) // Cannot access 'x' before initialization:初始化前无法访问“x”

let x=1;
```
### 解释：上面代码中，变量x使用let命令声明，所以在声明之前，都属于x的“死区”，只要用到该变量就会报错。因此，typeof运行时就会抛出一个ReferenceError。

### 有些“死区”比较隐蔽，不太容易发现。
### 例子1
  ```
function bar(x = y, y = 2) {
  return [x, y];
}

console.log(bar())//Cannot access 'y' before initialization:初始化前无法访问“y”

```
### 解释：上面代码中，调用bar函数之所以报错（某些实现可能不报错），是因为参数x默认值等于另一个参数y，而此时y还没有声明，属于”死区“。

### 例子2
  ```
function bar(x = 2, y = x) {
  return [x, y];
}

console.log(bar())//// [2, 2]

```

### 解释：上面代码中，y的默认值是x，不会报错，因为此时x已经声明了。

总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

## 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。

```
//Identifier 'a' has already been declared:已声明标识符“a”
function a() {
  let a = 10;
  var a = 1;
}
console.log(a())



// 报错
function b() {
  let a = 10;
  let a = 1;
}
```
因此，不能在函数内部重新声明参数。


function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}


## 块级作用域
需要原因：ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

第一种场景，内层变量可能会覆盖外层变量。

```
var tmp = new Date();

function f() {
  console.log(tmp);
 
}

f(); // 会得到当前时间，因为var是全局变量
```
改进代码
  ```
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = "hello world";
  }
}

f(); // 会得到当前时间，因为var是全家变量
```

### 解释：上面代码中，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，用来计数的循环变量泄露为全局变量。

```
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);//hello
}

console.log(i); // 5
```
### 解释：上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。



## ES6的块级作用域
let实际上为JavaScript新增了块级作用域。

```
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

### 解释：上面代码中，都声明了变量n，运行后输出5。表示外层代码块不受内层代码块的影响。如果使用var定义变量n，最后输出的值就是10。

ES6允许块级作用域的任意嵌套。
```
{{{{{let insane = 'Hello World'}}}}};
```
### 解释：上面代码使用了一个五层的块级作用域。外层作用域无法读取内层作用域的变量。

块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。
```
// IIFE 写法
(function () {
  var tmp = "A"
  console.log(tmp); // A

}());
```
等于
```
// 块级作用域写法
{
  let tmp = "A";
  console.log(tmp); // A
}
```
## 块级作用域与函数声明
函数能不能在块级作用域之中声明，是一个相当令人混淆的问题。

ES5规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。

```
// 情况一
if (true) {
  function f() {}
}

// 情况二
try {
  function f() {}
} catch(e) {
}
```
### 解释：上面代码的两种函数声明，根据ES5的规定都是非法的。
但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。不过，“严格模式”下还是会报错。
```
// ES5严格模式
'use strict';
if (true) {
  function f() {}
}
// 报错
```

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。
```
// ES6严格模式
'use strict';
if (true) {
  function f() {}
}
// 不报错

```

ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。
```
function f() { console.log('I am outside!'); }
(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());

```
### 解释：上面代码在 ES5 中运行，会得到“I am inside!”，因为在if内声明的函数f会被提升到函数头部，实际运行的代码如下。
```
// ES5版本
function f() { console.log('I am outside!'); }
(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```

ES6 的运行结果就完全不一样了，会得到“I am outside!”。因为块级作用域内声明的函数类似于let，对作用域之外没有影响，实际运行的代码如下。
```
// ES6版本
function f() { console.log('I am outside!'); }
(function () {
  f();
}());
```
