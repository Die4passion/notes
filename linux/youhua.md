#大型网站优化

###day1

> 三个概念：

- PV: Page Views 每一次访问每一个页面均+1
 - UV：Unique Visitor 每个人记录一次
 - IP： 根据访问的ip记录 +1

> 带来的问题：

- 大并发： 在同一个时间点访问量过大
	1. 分层
	1. 负债均衡
		- 硬件:F5 
		- 软件：Nginx LVS
	1. 数据库
		- 读写分离
		- 主从同步
	1. 集群 *大量服务器*
- 大流量： 大量的图片视频等，需要更大的带宽
	1. 设置背景图
	2. apache自带压缩机制`gzip` `deflate`等压缩文件的方法
		- 图片尽量使用jpg格式，音频可考虑wma或mp3格式
	1. 使用CDN服务
		- 全局负载均衡dns+每个节点一台cache
		- 根据用户ip地址实现就近访问
	1. 升级服务器带宽
- 大存储： 大量的读写操作
	1. 使用缓存技术
		- 页面静态化
		- 内存缓存：redis memcached mysql的memory引擎等
	1. 优化数据库
		- 表的设计满足3NF
		    <small>为了提高查询的效率反3NF</small>
		- 优化sql语句
		    <small>使用慢查询和explain工具定位速度慢的sql语句，进行优化</small>

		- 分表分区
		- 读写分离
		- 优化my.ini配置
		- 升级软硬件
		- 经常用来查询的字段添加索引

			primary|fulltext|unique|key|spatial
			---|---|---|---|---
			主键索引|全文索引|唯一索引|普通索引|空间索引

		- 选择合适的存储引擎
		    
		    特点|Myisam|InnoDB
		    ---|---|---
		    批量插入的速度|高|低
		    事务安全| |支持
		    全文索引 |支持|
		    锁机制|表锁|行锁
		    存储限制|没有|64TB
		    哈希索引||支持
		    数据缓存||支持
		    数据压缩|支持|
		    空间使用|低|高
		    内存使用|低|高
		    支持外键||支持

>多使用静态网址

- 实现方式：
	1.  php的ob缓存技术
	1.  模板替换技术
- 优点：
	1. 减少服务器对数据响应的负荷
	2. 加载不用调用数据库，响应速度快
	3. 安全性高，没有sql注入可能
	4. 利于seo
- 缺点：
	1. 空间占用比较大
	2. 生成文件太多，服务器响应变慢（一定要多分目录）

>伪静态网址

- 实现方式：
	1. 直接在页面使用正则表达式解析成伪静态 `PATH_INFO`
	2. 通过apache的rewrite来实现


>何时使用真静态何时使用伪静态？

- 网页实时性不高，不需要真静态
- 网站访问量小，不需要静态化
- 数据不是经常变化，访问频率极大，使用真静态
- 海量数据，使用伪静态分目录创建静态页面，并使用网络硬盘
- 安全性要求高、不愿意被seo、不希望数据暴露给spider 不使用静态化 `网站后台`

###day2

>Nginx的优点

- 高并发可达5万
- 内存消耗很低
- 配置文件程序员友好
- 成本低
- 支持rewrite重写规则
- 内置健康检查功能
- 节省带宽
- 稳定性高


```
	    head += self.get_meta()
            if not self.settings.get('skip_default_stylesheet'):
                head += self.get_stylesheet()
            head += self.get_javascript()
            head += self.get_highlight()
            head += self.get_mathjax()
            head += self.get_uml()
            head += self.get_title()
            head += '<link rel="stylesheet" href="http://cdn.jsdelivr.net/highlight.js/9.11.0/styles/qtcreator_dark.min.css"> <script src="http://cdn.jsdelivr.net/highlight.js/9.11.0/highlight.min.js"></script> <script>hljs.initHighlightingOnLoad();</script>'
            html = load_utf8(html_template)
            html = html.replace('{{ HEAD }}', head, 1)
            html = html.replace('{{ BODY }}', body, 1)
        else:
            html = u'<!DOCTYPE html>'
            html += '<html><head><meta charset="utf-8">'
            html += self.get_meta()
            html += self.get_stylesheet()
            html += self.get_javascript()
            html += self.get_highlight()
            html += self.get_mathjax()
            html += self.get_uml()
            html += self.get_title()
            html += '<link rel="stylesheet" href="http://cdn.jsdelivr.net/highlight.js/9.11.0/styles/qtcreator_dark.min.css"> <script src="http://cdn.jsdelivr.net/highlight.js/9.11.0/highlight.min.js"></script> <script>hljs.initHighlightingOnLoad();</script>'
            html += '</head><body>'
            html += '<article class="markdown-body">'
            html += body
            html += '</article>'
            html += '</body>'
            html += '</html>'
```


```html
	<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf8">
	{{ HEAD }}
	<link rel="stylesheet" type="text/css" href="http://netdna.bootstrapcdn.com/bootstrap/3.1.0/css/bootstrap.min.css">
	<script type="text/javascript" src="http://code.jquery.com/jquery-2.1.0.min.js"></script>
	<style type="text/css">
		body {
			background-color: #EEE;
			margin: 2em 0;
			font-size: 12pt;
			font-family: "Helvetica Neue", "Helvetica", "Arial", "Hiragino Sans GB", sans-serif;
			line-height: 150%;
		}
		blockquote {
			font-size: inherit;
		}
		#container {
			background-color: #FFF;
			/*box-shadow: 0px 0px 10px #CCC;*/
			/*border: 3px solid #DDD;*/
			padding: 1em 2em;
			box-shadow: 0 0 0.5em #e6e6e6 inset,0 0 0 1px #d3d3d3;
			border-radius: 0.5em;
			margin-bottom: 1em;
		}
		h1, h2, h3, h4, h5, h6 {
			border-bottom: 1px solid #EEE;
			padding-bottom: 0.25em;
			font-weight: bold;
			margin-top: 1em;
		}
		img {
			border: 3px solid #e6e6e6;
			padding: 1em;
			box-shadow: 0px 0px 10px #d3d3d3;
		}
		@media (-webkit-min-device-pixel-ratio: 1.5), (min-resolution: 144dpi) {
			img { zoom: 0.5; }
		}
	</style>
</head>
<body>
	<div id="container" class="container">
		{{ BODY }}
	</div>
	<footer class="container">
		<p class="text-muted text-center">
			Markdown Preview Theme designed by <a href="https://twitter.com/hozaka" target="_blank">@hozaka</a>.
		</p>
	</footer>
</body>
</html>

<script type="text/javascript">
$(document).ready( function() {
	$('table').addClass('table table-bordered');
});
</script>


```