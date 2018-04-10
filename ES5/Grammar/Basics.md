# 基本语法
### 1、语句
语句以分号结尾，一个分号就表示一个语句结束。多个语句可以写在一行内。
分号前面可以没有任何内容，JavaScript引擎将其视为空语句。
```javascript
;;;//表示三个空语句
1 + 3;
'abc';
//上面两行语句只是单纯地产生一个值，并没有任何实际的意义。
```
### 2、变量
##### 2.1变量提升
概念:JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。
```javascript
console.log(a);
var a = 1;
```
上面代码首先使用console.log方法，在控制台（console）显示变量a的值。这时变量a还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。
```javascript
var a;
console.log(a);
a = 1;
```
最后的结果是显示undefined，表示变量a已声明，但还未赋值。
### 3、标识符
概念:标识符（identifier）指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及后面要提到的函数名。JavaScript 语言的标识符对大小写敏感，所以a和A是两个不同的标识符。
- 简单命名规则
```javascript
第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）。
第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字0-9。
```
中文是合法变量，可以用作变量名。
```javascript
JavaScript有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。
```
### 4、区块
JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。
对于var命令来说，JavaScript 的区块不构成单独的作用域（scope）。
```javascript
{
  var a = 1;
}
a // 1
```
上面代码在区块内部，使用var命令声明并赋值了变量a，然后在区块外部，变量a依然有效，区块对于var命令不构成单独的作用域，与不使用区块的情况没有任何区别。在 JavaScript 语言中，单独使用区块并不常见，区块往往用来构成其他更复杂的语法结构，比如for、if、while、function等
### 5、条件语句
##### 5.1switch结构
多个if...else连在一起使用的时候，可以转为使用更方便的switch结构。
```javascript
switch (fruit) {
  case "banana":
    // ...
    break;
  case "apple":
    // ...
    break;
  default:
    // ...
}
```
上面代码根据变量fruit的值，选择执行相应的case。如果所有case都不符合，则执行最后的default部分。需要注意的是，每个case代码块内部的break语句不能少，否则会接下去执行下一个case代码块，而不是跳出switch结构。
