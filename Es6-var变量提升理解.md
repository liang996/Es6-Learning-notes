# 例子1：
```
a = 2;
var a;
 
console.log(a);//2
```
# 等价于
```
var a;
a = 2;
 
console.log(a);//2
```

## 解释：javascript并不是严格的自上而下执行的语言。这一段代码的输出结果是2，是不是感到很意外？为什么会这样呢？这个问题的关键就在于变量提升(hoisting)。它会将当前作用域的所有变量的声明提升到程序的顶部.

# 例子2：
```
console.log(a);
 
var a = 2;//undefined
```
# 等价于：
```
var a;
 
console.log(a);//undefined
 
a = 2;
```
## 解释：js会将变量的声明提升到顶部，可是赋值语句并不会提升。对于js来说，其实var a = 2是分为两步的:


# 为什么有变量提升？
## 解释：js和其他语言一样，都要经历编译和执行阶段。而js在编译阶段的时候，会搜集所有的变量声明并且提前声明变量，而其他的语句都不会改变他们的顺序，因此，在编译阶段的时候，第一步就已经执行了，而第二步则是在执行阶段执行到该语句的时候才执行。

# 变量声明
## js的变量声明其实大体上可以分为三种：var声明、let与const声明和函数声明。

## 函数声明与其他声明一起出现的时候，就可能会引起一些困扰。我们来看下面的例子。

# 例子1：

```
foo();
 
function foo() {
    console.log('foo');//foo
}
var foo = 2;
```
## 解释：当函数声明与其他声明一起出现的时候，会以函数声明为准，因为函数声明高于一切，毕竟函数是js的第一公民。

# 例子2：
```
foo();
 
function foo() {
    console.log('1');
}
 
function foo() {
    console.log('3');//3
}
```
## 解释：因为有多个函数声明的时候，是由最后面的函数声明来替代前面的

# 测试题
```
foo();
 
var foo = function() {
    console.log('foo');//foo is not a function
}

```
## 解释：var foo = function() {}这种格式我们叫做函数表达式。它其实也是分为两部分，一部分是var foo,而一部分是foo = function() {}，这道题的结果应该是报了TypeError(因为foo声明但未赋值，因此foo是undefined)。

# 总结
1.js会将变量的声明提升到js顶部执行，因此对于这种语句:var a = 2;其实上js会将其分为var a;和a = 2;两部分，并且将var a这一步提升到顶部执行。

2.变量提升的本质其实是由于js引擎在编译的时候，就将所有的变量声明了，因此在执行的时候，所有的变量都已经完成声明。

3.当有多个同名变量声明的时候，函数声明会覆盖其他的声明。如果有多个函数声明，则是由最后的一个函数声明覆盖之前所有的声明。
