# 数值
### 1、概述
##### 1.1整数和浮点数
JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，`1`与`1.0`是相同的，是同一个数。
##### 1.2数值精度
JavaScript 浮点数的64个二进制位，从最左边开始，是这样组成的。
```javascript
第1位：符号位，0表示正数，1表示负数
第2位到第12位（共11位）：指数部分
第13位到第64位（共52位）：小数部分（即有效数字）
```
符号位决定了一个数的正负，指数部分决定了数值的大小，小数部分决定了数值的精度。`JavaScript 提供的有效数字最长为53个二进制位。`
##### 1.3数值范围
JavaScript 能够表示的数值范围为`21024到2-1023`（开区间），超出这个范围的数无法表示。
如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回`Infinity`。
如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，即 JavaScript 无法表示这么小的数，这时会直接返回`0`。
JavaScript 提供Number对象的`MAX_VALUE`和`MIN_VALUE`属性，返回可以表示的具体的最大值和最小值。
### 2、数值的表示方法
十进制，十六进制，科学计数法。
```javascript
123e3 // 123000
123e-3 // 0.123
```
科学计数法允许字母e或E的后面，跟着一个整数，表示这个数值的指数部分。</br>

以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示。
- （1）小数点前的数字多于21位。
- （2）小数点后的零多于5个。
### 3、数值的进制
JavaScript 对整数提供四种进制的表示方法：十进制、十六进制、八进制、二进制。
```javascript
十进制：没有前导0的数值。
八进制：有前缀0o或0O的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
十六进制：有前缀0x或0X的数值。
二进制：有前缀0b或0B的数值。
```
### 4、特殊数值
###### 4.1正零和负零
JavaScript 内部实际上存在2个0：一个是+0，一个是-0
唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。
```javascript
(1 / +0) === (1 / -0) // false
```
上面的代码之所以出现这样结果，是因为除以正零得到`+Infinity`，除以负零得到`-Infinity`，这两者是不相等的
##### 4.2NaN
###### （1）含义
NaN是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。</br>
`0`除以`0`也会得到`NaN`。
- NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number。
###### （2）运算规则
- NaN不等于任何值，包括它本身。
- NaN在布尔运算时被当作false。
- 数组的indexOf方法内部使用的是严格相等运算符，所以该方法对NaN不成立。
- NaN与任何数（包括它自己）的运算，得到的都是NaN。
##### 4.3Infinity
###### （1）含义
Infinity表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到Infinity。
```javascript
// 一
Math.pow(2, 1024)
// Infinity
// 二
0 / 0 // NaN
1 / 0 // Infinity
```
上面代码中，第一个是一个表达式的计算结果太大，超出了能够表示的范围，因此返回Infinity。第二个是0除以0会得到NaN，而非0数值除以0，会返回Infinity。</br>
`Infinity有正负之分，Infinity表示正的无穷，-Infinity表示负的无穷。`</br>
 `Infinity大于一切数值（除了NaN），-Infinity小于一切数值（除了NaN）。`</br>
 `Infinity与NaN比较，总是返回false。`
###### （2）运算规则
Infinity的四则运算，符合无穷的数学计算规则。
```javascript
5 * Infinity // Infinity
5 - Infinity // -Infinity
Infinity / 5 // Infinity
5 / Infinity // 0
```
0乘以Infinity，返回NaN；0除以Infinity，返回0；Infinity除以0，返回Infinity。</br>
Infinity加上或乘以Infinity，返回的还是Infinity。</br>
Infinity减去或除以Infinity，得到NaN。</br>
Infinity与null计算时，null会转成0，等同于与0的计算。</br>
Infinity与undefined计算，返回的都是NaN。
### 5、与数值相关的全局方法
##### 5.1parseInt()
###### （1）基本用法
```javascript
parseInt方法用于将字符串转为整数。
如果parseInt的参数不是字符串，则会先转为字符串再转换。
字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回NaN。
如果字符串以0x或0X开头，parseInt会将其按照十六进制数解析。
如果字符串以0开头，将其按照10进制解析。
对于那些会自动转为科学计数法的数字，parseInt会将科学计数法的表示方法视为字符串，因此导致一些奇怪的结果。
```
###### （2）进制转换
```javascript
parseInt('1000', 2) // 8
parseInt('1000', 6) // 216
parseInt('1000', 8) // 512
//二进制、六进制、八进制的1000，分别等于十进制的8、216和512。这意味着，可以用parseInt方法进行进制的转换。
```
```javascript
parseInt('10', 37) // NaN
parseInt('10', 1) // NaN
parseInt('10', 0) // 10
parseInt('10', null) // 10
parseInt('10', undefined) // 10
//如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回NaN。如果第二个参数是0、undefined和null，则直接忽略。
```
```javascript
parseInt('1546', 2) // 1
parseInt('546', 2) // NaN
//如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回NaN。
```
```javascriptparseInt(011, 2) // NaN
// 等同于
parseInt(String(011), 2)
// 等同于
parseInt(String(9), 2)
//上面代码中，第一行的011会被先转为字符串9，因为9不是二进制的有效字符，所以返回NaN。如果直接计算parseInt('011', 2) ，011则是会被当作二进制处理，返回3。
//JavaScript 不再允许将带有前缀0的数字视为八进制数，而是要求忽略这个0。但是，为了保证兼容性，大部分浏览器并没有部署这一条规定。
```
##### 5.2parseFloat()
`parseFloat`方法用于将一个字符串转为浮点数。
```javavscript
//parseFloat方法会自动过滤字符串前导的空格。
parseFloat('\t\v\r12.34\n ') // 12.34
```
```javascript
//如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回NaN。
parseFloat([]) // NaN
parseFloat('FF2') // NaN
parseFloat('') // NaN
//parseFloat会将空字符串转为NaN。
```
```javascript
parseFloat(true)  // NaN
Number(true) // 1

parseFloat(null) // NaN
Number(null) // 0

parseFloat('') // NaN
Number('') // 0

parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```
##### 5.3isNaN()
isNaN方法可以用来判断一个值是否为NaN。
```javavscript
//但是，isNaN只对数值有效，如果传入其他值，会被先转成数值。比如，传入字符串的时候，字符串会被先转成NaN，所以最后返回true，这一点要特别引起注意。也就是说，isNaN为true的值，有可能不是NaN，而是一个字符串。

isNaN('Hello') // true
// 相当于
isNaN(Number('Hello')) // true

//但是，对于空数组和只有一个数值成员的数组，isNaN返回false。
```
`因此，使用isNaN之前，最好判断一下数据类型。`
```javascript
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value);
}
//判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断。
function myIsNaN(value) {
  return value !== value;
}
```
##### 5.4isFinite()
isFinite方法返回一个布尔值，表示某个值是否为正常的数值。
```javascript
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```
除了Infinity、-Infinity、NaN和undefined这几个值会返回false，isFinite对于其他的数值都会返回true。
