# const命令
## 基本用法
### const声明一个只读的常量。一旦声明，常量的值就不能改变。
### 例子1

  ```
  const PI = 3.1415;
  console.log('PI,',PI) // 3.1415。

  再次赋值后

  PI = 3;
console.log('PI,',PI) // Assignment to constant variable：常数变量赋值。
```
### 解释：上面代码表明改变常量的值会报错。

const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。



###  例子

  ```
  const foo;
  console.log('foo,',foo) // Missing initializer in const declaration：常量声明中缺少初始值设定项


```
### 解释：上面代码表示，对于const来说，只声明不赋值，就会报错。

const的作用域与let命令相同：只在声明所在的块级作用域内有效。



#### 例子
  ```
  if (true) {
    const MAX = 5;
    console.log('MAX,',MAX)//5

  }
  
  MAX 
  console.log('MAX,',MAX)//MAX is not defined
```
### 解释：const命令声明的常量也是不提升，只在声明所在的块级作用域内有效。

const命令声明的常量同样存在暂时性死区，只能在声明的位置后面使用。

####
  ```
  if (true) {
    console.log('MAX,11',MAX); //Cannot access 'MAX' before initialization:初始化前无法访问“MAX”
    const MAX = 6;
    console.log('MAX,22',MAX)//6

  }
```

### 解释：上面代码在常量MAX声明之前就调用，结果报错。


const声明的常量，也与let一样不可重复声明。


#### 
  ```
  const message = "Goodbye!";
  console.log('message,,',message)//Goodbye

  同时声明两个age时
  let age = 25;
  const age = 30;
  console.log('age,,',age)//Identifier 'age' has already been declared:已声明标识符“age”


```

对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变，所以将一个对象声明为常量必须非常小心。



### 例子1

```
const foo = {};
foo.prop = 123;
console.log('foo,,',foo)//123

foo = {}; 
foo.a = 23;

console.log('foo,,',foo)//23

```
### 解释：上面代码中，常量foo储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

### 例子2

```
const a = ["we"];
a.push('Hello'); 
console.log('a,,',a)// 可执行，结果为：[ "we", 'Hello' ]

const a = [];
const b=a.length
console.log('b,,',b)//0


const c = [];
c = ['Dave'];   
console.log('c,,',c)//Assignment to constant variable.:常数变量赋值。

```


###  解释：上面代码中，常量a是一个数组，这个数组本身是可写的，但是如果将另一个数组赋值给a，就会报错。



### 如果真的想将对象冻结，应该使用Object.freeze方法。

#### 说明：Object.freeze()方法接收一个参数，如果此参数是一个对象，则此方法把这个对象冻结，如果是其他类型则不会报错，无影响。

### 例子1
被冻结的对象不能修改、添加、删除其属性或者属性值

```
let obj = {
  prop: 42
};

Object.freeze(obj);

obj.prop = 33;
// 在严格模式下引发错误：使用"use strict"时，会报 Cannot assign to read only property 'prop' of object '#<Object>'：无法分配给对象“#<object>”的只读属性“prop”
console.log(obj.prop);//42


```
### 解释：上面代码中，常量foo指向一个冻结的对象，所以添加新属性不起作用

### 例子2
另外，freeze冻结的是堆内存中的值，和栈中的引用无关。

```
let  obj = {
  a: 42
};

Object.freeze(obj); //return 此obj

obj.a = 3;  //不会报错

console.log(obj); //仍然是 { a: 42 }

obj = 2; //这是可以成功的


console.log(obj);//2

```


### Object.freeze()用处总结：
一个大的数据对象里，在你确信它不需要改变的时候，你可以给他freeze()，可以大大的增加性能。也可用作冻结线上的配置文件中的对象，以防有人误改。
### 有些“死区”比较隐蔽，不太容易发现。


ES5只有两种声明变量的方法：var命令和function命令。ES6除了添加let和const命令，后面章节还会提到，另外两种声明变量的方法：import命令和class命令。所以，ES6一共有6种声明变量的方法。

### global 对象

顶层对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。
ES5的顶层对象，本身也是一个问题，因为它在各种实现里面是不统一的。
浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。
浏览器和 Web Worker 里面，self也指向顶层对象，但是Node没有self。
Node 里面，顶层对象是global，但其他环境都不支持。
同一段代码为了能够在各种环境，都能取到顶层对象，现在一般是使用this变量，但是有局限性。

全局环境中，this会返回顶层对象。但是，Node模块和ES6模块中，this返回的是当前模块。
函数里面的this，如果函数不是作为对象的方法运行，而是单纯作为函数运行，this会指向顶层对象。但是，严格模式下，这时this会返回undefined。
不管是严格模式，还是普通模式，new Function('return this')()，总是会返回全局对象。但是，如果浏览器用了CSP（Content Security Policy，内容安全政策），那么eval、new Function这些方法都可能无法使用。
  
