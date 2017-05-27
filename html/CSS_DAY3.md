# CSS基础 DAY3 #
### 浮动 float ###

----------

> **浮动的概念**

- float是css样式中的定位属性，用于设置标签对象的浮动布局
- 浮动是标签对象浮动居左靠左（float:left）和浮动居右靠右(float:right)
- 浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止
- 只要设置了浮动，就将盒子脱离了文本流
- 由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样

>**浮动属性 float**

- 作用： 该属性的值指出了对象是否及如何浮动
- 语法： `float :none| left|right`
- 示例：

```
/*左浮动和右浮动*/
div { float:left; }
div { float:right; }
```

>**清除浮动**

- 作用： 清除浮动造成的影响
- 语法： clear :none| left|right| both
- 示例：

```
div { clear:left; } /* 清除左浮动 */
div { clear:both; } /* 清除左右浮动 */
```

- 注意：

	1. 清除浮动的属性应该写浮动盒子的后面
	2. 设置clear清除浮动，在后面盒子上设置外边距会塌陷

> **超出 overflow**

- 作用： 检索或设置当对象的内容超过其指定高度及宽度时如何管理内容。
- 语法： overflow :visible| auto| hidden| scroll
- 示例：`div { overflow: hidden; } /* 超出部分隐藏 */`
- 设置textarea对象为hidden值将隐藏其滚动条
- 注意：
	1. oveflow有一个特殊功能，可以用于清除浮动造成的影响:
	2. 在浮动盒子的父标签上面设置 overflow:hidden;

>**显示与隐藏 display**

- 作用： 设置或检索对象是否及如何显示。
- 语法： display : block| none|inline
- 示例：

```
a { display: block; } /* 以块的方式显示A标签 */
div { display: none; } /* 不显示DIV标签 */
div { display: inline; } /* 以行内元素的方式显示DIV标签 */
```

- 实例

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<meta charset="utf-8"/>
	<title>float实例</title>
	<style type="text/css">
		html,body{background:#ff9;}
		div{margin:0 10px;border:1px dashed #333;background:#90baff;padding:12px;}
		p{border:1px dashed #333;background:#ff90ba;}
		
		.box1{float:left;}
		.box2{float:left;}
		.box3{float:right;}
		
		p{clear:both;}
		
		.box{overflow:hidden;}
		
		/*
		要实现多栏布局的排版，必须将浮动的盒子装在一个大的父容器中，再来设置清除浮动带来的影响：
			办法1) 在浮动盒子的下面（兄弟节点）设置一个空的DIV ，使用清除浮动的样式
				有一个副作用：会产生一个多余的标签
				
			办法2) 也可以在父标签上面设置overflow:hidden;
				没有副作用
		*/
	</style>
</head>
<body>
	<div class="box">
		<div class="box1">Box-1</div>
		<div class="box2">Box-2</div>
		<div class="box3">Box-3<br/>Box-1<br/>Box-1</div>
	</div>
	<p>这里是浮动的外围文字，这里是浮动的外围文字，这里是浮动的外围文字，这里是浮动的外围文字</p>
</body>
</html>
```

### 定位 position ###

----------

>**position说明**

- 广义的“定位”：要将某个元素放到某个位置的时候，这个动作可以称为定位操作，可以使用任何CSS规则来实现，这就是泛指的一个网页排版中的定位操作，使用传统的表格排版时，同样存在定位的问题。
- 狭义的“定位”：在CSS中有一个非常重要的属性position，这个单词翻译为中文也是定位的意思。然而要使用CSS进行定位操作并不仅仅通过这个属性来实现。

>**定位 position**

- 作用： 设置或检索对象的定位方式。
- 语法： position :static| absolute | fixed| relative
- 示例：

```
a { position: absolute; } /* A标签设置成绝对定位 */
div { position: fixed; } /* DIV标签设置成固定定位 */
div { position: relative; } /* DIV标签设置成相对定位 */
```

- 注：absolute 和 fixed，将对象从文档流中拖出，配合left/right/top/bottom进行独立定位控制

>**absolute 绝对定位**

- 将盒子从普通文本流中脱离出来，可以任意在页面上进行定位
- 会随着滚动条的滚动而移动

>**fixed 固定定位**

- 定位到某个位置后，不会随着滚动条的滚动而变化

>**relative 相对定位**

- 设置相对定位以后，会保留原来的位置，可以进行偏移（相对原来的位置进行偏移）

>**层次 z-index**

- 作用： 检索或设置对象的层叠顺序。
- 语法： z-index :auto| number
- 示例:`div { position: absolute; z-index:999; } `
- 注：`与 position搭配使用。只有当DIV定位方式设置成 absolute 或 fixed 时，该属性才起作用，层次值高的会遮住层次值低的对象。`

>**位置控制 left right top bottom**

- 作用： 控制标签对象的位置。
- 语法：

```
left : auto|number
right: auto|number
top: auto|number
bottom: auto|number
```

- 示例：

```
div { position: absolute; left:100px; } 
div { position: absolute; right:100px; } 
div { position: absolute; top:100px; } 
div { position: absolute; bottom:100px; } 
```

- 与 position搭配使用
    1. 相对定位时，表示的是相对于盒子所在位置的偏移量
	2. 绝对定位或固定定位时，表示的是与浏览器边框之间的距离
	3. 当子元素是绝对定位，父元素是相对定位（不仅限于相对定位）时，位置控制属性是以父元素的边界为基准


:octocat:———— end ————:octocat:
------------------------------