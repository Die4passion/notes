# HTML基础　DAY3 #

### 表单标签 ###

> 什么是表单

- 表单标签：这里面包含了处理表单数据所用PHP程序的URL以及数据提交到服务器的方法
- 表单域：包含了文本框、密码框、隐藏域、多行文本框、复选框、单选框、下拉选择框和文件上传框等
- 表单按钮：包括提交按钮、复位按钮和一般按钮；用于将数据传送到服务器上的处理程序或者取消输入，还可以用表单按钮来控制其他定义了处理脚本的处理工作

### Form 标签 ###

>form 说明

- 用于声明表单区域，定义采集数据的范围，也就是<form>和</form>里面包含的数据将被提交到服务器

>语法

- `<form action="URL" method="get|post">...</form>`

>属性

 属性          |     值                | 简介
 :------------ | :---------------------: | :-----------------------------------
 action       |       URL             | 规定当提交表单时向何处发送表单数据
 autocomplete |     on   off          | 规定是否启用表单的自动完成功能
 enctype      | text/plain 空格转+    | 发送表单数据进行编码 上传文件：multipart/form-data
 method       |       get post        | 发送 form-data 的 HTTP 方法
 name         |     form_name         | 表单的名称
 novalidate   |     novalidate        | 用该属性，则提交表单时不进行验证
 target       | _blank _parent _top   | 新，父，当前    规定在何处打开

>示例

```
<form action="post.php" method="get">
  <p>姓名：<input type="text" name="username" /></p>
  <p>密码：<input type="password" name="userpass" /></p>
  <input type="submit" value="提交" />
</form>
```

### 输入框 input ###

>input说明

- `<input>`标签用于搜集用户信息
- 根据不同的 type 属性值，输入字段拥有很多种形式
- `<input>`是一个行内元素，也是一个单标签，需要自关闭

>type属性

 类型          |     名称                | 特点
 :------------ | :---------------------: | :-----------------------------------
 text         |       普通文本框        | 默认
 password     |    密码框              | 内容显示为*号
 submit       | 提交按钮    			| 点击后就提示表单
 button       |       普通按钮       | 自定义
 reset         |     重置按钮         | 回到最初状态(注：不是清空)
 radio         |     单选        	| 一组单选必需name相同
 checkbox      | 			 多选   | 一组多选必需name相同
 file         |       文件上传框       | 可以选择文件进行提交
 hidden       |       隐藏域       	| 隐藏控件，但是会被提交
 image        |       图片按钮       | 必需配合src属性来展示
 <span style="color: green">---以下---</span>|   <span style="color: green">---是HTML5---</span> |     <span style="color: green">--- 新特性 ---</span>
 color        |       拾色器       	| 自定义
 date          |       日期字段       | 带有 calendar 控件
 datetime       |       日期字段       | 自定义
 datetime-local |       日期字段       | 带有 calendar 和 time 控件
 month         |       日期字段的月       | 带有 calendar 控件
 week         |       日期字段的周       | 带有 calendar 控件
 time         |       时、分、秒       | 带有 time 控件
 email        |       邮箱格式       | 用于 e-mail 地址的文本字段
 number       |       数字输入框       | 带有 spinner 控件的数字字段
 range        |       数字输入框       | 带有 slider 控件的数字字段。
 search       |       搜索框       | 用于搜索的文本字段
 tel          |       电话号码       | 用于电话号码的文本字段
 url          |       URL 文本框       | 用于 URL 的文本字段

>属性注意

>name属性：

- 表示控件的名称，有名称的控件数据才会提交到后台
- 凡是要提交到后台的控件，都要加上name
- 用于单选与多选的分组，同一组的元素name需要一致

>value属性

- 有不同的类型(type)中，value的意义是不同的
- text/password 如果我们提交写上,它就是默认值(也是我们要提交的值)，提交value的值
- submit/button/reset 表示显示在按钮中的文字
- radio/checkbox 是一个元素所代表的值

>其它属性

- maxlength：设置文本框最多输入多少字符
- readonly：只读(不可输入)
- disabled：禁用(不可输入) -> 不会提交这个表单元素的数据
- checked：仅用于 radio/checkbox，作用是设置默认选中项

>实例

```
<input type="hidden" name="id" value="0001" />
用户名：<input type="text" name="name" disabled="disabled" value="张三"/><br />
密码：  <input type="password" name="pwd" /><br />
单选：  <input name="gender" value="1" type="radio" />男  
       <input checked="checked" name="gender" value="2" type="radio" />女<br />
多选：  <input type="checkbox" checked value="1" name="likes" />写代码   
       <input type="checkbox" value="2" name="likes" />改BUG  
       <input type="checkbox" checked value="3" name="likes" />定注释  <br />
附件：  <input type="file" name="headImg" /> <br />
        <input type="submit" value="注册" />
        <input type="button" value="我很普通" />
	    <input type="reset" value="回到最初" />

```

### 下拉框 select ###

>select说明

- select 元素可创建单选或多选菜单
- 下拉框提供给用户相关选项供用户选择，比如学历、职位、城市；<seclet>和<option>一般同时使用
- select代表的下拉框，option表示它的每一项

>select属性

 属性         |     值                | 简介
 :------------ | :---------------------: | :-----------------------------------
 autofocus    |       autofocus       | 规定在页面加载后文本区域自动获得焦点。
 disabled     |     disabled          | 规定禁用该下拉列表
 multiple     | multiple              | 规定可选择多个选项
 method       |       get post        | 发送 form-data 的 HTTP 方法
 name         |     name              | 规定下拉列表的名称
 size         |     number            | 规定下拉列表中可见选项的数目

### option标签 ###
>option说明

- option 元素定义下拉列表中的一个选项（一个条目）
- 浏览器将 <option> 标签中的内容作为 <select> 标签的菜单或是滚动列表中的一个元素显示。option 元素位于 select 元素内部

> option属性

  属性         |     值                | 简介
 :------------ | :---------------------: | :-----------------------------------
 disabled    |       disabled       | 规定此选项应在首次加载时被禁用。。
 selected      |     selected          | 规定选项（在首次显示在列表中时）表现为选中状态。
 value       | text                 | 定义送往服务器的选项值。

### 多行文本域 textarea ###
>textarea说明

- 文本区中可容纳无限数量的文本，其中的文本的默认字体是等宽字体（通常是 Courier）
- 可以通过 cols 和 rows 属性来规定 textarea 的尺寸，不过更好的办法是使用 CSS 的 height 和 width 属性

>实例

```
<textarea rows="3" cols="20" name="intro">
我是一个中国人
</textarea>
```

### Label标签 ###

>Label说明

- <label> 标签为 input 元素定义标注（标记）
- label 元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性
- 通常用于单选或复选框

>实例

```
1）<label> <input type="radio" name="gender" id="male" />男 </label>
2）<input type="radio" name="gender" id="male" /><label for="male">男</label>
```

### 按钮 button ###

>button说明

- <button> 标签定义一个按钮
- button 元素内部，您可以放置内容，比如文本或图像
- <button> 控件 与 <input type="button"> 相比，提供了更为强大的功能和更丰富的内容
- <button> 与 </button> 标签之间的所有内容都是按钮的内容

>实例

```
<button type="button">提交</button>
```

### 框架 ###

>活动框架 iframe

- iframe 定义：创建包含另外一个文档的内联框架
- 语法` <iframe></iframe>`

>属性

- frameborder : 设置是否显示框架周围的边框
- 1 ：显示边框
- 0 ：不显示边框
- height : 设置 iframe 的高度
- width : 设置 iframe 的宽度
- name : 设置 iframe 的名称，在`<a>`标签的 taget属性写这个名字会让连接在这个活动框架中打开网页
- srolling : 设置是否在 iframe 中显示滚动条
- yes : 始终显示滚动条
- no : 始终不显示滚动条
- auto : 自动处理（内容超出框架则显示，不超出则不显示）
- src : 在 iframe 中显示的文档的 URL

>实例

```
<iframe src="http://www.baidu.com" frameborder="1" scrolling="auto" width="500" height="500">
您的浏览器不支持iframe框架
</iframe>
```

>框架集

><frameset>标签

- 属性rows 表示要分成几行,一般用”,”隔开，其中“”表示占据剩下所有的区域
- 属性 clos 表示要分成几列,一般用”,”隔开，其中“”表示占据剩下所有的区域

><frame>标签

- 属性scrolling="yes" 表示是否要显示滚动条，滚动条是竖着的
- 属性noresize="noresize" 表示 不能拖动和缩小。

>使用实例

```
<!DOCTYPE html>
<html>
    <head>
    	<meta charset="utf-8"/>
    	<title>学校管理系统</title>
    </head>
    <frameset rows="20%,80%" >
            <!-- 上面的内容 -->
            <frame src="/head.html">
            <!-- 下面的内容 -->
            <frameset cols="20%,80%">
               <frame src="/left.html" >
               <frame name="main_page" src="/main.html">
            </frameset>
     </frameset>
</html>
```


------------------------------