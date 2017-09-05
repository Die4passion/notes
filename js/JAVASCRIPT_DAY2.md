# JAVASCRIPT基础 DAY2 #
### DOM ###

----------

>**什么是DOM**

- 文档对象模型 document object model
- 当浏览器载入一个页面的时候,实际就是载入的一个DOM对象
- 文档类型:html/xml
- 在页面中可以将每个对象或者是标签看做一个DOM
- DOM的组成: 元素节点  属性节点  文本节点

### 寻找dom节点 ###

----------

>**根据ID寻找节点**

- document.getElementById(id):根据id寻找节点  返回的第一个匹配到的ID值对应的元素
- DOM对象.innerText:获取dom对象的文本内容 
- DOM对象.innerText=”值”:设置DOM对象的文本内容
- DOM对象.innerHTML:获取dom对象的所有HTML代码 
- DOM对象.innerHTML=”值”:设置DOM对象的HTML代码 
- 实例
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			div {
				width: 100px;
				height: 100px;
				border: 1px solid blue;
				margin: 5px auto;
			}
		</style>
	</head>
	<body>
		<div id="box"><h1>box</h1></div>
		<div id="box1">
			box1
		</div>
		<script type="text/javascript">
			var box = document.getElementById('box');
			console.debug(box.innerHTML);//获取html代码
			console.debug(box.innerText)//获取纯文本内容
			box.innerHTML = '<span style="background:pink;">这是通过js修改的内容</span>';
			box.innerText = '<span style="background:pink;">这是通过js修改的内容！</span>';
		</script>
	</body>
</html>
```

>**根据样式名字查找**

- document.getElementsByClassName(样式的名字):根据样式的名字查找dom节点
- 实例
```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			.box {
				border: 1px solid red;
				width: 100px;
				height: 100px;
				margin: 5px auto;
			}
		</style>
	</head>
	<body>
		<div class="box"></div>
		<div class="box"></div>
		<script type="text/javascript">
			var boxes = document.getElementsByClassName('box');
			console.debug(boxes[0]);
		</script>
	</body>
</html>
```

>**根据标签的名字查找**

- document.getElementsByTagName(标签名字):根据标签名字查找dom节点
- 实例
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			div{
				border: 1px solid red;
				width: 100px;
				height: 100px;
				margin: 5px auto;
			}
		</style>
	</head>
	<body>
		<div>
			div 1
		</div>
		<div>
			div 2
		</div>
		<script type="text/javascript">
			var divs = document.getElementsByTagName('div');
			console.debug(divs);
		</script>
	</body>
</html>
```

>**根据标签的name属性寻找节点:**

- document.getElementsByName(name的属性值):获取对应name属性值的dom元素
- 实例
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<form action="" method="post">
			<input type="checkbox" name="hobby" id="hobby" value="1" />运动
			<input type="checkbox" name="hobby" id="hobby" value="2" />旅游
			<input type="checkbox" name="hobby" id="hobby" value="3" />摄影
			<input type="checkbox" name="hobby" id="hobby" value="4" />看电影
			<input type="submit" value="提交"/>
		</form>
		<script type="text/javascript">
			var hobbies = document.getElementsByName('hobby');
			console.debug(hobbies)
		</script>
	</body>
</html>
```

>**其它**

- Document.title:寻找当前页面的标题
- document.links:获取当前页面所有的超链接标签a
- document.images:获取所有图片标签dom对象
- document.body:获取页面的body对象
- document.forms:获取页面所有的表单对象
- document.all.id:找指定id的dom元素  等同于document.getElementById()
- 获取DOM对象的本身的document  document.documentElement;
- DOM对象.clientWidth|Height;找到浏览器中可见内容的宽高度
- DOM对象.scrollWidth|Height;dom中的实际内容的宽高度

> 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>今天下雨了</title>
	</head>
	<body>
		<form action="" method="post">
			<input type="text" name="username" id="username" value="" />
			<input type="submit" value="点一下"/>
		</form>
		<a href="http://www.itsource.cn">源码时代，成就未来</a>
		<a href="http://bbs.itsource.cn">源码论坛，畅所欲言</a>
		<a href="https://blog.kunx.org">节操博客，值得一阅</a>
		<img src="https://blog.kunx.org/images/payment.png"/>
		<img src="https://secure.gravatar.com/avatar/099592e58148faedc12ab8c26f206805?s=40&r=G&d="/>
		<script type="text/javascript">
			console.debug(document.title);
			document.title = '今天下雨了，好凉快呀！~'
			
			//获取所有的a标签
			var links = document.links;
			console.debug(links);
			
			//获取所有的图片
			var imgs = document.images;
			console.debug(imgs)
			
			//找到body区域
			console.debug(document.body)
			//表单
			console.debug(document.forms);
			
			//找到所有节点
			console.debug(document.all);
			console.debug(document.all.username);
			//找到文档节点本身
			console.debug('');
			console.debug(document.documentElement);
			
			//获取浏览器可见区域
			console.debug('宽度' + document.documentElement.clientWidth);
			console.debug('高度' + document.documentElement.clientHeight);
			//获取实际内容的尺寸
			console.debug('宽度' + document.documentElement.scrollWidth);
			console.debug('高度' + document.documentElement.scrollHeight);
		</script>
	</body>
</html>
```

###	事件 ###

----------

>**鼠标事件**


  事件         |         动作
 :----------- | :---------------
 onclick      |       单击
 ondblclick   |      双击
 onmousedown  |     摁下
 onmouseup    |    抬起
 onmouseover  |    移入
 onmouseout   |    移除
 onmouseenter |    进入
 onmouseleave |    离开
 onmousemove  |    移动  经常用在网游中

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			#box{
				width: 500px;
				height: 300px;
				background: purple;
				margin: 0 auto;
			}
		</style>
	</head>
	<body>
		<div id="box" onclick="myclick()" ondblclick="mydblclick()" onmouseover="mymouseover()" 
					  onmouseout="mymouseout()" onmousemove="mymousemove()">
		</div>
		<script type="text/javascript">
			function myclick(){
				console.debug('你点击了鼠标')
			}
			function mydblclick(){
				console.debug('你双击了鼠标')
			}
			function mymouseover(){
				console.debug('鼠标进入了')
			}
			function mymouseout(){
				console.debug('鼠标移出了')
			}
			function mymousemove(){
				console.debug('看，他在动')
			}
		</script>
	</body>
</html>
```

>**event**

 事件            |         动作
 :----------------- | :---------------
 event.clientX      |   获取光标距离左边的距离
 event.clientY      |   获取光标距离上边的距离
 event.target       |     获取事件作用的对象
 event.srcElement   |    获取事件作用的对象
 event.which        |   获取鼠标的键值  左键=1 中键=2 右键=3
 event.keyCode      |    获取键盘事件中键盘的按键值

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			#box{
				width: 500px;
				height: 300px;
				background: purple;
				margin: 0 auto;
			}
		</style>
	</head>
	<body onkeydown="mykeydown()">
		<div id="box" onmousedown="myclick()">
			
		</div>
		<script type="text/javascript">
			function myclick(){
				console.debug(event)
			}
			function mykeydown(){
				console.debug(event)
			}
		</script>
	</body>
</html>
```

- 瞄准器实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			*{
				cursor: crosshair;
			}
			body{
				background: black;
				
			}
			#pig{
				width: 30px;
				height: 30px;
				position: absolute;
				left: 300px;
				top: 200px;
				background: yellow;
			}
		</style>
	</head>
	<body>
		<div id="pig" onclick="check(event)">
			
		</div>
		
		<script type="text/javascript">
			function check(event){
				//判断当前点击的位置
				var x = event.clientX;
				var y = event.clientY;
				//获取目标的位置
				var ele = document.getElementById('pig');
				var left = ele.offsetLeft;
				var top = ele.offsetTop;
				
				//判断点击位置
				if(x-left>=0 && x-left<=30 && y-top>=0 && y-top<=30){
					alert('恭喜你');
				}else{
					alert('很遗憾');
				}
			}
		</script>
	</body>
</html>
```

>**键盘事件**

  事件               |         动作
 :----------------- | :---------------
 onkeydown          |   按键被按下
 onkeyup            |   按键抬起
 onkeypress         |    按键一次

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body onkeydown="down(event)" onkeyup="up(event)" onkeypress="press(event)">
		<script type="text/javascript">
			function down(event){
				console.debug(event.keyCode);
			}
			function up(){
				console.debug(event);
			}
			function press(event){
				console.debug('一次完整的击键操作');
			}
		</script>
	</body>
</html>
```

- 移动的盒子实例

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Title</title>
	<style>
		body {
			padding: 0;
			margin: 0;
		}
		#box {
			width: 50px;
			height: 50px;
			background: lightblue;
			position: absolute;
		}
	</style>
</head>
<body>
<div id="box"></div>
<script type="text/javascript">
document.documentElement.onkeydown = function (event) {
	var keycode = event.keyCode;
	var ele = document.getElementById('box');
	var x = ele.offsetLeft;
	var y =ele.offsetTop;
	switch (keycode){
		case 37:
			ele.style.left=x-10+'px';
			break;
		case 38:
			ele.style.top=y-10+'px';
			break;
		case 39:
			ele.style.left=x+10+'px';
			break;
		case 40:
			ele.style.top=y+10+'px';
			break;
	}
}
</script>
</body>
</html>
```

>**表单事件**

  事件               |         动作
 :----------------- | :---------------
 onsubmit           |   提交的时候触发的事件
 onreset            |   点击重置按钮触发的事件
 onfocus            |   获取焦点触发事件
 onblur             |   失去焦点触发事件
 onchange           |  下拉改变内容的时候触发的事件
 oninput            | 填写内容时会增加

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<form action="" method="post" onsubmit="return dosubmit()">
			用户名： <input type="text" id="username" value="" /><br />
			密码： <input type="text" id="password" value="" /><br />
			<select name="" id="level">
				<option value="1">1</option>
				<option value="2">2</option>
				<option value="3">3</option>
				<option value="4">4</option>
			</select>
			<input type="reset" value=" 重置 "/>
			<input type="submit" value=" 提交 "/>
		</form>
		<script type="text/javascript">
			//1.当点击提交的时候
			//切记，在onsubmit的时候要return，在函数中也要return
			function dosubmit(){
				alert('点击了提交按钮');
				return false;
			}
			//2.当点击重置按钮的时候
			document.forms[0].onreset = function(){
				alert('重置');
				return false;
			}
			
			//3.获取焦点的时候
			document.getElementById('username').onfocus=function(){
				console.debug('我睡的好好的，点我干什么');
			}
			document.getElementById('username').onblur=function(){
				console.debug('你要弃我而去了吗');
			}
			document.getElementById('username').onchange=function(){
				console.debug('看他又变了');
			}
			document.getElementById('username').oninput=function(){
				console.debug('看他正在变');
			}
			document.getElementById('level').onchange=function(){
				console.debug('级别变了');
			}
		</script>
	</body>
</html>
```

>**其它**

  事件               |         动作
 :----------------- | :---------------
 onload             |   页面加载完毕后执行的事件
 onresize           |   浏览器的窗口大小改变的时候触发的事件
 onscroll           |   滚动条触发事件
 onblur             |   失去焦点触发事件
 onchange           |  下拉改变内容的时候触发的事件

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div style="height: 1000px;"></div>
		<script type="text/javascript">
			window.onload = function (){
				console.debug('页面加载完毕');
			}
			window.onresize = function(){
				console.debug('窗口尺寸变化');
			}
			window.onscroll = function(){
				console.debug('窗口滚动进行中');
			}
		</script>
	</body>
</html>
```

### DOM对象的操作 ###

----------

>**添加dom节点**

- 第一步:创建dom对象,	var dom = document.createElement(“标签名”)
- 第二步:设置dom对象的属性  `dom.属性名=属性值:设置值`  `dom.setAttribute(“属性名”,属性值””):设置值`  `dom.getAttribute(“属性名”):获取对应属性名的属性值`
- 第三步:将创建的dom对象加入到父对象中 `父对象.appendChild(子对象);`
- ele.属性名和ele.getAttribute(‘属性名’)的区别

```
.属性名用于获取所有标签的内置属性，
比如id，title，name等，对于自定义属性，会有问题，可以通过：
ele.attributes.属性名或者ele.getAttribute()
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
			//1.创建节点
			var img = document.createElement('img');
			//2.设定相关属性
//			img.src = 'img/1.jpg';
			img.setAttribute('src','img/1.jpg')
			//将这个节点放到父级节点中
			document.documentElement.appendChild(img);
		</script>
	</body>
</html>
```

>**删除dom节点**

- 父对象.removeChild(子对象):从父对象中删除对应子对象
- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<img src="img/1.jpg" id="img1" onclick="myremove('img1')"/>
		<img src="img/2.jpg" id="img2" onclick="myremove('img2')"/>
		<script type="text/javascript">
			function myremove(ele){
				//找到要移出的节点
				var ele = document.getElementById(ele);
				console.debug(ele);
				//找到父级元素
				var parent = document.body;
				//从父级盒子中移出对应的节点
				parent.removeChild(ele);
			}
		</script>
	</body>
</html>
```

- 打地鼠实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>打地鼠</title>
	</head>
	<body>
		<script type="text/javascript">
			//创建函数，生地鼠
			function create_mouse(){
				//创建节点
				var ele = document.createElement('img');
				//设置属性
				ele.setAttribute('src','img/3.jpg');
				ele.style.position = 'absolute';
				ele.style.width = '100px';
				//随机出现
				ele.style.left = Math.random() * 1200 + 'px';
				ele.style.top = Math.random() * 600 + 'px';
				//老鼠一打就消失
				ele.onclick = function(){
					document.body.removeChild(this);
				}
				//添加节点
				document.body.appendChild(ele);
			}
			setInterval(create_mouse,500);
		</script>
	</body>
</html>
```

### 计时器 ###

----------

>**每隔固定的毫秒执行对应的函数**

  事件                                        |         动作
 :------------------------------------------- | :---------------
 setInterval(函数名,毫秒数)                     |   在没有参数的情况下才可以这样用
 setInterval(“函数名([实参])”,毫秒数)           |   浏览器的窗口大小改变的时候触发的事件
 clearInterval(id):id值是setInterval()的返回值 |   停止计时器
 setTimeout(函数名,毫秒数)                     |   在没有参数的情况下才可以这样用
 setTimeout(“函数名([实参])”,毫秒数)            | 第二种调用方式

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			#box{
				width: 50px;
				height: 50px;
				background: pink;
				position: absolute;
				left: 0;
				top: 0;
			}
		</style>
	</head>
	<body>
		<div id="box"></div>
		<div style="float: right;">
			<input type="button" value="继续" onclick="start()" />
			<input type="button" value="停止" onclick="stop()" />
		</div>
		<script type="text/javascript">
			function move(num){
				console.debug(num)
				//找到要移动的节点
				var ele = document.getElementById('box');
				//找到位置
				var x = ele.offsetLeft;
				var y = ele.offsetTop;
				//移动，每次5像素
				ele.style.left = x + 5 +'px';
				ele.style.top = y + 5 + 'px';
				
			}
			var id;
			//计时器实参
			function start(){
				stop();
				id = setInterval(move(5),50);
			}
			
			function stop(){
				clearInterval(id);
			}
		</script>
	</body>
</html>
```

### JS中常用的内置对象 ###

----------

>**String对象**

- 属性:字符串对象.length
- 方法:字符串对象.方法()


  事件                                         |         动作
  :------------------------------------------- | :---------------
  indexOf(子串)                                |   返回字符串对象中子串第一次出现的位置,如果找不到返回-1
  lastIndexOf(子串)                            |   回字符串对象中子串最后一次出现的位置,如果找不到返回-1
  charAt(index)                                |   返回字符串中指定下标位置的字符  下标从0开始
  charCodeAt(index)                            |   返回字符串中指定下标位置的字符的Unicode编码 
  substr(start,[length])                       |  从开始位置截取指定个数的字符串 ,如果没有第二个参数截取到末尾
  substring(start,end)                         |   截取开始到结束位置的子串 包头不包尾
  toUpperCase()                                |  将字母字符串转为大写
  toLowerCase()                                |   将字母字符串转为小写
  trim()                                       |  去掉字符串前后的空格 ,字符串中间的空格不会去掉

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
			var str = '  Hello World! ';
			console.debug('字符串长度是' + str.length);
			console.debug('W第一次出现的下标'+str.indexOf('W'));
			console.debug('l最后一次出现的下标'+str.lastIndexOf('l'));
			console.debug('下标为4的字符'+str.charAt(4));
			console.debug('下标为4的字符的Unicode编码'+str.charCodeAt(4));
			console.debug('截取从下标为3的开始，截取5个字符'+str.substr(3,5));
			console.debug('从下标为2的开始截取，截取到下标为6的（包含6）'+str.substring(2,7));
			console.debug('转换为大写' + str.toUpperCase());
			console.debug('转换为小写'+str.toLowerCase());
			console.debug('取出空白字符' + str.trim())
		</script>
	</body>
</html>
```

>**Math对象**

  事件              |         动作
 :---------------- | :---------------
 abs()             |   绝对值
 ceil()            |   向上取整
 floor()           |   向下取整
 max()             |   取最大值
 min()             |  取最小值
 random()          |   取0-1之间的随机数
 round()           |  四舍五入

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
			console.debug(Math.PI);
			//求最大值
			console.debug(Math.max(100,200,-1000));
			//求最小值
			console.debug(Math.min(100,200,-1000));
			
			//向上取整
			console.debug(Math.ceil(0.01));
			//向下取整
			console.debug(Math.floor(0.91));
			//四舍五入
			console.debug(Math.round(0.55));
		</script>
	</body>
</html>
```

>**日期对象Date**

  事件                           |         动作
 :----------------------------- | :---------------
 new Date()                     |   获取当前时间
 new Date(yyyy-mm-dd hh:mm:ss)  |   设置指定的时间创建date对象 
 new Date(year,month,day,h,m,s)  |   根据年月日时分秒创建对应的date对象
 new Date(0)                    |   初始化基于元年的时间   1970 年 1 月 1
 getFullYear()                  |  获取年份
 getMonth()                     |   获取月份 月份是从0开始的 如果现在是3月份 这个值就是2
 getDate()                      |  获取哪一天
 getDay()                       |   星期几
 getHours()                     |   获取小时
 getMinutes()                   |   获取分钟数
 getSeconds()                   |  获取秒
 getTime()                      |  获取指定时间的毫秒数
 toLocaleString()               |  返回本地格式的时间

- 实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			div{
				border: 1px solid red;
				margin-bottom: 10px;
				min-height: 50px;
				width: 500px;
			}
		</style>
	</head>
	<body>
		日期时间<div id="box"></div>
		完整年份<div id="box1"></div>
		月份<div id="box2"></div>
		日期<div id="box3"></div>
		星期<div id="box4"></div>
		小时<div id="box5"></div>
		分钟<div id="box6"></div>
		秒<div id="box7"></div>
		时间戳<div id="box8"></div>
		本地时间<div id="box9"></div>
		<script type="text/javascript">
			var now = new Date();
			document.getElementById('box').innerText = now;
			document.getElementById('box1').innerText = now.getFullYear();
			document.getElementById('box2').innerText = now.getMonth()+1;
			document.getElementById('box3').innerText = now.getDate();
			document.getElementById('box4').innerText = now.getDay();
			document.getElementById('box5').innerText = now.getHours();
			document.getElementById('box6').innerText = now.getMinutes();
			document.getElementById('box7').innerText = now.getSeconds();
			document.getElementById('box8').innerText = now.getTime();
			document.getElementById('box9').innerText = now.toLocaleString();
			
		</script>
	</body>
</html>
```

- 时间转换实例

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<input type="text" id="start_time" />
		<input type="button" value="计算" onclick="calc()"/>
		<div id="result"></div>
		<script type="text/javascript">
			function calc(){
				var start_time = document.getElementById('start_time').value;
				var start_obj = new Date(start_time);
				var start_timestamp = start_obj.getTime();
				var now = new Date();
				var now_timestamp = now.getTime();
				var tmp = now_timestamp - start_timestamp;
				var seconds = parseInt(tmp/1000);
				var days = seconds/24/3600;
				document.getElementById('result').innerHTML = '过了<strong>'+seconds+'</strong>秒<br />
				过了<strong>'+days+'</strong>天';
			}
		</script>
	</body>
</html>
```


:octocat:———— end ————:octocat:
------------------------------