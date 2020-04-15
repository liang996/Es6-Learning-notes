ES5 只有两种声明变量的方法：var命令和function命令。
ES6 除了添加let和const命令，还有两种声明变量的方法：import命令和class命令。

所以，ES6 一共有 6 种声明变量的方法，分别是：var let const import class function

### 1. var命令。

```
var a ;　　//undefined
var b = 1;
```
### 值得注意的几点：
1.var定义的变量可以修改，如果不初始化会输出undefined，不会报错

2.var 声明的变量在window上，用let或者const去声明变量，这个变量不会被放到window上

3.很多语言中都有块级作用域，但JS没有，它使用var声明变量，以function来划分作用域，大括号“{}” 却限定不了var的作用域，因此用var声明的变量具有变量提升的效果

4.var 声明的变量作用域是全局的或者是函数级的

### 2. function命令 

```
function add(a) {
　　var sum = a + 1;
　　return sum;
}
console.log(add(2))//3
```
### 值得注意的几点：
1.声明了一个名为 add的新变量，并为其分配了一个函数定义

2.{}之间的内容被分配给了 add

3.函数内部的代码不会被执行，只是存储在变量中以备将来使用

### 3. cosnt命令

```
const a;     //报错，必须初始化
const b = 1; 

```
### 值得注意的几点：

1.const定义的变量不可以修改，而且必须初始化

2.该变量是个全局变量，或者是模块内的全局变量

3.如果一个变量只有在声明时才被赋值一次，永远不会在其它的代码行里被重新赋值，那么应该使用const，但是该变量的初始值有可能在未来会被调整（常变量）

4.创建一个只读常量，在不同浏览器上表现为不可修改；建议申明后不修改；拥有块级作用域

5.const 代表一个值的常量索引 ，也就是说，变量名字在内存中的指针（地址）不能够改变，但是指向这个变量的值可能 改变

5.const定义的变量不可修改，一般在require一个模块的时候用或者定义一些全局常量

6.可以在全局作用域或者函数内声明常量，但是必须初始化常量

7.常量不能和它所在作用域内其它变量或者函数拥有相同名称


### 4. let命令
```
let a;　　//undefined
let b = 1; 
function add(b) {
　　let sum = b + 1;
　　return sum;
}
let c = add(b);
```
### 值得注意的几点：

1.不存在变量提升

2.不允许重复声明

3.let声明的变量作用域是在块级域中，函数内部使用let定义后，对函数外部无影响(块级作用域)

4.可以在声明变量时为变量赋值，默认值为undefined,也可以稍后在脚本中给变量赋值，在生命前无法使用(暂时死区)

5.需要”javascript 严格模式”：'use strict'

 
### 5. Import 导入命令

```
import Vue from 'vue';
import App from './App';
import router from './route';
import axios from 'axios';
import './less/index';

```

### 6. class命令

ES6中的class是一个基于prototype继承的语法糖，它提供了更接近传统语言的写法，引入了class（类）这个概念作为对象的模版。通过class关键字可以定义类，但是通过class做的工作，es5也可以做到，只是通过class，可以使我们的工作更加方便。

在我们JavaScript语言，生成实例对象的传统办法是通过构造函数，如下：
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
console.log(p )//Point { x: 1, y: 2 }
```
ES6class命令后可以把上述代码换成下面这种

```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

//等同于，ES5写法

Point.prototype={
　　constructor(){},
　　toString(){},

}
//注：类的所有方法都定义在类的prototype属性上面。
```

### 值得注意的几点：

1.  通过class关键字创建类,类名我们还是习惯性定义首字母大写

2.  类里面的constructor函数,可以接受传递过来的参数,同时返回实例对象

3.  constructor 函数只要new生成实例时,就会自动调用这个函数,如果不写也会自动调用

4.  创建类,类名后面不加小括号;生成实例,类后面加小括号,构造函数不需要加function

5.  类没有变量提升，所以必须先定义类，才能通过类实例化对象；

6. 类里面的共有的属性和方法一定要加this使用
