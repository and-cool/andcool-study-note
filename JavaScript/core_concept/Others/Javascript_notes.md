# JavaScript 高级程序设计3 笔记

**ECMAScript 3.1 成为 ECMA-262 第 5 版，并于 2009 年 12 月 3 日正式发布。**

一个完整的 JavaScript 实现应该由下列三个不同的部分组成：
- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM）

## script元素
- **src**可选。表示包含要执行代码的外部文件。
- **type** 可选。可以看成是 language 的替代属性；表示编写代码使用的脚本语言的内容类型（也称为 MIME 类型）。
- **async** 可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。
- **defer** 可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。
- **charset** 不常用
- **language** 废弃，type代替。

## script位置
不要放在head里面，防止延迟加载，过慢。如果要放到head里面，可以引入外部文件，然后加入defer参数。
```html
<!DOCTYPE html>
<html>

<head>
    <title>Example HTML Page</title>
</head>

<body>
    <!-- 这里放内容 -->
    <script type="text/javascript" src="example1.js"></script>
    <script type="text/javascript" src="example2.js"></script>
</body>

</html>

<!DOCTYPE html>
<html>

<head>
    <title>Example HTML Page</title>
    <script type="text/javascript" defer="defer" src="example1.js"></script>
    <script type="text/javascript" defer="defer" src="example2.js"></script>
</head>

<body>
    <!-- 这里放内容 -->
</body>

</html>
```
## 文档模式
- 混杂模式（quirks mode）
- 标准模式（standards mode）
```html
<!-- HTML 4.01 严格型 -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
"http://www.w3.org/TR/html4/strict.dtd">
<!-- XHTML 1.0 严格型 -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!-- HTML 5 -->
<!DOCTYPE html>
```

## Noscript
``` html
<html>

<head>
    <title>Example HTML Page</title>
    <script type="text/javascript" defer="defer" src="example1.js">

    </script>
    <script type="text/javascript" defer="defer" src="example2.js">

    </script>
</head>

<body>
    <noscript>
        <p>本页面需要浏览器支持（启用） JavaScript。
    </noscript>
</body>

</html>
```

## 语法
区分大小写  
分号可有可无，但最好带上

## 严格模式 
"use strict";

## 变量
松散类型，可保存任何类型。  
**var** 操作符定义的变量将成为定义该变量的作用域中的局部变量。也就是说，如果在函数中使用 var 定义一个变量，那么这个变量在函数退出后就会被销毁。

## 数据类型
- **Undefined**  
  如：var message;  
  注意区分包含 undefined 值的变量与尚未定义的变量。但typeof返回的都是undefined。
- **Null**
- **Boolean**  
  true 和 false 是区分大小写的。  
  Boolean()用来强制转换
- **Number**  
  整数：55, 070, 0xa0  
  浮点：1.1, 3.125e7  
  范围：Number.MIN_VALUE ...... Number.MAX_VALUE;  
  超出范围：Infinity -Infinity, 使用isFinite()来判断。  
  **NaN**：Not a Number, 如除零操作。NaN 与任何值都不相等，包括 NaN 本身。使用isNaN()来判断。
  isNaN()在接收到一个值之后，会尝试将这个值转换为数值。
- **String**  
  ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变
  某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量
- **Object**包含一些属性和方法  
    - **constructor**：构造函数。
    - **hasOwnProperty(propertyName)**：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如： o.hasOwnProperty("name")）。
    - **isPrototypeOf(object)**：用于检查传入的对象是否是传入对象的原型。
    - **propertyIsEnumerable(propertyName)**：用于检查给定的属性是否能够使用 for-in 语句来枚举。与 hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定。
    - **toLocaleString()**：返回对象的字符串表示，该字符串与执行环境的地区对应。
    - **toString()**：返回对象的字符串表示。
    - **valueOf()**：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。

## 检测类型 ，typeof：
- "undefined"——如果这个值未定义或未初始化；
- "boolean"——如果这个值是布尔值；
- "string"——如果这个值是字符串；
- "number"——如果这个值是数值；
- "object"——如果这个值是对象或 null；
- "function"——如果这个值是函数。
> 从技术角度讲，函数在 ECMAScript 中是对象，不是一种数据类型。  
**typeof** 主要用来检测基本类型，对引用类型检测，作用不大。

> **undefined && null**   
只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存 null 值

### 类型检测，instanceof
返回True或False。
```javascript
result = variable instanceof constructor
```
### 数组检测
Array.isArray(value)

## 非数值转为数值
- Number()
- parseInt()
- parseFloat()

建议使用后面两个函数，转换更加合理。  
```javascript
var num1 = parseInt("1234blue");
var num = parseInt("0xAF", 16);
```
## 转为字符串
- **toString()**, 不适用于null和undefined
- **String()**

```javascript
var age = 11;
var ageAsString = age.toString(); // 字符串"11"
var found = true;
var foundAsString = found.toString(); // 字符串"true"

var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a"

var value1 = 10;
var value2 = true;
var value3 = null;
var value4;
alert(String(value1)); // "10"
alert(String(value2)); // "true"
alert(String(value3)); // "null"
alert(String(value4)); // "undefined"
```

## 相等操作符
- ==  相等和不相等——先转换再比较  
  比较的基本原则为，先转为可以比较的数值（Number），再进行比较。null 和 undefined是相等的。
- === 全等和不全等——仅比较而不转换。null 和 undefined不全相等的。

## for-in语句
for-in 语句是一种精准的迭代语句，可以用来枚举对象的属性。枚举顺序是不可预测的。
```javascript
for (var propName in window) {
document.write(propName);
}
```

## 函数
```javascript
function functionName(arg0, arg1,...,argN) {
statements
}
```
ECMAScript 中的参数在内部是用一个数组来表示。在函数体内可以通过 **arguments** 对象来
访问这个参数数组。可以利用此特性实现变参。

```javascript
function howManyArgs() {
alert(arguments.length);
}

howManyArgs("string", 45); //2
howManyArgs(); //0
howManyArgs(12); //1
```

>**ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数。**

## 作用域和内存

### 执行环境
**执行环境 (execution context)**。每个环境对应一个**环境变量**。环境中定义的所有变量和函数都保存在**环境变量**中。代码无法访问这个对象，但解析器在处理数据时会在后台使用它。

- **全局执行环境**  
是最外围的一个执行环境。在 Web 浏览器中，全局执行环境被认为是 **window** 对象，因此所有全局变量和函数都是作为 window 对象的属性和方法创建的。

- **函数执行环境**  
当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。 

### 生命周期
某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁。  
全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁。

### 作用域链
当代码在一个环境中执行时，会创建的一个作用域链(链上为变量对象们）。用来保证对执行环境有权访问的所有变量和函数的有序访问。

作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其**活动对象**作为变量对象。活动对象在最开始时只**包含**一个变量，即**arguments**对象（这个对象在全局环境中是不存在的）。

作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。(作用域链上保存的应该是和环境关联的**环境变量**，环境变量包含所有各自定义的变量和函数)。（注意：作用域链是在函数创建时形成，是静态的，而不是在执行阶段形成的，不是动态创建的）。

### 访问方式
内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。

每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境。

### 延长作用域链
- try-catch
- with
- 闭包

### 没有块级作用域
JavaScript 没有块级作用域，只有执行环境的概念。  
声明的某个变量，应该属于某个执行环境。

使用 var 声明的变量会自动被添加到最接近的环境中。在函数内部，最接近的环境就是函数的局部
环境；在 with 语句中，最接近的环境是函数环境。

如果初始化变量时没有使用 var 声明，该变量会自动被添加到全局环境。

### 管理内存
一旦数据不再有用，最好通过将其值设置为 null 来释放其引用——这个做法叫做解除引用（dereferencing）。

## 创建Object实例
有两种方式：
1. 使用 new 操作符后跟 构造函数。
1. 对象字面量表示法。

```javascript
var person = new Object();
person.name = "Nicholas";
person.age = 29;

var person = {
name : "Nicholas",
age : 29
};

var person = {
"name" : "Nicholas",
"age" : 29,
5 : true
};
```

在通过对象字面量定义对象时，实际上不会调用 Object 构造函数。

## 访问对象属性
1. 点表示法
2. 使用方括号

```javascript
alert(person["name"]); //"Nicholas"
alert(person.name); //"Nicholas"
```

## Array 类型
ECMAScript 数组的每一项可以保存任何类型的数据，而且数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。

如果索引小于数组中的项数，则返回对应项的值，就像这个例子中的colors[0]会显示"red"一样。

设置数组的值也使用相同的语法，但会替换指定位置的值。

如果设置某个值的索引超过了数组现有项数，如这个例子中的 colors[3]所示，数组就会自动增加到该索引
值加 1 的长度

```javascript
var colors = new Array();
var colors = new Array(20); // length: 20
var colors = new Array("red", "blue", "green");

var colors = Array(20);     // length: 20
var colors = Array("red", "blue", "green");

var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
var names = [];                        // 创建一个空数组
var values = [1,2,];                   // 不要这样！这样会创建一个包含 2 或 3 项的数组
var options = [,,,,,];                 // 不要这样！这样会创建一个包含 5 或 6 项的数组

colors.length = 2;  // 不是只读的，从2开始，后面的元素全部被删除
alert(colors[2]);   //undefined

colors[colors.length] = "black"; // 动态在尾部添加新元素。

/////// 转换字符串 ///////
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
alert(colors.toString());              // red,blue,green
alert(colors.join(","));               // red,green,blue
alert(colors.join("||"));              // red||green||blue


/////// 栈操作 ///////
var colors = new Array(); // 创建一个数组
var count = colors.push("red", "green"); // 推入两项
alert(count); //2

count = colors.push("black"); // 推入另一项
alert(count); //3

var item = colors.pop(); // 取得最后一项
alert(item); //"black"
alert(colors.length); //2


/////// 队列操作 ///////
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green"); //推入两项
alert(count); //2

count = colors.push("black"); //推入另一项
alert(count); //3

var item = colors.shift(); //取得第一项
alert(item); //"red"
alert(colors.length); //2


var colors = new Array(); //创建一个数组
var count = colors.unshift("red", "green"); //推入两项
alert(count); //2

count = colors.unshift("black"); //推入另一项
alert(count); //3

var item = colors.pop(); //取得最后一项
alert(item); //"green"
alert(colors.length); //2


/////// 排序 ///////
function compare(value1, value2) {
    if (value1 < value2) {
        return 1;
    } else if (value1 > value2) {
        return -1;
    } else {
        return 0;
    }
}

var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); //15,10,5,1,0

values.reverse(); //0,1,5,10,15
```

## Function 类型
函数实际上是对象。每个函数都是 Function 类型的实例，而且都与其他引用类型一样具有属性和方法。

```javascript
// 以下几种定义方式基本等价。
function sum (num1, num2) {
    return num1 + num2;
}

var sum = function(num1, num2){
    return num1 + num2;
};

var sum = new Function("num1", "num2", "return num1 + num2"); // 不推荐

// 多个名字
function sum(num1, num2){
return num1 + num2;
}
alert(sum(10,10)); //20

var anotherSum = sum;
alert(anotherSum(10,10)); //20

sum = null;
alert(anotherSum(10,10)); //20

```

函数没有重载，可以将函数名想象为指针。

### 函数声明与函数表达式
上述示例的区别在于此：
- 解析器会率先读取**函数声明**，并使其在**执行任何代码之前可用**（可以访问）
- 至于**函数表达式**，则必须等到解析器**执行到它所在的代码行，才会真正被解释执行**

```javascript
// 函数返回函数
function createComparisonFunction(propertyName) {
    return function(object1, object2){
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];
        if (value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        } else {
            return 0;
        }
    };
}

var data = [{name: "Zachary", age: 28}, {name: "Nicholas", age: 29}];
data.sort(createComparisonFunction("name"));
alert(data[0].name); //Nicholas

data.sort(createComparisonFunction("age"));
alert(data[0].name); //Zachary
```

### 函数内部属性
- 函数内部的对象
  - **arguments** （属于AO）
    - **callee**
    - **length**
    - **properties-indexes**(数字，转换成字符串)其值是函数参数的值（参数列表中，从左到右）。properties-indexes的个数 == arguments.length;
  - **this**  （属于AO）
  this 引用的是函数据以执行的环境对象。当在网页的全局作用域中调用函数时，this 对象引用的就是 window。

- 函数对象本身属性和方法
  - **caller**  这个属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 null。  
  利用 arguments.callee 和 arguments.callee.caller 来解耦。
  - **length**  
  表示函数希望接收的命名参数的个数
  - **name**  
  表示函数的名字。
  - **prototype**
  对于ECMAScript 中的引用类型而言， prototype 是保存它们所有实例方法的真正所在。诸如toString()和 valueOf()等方法实际上都保存在 prototype 名下，只不过是通过各自对象的实例访问罢了。在创建自定义引用类型以及实现继承时， prototype 属性的作用是极为重要的。（有些和C++的虚拟指针类似）
  - **[[prototype]]** 指向Function对象原型，继承下面上方法。
  - **apply()** this+数组 （继承Function对象的原型对象中）
  - **call()**  this+所有参数（变参） （继承Function对象的原型对象中）
  - **bind()**  this  （继承Function对象的原型对象中）

```javascript
window.color = "red";
var o = { color: "blue" };
function sayColor(){
    alert(this.color);
}

sayColor(); //"red"
o.sayColor = sayColor;
o.sayColor(); //"blue"

var sayGlobalCorlor = o.sayColor;
sayGlobalCorlor(); // "red" !!!

function sum(num1, num2){
    return num1 + num2;
}
function callSum1(num1, num2){
    return sum.apply(this, arguments); // 传入 arguments 对象
}
function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]); // 传入数组
}
alert(callSum1(10,10)); //20
alert(callSum2(10,10)); //20

function callSum(num1, num2){
    return sum.call(this, num1, num2);
}
alert(callSum1(10,10)); //20

// call和apply真正意义
window.color = "red";
var o = { color: "blue" };
function sayColor(){
    alert(this.color);
}

sayColor(); //red
sayColor.call(this); //red
sayColor.call(window); //red
sayColor.call(o); //blue

var objectSayColor = sayColor.bind(o);
objectSayColor(); //blue

```
## 面向对象
ECMA-262 把对象定义为：“无序属性的集合，其属性可以包含基本值、对象或者函数”。对象的每个属性或方法都有一个名字，而每个名字都映射到一个值。可以把 ECMAScript 的对象想象成散列表： 无非就是一组名值对，其中值可以是数据或函数。

### 对象的属性类型
- **数据属性**  
数据属性包含一个数据值的位置(slot)。在这个位置可以读取和写入值。数据属性有 4 个描述其行为的特性。
    - **[[Configurable]]**  
    表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。默认为true。**一旦把属性定义为不可配置的，就不能再把它变回可配置了**。
    - **[[Enumerable]]**  
    表示能否通过 for-in 循环返回属性。默认值为 true。
    - **[[Writable]]**  
    表示能否修改属性的值。默认值为 true。
    - **[[Value]]**
    包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined。

- **访问器属性**
    - **[[Configurable]]**   
    表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为true。
    - **[[Enumerable]]**  
    表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这个特性的默认值为 true。
    - **[[Get]]**  
    在读取属性时调用的函数。默认值为 undefined。
    - **[[Set]]**  
    在写入属性时调用的函数。默认值为 undefined。

访问器属性不能直接定义，必须使用 Object.defineProperty()来定义

```javascript
/// 数据属性
var person = {};
Object.defineProperty(person, "name", {
    writable: false,
    value: "Nicholas"
});

alert(person.name); //"Nicholas"
person.name = "Greg";
alert(person.name); //"Nicholas"


var person = {};
Object.defineProperty(person, "name", {
    configurable: false,
    value: "Nicholas"
});

alert(person.name); //"Nicholas"
delete person.name;
alert(person.name); //"Nicholas"

/// 访问器属性
var book = {
    _year: 2004, // 下划线是一种常用的记号，用于表示只能通过对象方法访问的属性
    edition: 1
};
Object.defineProperty(book, "year", {
    get: function(){
        return this._year;
    },
    set: function(newValue){
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
});

book.year = 2005;
alert(book.edition); //2

/// Multiple
var book = {};
Object.defineProperties(book,
{
    _year: {
        value: 2004
    },
    edition: {
        value: 1
    },
    year: {
        get: function () {
            return this._year;
        },
        set: function (newValue) {
            if (newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }
        }
    }
});

// 获取属性
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value); //2004
alert(descriptor.configurable); //false

alert(typeof descriptor.get); //"undefined"
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value); //undefined
alert(descriptor.enumerable); //false
alert(typeof descriptor.get); //"function"
```
## 创建对象

ECMAScript 中无法创建类，开发人员就发明了一种函数，用函数来封装以特定接口创建对象的细节。

- 工厂模式
- 构造函数模式
- 原型模式
- 构造&原型组合模式
- 动态原型模式
- 寄生构造模式
- 稳妥构造模式

### 工厂模式
解决了创建多个相似对象的问题  
未解决对象识别的问题（即怎样知道一个对象的类型）
```javascript
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function () {
        alert(this.name);
    };
    return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```

### 构造函数模式
构造函数始终都应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。  
任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；  
而任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样。

解决了工厂模式的类型判断问题，但每个实例指向的不同的成员函数实例。
```javascript
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function () {
        alert(this.name);
    };
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

// 对象的constructor（构造函数）属性。
alert(person1.constructor == Person); //true
alert(person2.constructor == Person); //true

// 构造函数 标识特定类型
alert(person1 instanceof Object); //true
alert(person1 instanceof Person); //true
alert(person2 instanceof Object); //true
alert(person2 instanceof Person); //true
```
- 没有显式地创建对象
- 直接将属性和方法赋给了 this 对象
- 没有 return 语句

1. 创建一个新对象；
2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
3. 执行构造函数中的代码（为这个新对象添加属性）；
4. 返回新对象。

### 原型模式
创建的每个函数都有一个 prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。

**可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值**。如果我们
在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。

问题：原型模式的最大问题是由其共享的本性所导致的。对于基本值类型，通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。然而，对于包含**引用类型值**的属性来说，问题就比较突出了。
```javascript
function Person() {
}
Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    friends: ["Shelby", "Court"], // 被共享了，由于并没有重写操作，所有子对象不会覆盖。
    sayName: function () {
        alert(this.name);
    }
};

var person1 = new Person();
var person2 = new Person();
person1.friends.push("Van");
alert(person1.friends); //"Shelby,Court,Van"
alert(person2.friends); //"Shelby,Court,Van"
alert(person1.friends === person2.friends); //true

```
实例一般都是要有属于自己的全部属性的。而这个问题正是我们很少看到有人单独使用原型模式的原因所在。
所以，引入构造&原型模型，取长补短。

```javascript
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
alert(this.name);
};

var person1 = new Person();
person1.sayName(); //"Nicholas"
var person2 = new Person();
person2.sayName(); //"Nicholas"
alert(person1.sayName == person2.sayName); //true
```

### 构造&原型组合模式
每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。
这种构造函数与原型混成的模式，是目前在 ECMAScript 中使用最广泛、认同度最高的一种创建自定义类型的方法。可以说，这是用来定义引用类型的一种**默认模式**。
```javascript
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}

Person.prototype = {
    constructor: Person, // 此时，constructor变为可枚举类型[[Enumable]] = true。
    sayName: function () {
        alert(this.name);
    }
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");

alert(person1.friends); //"Shelby,Count,Van"
alert(person2.friends); //"Shelby,Count"
alert(person1.friends === person2.friends); //false
alert(person1.sayName === person2.sayName); //true
```

### 动态原型模式
```javascript
function Person(name, age, job) {
    //属性
    this.name = name;
    this.age = age;
    this.job = job;

    //方法
    if (typeof this.sayName != "function") {
        Person.prototype.sayName = function () {
            alert(this.name);
        };
    }
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```

### 寄生构造模式
除了使用 new 操作符并把使用的包装函数叫做构造函数之外，这个模式跟工厂模式其实是一模一样的。构造函数在不返回值的情况下，默认会返回新对象实例。而通过在构造函数的末尾添加一个 return 语句，可以重写调用构造函数时返回的值。
```javascript
function Person(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function () {
        alert(this.name);
    };
    return o;
}

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName(); //"Nicholas"

///应用
function SpecialArray() {
    //创建数组
    var values = new Array();
    //添加值
    values.push.apply(values, arguments);
    //添加方法
    values.toPipedString = function () {
        return this.join("|");
    };
    //返回数组
    return values;
}
var colors = new SpecialArray("red", "blue", "green");
alert(colors.toPipedString()); //"red|blue|green"
```
### 稳妥构造模式
某些安全环境中会禁止使用 this 和 new 稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用 this；二是不使用 new 操作符调用构造函数。
```javascript
function Person(name, age, job) {
    //创建要返回的对象
    var o = new Object();
    //可以在这里定义私有变量和函数
    //添加方法
    o.sayName = function () {
        alert(name);
    };
    //返回对象
    return o;
}

var friend = Person("Nicholas", 29, "Software Engineer");
friend.sayName(); //"Nicholas"
```

## 继承
ECMAScript 只支持实现继承（不支持接口继承），而且其实现继承主要是依靠原型链来实现的。

### 原型链
```javascript
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function () {
    return this.property;
};

function SubType() {
    this.subproperty = false;
}

//继承了 SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function () {
    return this.subproperty;
};



var instance = new SubType();
alert(instance.getSuperValue()); //true
```
![prototype chain](https://raw.githubusercontent.com/HowieLiuX/Javascript_Core_Notes/master/JS_Core/images/prototype_chain.png)

由图可见，constructor在原型对象中，重新赋值的prototype，并没有constructor，还是在原型链中，直接继承了constructor。

确定实例和原型的关系：
1. **instanceof**  
    ```javascript
    alert(instance instanceof Object); //true
    alert(instance instanceof SuperType); //true
    alert(instance instanceof SubType); //true
    ```
2. **isPrototypeOf**  
    ```javascript
    alert(Object.prototype.isPrototypeOf(instance)); //true
    alert(SuperType.prototype.isPrototypeOf(instance)); //true
    alert(SubType.prototype.isPrototypeOf(instance)); //true
    ```
3. **getPrototypeOf** （继承中，判断确切类型）
   ```javascript
    alert(Object.getPrototypeOf(s1) == SubType.prototype); //true
    alert(Object.getPrototypeOf(s1) == SuperType.prototype); //false
    alert(Object.getPrototypeOf(s1) == Object.prototype); //false
   ```
在通过原型链实现继承时，**不能使用对象字面量创建原型方法**。因为这样做就会重写原型链。

> **问题**  
如果原型对象中包含 **引用类型的值**，该属性则被所有实例所共享。对该值的修改，会反映到所有实例中。

> 原型链的第二个问题是：在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。

### 解决原型链的问题，借用构造函数
```javascript
// 解决共享
function SuperType() {
    this.colors = ["red", "blue", "green"];
}
function SubType() {
    //继承了 SuperType
    SuperType.call(this);
}
//继承了 SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green"

// 参数传递
function SuperType(name) {
    this.name = name;
}
function SubType() {
    //继承了 SuperType，同时还传递了参数
    SuperType.call(this, "Nicholas");
    
    //实例属性
    this.age = 29;
}
var instance = new SubType();
alert(instance.name); //"Nicholas";
alert(instance.age); //29
```
> **新的问题**  
如果仅仅是借用构造函数，那么也将无法避免构造函数模式存在的问题——方法都在构造函数中定义，因此函数复用就无从谈起了。而且，在超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式。考虑到这些问题，借用构造函数的技术也是很少单独使用的。

### 组合继承（经典继承）
将原型链和借用构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有它自己的属性。

```javascript
// 父类构造函数
function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

// 父类原型共享
SuperType.prototype.sayName = function () {
    alert(this.name);
};

// 子类构造函数
function SubType(name, age) {
    //继承属性
    SuperType.call(this, name);
    this.age = age;
}

//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;

//子类新方法
SubType.prototype.sayAge = function () {
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27

// SubType.prototype中仍然会存在name和colors属性，只不过是在子对象中创建了这两个属性，
// 从搜索上屏蔽了这两个属性，如果执行如下，结构仍然能找到colors，找到的其实是原型对象中的colors。
// 此问题可通过 “寄生组合式继承” 来解决
delete instance1.colors;
alert(instance1.colors); //"red,blue,green"

```

### 原型式继承
通过赋值 函数对象的 prototype 到一个父类实例对象，父实例对象既可以是实例本身，也可以是原型对象。

此想法是借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。克罗克福德主张的这种原型式继承，要求你必须有一个对象可以作为另一个对象的基础。
```javascript
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}

// 示例
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
```
ECMAScript 5 通过新增 Object.create()方法规范化了原型式继承。
```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = Object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"

// 第二个参数
var anotherPerson = Object.create(person, {
    name: {
        value: "Greg"
    }
});
alert(anotherPerson.name); //"Greg"
```
不过别忘了，包含引用类型值的属性始终都会共享相应的值，就像使用原型模式一样。

### 寄生式继承（parasitic）
**寄生式继承**的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来**增强对象**，最后再像真地是它做了所有工作一样返回对象。
```javascript
function createAnother(original){
    var clone = object(original); //通过调用函数创建一个新对象
    clone.sayHi = function(){ //以某种方式来增强这个对象
        alert("hi");
    };
    return clone; //返回这个对象
}

var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"

```
> 使用寄生式继承来为对象添加函数，会由于不能做到函数复用而降低效率；这一点与构造函数模式类似。

### 寄生组合式继承
组合继承最大的问题就是无论什么情况下，都会调用两次超类型构造函数：
1. 一次是在创建子类型原型的时候
2. 另一次是在子类型构造函数内部。

子类型最终会包含超类型对象的全部实例属性，但我们不得不在调用子类型构造函数时重写这些属性。

所谓寄生组合式继承，**即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法**。其背
后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型
原型的一个副本而已。本质上，**就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型
的原型**。

```javascript
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype);    //创建对象,创建的是原型对象，
                                                    //所以不需要创建和初始化superType中的属性
    prototype.constructor = subType;                //增强对象
    subType.prototype = prototype;                  //指定对象
}
```

1. 创建超类型原型的一个副本
2. 为创建的副本添加 constructor 属性，从而弥补因重写原型而失去的默认的 constructor 属性。
3. 将新创建的对象（即副本）赋值给子类型的原型。

```javascript
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}

function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype);    //创建对象,创建的是原型对象，
                                                    //所以不需要创建和初始化superType中的属性
    prototype.constructor = subType;                //增强对象
    subType.prototype = prototype;                  //指定对象
}

function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function () {
    alert(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name);  // 只调用一次
    this.age = age;
}

inheritPrototype(SubType, SuperType); // name 和 colors也不会出现在原型中

SubType.prototype.sayAge = function () {
    alert(this.age);
};

```

## 函数表达式

函数声明，它的一个重要特征就是函数声明提升（function declaration hoisting），意思是在执行代码之前会先读取函数声明。这就意味着可以把函数声明放在调用它的语句后面。而函数表达式没有提升的特性。

### 匿名函数（anonymous function）表达式

```javascript
var functionName = function(arg0, arg1, arg2){
    //函数体
};
```

为何引入表达式？如下面的例子：
```javascript
//不要这样做！ 
if(condition){
    function sayHi(){
        alert("Hi!");
    }
} else {
    function sayHi(){
        alert("Yo!");
    }
}

//可以这样做
var sayHi;
if(condition){
    sayHi = function(){
        alert("Hi!");
    };
} else {
    sayHi = function(){
        alert("Yo!");
    };
}

// 递归用法
var factorial = (function f(num){
    if (num <= 1){
        return 1;
    } else {
        return num * f(num-1);
    }
});
```

表面上看，以上代码表示在 condition 为 true 时，使用一个 sayHi()的定义；否则，就使用另一个定义。实际上，这在 ECMAScript 中属于无效语法， JavaScript 引擎会尝试修正错误，将其转换为合理的状态。但问题是浏览器尝试修正错误的做法并不一致。大多数浏览器会返回第二个声明，忽略condition； Firefox 会在 condition 为 true 时返回第一个声明。因此这种使用方式很危险，不应该出现在你的代码中。

### 模仿块级作用域
```javascript
(function(){
    //这里是块级作用域
})();
```
这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。
一般来说，我们都应该尽量少向全局作用域中添加变量和函数。在一个由很多开发人员共同参与的大型
应用程序中，过多的全局变量和函数很容易导致命名冲突。而通过创建私有作用域，每个开发人员既可
以使用自己的变量，又不必担心搞乱全局作用域。

```javascript
(function () {
    var now = new Date();
    if (now.getMonth() == 0 && now.getDate() == 1) {
        alert("Happy new year!");
    }
})();
```
> 这种做法可以减少闭包占用的内存问题，因为没有指向匿名函数的引用。只要函数执行完毕，就可以立即销毁其作用域链了。

### 私有变量
严格来讲， JavaScript 中没有私有成员的概念；所有对象属性都是公有的。
任何在**函数中定义的变量**，都可以认为是私有变量，因为不能在函数的外部访问这些变量。私有变量包括**函数的参数**、**局部变量**和**在函数内部定义的其他函数**。

结合闭包，可以创建用于**访问私有变量的公有方法**。把有权访问私有变量和私有函数的公有方法称为**特权方法（privileged method）**。

#### 在构造函数中定义特权方法
```javascript
function MyObject() {
    //私有变量和私有函数
    var privateVariable = 10;
    function privateFunction() {
        return false;
    }
    
    //特权方法
    this.publicMethod = function () {
        privateVariable++;
        return privateFunction();
    };
}
```
在创建 MyObject 的实例后，除了使用 publicMethod()这一个途径外，没有任何办法可以直接访问 privateVariable 和 privateFunction()。

#### 静态私有变量
```javascript
(function () {
    //私有变量和私有函数
    var privateVariable = 10;
    function privateFunction() {
        return false;
    }
    
    //构造函数, (global对象)
    MyObject = function () {
    };
    
    //公有/特权方法
    MyObject.prototype.publicMethod = function () {
        privateVariable++;
        return privateFunction();
    };
})();


//静态变量
(function () {
    var name = "";
    Person = function (value) {
        name = value;
    };
    Person.prototype.getName = function () {
        return name;
    };
    Person.prototype.setName = function (value) {
        name = value;
    };
})();

var person1 = new Person("Nicholas");
alert(person1.getName()); //"Nicholas"
person1.setName("Greg");
alert(person1.getName()); //"Greg"

var person2 = new Person("Michael");
alert(person1.getName()); //"Michael"
alert(person2.getName()); //"Michael"
```

以这种方式创建静态私有变量会因为使用原型而增进代码复用，但每个实例都没有自己的私有变量。

####  模块模式
所谓单例（singleton），指的就是只有一个实例的对象。  

```javascript
// 字面量的方式来创建单例对象
var singleton = {
    name : value,
    method : function () {
        //这里是方法的代码
    }
};

// 模块模式来创建对象
var singleton = function () {
    //私有变量和私有函数
    var privateVariable = 10;
    function privateFunction() {
        return false;
    }
    
    //特权/公有方法和属性
    return {
        publicProperty: true,
        publicMethod: function () {
            privateVariable++;
            return privateFunction();
        }
    };
}();
```

如果必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有
数据的方法，那么就可以使用模块模式。以这种模式创建的每个单例都是 Object 的实例，因为最终要通
过一个对象字面量来表示它。事实上，这也没有什么；毕竟，单例通常都是作为全局对象存在的，我们不
会将它传递给一个函数。因此，也就没有什么必要使用 instanceof 操作符来检查其对象类型了。
####  增强的模块模式
```javascript
var singleton = function () {
    //私有变量和私有函数
    var privateVariable = 10;
    function privateFunction() {
        return false;
    }
    
    //创建对象
    var object = new CustomType();
    
    //添加特权/公有属性和方法
    object.publicProperty = true;
    object.publicMethod = function () {
        privateVariable++;
        return privateFunction();
    };
    
    //返回这个对象
    return object;
}();
```
## BOM
### windows
在浏览器中， window 对象有双重角色:
1. 通过 JavaScript 访问浏览器窗口的一个接口
2. ECMAScript 规定的 Global 对象

定义全局变量与在 window 对象上直接定义属性还是有一点差别：全局变量不能通过 delete 操作符删除，而直接在 window 对象上的定义的属性可以。
```javascript
var age = 29;
window.color = "red";

//在 IE < 9 时抛出错误，在其他所有浏览器中都返回 false
delete window.age;

//在 IE < 9 时抛出错误，在其他所有浏览器中都返回 true
delete window.color; //returns true

alert(window.age); //29
alert(window.color); //undefined
```
> 使用 var 语句添加的 window 属性有一个名为[[Configurable]]的特性，这个特性的值被
设置为false，因此这样定义的属性不可以通过delete 操作符删除

尝试访问未声明的变量会抛出错误，但是通过查询 window 对象，可以知道某个可能未声明的变量是否存在。

```javascript
//这里会抛出错误，因为 oldValue 未定义
var newValue = oldValue;
//这里不会抛出错误，因为这是一次属性查询
//newValue 的值是 undefined
var newValue = window.oldValue;
```
### 窗口关系及框架
如果页面中包含框架，则**每个框架都拥有自己的 window 对象**，并且保存在 frames 集合中。在 frames集合中，可以通过数值索引（从 0 开始，从左至右，从上到下）或者框架名称来访问相应的 window 对象。每个 window 对象都有一个 name 属性，其中包含框架的名称。
```html
<html>

<head>
    <title>Frameset Example</title>
</head>
<frameset rows="160,*">
    <frame src="frame.htm" name="topFrame">
    <frameset cols="50%,50%">
        <frame src="anotherframe.htm" name="leftFrame">
        <frame src="yetanotherframe.htm" name="rightFrame">
    </frameset>
</frameset>

</html>
```
