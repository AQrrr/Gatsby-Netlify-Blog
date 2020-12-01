---
title: JavaScript基础整理
date: "2020-10-16T23:46:37.121Z"
---
## 一、数据类型
基本数据类型 Number Srting Boolean Null Undefined BigInt Symbol
 Null Undefined的区别
 Null不是对象
 
引用数据类型
对象Object（包含普通对象-Object，数组对象-Array，正则对象-RegExp，日期对象-Date，数学函数-Math，函数对象-Function）
 <br/>
 ### 判断数据类型
 - typeof 判断基本数据类型  instanceof判断实例； <br/>
 
	typeof 对于基本类型来说，除了 null 都可以显示正确的类型。 <br/>

	typeof 对于对象来说，除了函数都会显示 object <br/>

- instanceof 可以判断对象类型。 

	instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型，不同环境下不是同一个构造函数<br/>
    判断基本数据类型可以使用`Symbol.hasInstance`
```js 
class PrimitiveNumber {
  static [Symbol.hasInstance](x) {
    return typeof x === 'number'
  }
}
console.log(111 instanceof PrimitiveNumber) // true
```
- Array.isArray() 方法 <br/>
	该方法用以确认某个对象本身是否为 Array 类型，而不区分该对象在哪个环境中创建<br/>

关于JS数据类型，更详细的推荐阅读这篇文章。

https://juejin.im/post/6844904190515380238#heading-51  by芋头


## 二、数据类型转换


基本数据类型转布尔，数字，字符
 - 转布尔
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e41ae1c2d895479b8a78d8bbe0c372d1~tplv-k3u1fbpfcp-zoom-1.image)
在条件判断时，除了 undefined， null， false， NaN， ''， 0， -0，其他所有值都转为 true，包括所有对象。
对象在转换类型的时候，会调用内置的 `[ToPrimitive]` 函数，对于该函数来说，算法逻辑一般来说如下：

	如果已经是原始类型了，那就不需要转换了
	调用 x.valueOf()，如果转换为基础类型，就返回转换的值
	调用 x.toString()，如果转换为基础类型，就返回转换的值
	如果都没有返回原始类型，就会报错

不同类型对象的valueOf()方法的返回值


| 对象| 返回值 |
| :------| ----------: | 
|Array  | 返回数组对象本身。 | 
| Boolean | 布尔值。  | 
|Date   |  存储的时间是从 1970 年 1 月 1 日午夜开始计的毫秒数 UTC。 | 
| Function  | 函数本身。  | 
|  Number	 |  数字值。 | 
|Object	|对象本身。这是默认情况。|
|String	|字符串值。|
| | Math 和 Error 对象没有 valueOf 方法。 |
- 转数字

调用 ToNumber 处理该值， 

| 参数 | 结果 |
| :------| ----------: | 
| Undefined | NaN | 
| Null | +0 | 
|Boolean| true，返回 1; false，返回 +0|
	
如果通过 Number 转换函数传入一个字符串，它会试图将其转换成一个整数或浮点数，而且会忽略所有前导的 0，如果有一个字符不是数字，结果都会返回 NaN<br/>
```JS
console.log(Number("000123")) //123
console.log(Number("123 123")) // NaN
console.log(Number("foo")) // NaN
```

- 转字符

使用 String 函数将类型转换成字符串类型
 
如果 String 函数不传参数，返回空字符串，如果有参数，调用 ToString(value)，而 ToString 也给了一个对应的结果表。<br/>

| 参数 | 结果 |
| :------| ----------: | 
| Undefined | "undefined"  | 
| Null | "null" | 
|Boolean| true，返回 "true"; false，返回  "false"|
| String | 返回与之相等的值  | 

Number	比较复杂，例子如下
```js
console.log(String(0))// 0
console.log(String(-0)) //0
console.log(String(8))//8
console.log(String(-8))//-8
console.log(String(NaN)) // NaN
console.log(String(Infinity)) // Infinity
console.log(String(-Infinity)) // -Infinity
```

- 转对象

基本数据类型通过调用 String()、Number() 或者 Boolean() 构造函数，转换为它们各自的包装对象。

null 和 undefined 属于例外，当将它们用在期望是一个对象的地方都会造成一个类型错误 (TypeError) 异常，而不会执行正常的转换。
```js
var a = 1;
console.log(typeof a); // number
var b = new Number(a);
console.log(typeof b); // object
```
更详细的内容参见https://github.com/mqyqingfeng/Blog/issues/159 by 冴羽
### 运算符
- **+运算符 <br/>**
1. 作为一元操作符：调用 `ToNumber` 处理该值

但当输入的值是对象的时候，先调用 `ToPrimitive(input, Number)` 方法，执行的步骤是：

	如果 obj 为基本类型，直接返回
	否则，调用 valueOf 方法，如果返回一个原始值，则 JavaScript 将其返回。
	否则，调用 toString 方法，如果返回一个原始值，则JavaScript 将其返回。
	否则，JavaScript 抛出一个类型错误异常。


举例：
	
```js
console.log(+[]); //0
console.log(+['1']); //1
console.log(+['1', '2', '3']); //NaN
console.log(+{});//NaN

```

以 +[] 为例，[] 调用 valueOf 方法，返回一个空数组，因为不是原始值，调用 toString 方法，返回 ""。 再调用 ToNumber 方法，"" 对应的返回值是 0，所以最终返回 0。

2. 二元操作符 
+ 作为二元操作符，简单的情况比如，`1 + "J"// "1J"`

具体的运算逻辑如下：

	当计算 value1 + value2时：
	lprim = ToPrimitive(value1) 
	rprim = ToPrimitive(value2) 
	如果 lprim 是字符串或者 rprim 是字符串，那么返回 ToString(lprim) 和ToString(rprim)的拼接结果 
	返回 ToNumber(lprim) 和 ToNumber(rprim)的运算结果 
   
 举例：
```js
console.log(null + 1); //1
console.log(undefined + 1); //NaN

```
第二行例子，

	 lprim = ToPrimitive(undefined) 因为undefined是基本类型，直接返回，所以 lprim = undefined；
	 rprim = ToPrimitive(1) ，基本类型，所以 rprim =1；
	 lprim 和 rprim 都不是字符串
	 返回 ToNumber(undefined) 和 ToNumber(1) 的运算结果 NaN

第一行例子运算过程和第二行基本一致，不同的是ToNumber(null) 是0，最终ToNumber(null) 和 ToNumber(1) 的运算结果是 1

更复杂的是数组+数组，涉及对象转字符，
比如
```js
console.log([] + []); // ""
```
	lprim = ToPrimitive([])，[]是数组，相当于ToPrimitive([], Number)，
    先调用valueOf方法，返回对象本身，因为不是原始值，调用toString方法，返回空字符串""
	rprim类似。
	lprim和rprim都是字符串，拼接操作

 
所以，[] + []相当于 "" + ""，最终的结果是空字符串""。 
这部分推荐阅读 [JavaScript 深入之头疼的类型转换(上)
](https://github.com/mqyqingfeng/Blog/issues/159) 讲得很详细。

 

**还有一些比较烦人的+运算符例子：**

```js
true + false //1
1 + [2,3,4] // "12,3,4"
[] + {}// "[object Object]"
'a' + + 'b' //  "aNaN" 因为 + 'b' 等于 NaN，所以结果为 "aNaN"，
 + '1' 的形式可以快速获取 number 类型。
```
- **==运算符 <br/>**
== 会先强制转型再做比较。一般转换为数值。<br/>
如果操作数是对象，会调用valueOf方法，得到基本类型，转换为数值再比较。<br/>
null 和 undefined相等，且比较前不能将二者转换成其它任何值。<br/>
```js
null == undefined //true
"NaN" == NaN //false
NaN == NaN //false
NaN != NaN //true
[] == ![] //true
 ```
 
== 中，左右两边都需要转换为数值然后进行比较。

[]转换为数字为0。

![] 首先是转换为布尔值，由于[]作为一个引用类型转换为布尔值为true,

因此![]为false，进而在转换成数字，变为0。

0 == 0 ， 结果为true

**=== 全等操作符**

比较前不进行类型转换，左右两边不仅值要相等，类型也要相等

## 三、 对象
 JS高程定义：对象(Object)，一组数据和功能的集合。
 
 MDN:成对的名称（字符串）与值（任何值），其中名称通过冒号与值分隔。
JavaScript中，几乎所有的对象都是Object类型的实例，它们都会从Object.prototype继承属性和方法。Object 构造函数为给定值创建一个对象包装器。

Object构造函数，会根据给定的参数创建对象，具体有以下情况：

如果给定值是 null 或 undefined，将会创建并返回一个空对象
如果传进去的是一个基本类型的值，则会构造其包装类型的对象
如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址。
当以非构造函数形式被调用时，Object 的行为等同于 new Object()。


**创建对象：**

1.字面量 (object literal)<br/>
简化包含大量属性的对象的创建过程；多个属性用逗号隔开，最后一个属性结尾不要加逗号。
```js
Var objectname = {
name1 : value1,
name2 : value2,
 
 …nameN : valueN
 }
 ```
使用对象字面量时，属性名也可以用字符串
```js
var person = {
"name" = "Amy",
"age" = 999
}
```
 花括号留空，定义只包含默认属性和方法的对象，
 ```js
 var person = {}
 person.name = "Amy";
 person.age = 999;
 ```
 
2.newObject


new Object([value])
```js
var person = new Object();
person.name = "Amy"；
person.age = 999；
```
3.自定义构造函数 

通过new构造函数创建多个对象 （构造函数命名遵循帕斯卡，大写第一个单词和后续单词首字母）
```js
function Hero(name, weapon){
            this.name = name;
            this.weapon = weapon;
        }
        var p1 = new Hero("铁头", ["刀", "左轮"]);
        var p2 = new Hero("肚财", ["飞船", "激光枪"]);
```
以这种方式调用构造函数实际上会经历一下4个步骤<br/>
（1）创建一个新对象，在内存中开辟一个空闲的空间<br/>
（2）将构造函数的作用域个给新对象（因此this就指向了这个新对象）<br/>
（3）执行构造函数中的代码（为这个新对象添加属性）<br/>
（4）返回新对象<br/>


4.使用工厂模式创建对象<br/>
function create 对象名(){}
```js
function createObject(name){
            var o = new Object();
            o.name = name;
            o.sayName = function(){
                alert(this.name);
            };
            return o;
        }

        var o1 = createObject("铁头");
        var o2 = createObject("肚财");

```

**访问对象的方法**

* 点表示法：alert(person.name); 最常用。
* 方括号表示法： alert(person["name"]); <br/>
优点是可以通过变量访问属性；<br/>
```js
var propertyName = "name";
alert(person["propertyName"])
```
以及属性名中包含特殊字符或关键字/保留字；
```js
person["first name"] = "Amy";
包含空格的属性名，可以通过方括号表示法访问属性
```



## 四、 函数
 函数实际上是对象，每个函数都是Function类型的实例。也可以理解为相对独立具有特点功能的代码块。
  
1. 函数声明

解析器会先读取函数声明，并使其在执行任何代码之前可以访问；
```js
function sum1(n1,n2){
    return n1+n2;
  };
```
2. 自定义函数（命名函数）
function fn(){};
```js
function fn1(){}
```
3. 函数表达式（匿名函数）

必须等到解析器执行到它所在的代码行才会真正被解释执行
```js
var fun = function (){};
```

4. 函数构造法，利用new Function，参数必须加引号
```js
var sum3=new Function('n1','n2','return n1+n2');
console.log(sum3(2,3));//5
一般不推荐用这种方法定义函数，因为这种语法会导致解析两次代码;
第一次是解析常规ECMAScript代码，第二次是解析传入构造函数中的字符串，从而影响性能
```
**箭头函数**

ES6新增，任何可以使用函数表达式的地方，都可以使用箭头函数。
```js
let arrowSum = (a, b) =>{
	return a + b;
    };
等同于
let sum = fucntion (a,b) {
	return a + b;
	};
```
只有一个参数，可以省略小括号
```js
let duble = x => { return 2 * x; };
```
无参数时需要小括号
```js
let getRandom = () => { return Math.random(); };
```