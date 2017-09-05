# CSS基础 DAY1 #
### WEB标准 ###

----------

- WEB标准不是某一个标准，而是一系列标准的集合
- 网页主要由四部分组成：内容（Content）、结构（Structure）、表现（Presentation）和行为（Behavior）
- 内容：就是制作者放在页面内真正想要让访问者浏览的东西（内容为王）
- 结构：使内容更加更加具有逻辑性与易用性，更容易懂
- 表现：用于修饰内容的外观的样式，称为表现
- 行为：对内容的交互及操作效果，如通过javascript判断表单的提交

>**WEB标准的目的**

- 创建一个统一的用于Web表现层的技术标准，以便通过不同浏览器或终端设备向最终用户展示统一的信息内容

 结构  | HTML / XHTML / XML
 ---- | ----------------
 样式  | CSS
 行为  | JS

### CSS ###

----------

>**什么是CSS**

- CSS是（Cascading Style Sheets）层叠样式表的缩写，简称样式表
- 网页设计者使用CSS可以定义元素的样式，包括字体、背景等HTML无法表现的高级样式

>**为什么要使用CSS**

-  符合WEB2.0标准
-  大大缩减页面代码，提高页面浏览速度,缩减带宽成本
-  结构清晰，容易被搜索引擎搜索到，天生优化了seo
-  缩短改版时间。只要简单的修改CSS文件就可以重新表现一个有成千上万页面的站点
-  强大的字体控制和排版能力。CSS控制字体的能力比糟糕的FONT标签更强大，我们不再需要用FONT标签或者透明的1 px GIF图片来控制间距，改变字体颜色，字体样式等等

>**CSS的语法**

- 单个CSS：样式属性名：样式属性值；
- 多个CSS：样式属性名：样式属性值； 样式属性名：样式属性值；
- 每一条格式声明语句由“属性名:属性值;”对组成，属性名和属性值间以冒号隔开，每条声明语句以分号“；”结束

>**CSS注意**

- CSS对大小写不敏感。（建议使用小写）
- 属性值不需引号包裹，除非属性值是由多个单词组成
- 指定多个属性值时，多个属性值间以“,”隔开
- 定义多个值时，浏览器按照从前向后顺序选择属性值。如果第1个值有效，则尝试使用，如果无效，则使用第2个，依次类推

>**引入CSS的三种方式**

- 行内（内联）CSS
    1. 内联定义即是在对象的标记内使用对象的style属性定义适用其的样式表属性。
    2. 示例:`<p style="font-size:50px; ">源代码教育<p>`
- 内部CSS
    1. 可以在HTML文档的<HEAD>和<BODY>标记中插入一个<STYLE>...</STYLE>标签
    2. 示例:

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>这里是标题</title>
		<style type="text/css">
			h1{
				font-size:50px;
				background: red;
			}
		</style>
	</head>
	<body>
		<h1>源代码教育</h1>
	</body>
</html>
```

>**上面两种方式都有缺陷**

- 实际项目是一个网站(系统)，它整体的样式格调通常是一致的。样式写在当前页面并不能解决网站修改或改版的复杂性问题

>**外部CSS**

1. 新建一个CSS文件
2. 在CSS文件里直接写样式表
3. 使用link标签将它们链接起来
4. 语法`<link rel="stylesheet" type="text/css" href="hello.css"  />`

### CSS选择器 ###

----------

>**什么是选择器**

- 在CSS 中，选择器是一种匹配模式，用于选择需要添加样式的元素
- 一个页面有很多的标签，我们需要从很多的标签中选择我们要应用样式的目标标签，这就是选择器
- 行内样式不需要用选择器， 内部或外部CSS需要用选择器

>**选择器的语法**

- 选择器{ 样式1; 样式2；样式3；... }

>**标签选择器**

- 作用：选择HTML网页中所有指定的标签，并对其应用样式
- 语法：标签{ 样式表 }

>**类选择器**

- 作用：选择指定了class属性值的标签
- 语法：.类的值{ 样式表 }
- 使用类选择器必须在值的前面加一个点符号（.）

> **ID选择器**

- 作用：选择指定的ID属性值的标签
- 语法：#ID值{ 样式表 }

>**选择器**

- 作用：选择HTML页面上所有的元素
- 语法：*{ 样式表 }

>**包含选择器E1 E2**

- 作用：选择所有被E1包含的E2
- 语法：E1 E2
- 示例：

```
table td { font-size:14px; } 
div.sub a { font-size:14px; } 
```

>**子选择器 E1 > E2**

- 作用：选择所有作为E1子对象的E2
- 示例

```
body > p { font-size:14px; }
```

>**选择器分组 E1,E2,E3**

- 作用：将同样的样式应用于多个选择符，可以将选择符以逗号分隔的方式并为组。
- 示例：

```
.td1,div a,body { font-size:14px; } 
```

>**E1[attr]**

- 选择具有attr属性的E1
- 示例：

```
/* 所有具有title属性的div标签 */
div[title] { color: blue; }
```

>**E1[attr=value]**

- 选择具有attr属性且属性值等于value的E1 
- 示例

```
span[class=demo] { color: red; }
```

###  CSS伪类 ###

----------

>**A标签的四种状态**

- a:link	未访问的链接
- a:hover	鼠标移动到链接上
- a:active	选定的链接
- a:visited	已访问的链接

>**示例**

```
/* 按规则的写法 */
a:link{color:black;text-decoration: none;}
a:visited{color:black;text-decoration: none;}
a:active{color:black;text-decoration: none;}
a:hover{color:red;text-decoration: underline;font-size:30px;}

/* 通常我们是这样写的：*/
a:link,a:visited{color:black;text-decoration: none;}
a:hover{color:red;text-decoration: underline;font-size:30px;}

/* 或者：*/
/* 将所有的a标签的状态都修改成下面这样 */
a{color:black;text-decoration: none;}

/* 当鼠标称上来的状态我们进行单独的修改 */
a:hover{color:red;text-decoration: underline;font-size:30px;}
```

>**注意**

1. active 这个伪类现在已经不常用了。
2. a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的
3. a:active 必须被置于 a:hover 之后，才是有效的

>**after和:before**

- :after 选择器在被选元素的内容后面插入内容。
- :before 选择器在被选元素的内容前面插入内容。
- 通常使用 content 属性配合，来指定要插入的内容

>**示例**

`p:before{content:"台词："；}`

### CSS的继承与优先级 ###

----------

>**CSS的继承特性**

- 具有层次关系的元素之间，内层元素将继承外层元素的样式，多个外层元素中定义的样式将叠加到内层元素
- HTML中，<body>是其他元素的容器，是其他元素的最外层元素，所以<body>元素中定义的样式将影响其他所有元素的显示格式

>**文本相关属性继承**

```
text-align、color、text-indent、font-family、font-size
	font-style、font-weight、 letter-spacing、word-spacing
	text-transform、line-height
```

>**列表相关属性继承**

```
list-style、 list-style-image、list-style-position、list-style-type
```

>**CSS的优先级**

- !important  > 行内样式 > id选择器 > 类选择器 > 标签选择器
### 字体属性 Font ###

----------

>**font-size**

- 作用： 设置或检索对象中的字体尺寸
- 语法： `font-size : absolute-size| relative-size| length`

>**字体属性**

 写法  |                      含义             | 示例
 :---- | :-------------------------------------: | :--------
 em   | 相对于当前对象内文本的字体尺寸            | font-size: 1.5em;
 px   | 像素（Pixel），相对于显示器屏幕分辨率而言  | font-size:12px;
 rem  | 相对于当前根标签内文本的字体尺寸          | font-size:1rem;

>**注意**

- 在PC上通常用 px ， 中文字大小通常为  12px  14px  18px
- 在手机中通常用 rem ， 中文字大小通常为  0.75rem  0.8rem   1rem  1.2rem

>**font-weight 字体粗细**

- 作用： 设置或检索对象中的文本字体的粗细
- 语法： `font-weight :normal| bold| bolder| lighter| number`
- 示例：`p { font-weight:bold; }`

>**font-family 字体名称**

- 作用： 设置或检索用于对象中文本的字体名称序列
- 语法： `font-family : name`
- 示例：`p{ font-family: Courier, "微软雅黑" }`

>**font-style 字体样式**

- 作用： 设置或检索对象中的字体样式
- 语法： `font-style :normal| italic| oblique`
- 示例： `p { font-style: italic; } `

>**Font 字体**

- 作用： 设置或检索对象中的文本特性。该属性是复合属性
- 语法： `font :font-style||font-variant||font-weight||font-size||line-height||font-family`
- 示例：

```
/*完整写法*/
p { font: italic small-caps bold 12px/18px 宋体; }

/*常用简写形式*/
p { font: 12px/24px 宋体; }
p { font: bold 12px/24px 宋体; }
```

- 注意
    1. 复合属性的每一个属性值之间通常用空格隔开，特殊要求除外。
    2. font最精简的形式也必须是font: 12px/24px 宋体; 否则不会生效

>**color字体颜色**

- 作用： 检索或设置对象的文本颜色。无默认值。
- 语法： color :color
- 示例：`div {color: red; }`
- CSS中颜色的三种表现方式
    1. 英文名称 `红色：red   绿色：green  蓝色：blue  黄色：yellow ...`
    2. 16进制颜色值（#RRGGBB） `红色：#ff0000  绿色：#00ff00  蓝色：#0000ff  黄色：#ffff00 ...`
    3. 10进制的rgb或 rgba ` 红色： Rgb(255,0,0)  Rgba(255,0,0,1) 最后一位是alpha透明通道`

>**Line-height 行高**

- 作用： 检索或设置对象的行高
- 语法： line-height :normal| length
- 示例：`div {line-height:30px; }`

>**Text-decoration 下划线**

- 作用： 检索或设置对象中的文本的装饰。
- 语法： `text-decoration :none|| underline|| blink|| overline|| line-through`
- 示例：`div { text-decoration : underline; }`

>**注意**

1. 通常用于清除A标签的默认下划线
2. 除A标签以外，一般不给其它标签加下划线效果，以免让用户产生误解

>**letter-spacing 字间距**

- 作用： 检索或设置对象中的文字之间的间隔。
- 语法： `letter-spacing :normal| length`
- 示例： `div {letter-spacing:6px; }  `

>**word-spacing 词间距**

- 作用： 检索或设置对象中的单词之间插入的空格数
- 语法： word-spacing :normal| length
- 示例： `div { word-spacing : 20px; } `
- 注 ：对英文有用，中文词没有效果

### 文本属性 Text ###

----------

>**text-indent 文本缩进**

- 作用： 检索或设置对象中的文本的缩进。
- 语法： text-indent :length
- 示例：

```
/* 常用于中文段落的首行缩进2个字*/
p { text-indent :24px; }
```

>**text-align 水平对齐**

- 作用： 设置或检索对象中文本的水平对齐方式
- 语法： `text-align :left| right| center| justify`
- 示例：`p { text-align : center; }`

>**vertical-align 垂直对齐**

- 作用： 设置或检索对象内容的垂直对齐方式
- 语法： `vertical-align :baseline|sub| super|top|text-top|middle|bottom|text-bottom|length`
- 示例：

```
/* 通常用于表格的单元格垂直对齐  段落里的垂直对齐一般使用行高*/
td { vertical-align : center; }
```

>**white-space 是否自动换行**

- 作用： 设置或检索对象内空格的处理方式
- 语法： `white-space :normal| pre| nowrap`
- 示例：`p { white-space: nowrap; }`

### 背景 Background ###

----------

>**background-color 背景颜色**

- 作用： 设置或检索对象的背景颜色。
- 语法： `background-color :transparent| color`
- 示例：

```
/* 常用于中文段落的首行缩进2个字*/
p { background-color: red; }
```

>**background-image 背景图片**

- 作用： 设置或检索对象的背景图像
- 语法： `background-image :none| url (url)`
- 示例：

```
/* 背景图路径不用加引号 */
p { background-image: url(comet.jpg); }
```

>**background-position 背景图位置**

- 作用： 设置或检索对象的背景图像位置。必须先指定background-image属性。默认值为：(0% 0%)
- 语法： background-position :position|| position
- 示例：`p{ background: url(images/aardvark.gif); background-position: left top; }`
- 注：如果只指定了一个值，该值将用于横坐标。纵坐标将默认为50%。第二个值是纵坐标。	值可以写英文，也可以写数字，甚至可以是负数。常用于 CSS sprites（CSS精灵）

>**background-repeat 背景平铺**

- 作用： 设置或检索对象的背景图像是否及如何铺排。必须先指定对象的背景图像
- 语法： background-repeat :repeat| no-repeat| repeat-x| repeat-y
- 示例：

```
/* Y轴平铺 */
p { background: url(images/aaa.gif); background-repeat: repeat-y;}
/* X轴平铺 */
p { background: url(images/aaa.gif); background-repeat: repeat-x;}
/* 不平铺 */
p { background: url(images/aaa.gif); background-repeat: no-repeat;}
/* Y轴X轴都平铺（默认） */
p { background: url(images/aaa.gif); background-repeat: repeat;}
```

>**background-attachment 背景滚动**

- 作用： 设置或检索背景图像是随对象内容滚动还是固定的。
- 语法： background-attachment :scroll| fixed
- 示例：`p { background: url(images/aaa.gif);background-attachment: fixed; }`

>**Background 背景复合属性**

- 作用： 设置背景属性，该属性是复合属性，是前面5个知识点的集合。
- 语法：`background:background-color||background-image||background-repeat||background-attachment||background-position `
- 示例：

```
/* 完整写法：*/
p{ backgrouond-color:red; background-image:url(aaa.jpg); background-position: left 30px; background-repeat: repeat-x; background-attachment:fixed; }

/* 常见写法（简写）*/
p { background: red url(aaa.jpg) repeat-x left 30px fixed; }
```

