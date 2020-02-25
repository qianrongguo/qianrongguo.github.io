**1. 如何判断一个值是否为NaN？运行环境内建的方法isNaN()有坑**
第一种，根据上面的实验，我们可以先判断输入参数的类型是否为number，再调用isNaN方法，这样就避免了对于非数字类型的判断错误。代码如下：
```javascript
if(!Number.isNaN){
    Number.isNaN = function(n){
        return (
            typeof n === 'number' && window.isNaN(n)
        )
    }
}

Number.isNaN(5*'abc');    //true
Number.isNaN('what?');    //false
```
第二种，利用NaN的一个特性，它是JS语言中唯一一个不等于他本身的值，所以我们也可以这么写。
```javascript
if(!Number.isNaN){
    Number.isNaN = function(n){
        return n !== n;
    }
}

Number.isNaN(5*'abc');    //true
Number.isNaN('what?');    //false
```
还有一种，可以利用ES6中提供的Object.is()方法来进行验证
```javascript
Object.is(5*'abc', NaN);    //true
Object.is('what?', NaN);    //false
```
**2. 如何判断两个浮点数相等？如fn(0.1+0.2 , 0.3) => { /*返回true*/}**
Number.EPSILON 属性表示 1 与大于 1 的最小浮点数之间的差。
es6之前的版本：
```javascript
if(!Number.EPSILON){
    Number.EPSILON = Math.pow(2, -52);
}
```
也可以使用Number.EPSILON来比较连个值是否相等（在指定的误差范围内）：
```javascript
Math.abs(0.1 - 0.3 + 0.2) < Number.EPSILON
```
**3. 如何检测一个值是否整数？**
要检测一个值是否为整数，可以使用Es6的Number.isInterge(...)方法

```javascript
Number.isInterger(1.000);        //true
Number.isInterger(1);        //true
Number.isInterger('1');        //false
```
如果不允许使用ES6的话，可以自行写一个pollyFill方法。
```
if(!Number.isInterger){
    Number.isInterger = function(num){
        return (typeof num === 'number') && num%1 === 0;
    }
}
```
**4. 对于一个数字进行取整，你能说出多少种方法？**
`8.84|0;       //8`
`~~8.84;        //8`
`8.84>>0;       //8`
这三种方法都是可以的，分别说一下：

8.84|0或者 写成0 | 8.84 都是一样的，从语法上看，他是让0与指定值进行按位“或”运算，在JavaScript中，它先对指定值执行了ToInt32的转换，在按位进行或运算，所以最终结果就是把指定值转换为32位的整数。

而~~8.84也是对变量进行ToInt32的转换；再进行一步按位“取非”运算，即对每个字节进行反转；然后，再对结果再次“取非”。

那么8.84>>0的操作就同理可证了……

但是，上面的三种方法也是有其局限性的，因为他们是遵循ToInt32的转化规范，所以他们也只能对于32位的数字进行转换，所以再加上一个符号位，那么他们所能处理的数字范围在2的正负31次幂之间，即-2147483648 ≤ x ≤ 2147483647。
**5. 当一个变量显式类型转换时（利用Number()方法），遵循的规则是什么？**
ES5规范中规定了这个抽象操作ToNumber。

对于布尔型：true的结果为1，false的结果为0；

对于undefined： 结果为NaN

对于null：结果为0

对于字符串类型：遵循数字常量的相关规则和语法。处理失败时会返回NaN。

对于复杂类型：会先调用该值得valueOf()方法，如果有并且返回基本类型之，就是用该值进行强制类型转换。如果没有就是使用toString()的返回来进行强制类型转换。
  ```javascript
var test1 = {
    valueOf: function(){
		return 10;	//valueOf方法的将被调用；
	},
	toString: function(){
		return 666;
	}
}

Number(test1) ;   //10
2*test1;    //20


Number(test2);    //666
Number(test2)/2;    //333
```
**6. Number([])和Number([1,2,3])的值分别是什么？说明其原理？**
先调用valueOf()，再调用toString()方法，那么空数组和[1,2,3]有什么区别呢？

因为数字的valueOf()方法返回的是数组本身，不是一个基本类型，所以还会调用toString()方法；而数组的toString()方法返回的是数组各项通过逗号拼接一起的字符串（可以理解调用了Array.prototype.join(",")方法），所以空数组返回空字符串，转换为数组自然就是0；而数组[1,2,3]则只能转换为NaN了.

那么，大家觉得下面的代码应该输出什么呢？为什么？
`Number([100]);    //???`
**7. 讲一讲parseInt()方法遵循的运算规则？**
`parseInt('52px');        //52`
`parseInt(string, radix);`方法的接受两个参数：

string：
要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用 ToString 抽象操作)。字符串开头的空白符将会被忽略。

radix：
一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。比如参数"10"表示使用我们通常使用的十进制数值系统。始终指定此参数可以消除阅读该代码时的困惑并且保证转换结果可预测。当未指定基数时，不同的实现会产生不同的结果，通常将值默认为10。

返回值：

返回解析后的整数值。 如果被解析参数的第一个字符无法被转化成数值类型，则返回 NaN。

如果 parseInt 遇到了不属于radix参数所指定的基数中的字符那么该字符和其后的字符都将被忽略。接着返回已经解析的整数部分。

所以，这里就明白为什么字符串'52px'会被parseInt()解析为52，因为没有传递第二个参数radix，所以默认按照10进制进行解析，而字符'p'不在10进制内，所以字符'p'和后面的字符全部被忽略，直接返回数字52.

如果不亲自一试，你绝不会相信上面代码的输出是18。
`parseInt(1/0, 19);    //18`
这里需要知道的是，1/0运算结果是“无穷”，在JavaScript中为Infinity，而这个Infinity转换为字符串则为'Infinity'，第一个字符是'I'，在以19为基数时他的值为18。第二个字符‘n’不是一个有效的数字字符，所以除第一个字符外，后面的字符全部被忽略，所以最后就返回了18。
