[TOC]
# JAVASCRIPT基础 DAY3 #
### JSON对象 ###

----------

>**JSON的重要性**

- JSON非常重要,经常和AJAX一起来工作.
- JSON是跨语言数据交互的重要实现方式！

>**什么是JSON**

- JSON本身就是一种数据类型,复合数据类型,是一种对象
- JSON主要用来存储数据
- 存储数据的方式:数组 变量 ,持久化(写入到硬盘):数据库 xml  JSON
- JSON的特点: 简洁  小  没有冗余数据 相对于 xml更精简
- JSON常见在客户端和服务器端之间进行数据通信,JSON主要存放对应从数据库中查询出来的数据,以JSON格式返回给客户端,客户端得到数据之后进行一个回显
- 在站点与站点之间,系统与系统之间需要进行数据接口交互数据,在交换数据的时候,经常返回的数据的格式就是JSON格式

>**JSON的声明**

- JSON的声明语法

```
var JSON的变量={
    键:值,
    键:值,
    键:值,
    键:function(){
    //方法的实现
    }	
}
```

- JSON对象的属性和方法的调用
    1. JSON对象.属性名
    2. JSON对象.方法名()
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var json = {
				key:'value',
				key1:'value1',
				key1:'value2',
				sport:function(){
					return '要想瘦，少吃肉';
				},
			};
			console.debug(json)
			var rst = json.sport();
			alert(rst);
		</script>
	</body>
</html>
```

### PHP中转化JSON格式 ###

----------

>**在php中将数组对象转换为JSON格式**

- json_encode(数组对象)

>**在php中将类对象转换为JSON对象**

- json_encode(实例化的类对象);

>**json格式的字符串转为JSON对象**

- json_decode(JSON格式的字符串)
- JSON格式的字符串必须要以以下格式:
- ‘{”键”:”值”,”键”:”值”.......}’
- 转换为JSON对象之后  就可以在php中用成员符来调用属性
- 注意:JSON格式的字符串中如果有双引号必须用单引号来包,如果用双引号来包单引号不可以
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var str = '{"name":"\u723d\u80a4\u6c34","shop_price":19.9,"stock":100}';
			var data = JSON.parse(str);
			var json = {name:'zhangsan',age:17};
			var json_str = JSON.stringify(json);
		</script>
	</body>
</html>
```

### 前端js对JSON的处理 ###

----------

>**将json字符串转为JSON对象**

- JSON:浏览器提供了一个JSON对象,这个对象可以来转换JSON格式的字符串
- JSON.parse(JSON格式的字符串):将JSON格式的字符串转为JSON对象
- 客户端（网页）需要从服务器端（如PHP）获取数据，这时候就出现了多程序/语言之间的通信，就会使用json

>**将JSON对象转为字符串**

- 将JSON对象转换为JSON格式的字符串:
- var JSON格式的字符串变量 =  JSON.stringify(JSON对象):
- 不是所有浏览器都有JSON对象,如果没有JSON对象就不能调用上面的parse和stringify方法
- 最佳解决方法:  用JQuery,在JQuery中有专门处理json的方法

### 一切皆为对象 ###

----------

>**js中的对象**

- 一切皆为对象
- Object:基类

>**对象属性和方法的调用**

- 对象.属性
- 对象.方法名([实参])

>**Window对象**

- 在浏览器中window对象是最大的对象,也就是其他的对象都可以通过window来访问
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			for(var i in window){
				document.write(i + '<br />');
			}
		</script>
	</body>
</html>
```

>**变量的作用域**

- 全局变量
    - 在函数体外声明的变量就是全局变量,在整个页面中都能得到它,全局变量一旦声明之后就会在window中,相当于window的一个属性可以通过window.变量来访问
- 局部变量
    - 在函数体内用var关键字声明的变量就是局部变量,它的作用域就只在函数体内,在函数体外不能得到它,此时在window中也不能得到 
- 注意:在函数体内不用var关键字声明的变量是全局变量,它的作用域在window中,通过window.变量名就能访问到
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var big_house = '小花';
			function demo(){
				var second_house = '小花花';
				third_house = '花花';
			}
			demo();//要想使用函数中的全局变量，需要先执行函数体
			console.debug(third_house);
			console.debug(big_house,second_house);//局部变量无法使用
		</script>
	</body>
</html>
```

>**变量的预编译过程**

- 变量的声明会被提前,优先声明处理,然后才是去执行其他的代码,比如赋值 打印等等

>**函数的预编译过程**

- 函数的声明也会提前,函数体内的变量声明都会提前
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var scope = 'global';
			function demo(){
				console.debug(scope);
				scope = 'local';
				console.debug(scope)
			}
			console.debug(scope);//global
			demo();//global   local
			console.debug(scope);//local
		</script>
	</body>
</html>
```

### 对象的属性管理 ###

----------

>**添加属性和方法**

- 给json对象添加属性和方法:
    - 添加属性: `JSON对象变量.属性名=属性值;`
    - 添加方法: `JSON对象变量.方法名=function(){//函数体}`
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var json = {
				'name':'张三',
				'age':17,
				'sayHello':function(){
					return '感觉今天好热啊...';
				},
			};
			json.tel = '18888888888';//属性添加
			json.name = 'borther four';//属性修改
			console.debug(json.sayHello());
			json.sayHello = function() {
				return '我建议以后夏天全校实行比基尼授课方式';
			};
			console.debug(json.sayHello());
			
			json.sayBye = function(){
				return '我妈喊我回家吃饭了';
			};
			
			console.debug(json.sayBye());
		</script>
	</body>
</html>
```

>**删除对象的属性和方法**

- delete JSON对象.属性名;
- delete JSON对象.方法名;
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var json = {
				name:'zhangsan',
				age:17,
				sayHello:function(){
					return '不想理你';
				}
			};
			console.debug(json.name);
			delete json.name;//删除属性
			console.debug(json.name);
			json.sayHello();
			delete json.sayHello;//删除方法
			json.sayHello();
		</script>
	</body>
</html>
```

>**对象的遍历**

- 对象里面的所有的属性和方法都可以遍历,for...in
- 用for...in循环的时候,对应的变量的值等于对象的属性名或者是方法名
- 如果需要取得对象的属性值或者是方法体  用数组下标的方式来获取 : 对象[变量]
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var json = {
				100:100,
				name:'zhangsan',
				age:17,
				sayHello:function(){
					return '不想理你';
				}
			};
			for(var i in json){
				console.debug(i,json[i]);
			}
			
		</script>
	</body>
</html>
```

### 函数的加强 ###

----------

>**函数的分类**

- 内置对象的函数
- 自定义的函数

>**函数的声明**

```
function 函数名([形参]){
//函数体
}	
```

>**函数的调用**

- 函数名([实参])
- 在哪些地方调用函数?
- 事件中:事件名=”函数名()”  比如:onclick=”check()”
- 在JS代码中直接调用:函数名()
- 函数赋值调用: DOM对象.事件=函数名;
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			body{
				height: 400px;
				background: greenyellow;
			}
		</style>
	</head>
	<!--1.直接在标签属性上调用-->
	<body onclick="fun()">
		<script type="text/javascript">
			function fun(){
				alert('have fun!');
			}
			
			fun();//2.js标签中直接调用
			
			//3.将函数对象赋值给事件
			document.body.onclick = fun;
			
			//4.将匿名函数声明赋值给事件
			document.body.onclick = function(){
				alert('匿名函数');
			};
		</script>
	</body>
</html>
```

>**函数的参数是函数**

- 函数的参数是函数,通常用作于回调函数,尤其是在JQuery中大量的运用了回调函数

```
function 函数1(函数2){
//执行函数1的函数体
//调用函数2
函数2();
}
```

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			function father(son){
				console.debug('叫爸爸');
				son();
			}
			//函数名称是tim，将会被father调用
			function tim(){
				console.debug('你养我小，我养你老！');
			}
			father(tim);
		</script>
	</body>
</html>
```

>**匿名函数**

- 第一种匿名函数

```
var 变量|对象 = function(){
//函数体
 }
```

- 这种匿名通常用作函数的赋值,可以解决函数被重复赋值的问题
- 此时可以将变量名称看做函数的名字
- 第二种匿名函数:完全匿名函数

```
(funtion(){
//函数体
})();
```

- 完全匿名函数找不到名字,程序一旦执行就运行
- 完全匿名函数可以完全解决函数被重复赋值的问题
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			//匿名函数
			var sum = 10;
			sum = function(i) {
				return 100 + i;
			}
			console.debug(sum(100));
			//完全匿名函数
			(function() {
				console.debug('完全匿名函数');
			})();
		</script>
	</body>
</html>
```

>**函数的返回值可以是函数**

- 作用域:局部变量的作用域是在函数体内,全局变量的作用域在当前的窗口window中都能找到
- 作用域链:在函数执行过程中,函数体内的变量先在函数体内找,如果找到就直接用,如果找不到对应的变量就去函数体外找对应的变量,如果找到就用,找不到就去window对象中去找,找到就直接用,找不到就报错,报变量未被定义(not defined)的错误:
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			function father(){
				return function(i,j){
					return i+j;
				};
			}
			
			var returnFunc = father();
			console.debug(returnFunc);
			console.debug(returnFunc(100,200));
		</script>
	</body>
</html>
```

- 闭包
    - 如果希望在函数体外去改变函数体内局部变量的值,由于函数体内的局部变量的作用域只在函数体内
    - 在函数体外无法找到函数体内的局部变量,所以不能直接取改变函数体内的局部变量的值
- 如何改变局部变量的值呢？
    1. 函数必须要有一个返回值,而且这个返回值是一个函数,在返回的这个函数中就可以去改变局部变量
    2. 当用到闭包的时候,原来函数体内的变量并没有被销毁,存放在内存中,就是因为一直存放在内存中,所以调用返回的那个函数时就可以继续得到这个变量 而且还可以改变它
    3. 闭包不能大量的运用,占据内存空间 ,如果数据量比较大可能会影响到效率,用的时候要谨慎使用
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			function demo(){
				var count = 0;
				return function(){
					count++;
					console.debug(count);
				}
			}
			var returnFun =demo();
			returnFun();
			returnFun();
		</script>
	</body>
</html>
```

### 函数对象 ###

----------

>**函数对象:将函数本身自己看做对象**
>**函数对象添加属性和方法**

- 添加属性:函数名.属性名=属性值;
- 添加方法:函数名.方法名=function(){//函数体的实现}
- 调用:
    - 函数名.属性名;
    - 函数名.方法名();

>**函数的参数**

- 在JS中函数调用时,传入的实参的个数可以大于形参
- 在函数体内可以得到传入的实参的个数以及参数的值
    - 获取参数的个数:arguments.length
    - 获取参数的值:arguments[下标]
- 实例

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>

	<body>
		<script type="text/javascript">
			function avg() {
				//获取实参个数
				var length = arguments.length;
				//获取实参数据
				console.debug(arguments);
				var sum = 0;
				for(var i = 0; i < length; ++i) {
					sum += arguments[i];
				}
				return sum/length;
			}
			console.debug(avg(100, 50));
		</script>
	</body>
</html>
```

>**函数当做类对象来使用**

- 将函数当做类来使用
- 实例化的时候用new关键字来实例化
- 通常在基于面向对象编程中会经常用到
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			//函数也可以看做是类，就可以使用new创建
			function Student(name,age){
				this.name = name;
				this.age = age;
				console.debug(this);//在类内部可以使用this代表调用此类的对象
			}
			//由于是类，所以可以使用new
			var stu1 = new Student('zhangsan',20);
			var stu2 = new Student('lisi',30);
			console.debug(stu1,stu2);
		</script>
	</body>
</html>
```

>**对象的方法调用**

- 原始的调用方法:对象.方法(实参);
- 对象.方法名.call(调用者,参数):
- 对象的某个方法被调用者来调用,此时的this指的就是调用者
    - this:谁发起的调用this指的就是谁
- 对象.方法名.apply(调用者,[数组形式传递参数]):
   - 作用和功能和上面call方法的一样,只是传递参数的方式不一样而已
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var person = {
				name:'张三',
				run:function(num,time){
					console.debug(this.name + '每天跑' +time+'小时'+ num +'公里！');
				},
			};
			person.run(5,3);
			person.run(10,4);
			
			person.run.call(person,8,2);
			var name = 'lisi';
			person.run.call(window,9,5);
			
			
			//使用数组的形式传递实参列表
			person.run.apply(window,[10,3]);
		</script>
	</body>
</html>
```

### 原型对象prototype ###

----------

- 原型对象:在JS中的所有的数据类型以及函数都有一个prototype属性,调用prototype属性相当于返回当前数据类型的原型引用.
- 原型对象有什么用:主要是用来扩展不同的属性和方法
- 常见的数据类型原型对象语法规则
    - 扩展属性:
        - 数据类型.prototype.属性名=属性值;
    - 扩展方法:
        - 数据类型.prototype.方法名=function([形参]){//函数体}
- 函数中运用prototype:
- 语法规则:

```
函数名.prototype.方法名=function(){
//函数体的实现
}
```

- 实例化对应的函数对象,然后直接调用对应的方法
- 经常用到的地方:
    - 在项目中,如果你的项目引入了其他的JS插件,在对应的插件中封装了很多的函数类对象,
    - 这时候如果需要在它原来的类对象基础上添加属性和方法的时候:
    - 不是直接去改变它的源码
    - 而是通过prototype属性来扩展对应的属性和方法 
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			//可以在代码中修改原来的对象，每一个对象的原始定义可以从其prototype获取
			//找到了prototype就等于找到了原始定义代码
			Number.prototype.sum = function(num){
				console.debug(this.valueOf() + num);
			}
			var num1 = 100;
			num1.sum(5);
			var num2 = 200;
			num2.sum(33);
		</script>
	</body>
</html>
```


:octocat:———— end ————:octocat:
------------------------------