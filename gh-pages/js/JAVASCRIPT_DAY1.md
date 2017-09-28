# JAVASCRIPT基础 DAY1 #
### JS介绍 ###

----------

>**Js的由来**

- 网景公司:NetScape,js最初是设计在运行在浏览器上的一种脚本语言livescript
- 1996年的时候Sun公司的Java炙手可热，NetScape想搭顺风车，于是某产品经理，命令更名
- ECMAScript:ECMA制定了ECMAScript标准,定义了脚本语言的语法规则以及常用的内置对象

>**Js的作用**

- 页面效果
- 表单的验证
- 页面跳转
- 页面局部刷新:AJAX 

>**专业术语**

- ECMAScript:提供脚本语言的语法规则和常用的内置对象
- DOM:document object model文档对象模型 提供对页面操作的接口和函数对象
- BOM:browser object model 浏览器对象模型  提供操作浏览器的接口和内置对象

>**Js的特点**

- 基于对象设计的语言
- 弱类型语言   变量可以存储不同类型的值
- 基于事件驱动的脚本语言
- 解释性语言   java、c、c++ 需要编译
- 很好的跨平台 跨操作系统/跨设备
- 具有良好的扩展性
- **js,是一种基于对象的脚本语言,弱类型 解释型 跨平台 基于原型设计的一种脚本语言**

> **JS的标签和注释**

- 标签

```
<script type=”text/javascript”>
//js代码
</script>
```

- 注释

```
单行: //
多行:`/*注释内容*/`
```

### JS的用法 

----------

**Js写在标签属性中**

>语法

`事件名=”js代码”`

>超链接标签: a

- A标签本身是页面刷新和跳转,如果希望页面不刷新也不跳转
- 如果需要执行对应的js代码,直接在href属性上添加js
- `href = “javascript:函数的调用|js代码”`

>void操作符

- 阻止页面跳转和刷新可以用void
- `href=”javascript:void(0)”`
- 通过采用void 0更靠谱更安全，应该优先采用void 0这种方式
- 填充`<a>`的href确保点击时不会产生页面跳转; 填充`<image>`的src，确保不会向服务器发出垃圾请求

>**Js写在script标签中**

>最佳出现位置

- `<head>和</head>之间,</head>之前`
- `<body>和</body>之间,紧跟在</body>结束之前`

>**外链模式**

- 语法:`<script src=”js文件的路径” type=”text/javascript”></script>`
- 作用:js和html的分离  简洁  维护  重用性

### JS的输出 ###

----------

>**文档输出**

- document.write();
- 向页面或者是文档中写入内容:可以是文本 也可以是html代码

>**对话框输出**

- alert(“提示信息”):提示警告对话框
- confirm(“提示的内容”):确认提示对话框
    - 返回一个布尔值,如果点击确认返回true  点击取消返回false
- prompt(“提示信息”[,默认输入值]):提示输入对话框
    - 返回用户输入的内容,如果点击取消的返回null
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>输出</title>
	</head>
	<body>
		111
		<script type="text/javascript">
			alert('我先走');
			//用于在网页中输出一些内容
			document.write("再瞅试试");
			document.write('<h1>乱弹琴</h1>');
			
			//对话框/弹出框
			//警告提示框
			alert('弹弹弹！');
			//确认框，返回值如果点击了确定就是true，点击了取消就是false
			var rst = confirm('你真的要狠心离开我吗？');
//			alert(rst);
			//输入对话框，点击了确定就是输入的内容，取消就是null
			var name = prompt('请输入你爱人的名字');
			alert(name);
		</script>
	</body>
</html>
```

>**控制台输出**

- console.debug(变量 | 对象| 值): 调试
- console.log(内容):输出日志内容
- console.dir(对象或者数组):查看有层次的对象或者是数组的结构
- 实例
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>console调试输出</title>
	</head>
	<body>
		<script type="text/javascript">
			var name = 'kunx';
			console.debug('今天天气不错，挺风和日丽的！\r\nhaha');
			console.debug(name);
			console.error('我犯了错误，请你纵容我');
			console.log('今天天气不错，挺风和日丽的！\r\nhaha');
			console.log('%c'+name,'color:blue;font-size:20px');
			
			var stu_list = [
				'小花','小花花','小花花花'
			];
			console.debug(stu_list);
			console.log(stu_list);
			console.dir(stu_list)
		</script>
	</body>
</html>
```

### JS的变量 ###

----------

- 变量的命名规则:字母 数字 下划线组成 不能以数字开头
- 变量的声明:`var 变量名;`
- 多个值相同的变量：`var 变量名=变量名2=变量名3...=值;`
- 变量的赋值:`var 变量名=值;`

### Js的数据类型 ###

----------

>**基本数据类型**

- 数字类型:Number(整数 小数)
- 字符串类型:String   单双引号完全一致，都不解析变量   使用\转义
- 布尔类型:Boolean (true  false)


>**复合数据类型**

- 数组的声明
    1. var 数组的变量名 = new Array(); 声明一个空的数组
    1. var 数组的变量名 = new Array(size);声明一个指定数组长度的数组
    1. var 数组的变量名 = new Array(元素1,元素2,元素3...); 声明带有元素的数组
    1. var 数组变量名 = [元素1,元素2,元素....];

- Json对象

```
  var json对象变量名 = {
    键:值,
    键2:值2,
    键3:值3,
    ......
    };
```

- 对象:属性和方法(函数)组成
- 调用对象的属性和方法: 对象.属性|方法()

>**其他**

- undefined:变量声明了但是没有赋值
- null:空

>**获取数据类型**

- typeof 变量|对象|值:  返回的是对应的数据类型的字符串的值
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>对象</title>
	</head>
	<body>
		<script type="text/javascript">
			//定义对象
			var person = {
				name:"小花",
				age:16,
				sex:'女',
				sayHello:function(){
					console.debug('在我心里有个家，家里早已有了他');
				},
			};
			console.debug(person)
			
			console.debug(typeof person, typeof 111,typeof 111.111,typeof []);
		</script>
	</body>
</html>
```

>**类型之间的自动转换**

- 字符串优先级>数字>null
    - null和num或字符串做运算时,自动向数字或者是字符串转换
    - 布尔值和数字或者是字符串做运算时,自动向数字或者是字符串转
    - 数字和字符串做运算:数字自动向字符串转换
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
			//不同类型进行运算，会自动类型转换，字符串>数字>null|undefined|bool
			var flag = true;
			var num = 99;
			var str = '1';
			var nul = null;
			sum = num+flag;
			console.debug(flag + '+' + num + '=',sum);
			
			var sum2 = flag+str;
			console.debug(flag + '+' + str + '='+sum2);
			
			var sum3 = flag+nul;
			console.debug(flag + '+' + nul + '='+sum3)
			
			var sum4 = str + num;
			console.debug(str + '+' + num + '='+sum4);
			var sum5 = nul+num;
			console.debug(nul + '+' + num + '=' + sum5);
			
			//强制类型转换
			//将字符串转换为number类型
			var v1 = Number('100');//100
			var v2 = Number(null);//0
			var v3 = Number(undefined);//NaN
			var v4 = Number(true);//1
			var v5 = Number('hello100');//NaN
			console.debug(v1,v2,v3,v4,v5);
			
			var sp = NaN;
			console.debug(sp==1,sp==NaN);
			console.debug(isNaN(sp),isNaN(100),isNaN(undefined),isNaN('HELLO100'))
			
			//转换成整数
			console.debug(parseInt(100),parseInt(100.100),parseInt('29.98'),parseInt('abcd100'),
                            parseInt('100abcd'),parseInt(null),parseInt(true))
			
			//转换成浮点数,null undefined bool都无法转换成浮点数
			console.debug(parseFloat(false),parseFloat(null));
		</script>
	</body>
</html>
```

>**强制转换**

- Number(变量|值):将其他的变量或者是值转换为Number类型
- 如果传入的参数是布尔值或者null值可以转换  true=1 false=0 null=0
- 如果传入的参数是数字型的字符串直接转换成功
- 如果是非数字型的字符串(完全是数字)不能转换成功,返回NaN
- NaN:not a number  不是一个数字
- NaN通常用来判断一个字符串是否是一个数字型的字符串
- isNaN(变量|值):判断变量或值是否是一个数字型的字符串  不是一个数值型的字符串返回true否则返回false

>**转为整数**

- parseInt(变量|值):将指定的变量或者是值转换为整数
- 不能转换布尔值和null 
- 如果传入的参数本身是数字,取数字的整数部分,如果是小数,直接去掉小数部分取整
- 如果传入的参数是数字型的字符串,直接取整数部分
- 如果传入的参数是以数字开头的字符串,取开头的整数部分,后面不要
- 如果是非数字开头的字符串,返回NaN

### JS的运算符 ###

----------

>**算术运算符**

- `+ - * / % ++ --`

>**逻辑运算符**

- `&& || !`

>**比较运算符**

- `> < >= <= == != === !==`

>**赋值运算符**

- `= += -= *= /= %=`

>**位运算符**

- `&:按位与 |:按位或 ~:取反 ^:异或（相同取0，相异取1） >>:右移 <<:左移`

>**字符串拼接**

- `+ +=`

>**三元运算符**

- `表达式?A:B   :当表达式为真的时候返回A,否则返回B`

### JS的流程控制 ###

----------

>**选择控制**

```
    If(条件表达式){
    //执行的语句组
    }else if(条件表达式2){
    //执行的语句
    }else if(条件表达式2){
    //执行的语句
    }...else{
    //当前面所有的条件都不满足的情况下 需要执行的语句块
    }

    switch(变量值){
    case 值:
    语句;
    break;
    case 值2:
    语句;
    break;
    case 值3:
    语句;
    break;
    .....
    default:
    语句;
    break;
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
			var score = 85;
			//如果分数>90优秀
			if(score >= 90) {
				console.debug('优秀');
				//如果分数>70良好
			} else if(score >= 70) {
				console.debug('良好');
				//如果分数>60及格
			} else if(score >= 60) {
				console.debug('及格');
				//不及格
			} else {
				console.debug('不及格')
			}

			//状态分支语句
			switch(parseInt(score / 10)) {
				case 10:
				case 9:
					console.debug('优秀');
					break;
				case 8:
				case 7:
					console.debug('良好');
					break;
				case 6:
					console.debug('及格');
					break;
				default:
					console.debug('不及格');
					break;
			}
		</script>
	</body>

</html>
```

>**循环控制**

```
        for循环:
        for(初始值;结束条件;改变变量){
        //语句
        }
        while循环:
        while(条件){
        语句;
        改变条件
        }
        do{}...while()
        
        for... In循环
        for(var 变量|下标 in  循环的对象){
        //arr[下标]
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
//			var sum = 0;
			//for循环，3个语句都可以写多个，也可以省略，但是要小心写成死循环
			for(var i=1,sum=0;i<=100;++i){
				sum+=i;
			}
			console.debug(sum)
			
			//while循环
			var j=1;
			var sum2 = 0;
			while(j<=100){
				sum2+=j;
				j++;
			}
			console.debug(sum2)
			
			//do ... while最少执行一次
			var k = 1;
			var sum3 = 0;
			do{
				sum3+=k;
				k++;
			}while(k<=100);
			console.debug(sum3)
			
			
			//数组对象专用循环
			var arr = ['猪肉','牛肉','狗肉','莴笋','豆腐'];
			for(var i=0;i<arr.length;++i){
				console.debug(i,arr[i]);
			}
			for(var i in arr){
				console.debug(i,arr[i]);
			}
			
			var person = {
				name:'四哥',
				age:29,
				sex:'爷们',
			};
			for(var i in person){
				console.debug(i,person[i]);
			}
			
			for(var i=1,sum=0;;i++){
				if(i==99){
					continue;
				}
				if(i>100){
					break;
				}
				sum+=i;
			}
			console.debug(sum);
		</script>
	</body>
</html>
```

>**break和continue**

- break:跳出当前循环
- continue:跳出当前循环的本次循环直接开始下一次循环
- return:直接结束函数或者方法,返回值

### 数组 ###

----------

**数组的遍历**

>标准for循环和for in循环的区别

- 标准for循环中的初始化的变量类型是数字型
- 在for in中初始化的变量是string字符串类型,如果要对变量做运算的时候,需要转换

>**数组的长度**

- 数组对象.length   
- 数组对象有一个属性length,返回数组的长度,通常在循环和遍历中会用到,也可以来判断数组中的元素个数

>**删除数组元素**

- delete 元素;
- 删除后.length 长度没有变化,值变化了

>**常用的方法**

- 数组对象.push(数组元素可以是多个):
- 在数组对象的尾部加入新的元素,返回最新数组的长度
- 数组对象.pop():删除数组中最后一个元素,并返回删除的元素
- shift():删除数组中的第一个元素并返回该元素
- unshift(新的元素):在数组的最前面插入新的元素,并返回最新数组的长度
- slice(start[,end]):截取数组中指定开始位置到结束位置的元素,并返回截取的元素数组,
    - 原来的数组没有发生变化,当第二个参数没有的时候,从开始位置一直取到数组的末尾
    - 如果第二个参数有的情况下,取包头不包尾的对应元素,并返回该数组
- splice(start,count,新的元素):
    - 删除功能:有start和count参数的时候,删除从指定开始位置后面的对应的count个数的元素
    - 删除的时候返回数组包含了删除的所有元素,原来的数组的改变了
    - 插入功能:start有值,count为0,新的元素,在指定start的位置处插入新的元素
    - 返回的是删除内容,空的数组
    - 注意:插入的元素是在开始的元素的前面  不限个数
    - 替换功能: start count 新的元素  将删除的元素用新的元素替换
    - 返回的依然是删除的对应的元素数组
- join([分隔符]):将数组中的元素按照指定的分隔符进行拼接,返回字符串,如果没有传参默认用逗号分隔并连接
- concat(元素|数组):将多个数组进行拼接,返回拼接后的数组,原来的数组没有变化
- reverse():反转功能,返回的是反转后的元素数组  原来的数组也变化了
- sort(fun):排序 默认是按照字符串的升序排序
    - 如果希望排序按照数字排序,写对应的回调函数
- indexOf(元素):返回元素第一次出现的位置
- lastIndexOf(元素):返回元素最后一次出现的位置

>实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			var arr = [
				'周一','周二','周三','周四','周五','周六','周日'
			];
			//for in语句结构中，键名的类型是string
			for(var i in arr){
				console.debug(i+1);
			}
			delete arr[2];
			console.debug(arr)
			for(var i in arr){
				console.debug(i+1);
			}
			
			arr.push('周八');
			console.debug(arr)
			arr.pop();
			console.debug(arr)
			
			var arr2 = arr.slice(3,5);
			console.debug(arr,arr2);
			
			var arr = [1,2,3];
			arr.splice(1,1,4,5);
			console.debug(arr);
			//将数组组合成字符串
			var str = arr.join('-');
			console.debug(str);
			var big_arr = [1].concat([2,3],[4,5]);
			console.debug(big_arr)
		</script>
//排序
        <script type="text/javascript">
			var arr = [1,3,5,100,97,96];
			var sarr = arr.sort(mysort);
			function mysort(a,b){
				if(a<b){
					return -1;
				}else{
					return 1;
				}
			}
			console.debug(sarr);
		</script>
//第一次出现的位置
            <script type="text/javascript">
			var str = 'good good study day day up';
			var i = str.indexOf('day');console.debug(i);
		</script>
	</body>
</html>
```
### 函数 ###

----------

>**函数的声明**

```
function 函数名([形参]){
//函数体
return 返回值;
}
```

- 函数调用：函数名([实参]);
- 在事件中: 事件名=函数名();
- 在js代码中：函数名([实参]);
- 赋值调用：对象.事件名=函数名;
    - 不带参的情况下：对象.事件名=函数名();
    - 如果有参数的情况下:对象.事件 = function(){函数(实参);}


:octocat:———— end ————:octocat:
------------------------------