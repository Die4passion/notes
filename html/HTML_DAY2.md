# HTML基础　DAY2 #
### a标签 ###
>a标签说明

- a标签是行内元素，也是一个双标签
- a标签最重要的属性是 `href` 指定连接目标

>a标签属性

- `href 属性`：超链接，跳转到其它网页
- `name 属性`：创建一个文档内部的书签，也就自身网页跳转，锚链接
- `target 属性`：`_blank` 在新窗口打开 `_parent` 在父窗口打开 `_top` 在当前窗体打开

>相对路径

- 以文件本身所在当前路径进行定位 `./`表示当前目录 `../` 回到上一层目录 `目录名/` 当前目录的下级目录

>绝对路径

- 绝对路径以协议 `http://`,`https://`,`file:///`或以`/`作为前缀

### img标签 ###
>img标签说明

- img标签是一个行内标签，并且是一个单标签（自关闭）
- img能够认识的图片格式 `jpg`，`bmp`，`png`，`gif`
- 属性src必须要有
- 修改图片宽度或者高度时会等比例缩放

>img语法

- `<img src="图片路径" alt="图片文字说明"/>`
- `<img src="图片路径" alt="当图片不存在时显示这里写的内容" width="宽" height="高" title="鼠标放上去显示的内容"/>`

### audio标签 ###
>audio说明

- audio定义声音内容，可以网页中加载音乐
- 双标签
- audio支持三种音频格式mp3，ogg vorbis，wav：注意不同浏览器支持格式不同，有的支持两个，有的一个

>audio语法

```
    <audio src="音乐文件路径" autoplay="autoplay">
    你的浏览器不支持audio标签 
    </audio>
```

>audio属性

- `autoplay`：出现该属性，音频在就绪后马上播放
- `controls`:出现该属性，显示控件，如播放按钮
- `loop`：出现该属性，当音频播放结束时重新开始播放
- `muted`：出现该属性，音频在页面加载时进行加载，并预备播放，如果同时出现`autoplay`，则忽略该属性
- `src` : 播放音频的URL

### video标签 ###
>video说明

- 定义视频，可以在网页中加载视频
- 双标签

>video语法

```
    <video src="视频文件路径" controls="controls">
        你的浏览器不支持video标签
    </video>
```

>video属性

- `autoplay`出现该属性，视屏就绪后马上播放
- `controls`出现该属性，显示播放控件，如播放按钮
- `height`出现该属性，可设置播放器高度
- `loop`出现该属性，当播放完后再次开始播放
- `muted`出现该属性，该视频播放时被静音
- `poster`出现该属性，设置URL，可下载该视频
- `preload`出现该属性，视频会在页面加载时进行加载，并预备播放，使用autoplay属性，则忽略该属性
- `src`设置要播放视频的路径
- `width`设置视频的宽度

### 列表标签 ###
>无序列表ul

- ul表示整个列表
- li表示整个列表里面的成员
- 如名字一样无序排序

>例子

```
    <ul>
        <li>苹果</li>
        <li>香蕉</li>
    </ul>
```
>有序列表ol

- ol表示整个列表
- li表示整个列表里面的成员
- 会出现123排序

>例子

```
    <ol>
        <li>苹果</li>
        <li>香蕉</li>
    </ol>
```
>定义列表dl

- dl定义一个定义列表
- dt创建列表中的上层项目
- dd创建列表中的下层项目
- 类似分类

>例子

```
    <dl>
        <dt>苹果</dt>
            <dd>红苹果</dd>
            <dd>青苹果</dd>
    </dl>
```

### 表格标签 ###

>表格属性

- table标签： 定义表格区域
- tr标签： 定义表格中的行
- td标签： 定义单元格

- border属性：定义表格和单元格的边框
- width属性：设置表格的宽度
- cellspacing属性：设置单元格之间的空隙
- cellpadding属性：设置单元格内容与边框之间的空隙  

>例子				

```
<table border="1" width="500" cellspacing="0" cellpadding="0">
			<tr>
				<td>学号</td>
				<td>姓名</td>
				<td>备注</td>
			</tr>
			<tr>
				<td>1</td>
				<td>刘备</td>
				<td> </td>
			</tr>
			<tr>
				<td>2</td>
				<td>曹操</td>
				<td>雄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>张飞</td>
				<td>牛肉</td>
			</tr>
		</table>
```
### 表格合并行列 ###

>表格单元格的合并

- 在td标签上设置属性进行单元格的合并
- colspan 合并列，跨列
- rowspan 合并行

>例子

```
<table border="1" width="500" cellspacing="0" cellpadding="4">
			<tr>
				<td>学号</td>
				<td>姓名</td>
				<td>备注</td>
			</tr>
			<tr>
				<td>1</td>
				<td colspan="2">刘备</td>
			</tr>
			<tr>
				<td>2</td>
				<td rowspan="2">曹操</td>
				<td>雄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>牛肉</td>
			</tr>
		</table>
```


------------------------------