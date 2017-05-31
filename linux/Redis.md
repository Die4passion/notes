#Redis速成

> 

常用操作|
:---|---
set | 将字符串的value关联到key
get| 返回key关联的字符串值
mset|同时设置多个key-value
mget|返回所有给定key的值
incr key|将key中存储的数字值+1
decr key| 同上，-1
keys *| 获取所有key列表
del key | 删除key
expire key xx| 设置key xx秒后过期
ttl key | 查看key的过期时间
flushall| 清空整个redis服务器数据

> 

列表操作|
---|---
lpush | 将一个或多个value插入key的表头
rpush| 将一个或多个value插入key的表尾
lpop|移除并返回key的头元素
rpop|移除并返回key的尾元素 
lrange key start stop|返回key中指定区间内的元素
lrem key count value|根据count值移除value(略)
lindex key index|返回key中下标为index的元素
ltrim key start stop|对一个列表进行裁剪


> 

set集合操作|
---|---
sadd key member|将一个或多个member加入到key中（忽略已存在member）
srem key member|移除集合key中的一个或多个member（忽略已存在member）
smembers key |返回集合key中的所有成员
hash操作|以下
hset key name value|添加一个name=>value键值对到key这个hash类型
hget key name|获取hash类型的name键的值
hmset key name value(name1, value2)|批量添加hash类型
hmget keys name1 name2|批量获取
hkeys|获取所有键
hvals|获取所有值
hgetall|获取所有键和值

