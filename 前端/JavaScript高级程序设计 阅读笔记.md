# JavaScript高级程序设计 阅读笔记

## 一 JavaScript简介

1. 一个完整的JavaScript实现应该由下列三种不同的部分组成：

   * 核心（ECMAScript），由ECMA-262定义，提供核心语言功能。
   * 文档对象模型（DOM），提供访问和操作网页内容的方法和接口。
   * 浏览器对象模型（BOM），提供与浏览器交互的方法和接口。

   > 1. ECMAScript就是实现ECMA-262标准规定的各个方面内容的语言描述。（ECMA-262标准规定这门语言由**语法、类型、语句、关键字、保留字、操作符、对象**组成。
   >
   > 2. 文档对象模型（DOM Document Object Model）
   >
   >    ​	DOM是针对XML但经过扩展用于HTML的应用程序编程接口（API），DOM把整个页面映射尾一个多层节点结构。
   >
   > 3. 浏览器对象模型（BOM Browser Object Model）
   >
   >    ​	支持可以访问和操作浏览器窗口的浏览器对象模型，开发人员可以使用BOM控制浏览器显示的页面以外的部分（BOM没有标准）

2.  **W3C ( World Wide Web Consortium , 万维网联盟）**

## 二 在HTML中使用JavaScript

### 2.1 < script >元素

1. HTML 4.01 为 <script> 定义了下列6个属性：

   1. async：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。
   2. charset：可选。表示通过src属性指定的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用。
   3. defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。
   4. language：已废弃。
   5. src：可选。表示包含要执行代码的外部文件。
   6. **type**：可选。可以看成是language的替代属性；表示编写代码使用的脚本语言的内容类型（也可称为MIME类型）。这个属性不是必须的，如果没有这个指定这个属性，则其默认值仍为 text/javascript。

2. 使用<script>元素嵌入JavaScript代码，只须为<script>指定type属性。

   ```html
   <script type="text/javascript">
       function sayHi(){ alert ("Hi!") };
   </script>
   ```

   包含在<script>元素内部的JavaScript代码将杯从上至下依次解释。

   使用这种方法嵌入JavaScript代码时，不要再代码中任何地方出现“</script>”字符串，会被认为是结束的</script>标签，需要改写为 `alert("\/script")`

3. 使用<script>元素来包含外部JavaScript代码，这时候src属性就是必须的。

   ```html
   <script type="text/javascript" src="example.js"></script>
   
   // 如果在XHTML文档中，也可以省略结束的</script>标签
   <script type="text/javascript" src="example.js" />
   // 但是，不能再HTML文档使用这种语法。原因是这种语法不符合HTML规范，也得不到某些（尤其是IE）浏览器的正确解析
   ```

   需要注意的是，带有src属性的<script>元素不应该再标签之间再包含额外的JavaScript代码。如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略。

#### 2.1.1 标签的位置

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Example HTML Page</title>
        <script type="text/javascript" src="example1.js"></script>
        <!-- 意味着所有外部文件都被下载、解析、执行完成后，才能开始呈现页面内容，导致出现明显延迟、窗口空白 -->
        
        <script type="text/javascript" defer="defer" src="example2.js"></script>
        <!-- 延迟脚本  脚本会被延迟到整个页面都解析完毕后再运行 -->
        <!-- HTML5规范要求按先后顺序执行，且先于DOMContentLoaded，但现实中并不一定会如此，因此最好只包含一个延迟脚本 （XHTML中，defer属性设置为defer="defer"） -->
        
        <script type="text/javascript" async="async" src="example3.js"></script>
        <!-- 异步脚本 并不保证按照指定它们的先后顺序执行，因此，确保两者之间互不依赖非常重要，建议异步脚本不要再加载期间修改DOM  （XHTML中，async属性设置为async="async"） -->
        <!-- 没有先后顺序却一定会在load事件前执行，DOMContentLoaded不一定。-->
        <!-- 支持异步脚本的浏览器有Firefox 3.6 、Safari 5 和 Chrome。-->
    </head>
    <body>
        <!-- 内容 -->
        
        <script type="text/javascript" src="example2.js"></script>
        <!-- 这样，在解析包含的JavaScript代码前，页面的内容将完全呈现在浏览器中。而用户也会因为浏览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了。 -->
    </body>
</html>
```

### 2.2 嵌入代码与外部文件

使用外部文件的优点：

* 可维护性
* 可缓存：如果有两个页面都使用同一个文件，那么这个文件只需下载一次
* 适应未来：通过外部文件来包含JavaScript无须使用XHTML或注释back

### 2.4 < noscript > 元素

符合下列任一条件，浏览器都会显示< noscript > 中的内容，而再除此之外的其他情况下，浏览器不会显示其中内容：

* 浏览器不支持脚本
* 浏览器支持脚本，但脚本被禁用

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Example HTML Page</title>
       <!-- ... --> 
    </head>
    <body>
        <noscript>
            <p>本页面需要浏览器支持（启用）JavaScript。</p>
        </noscript>
    </body>
</html>
```

## 三 基本概念

### 3.1 语法

1. 区分大小写： ECMAScript中的一切（变量、函数名、操作符）都区分大小写。

2. 使用 **Unicode** 字符集

3. 标识符：ECMAScript标识符采用小驼峰的命名格式

   * 第一个字符必须是字母、下划线或 $
   * 其他字符可以是字母、下划线、$ 或数字

4. 严格模式

   ```javascript
   function doSomething(){
       "use strict"; 
       // 函数体
   }
   ```

### 3.2 变量

ECMAScript的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。

### 3.4 数据类型

基本数据类型： Undefined、Null、Boolean、Number、String

复杂数据类型：Object

#### 3.4.1 typeof `操作符`

* “undefined”——如果这个值未定义
* “boolean”、“string”、“number”、“function”
* “object”——如果这个值是对象或null

```javascript
var message = "some string";
alert(typeof 95); 		//"number"
alert(typeof (message)); 	//"string"
```

**typeof是一个操作符而不是函数，**所以圆括号不是必须的

> 有些时候，typeof操作符会返回一些令人迷惑但是技术上却正确的值，比如，调用 typeof null 会返回 ”object“ ，因为特殊值null被认为是一个空的对象引用。Safari 5及之前版本、Chrome 7及之前版本在对正则表达式调用 typeof 操作符时会返回 “function”，而其他浏览器在这种情况下会返回 ”Object“。

#### 3.4.2 undefined 类型

未经初始化的值默认会去的undefined值。不过包含undefiend值的变量与尚未定义的变量还是不一样的。

````javascript
var message;	//这个变量声明之后默认取得了undefined值
// var age；  这个变量并没有声明

alert(message);		// "undefined"
alert(age);			// 产生错误
alert(typeof message);		// undefined"
alert(typeof age);		// undefined"
````

> 即使未初始化的变量会被赋予undefined值，但显式地初始化变量依然是明智的选择。如果能够做到这点，那么当 typeof 返回 “undefined”时，我们就知道被检测的变量还没有被声明，而不是尚未初始化。

#### 3.4.3 Null类型

​	从逻辑角度来看，null 值表示一个空对象指针，而这也正是使用typeof操作符检测null值会返回“object”的原因。

​	如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值，这样只要检查null值就可以知道相应的变量是否已经保存一个对象的引用。

​	实际上，undefined 值是派生自null值的，因此 ECMA-262 规定：

```javascript
alert( null == undefined ); 	//true
```

**只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null值。这样做不仅可以体现nukk作为空对象指针的惯例，而且也有助于进一步区分null和undefined。**

#### 3.4.4 Boolean类型

| 数据类型  | 转换为true的值              | 转换为false的值 |
| :-------: | :-------------------------- | :-------------- |
|  Boolean  | true                        | false           |
|  String   | 任何非空字符串              | “”（空字符串）  |
|  Number   | 任何非0数字值（包括无穷大） | 0 和 NaN        |
|  Object   | 任何对象                    | null            |
| Undefined | （不适用）                  | undefined       |

#### 3.4.5 Number类型

```javascript
// 八进制：第一位必须是 0 
var octalNum1 = 070;		// 八进制56
var octalNum2 = 079			// 无效的八进制数值 —— 解析为79

// 十六进制： 0x开头， 字母不分大小写
var hexNum1 = 0X1f;			// 十六进制的31
var hexNum1 = 0XA;			// 十六进制的10

// —— 在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换为十进制数值。

// 浮点数  由于保存浮点数值需要的内存空间是整数的两倍，所以ECMAScript会不失时机地将浮点数转换为整数
var floatNum1 = .1				// 有效，但不推荐
// 在默认情况下，ECMAScript会将小数点后面带6个0以上地浮点数转换为科学计数法
var floatNum2 = 3.125e7			// == 31250000
// 浮点数值的最高精度是17位小数，但在进行算数计算时其精确度远远不如整数
// 例： 0.1 + 0.2 = 0.30000000000000004 ！= 0.3

alert(Number.MIN_VALUE)			// ECMASCript能表示的最小数值，大多数浏览器中为5e-324
alert(Number.MAX_VALUE)			// ECMASCript能表示的最大数值，大多数浏览器中为1.7976931..e+308

// 超出数值范围的值，会被自动转换为特殊的 +-Infinity 值，该值不能参加计算。
// 确定一个值是不是有穷的 —— isFinite
var result = Number.MAX_VALUE + Number.MIN_VALUE;		// 可见这两个属性中分别保存着+—Infinity
alert(isFinite(result))			// false

// NaN（Not a Number)	1. 任何涉及NaN的操作都会返回NaN	2. NaN与任何值都不相等，包括其本身
alert( 4 / 0 )			// return NaN ， 在其他编程语言中则会报错
alert( NaN / 10 )		// return NaN
alert( NaN == NaN )		// return false
// isNaN() 判断参数是否“不是数值”，在接收到参数后，会尝试将参数转换为数值
alert(isNaN(NaN));			// true
alert(isNaN(10));			// false (10是一个数值)
alert(isNaN("10"));			// false（可以被转换为数值10）
alert(isNaN("blue"));		// true（不能被转换为数值）
alert(isNaN(true));			// false （可以被转换为数值1）
// isNaN() 甚至也适用于对象 —— 会首先调用对象的valueOf()方法，确定该方法返回值是否可以转换为数字，如果不能，则基于这个返回值再调用 toString() 方法，再测试返回值。而这个过程也是ECMAScript中内置函数和操作符的一般执行流程。
```

有三个函数可以把非数值转换为数值：Number() 、parseInt() 、parseFloat()。其中，Number()适用于任何数据类型，而另两个函数则专门用于把字符串转换为数值。

```javascript
// Number()  也适用于对象，与isNaN()规则相同
var num1 = Number(true);			// 1
var num2 = Number(null);			// 0
var num3 = Number(undefined); 		// NaN
var num4 = Number('0011');			// 11
var num5 = Number('0x1f');			// 31
var num6 = Number('');				// 0
var num7 = Number('hello world！');	// NaN

// parseInt()
var num8 = parseInt('  12');		// 12 parseInt会忽略字符串前面的空格
var num9 = parseInt('');			// NaN 第一个字符不是数字符号或负号
var num10 = parseInt('1234blue');	// 1234 解析数字知道遇到第一个非数字字符
var num11 = parseInt('22.5');		// 22 
var num12 = parseInt('0xA');		// 10 十六进制
var num13 = parseInt('070');		// 56 八进制
// 八进制时，ECMAScript 3 和 5 存在分歧，3 认为 56（八进制）， 5 认为 0（十进制）
// 在ECMAScript 5 中，parseInt()不具有解析八进制的能力，前导的0会被认为无效，从而将这个值当初’0‘
// 为了消除分歧，为这个函数提供第二个参数
var num12 = parseInt('0xAF');		// 175
var num14 = parseInt('0xAF',16);	// 175
var num15 = parseInt('AF',16);		// 175
var num16 = parseInt('AF');			// NaN

//parseFloat()
var num17 = parseFloat('22.5')		// 22.5
var num18 = parseFloat('22.34.5')	// 22.34  第二个小数点无效
var num19 = parseFloat('0xA');		// 0 始终忽视前导的0
var num19 = parseFloat('0908.5');	// 908.5
var num19 = parseFloat('24');		// 24（整型）
```

#### 3.4.6 String类型

>\f		换页符				\b		空格				\r		回车
>
>\xnn  		 以十六进制代码nn表示的一个字符（其中n为0-F）。例如，\x41表示 ‘A’
>
>\unnnn  	以十六进制代码nnnn表示的一个Unicode字符（其中n为0-F）。例如，\u03a3表示希腊字符Σ

```javascript
var text = "This is the letter sigma: \u03a3 ."
alert(text.length);		// 输出28 其中6个字符长的转义序列表示1个字符。
```

`ECMAScript 中的字符串时不可变的。`要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含数值的字符串填充该变量。

```javascript
// 转换为字符串
// 1. toString();		数值、布尔值、对象和字符串值都有这个方法，但是null和undefined没有。
var num = 10;
alert(num.toString());		// ’10‘		默认不必传递参数
alert(num.toString(2));		// ‘1010’	有需要也可以传递一个参数：输入数值的基数
alert(num.toString(16));	// ‘a'

// 2. 不知道要转换的值是不是null或undefined的情况下，可以使用转型函数String()
// （先将Object转换为primitive）使用内部函数ToPrimitive(String),与ToPrimitive(Number)类似，只是先调用obj.toString()，后调用obj.valueOf()。
var value1 = true;
var value2 = null;
var value3;
alert(String(value1));		// 'true'
alert(String(value2));		// 'null'
alert(String(value3));		// 'undefined'

// 3. "" + value	使用内部函数ToPrimitive(Number)(除了date类型)
// （先将Object转换为primitive）先调用obj.valueOf，若结果为primitive则返回；否则再调用obj.toString()，若结果为primitive则返回；否则返回TypeError。
alert('5' + 3 );		// '53'
alert( 5 + '3');		// '53'
alert( 3 + '' );		// '3'
alert( 5 - '3');		//  2
alert('5' - 3 );		//  2
```

#### 3.4.7 Object类型

```javascript
var o = new Object();
var o = new Object;		// 有效，但不推荐省略圆括号。
```

`在ECMAScript中，（就像Java中的 java.lang.Object 对象一样）Object 类型时所有它的实例的基础。`

Object 的每个实例都具有下列属性和方法：

* Constructor：保存着用于创建当前对象的函数。（构造函数）
* hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名(propertyName)必须以字符串形式指定。
* isPrototypeOf(object)：用于检查给定的属性是否能够使用for-in语句来枚举，与hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定。
* toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。
* toString()：返回对象的字符串表示，该字符串与执行环境的地区对应。
* valueOf()：返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同

> 从技术角度讲，ECMA-262中对象的行为不一定适用于JavaScript中的其他对象。浏览器环境中的对象，比如BOM和DOM中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262不负责定义宿主对象，因此宿主对象可能会也可能不会继承Object。

### 3.5  操作符

#### 3.5.1  一元操作符

1. 递增和递减操作符

```javascript
// 前置和后置
// 前置： 变量的值在语句被求值之前改变
var num1 = 2;
var num2 = 20;
var num3 = --num1 + num2;		// 21
var num4 = num1 + num2;			// 21
// 后置：在语句被求职之后才执行
var num1 = 2;
var num2 = 20;
var num3 = num1-- + num2;		// 22
var num4 = num1 + num2;			// 21
```

递增和递减操作符对任何值都适用：

* 能转换为数字的字符串/布尔值，则先将其转换为数字，再执行加减1的操作。字符串变量变成数值变量
* 不能转换的字符串，将变量的值设置为NaN。字符串变量变成数值变量
* 应用为对象时，先调用valueOf()方法以取得一个可供操作的值，再应用上述规则，如果结果是NaN，则调用toString()后再调用前述规则。对象变量成为数值变量。

```javascript
var s1 = '2';
var s2 = 'z';
var b = false ;
var f = 1.1 ;
var o = {
	valueOf: function(){
		return -1;
	}
}
s1++;		// 3
s2++;		// NaN
b++;		// 1
f--;		//	0.10000000000000000(由于浮点数舍入错误所致)
o--;		// -2
```

2. 一元加和减操作符

   ​	加号（+）放在数值前，对数值不会产生任何影响，减号（-）主要起到负号的作用，但是这两个操作符若是放在非数值前，会像Number()转型函数一样对这个值进行转换。

#### 3.5.2 位操作符

​	对于有符号的整数，32位的前31位用于表示整数的值，第32位用于表示数值的符号：0表示正数，1表示复数。这个表示符号的位叫做符号位。符号位的值决定了其他位数值的格式。

1. 正数（以纯二进制格式存储）						<img src="C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908151819189.png" alt="image-20200908151819189" style="zoom:80%;" />

2. 复数（二进制补码）

   >计算一个数值的二进制补码：
   >
   >1. 求这个数值绝对值的二进制码（例如，要求-18的二进制码，先求18的二进制码）
   >2. 求二进制的反码（即将0替换为1，1替换为0）；
   >3. 得到的二进制反码加1；![image-20200908152353953](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908152353953.png)

   ECMAScript会尽力向我们隐藏所有这些信息：

   ```
   var num = -18;
   alert(num.toString(2));		// "-10010"
   ```

   对数值应用位操作符时，后台会将 64位的数值转换为32位数值，然后执行位操作，最后再将32位的结果转换回64。导致负效应：**在对特殊的NaN和Infinity值应用位操作时，两个值都会被当作0来处理。

1. 按位非（NOT）   `~`

   执行按位非的结果就是返回数值的反码。**本质：操作数的负值-1。**![image-20200908153258274](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908153258274.png)

   按位非是数值表示的最底层执行操作，相比 num = - num - 1 速度更快。

2. 按位与（AND)    `&`

   按位与操作只在两个数值的对应位都是1时才返回1，任何以为都是0，结果都是0；

    ![image-20200908154754672](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908154754672.png)

3. 按位或（OR）   `|`

    ![image-20200908154839278](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908154839278.png)

4. 按位异或（XOR）    `^`

   在两个数值对应位上**只有一个1**时才返回1，如果对应的两位都是1或者0，则返回0.![image-20200908160349286](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908160349286.png)

5. 左移    `<<`

    左移并不会影响操作数的符号位。![image-20200908161111329](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908161111329.png)

6. 有符号右移    `>>`

   **以符号位的值来填充空位 **![image-20200908161148661](C:\Users\86137\AppData\Roaming\Typora\typora-user-images\image-20200908161148661.png)

7. 无符号右移    `>>>`

   这个操作符会将数值的所有32位都向右移动。对正数来说，无符号右移的结果与有符号右移相同。

   对于**负数**来说，**以0来填充空位**。

   ```
   // 无符号右移操作符会把复数的二进制当成正数的二进制码。而且，由于复数以其绝对值的二进制补码形式表示，就会导致无符号右移的结果非常大。
   var oldValue = -64;					// 等于二进制的 1111111111111111000000
   var newValue = oldValue >>> 5;		// 等于十进制的134217726
   // 无符号右移会把 -64 的二进制码当成正数的二进制码，换算成十进制就是 4294967232 ，再将这个值右移
   ```

#### 3.5.3 布尔操作符

1. 逻辑非    `!`

   ```javascript
   alert(!Object);		// false  对象
   alert(!'');			// true  空字符串
   alert(!'blue');		// false 非空字符串
   alert(!Infinity);	// false 任意非0数值
   alert(!null);		// true 
   alert(!NaN);		// true
   alert(!undefined);		// true
   ```

2. 逻辑与    `&&`

   * 如果第一个操作数是对象，则返回第二个操作数
   * 如果第二个操作数是对象，则只有再第一个操作数的求值结果为true的情况下才会返回该对象
   * 如果两个操作数都是对象，则返回第二个操作数
   * 如果有一个操作数是null / NaN / undefined，返回null / NaN / undefined

   ```javascript
   // 逻辑与操作属于短路操作，即如果第一个操作数能够解决结果，那么就不会再对第二个操作数求值。
   var found = true;
   var result = (found && someUndefinedVariable);	// error
   alert(result);	// 不执行
   
   var found = false;
   var result = (found && someUndefinedVariable);	// no error
   alert(result);	// 执行 false
   ```

3. 逻辑或    `||`

   与逻辑与（&&）一样的短路操作

   ```javascript
   // 可以利用短路操作的特点来避免为变量赋null和undefined值
   var myObject = preferredObject || backupObject ;
   // myObject 将被赋予等号后面两个值中的一个，如果preferredObject的值不是null，那么它的值将被赋给myObject
   ```

#### 3.5.4 乘性操作符

与Java、C中的相应用途类似，只是在操作数为非数值的情况下会执行自动的类型转换。非数值，先用Number()转换再己算

1. 乘法    `*`

   * Infinity * 0 = NaN
   * Infinity * (+-非0) = +- Infinity 
   * Infinity * Infinity = Infinity 

2. 除法    `/`

   * Infinity  / Infinity  = NaN
   * 0 / 0 = NaN
   * (+-非0)  / 0 = +- Infinity 
   * (+-非0) / Infinity = +- Infinity 

3. 求模    `%`

   var result = 26 % 5 ;     // 1

   * +Infinity  /  有限大 = NaN
   * 有限大  /  0 = NaN
   * Infinity / Infinity  = NaN
   * 有限大 / 无穷大 = 被除数

#### 3.5.5 加性操作符

1. 加法
   * Infinity  + Infinity   = Infinity  
   * +Infinity  + -Infinity   = NaN
   * +0 + -0 = +0
2. 减法
   * Infinity  - Infinity   = NaN
   * -Infinity - -Infinity = NaN
   * Infinity - -Infinity = Infinity
   * -Infinity - Infinity = -Infinity
   * +0 - +0 = +0
   * +0 - -0 = -0
   * -0 - -0 = +0
   * 