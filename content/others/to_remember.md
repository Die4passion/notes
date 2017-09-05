#一些不太好记的内容

- `form`表单上传文件时，必须加上：`enctype="mulipart/form-data"`
- 常见的http状态码 
	+ `200 ok` 客户端请求成功
	+ `301 Moved Permanently` 请求永久重定向
	+ `302 Moved Temporarily` 请求临时重定向
	+ `304 Not Moddified` 文件未修改，可以直接使用缓存的文件
	+ `400 Bad Request` 由于客户端请求有语法错误，不能被服务器所理解。
	+ `401 Unauthorized` 请求未经授权。这个状态代码必须和WWW-Authenticate报头域一起使用
	+ `403 Forbidden` 服务器收到请求，但是拒绝提供服务。
	+ `404 Not Found` 请求的资源不存在
	+ `500 Internal Server Error` 服务器发生了不可预期的错误
	+ `503 Service Unavailable` 服务器当前不能处理客户端的请求，一般是过载。

- `mysql`常用命令：
	+ `mysqldump -u用户名 -p密码 数据库名>导出的文件路径/文件名.sql` 导出数据库
	+ `mysql> source 文件名.sql`             恢复数据库
	+ 
