###基本语法

- `label`标签，配合`break`，`continue`跳出特定循环。

````
label:
	statement
````

- JS有三种方法，确定一个值到底是什么类型

>tpyeof 运算符

	返回值：number,string,boolean,function,undefined,
	经常用来判断是个值是否定义：if (typeof v === "undefined")

>instanceof 运算符（需要补充）
>Object.prototype.toString方法（需要补充）

- 由于字符串也是类似数组的对象，所以可以用`Array.protype.foeEach.call`遍历

- 遍历数组的方法：

````
var a = [1, 2, 3];

// for循环
for(var i = 0; i < a.length; i++) {
  console.log(a[i]);
}

// while循环
var i = 0;
while (i < a.length) {
  console.log(a[i]);
  i++;
}
//逆向遍历
var l = a.length;
while (l--) {
  console.log(a[l]);
}
````

- `toString()`方法
	+ 函数的`toString`方法返回函数的源码，包括注释。

- `arguments`对象
	+ `arguments.length`可以判断函数调用时到底带几个参数
	+ 将`arguments`转换为数组

````
var args = Array.prototype.slice.call(arguments);

// or

var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
````

- IIFE 完全匿名函数(立即调用的函数)几种写法

````
(function(){ /* code */ }());
// 或者
(function(){ /* code */ })();

var i = function(){ return 10; }();
true && function(){ /* code */ }();
0, function(){ /* code */ }();

!function(){ /* code */ }();
~function(){ /* code */ }();
-function(){ /* code */ }();
+function(){ /* code */ }();
````

- 判断一个值是否是`NaN`

````
function myIsNaN(value) {
  return value !== value;
}
````

- 将一个值转为整数的方法
	+ `parseInt()`方法
	`parseInt的返回值只有两种可能，不是一个十进制整数，就是NaN。`

- 不要使用“相等”（==）运算符，只使用“严格相等”（===）运算符。
- 建议自增（++）和自减（--）运算符尽量使用+=和-=代替。
- 建议避免使用switch...case结构，用对象结构代替。

````
function doAction(action) {
  var actions = {
    'hack': function () {
      return 'hack';
    },
    'slash': function () {
      return 'slash';
    },
    'run': function () {
      return 'run';
    }
  };

  if (typeof actions[action] !== 'function') {
    throw new Error('Invalid action.');
  }

  return actions[action]();
}
````