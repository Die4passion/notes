# CSS基础 DAY2 #
### 盒子模型 ###

----------

>**“盒子”与“模型”的概念**

- 属性名：内容(content)、填充(padding)、边框(border)、边界(margin)， 这些都是CSS盒子具备属性
- CSS盒子模型就是在网页设计中经常用到的CSS技术所使用的一种思维模型
- 特点：每个盒子都有：边界、边框、填充、内容四个属性；并且每个盒子都有尺寸
- 每个属性都包括四个部分：上、右、下、左;

>**盒子模型详解**

- 盒子有尺寸，用CSS写法为宽度（width）和高度（height）
- 盒子有边框，用CSS写法为上下左右边框（border）
- 盒子有内边距，CSS写法为上下左右内边距（padding）
- 盒子有外边距，CSS写法为外边距（margin）
- 在HTML中，所有的容器标签都可以认为是一个盒子，但是行内标签与块标签有着明显的差别
- 行内标签无法设置尺寸（宽度和高度）等等，块标签是一个完整的盒子

### 尺寸 width、height ###

----------

>**宽度width**

- 作用： 检索或设置对象的宽度
- 语法： width :auto| length
- 示例：`p { width: 200px; }`
- 注:只有块元素才可以设置高度，行内元素无法设置高度

>**高度 height**

- 作用： 检索或设置对象的高度
- 语法： height:auto| length
- 示例：`p { height: 200px; }`
- 注:只有块元素才可以设置高度，行内元素无法设置高度

>**最小高度min-height**

- 作用： 检索或设置对象的最小高度
- 语法： min-height :none| length
- 示例：`p { min-height: 200px; }`
- 注：只有块元素才可以设置最小高度，行内元素无法设置最小高度。实际高度大于最小高度时，以实际高度为准

### 边框 border ###

----------

>**边框宽度 borer-width**

- 作用： 检索或设置对象的边框宽度。
- 语法： border-width :medium| thin| thick| length 
- 示例：`p { border-style: solid; border-width: 1px; }`

>**注：**

1. 如果只提供一个，将用于全部的四条边。
2. 如果提供两个，第一个用于上－下，第二个用于左－右。
3. 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下。
4. 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四个边框。
5. 要有边框线型的情况下，才能设置边框的宽度。
6. 通常边框宽度都会设置成一个实际的象素值。

>**边框线型 border-style**

- 作用： 检索或设置对象的边框线型
- 语法： border-style :none| dotted| dashed| solid| double
- 示例：`p { border-style: double;  }`
- 注：边框线型的顺序与边框宽度一致。

>**边框颜色 border-colo**

- 作用： 检索或设置对象的边框颜色
- 语法： border-color : color
- 示例: `p { border-color: red; }`
- 注：color的表现方式有三种。顺序同上。

>**边框 border**

- 作用： 检索或设置对象的边框。这是一个复合属性
- 语法： border : border-width||border-style||border-color
- 示例:

```
/* 设置 p标签的边框为： 宽度2px ， 线型实线 ， 颜色为红色 */
p { border:2px solid red; }
```

### 边框的细节控制 ###

----------

>**上边框 border-top**

- 作用： 检索或设置对象的上边框。这是一个复合属性
- 语法： border-top: border-width||border-style||border-color
- 示例：

```
/* 设置 p标签的上边框为： 宽度2px ， 线型实线 ， 颜色为红色 */
p { border-top:2px solid red; }
```

>**下边框 border-bottom**

- 作用： 检索或设置对象的下边框。这是一个复合属性
- 语法： border-bottom : border-width||border-style||border-color
- 示例：

```
/* 设置 p标签的下边框为： 宽度2px ， 线型实线 ， 颜色为红色 */
p { border-bottom:2px solid red; }
```

>**左边框 border-left**

- 作用： 检索或设置对象的左边框。这是一个复合属性
- 语法： border -left: border-width||border-style||border-color
- 示例：

```
/* 设置 p标签的左边框为： 宽度2px ， 线型实线 ， 颜色为红色 */
p { border-left:2px solid red; }
```

>**右边框 border-right**

- 作用： 检索或设置对象的右边框。这是一个复合属性
- 语法： border -right: border-width||border-style||border-color
- 示例：

```
/* 设置 p标签的右边框为： 宽度2px ， 线型实线 ， 颜色为红色 */
p { border-right:2px solid red; }
```

### 内边距 padding ###

----------

>**内边距也称为内补丁，是边框与内容之间的距离**
1. 如果只提供一个，将用于全部的四条边。
2. 如果提供两个，第一个用于上－下，第二个用于左－右。
3. 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下。
4. 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。

>**上边内边距 padding-top**

- 作用： 检索或设置对象的上边内边距
- 语法： padding-top :length
- 示例：`p { padding-top: 36px; }`

>**右边内边距 padding-right**

- 作用： 检索或设置对象的右边内边距
- 语法： padding-right:length
- 示例：`p { padding-right: 36px; }`

>**下边内边距 padding-bottom**

- 作用： 检索或设置对象的下边内边距
- 语法： padding-bottom :length
- 示例：`p { padding-bottom: 36px; }`

>**左边内边距 padding-left**

- 作用： 检索或设置对象的左边内边距。
- 语法： padding-left:length
- 示例：`p { padding-left: 36px; }`

>**内边距 padding**

- 作用： 检索或设置对象四边的内边距。
- 语法： padding :length
- 示例：`p { padding: 20px; }`

### 外边距 margin ###

----------

>**外边距也称为外补丁，是边框与其它标签之间的距离**

1. 如果只提供一个，将用于全部的四条边。
2. 如果提供两个，第一个用于上－下，第二个用于左－右。
3. 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下。
4. 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。
5. 外边距在不同浏览器的效果会有很大不同，建议尽量少用。


>**右边外边距 margin-right**

- 作用： 检索或设置对象的右边外边距
- 语法： margin-right:length
- 示例：`p { margin-right: 36px; }`

>**下边外边距 margin-bottom**

- 作用： 检索或设置对象的下边外边距
- 语法： margin-bottom :length
- 示例：`p { margin-bottom: 36px; }`

>**左边外边距 margin-left**

- 作用： 检索或设置对象的左边外边距。
- 语法： margin-left:length
- 示例：`p { margin-left: 36px; }`

>**外边距 margin**

- 作用： 检索或设置对象四边的外边距
- 语法： margin:length
- 示例：`p { margin: 20px; }`

### 列表 list ###

----------

>**列表标记位置**

- 作用： 设置或检索作为对象的列表项标记如何根据文本排列
- 语法： list-style-position :outside| inside
- 示例：`Ul li { list-style-position: inside; }`

>**列表标记符号**

- 作用： 设置或检索对象的列表项所使用的预设标记
- 语法：

```
list-style-type :disc| circle| square| decimal| lower-roman| upper-roman|
 lower-alpha| upper-alpha| none| armenian| cjk-ideographic| georgian| 
lower-greek| hebrew| hiragana| hiragana-iroha| katakana| katakana-iroha|
 lower-latin| upper-latin
```

- 示例：`Ul li { list-style-type: square; }`
- 注： 实际应用中通常都不需要标记符号，而是使用图片实现效果

>**列表**

- 作用： 设置列表项目相关内容
- 语法： list-style : list-style-image||list-style-position||list-style-type
- 示例：`Ul li { list-style:outside , square; }`

### 表格 table ###

----------

>**table**

- 作用： 设置或检索表格的行和单元格的边是合并在一起还是按照标准的HTML样式分开。
- 语法： border-collapse :separate| collapse
- 示例：

```
table { border-collapse: collapse; }
td{ border:1px solid #ccc; }
```

### 鼠标形状 cursor ###

----------

>**cursor **

- 作用： 设置或检索在对象上移动的鼠标指针采用何种光标形状
- 语法： 

```cursor :auto| crosshair| default| hand| move| help| wait| text|
 w-resize|s-resize| n-resize|e-resize| ne-resize|sw-resize|
 se-resize| nw-resize|pointer| url (url)
```

- 示例：`div { cursor:pointer; }`
- 注： 常在非A标签上设置成手型，配合JavaScript效果，让用户认为是可以点击的模块。

