#PHP必会面试题

---

##基础篇

> 用PHP打印出前一天的时间格式是 2016-12-28 22:21:21

````php
//>>1.当前时间减去一天的时间，然后再格式化
echo date('Y-m-d H:i:s',time()-3600*24);

//>>2.使用strtotime，可以将任何字符串时间转换成时间戳，但是只支持英文
echo date('Y-m-d H:i:s',strtotime('-1 day'));
//在实际开发中，strtotime非常有用，因为表单提交过来的数据都是字符串。
//比如： 2016-12-28,需要通过strtotime转换为时间戳保存到数据库中
````

> 获取上个月最后一天

````php
//t 表示:给定月份所应有的天数 28 到 31
echo date('Y-m-t', strtotime('-1 month'));
````

> 写一个验证邮箱的正则表达式

````php
$email = 'xxxx@gmail.com';
$result = preg_match('/^[\w\-\.]+@[\w\-]+(\.\w+)+$/', $email);
//另一种写法             /^\w+([-.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
//写法有很多，要根据需求来
var_dump($result);  //0表示没有匹配到   非0表示匹配到。
````

> 获取上传文件类型的几种方法

````php
//第1种方法：
function get_extension($file)
{
substr(strrchr($file, '.'), 1);
}
//第2种方法：
function get_extension($file)
{
$info = pathinfo($file);
return $info['extension'];
}
//第3种方法：
function get_extension($file)
{
return pathinfo($file, PATHINFO_EXTENSION);
}
//第4种方法：
//该方法可以防止伪装图片，判断该文件的真实后缀。 最安全。
function ftype($filename){
	$file	= fopen($filename, "rb");
	$bin	= fread($file, 2); //只读2字节
	fclose($file);
	$strInfo	= @unpack("C2chars", $bin);
	$typeCode	= intval($strInfo['chars1'].$strInfo['chars2']);
	$fileType	= '';
	switch($typeCode){
		case 7790:
			$fileType = 'exe';
			break;
		case 7784:
			$fileType = 'midi';
			break;
		case 8297:
			$fileType = 'rar';
			break;
		case 255216:
			$fileType = 'jpg';
			break;
		case 7173:
			$fileType = 'gif';
			break;
		case 6677:
			$fileType = 'bmp';
			break;
		case 13780:
			$fileType = 'png';
			break;
		default:
			$fileType = 'unknown';
	}
	return $fileType;
}
//一般还是选用框架封装的方法
````

> 字符串的常用函数

````php
trim()  -- 去除字符串首尾处的空白字符（或者其他字符）
strlen()  -- 字符串长度
substr()  -- 截取字符串
str_replace()  -- 替换字符串函数
substr_replace()  -- 对指定字符串中的部分字符串进行替换
strstr()  -- 检索字符串函数
explode()  -- 分割字符串函数
implode()  -- 将数组合并成字符串
str_repeat() -- 重复一个字符串
addslashes(); -- 转义字符串
htmlspecialchars() -- THML 实体转义
````

> 数组常用函数

````php
array --  声明一个数组
count -- 计算数组中的单元数目或对象中的属性个数
foreach -- 遍历数组
list -- 遍历数组
explode -- 将字符串转成数组
implode -- 将数组转成一个新字符串
array_merge -- 合并一个或多个数组
is_array -- 检查是否是数组
print_r -- 输出数组
sort -- 数组排序
array_keys -- 返回数组中所有的键名
array_values -- 返回数组中所有的值
key -- 从关联数组中取得键名
````

> 单例模式（三私一公）

````php
class DB
{
    //私有化后在类内部保存对象并且防止外部访问到
    private static $obj=null;
    //私有化后防止在外部创建新的对象
    private function __construct() 
    {
    }
    //公有并且静态方法在类外面可以通过类名直接访问
    public static function getInstance()
    {
        if(self::$obj==null) {
            self::$obj=new self();
        }
        return self::$obj;
    }
    //私有化克隆执行的方法,防止在外部被克隆
    private function __clone(){
    }
}
````

> 设计无限极商品分类数据表

````php
//第一种，设置父id
缺点： 查询时 需要通过递归计算商品分类的层级，效率低
优点： 添加和更新速度快

//第二种,设置父id，层级和层级路径
缺点：添加和修改时需要重新计算层级和路径，查询时需要通过path排序。
优点：添加和更新速度快。

//第三种,嵌套集合,加左右边界和深度，还可以再多加tree
缺点：添加和更新复杂。
优点：将左右边界上添加索引后查询速度最快！

//根据需求选择，如果层级不太多，可选择前两种
````

> MySQL常用的存储引擎以及它们的区别

````php
MyISAM 和 InnoDB
构成上，MyISAM 的表在磁盘中有三个文件组成，分别是表定义文件（ .frm）、数据文件（.MYD）、索引文件（.MYI）,而 InnoDB 的表由表定义文件(.frm)、表空间数据和日志文件组成。

安全方面，MyISAM 强调的是性能，其查询效率较高，但不支持事务和外键等安全性方面的功能，而 InnoDB 支持事务和外键等高级功能，查询效率稍低。

对锁的支持，MyISAM 支持表锁，而 InnoDB 支持行锁。
````

> 手写部门以及员工统计的SQL(待做)

> 有一个网页地址www.baidu.com(该地址也可能是网络图片地址),使用PHP如何得到它的内容?

````php
//1. file_get_contents()技术
$content = file_get_contents("http://www.baidu.com/index.html");
file_put_contents("./baidu.html",$content);

//2. curl技术

//1.创建一个新cURL资源
$ch = curl_init();

//>>2.设置URL和相应的选项
curl_setopt($ch, CURLOPT_URL, "http://www.baidu.com/"); //设置url地址
curl_setopt($ch,CURLOPT_RETURNTRANSFER, 1);//不直接发送给浏览器
curl_setopt($ch, CURLOPT_HEADER, 0);  //头信息不作为输出

//>>3.抓取URL并把它传递给浏览器
$content = curl_exec($ch);
file_put_contents("./baidu.html",$content); //将响应内容保存

//>>4. 关闭cURL资源，并且释放系统资源
curl_close($ch);

//3. fopen技术

//>>1.打开读的文件
$resource = fopen("http://www.baidu.com/index.html", "rb");
//>>2.打开写的文件
$target = fopen("./baidu.html", "w");
//>>3.读取并且写入文件
while(($line = fgetc($resource))!==false){
      fwrite($target,$line);
}
//>>4.关闭文件
fclose($resource);
fclose($target);
````

> 谈谈对 mvc 的认识

````php
MVC 是一种使用 MVC（Model View Controller 模型-视图-控制器）设计创建 Web 应用程序的模式。

A.Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。
　　通常模型对象负责在数据库中存取数据。

B.View（视图）是应用程序中处理数据显示的部分。
　　通常视图显示模型获取的数据。

C.Controller（控制器）是应用程序中处理用户交互的部分。

　　控制器接受用户的输入并调用模型和视图去完成用户的需求。
所以当单击Web页面中的超链接和发送HTML表单时，控制器本身不输出任何东西和做任何处理。
它只是接收请求并决定调用哪个模型构件去处理请求，然后再确定用哪个视图来显示返回的数据。 
MVC 分层同时也简化了分组开发。不同的开发人员可同时开发视图、控制器逻辑和业务逻辑。它强制性的使应用程序的输入、处理和输出分开。
````

> 实现页面跳转的所有方法

````php
//方法一：php 函数跳转，缺点，header 头之前不能有输出，跳转后的程序继续执行，
//可用 exit 中断执行后面的程序。
header("Location: 网址");	//直接跳转
header("refresh:3;url=http://axgle.za.net");	//三秒后跳转
exit；
方法二：利用 meta
echo "<meta http-equiv=refresh content='0; url=网址'>";
````

> 冒泡排序，快速排序（手写）

````php
//冒泡排序：
function bubbleSort($arr)
{
    $len=count($arr);
    //该层循环控制 需要冒泡的轮数
    for($i=1;$i<$len;$i++)
    { //该层循环用来控制每轮 冒出一个数 需要比较的次数
        for($k=0;$k<$len-$i;$k++)
        {
            if($arr[$k]>$arr[$k+1])
            {
                $tmp=$arr[$k+1];
                $arr[$k+1]=$arr[$k];
                $arr[$k]=$tmp;
            }
        }
    }
    return $arr;
}

//快速排序：
function quick_sort($arr)
{
    //先判断是否需要继续进行
    $length = count($arr);
    if ($length <= 1) {
        return $arr;
    }
    //选择第一个元素作为基准
    $base_num = $arr[0];
    //遍历除了标尺外的所有元素，按照大小关系放入两个数组内
    //初始化两个数组
    $left_array = array();  //小于基准的
    $right_array = array();  //大于基准的
    for ($i = 1; $i < $length; $i++) {
        if ($base_num > $arr[$i]) {
            //放入左边数组
            $left_array[] = $arr[$i];
        } else {
            //放入右边
            $right_array[] = $arr[$i];
        }
    }
    //再分别对左边和右边的数组进行相同的排序处理方式递归调用这个函数
    $left_array = quick_sort($left_array);
    $right_array = quick_sort($right_array);
    //合并
    return array_merge($left_array, array($base_num), $right_array);;
}

$arr = array(5, 1, 0, 3, 9, 10, 59, 41, 78, 56, 45, 47, 12, 15, 45, 11);
$rs = quick_sort($arr);
print_r($rs);
````

---

##高级篇

> 你做过接口开发吗？（APP）

答： 做过， 接口开发分为两种：

1. 使用别人的接口。 
    - 我使用过支付宝、微信的接口
2. 开发接口给别人使用。
    1. 大致按照需求写出一个API的说明问题  
    2. 然后按照文档的约束来属性代码  
    3. 可以通过Google浏览器中的PostMan插件来模拟APP的请求来测试接口代码。

> 你做过微信开发吗

> 做过哪些项目？团队组成是什么,项目使用多长时间？

> 请简单说说你的项目？

> Redis在项目中的应用

> Session和Cookie的区别？

> 禁用 COOKIE 后 SEESION 还能用吗?

:   可以，COOKIE 和 SESSION 都是用来实现会话机制的，由于 http 协议是无状态的，所以要想跟踪一个用户在同一个网站之间不同页面的 状态，需要有这么一个机制----会话机制。
COOKIE：将会话信息的保存到浏览器端。
SESSION：将会话信息保存到服务器端。
SESSION 默认情况下是基于 COOKIE 的，对于 SESSION 来说，每生成一个 SESSIONID，都会将其发送到浏览器端，让后将其保存到 cookie 当中。
如果禁用了 COOKIE，则基于 COOKIE 的 SESSION 不好使了，我们可以使用 get，传递 SID，或者直接开启透明的 SID（此时需要关闭基于 cookie 的 SESSION 配置项）。
我认为：在项目中如何cookie被禁用了可以提示用户不让他来访问当前网站。因为通过get方式传递SID的话不安全。

> 你是如何学PHP的？

> 手写基本的常用的SQL语句。

> 给一个指定的地址发送POST请求

````php
/初始化
$curl = curl_init();
//设置抓取的url
curl_setopt($curl, CURLOPT_URL, 'http://www.baidu.com');
//设置头文件的信息作为数据流输出
curl_setopt($curl, CURLOPT_HEADER, 1);
//设置获取的信息以文件流的形式返回，而不是直接输出。
curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
//设置post方式提交
curl_setopt($curl, CURLOPT_POST, 1);
//设置post数据
$post_data = array(
    "username" => "coder",
    "password" => "12345"
);
curl_setopt($curl, CURLOPT_POSTFIELDS, $post_data);
//执行命令
$data = curl_exec($curl);
//关闭URL请求
curl_close($curl);
//显示获得的数据
print_r($data);
````

> 常用的Linux命令

````
vi:     vi编辑器
ls： 列出目录
cp： 复制
rm： 删除
cat：    将文件的内容打印到标准输出
mkdir：  建立目录
tar：    打包压缩
ps： 查看进程
top：    查看机器使用情况
df： 检查磁盘空间占用情况
find：   在指定路径下查找指定文件
grep：   过滤文本
cd： 改变当前工作目录
mount：  挂载/卸载指定的文件系统
ifconfig：   配置网络或显示当前网络接口状态
telnet:：    远程登录
````

> 对于大流量的网站, 您采用什么样的方法来解决访问量问题?

> Nginx和Apache的区别？

- nginx 相对 apache 的优点：
    + 轻量级，同样起web 服务，比apache 占用更少的内存及资源
    + 抗并发，nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能
    + 高度模块化的设计，编写模块相对简单
    + 社区活跃，各种高性能模块出品迅速
- apache 相对nginx 的优点：
    + rewrite ，比nginx 的rewrite 强大
    + 模块超多，基本想到的都可以找到
    + 少bug ，nginx 的bug 相对较多
    + 超稳定
    
> 列举 web 开发中的安全性问题

1. sql 注入攻击。
2. 数据库操作安全，UPDATE、 DELETE、INSERT 的操作没有限制用户操作权限，这将是一件很危险的事情。
3. 没有验证用户 http 请求的方式 POST 或者 GET，GET 请求被合法通过。
4. 没有验证表单来源的唯一性，不能识别是合法的表单提交还是黑客伪造的表单提交。
5. XSS 攻击。

> 如果某段与数据库交互的程序运行较慢你将如何处理 ?

第一：找到问题

1. 开启MySQL的慢查询，定位什么样的SQL执行速度慢
2. 使用explain关键字分析SQL为什么慢

第二：解决问题

1. 增加索引，优化表的结构。
2. 优化程序代码，如果查询比较多，可以尽量用条件查询，减少查询语句，比如能用条查询语句就不用两条。
3. 修改MySQL的配置来优化MySQL服务器。

> 如何解决Ajax跨越（站）问题？

使用JSONP或者是JQuery-jsonp插件,或者使用php中转

> 假设有一个博客系统，数据库存储采用 mysql，用户数量为 1000 万，预计文章总数为10 亿，每天有至少 10 万的更新量，每天访问量为 5000 万，对数据库的读写操作的比例超过 10：1，你如何设计该系统，以确保其系统高效，稳定的运行？提示：可以从数据库设计，系统框架，及网络架构方面进行描述，可以自由发挥 （新浪网技术部）

> 如何分析你的MySQL查询是否高效?

    explain

> 大型网站优化？

> PHP如何防止SQL注入

````php
//有两种方式：PDO和mysqli

//mysqli的形式:
//>>1.连接数据库
$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

//>>2.检查连接是否正确
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$city = "Amersfoort";
//>>3.设置预处理的SQL
if ($stmt = $mysqli->prepare("SELECT District FROM City WHERE Name=?")) {
    //>>4.设置SQL上的参数
    $stmt->bind_param("s", $city);
    //>>5.执行SQL
    $stmt->execute();
    //>>6.获取执行结果
    $result = $stmt->get_result();
    //>>7.接的结果中的每行数据
    while ($row = $result->fetch_assoc()) {
            var_dump($row);
    }
    //>>8.关闭执行结果
    $stmt->close();
}
    //>>9.关闭连接
$mysqli->close();

//PDO的形式
//>>1.连接数据库
$pdo = new PDO('mysql:dbname=dbtest;host=127.0.0.1;charset=utf8', 'user', 'pass');
//>>2.使用本地预处理使用预处理，如果不能够使用将强制模拟预处理
$pdo->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
//>>3.将错误模式修改为异常模式，发生错误时可以发生异常让程序员加以修改
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
//>>4.准备预处理语句
$stmt = $pdo->prepare('SELECT * FROM employees WHERE name = :name');
//>>5.设置预处理语句中的参数并且执行预处理语句
$stmt->execute(array(':name' => $name));
//>>6.循环执行结果
foreach ($stmt as $row) {
    var_dump($row);
}
````

> 如何实现页面静态化缓存以及应用场景。

结合项目说。
网站首页静态化，商品页面静态化。非静态的内容通过Ajax解决

> 八百万条海量数据的数据库设计和网站处理方案。

分表或者分库或者分区方面以及从MySQL的架构（读写分离和主从）。以及SQL的优化方案。
最好是举例说明并且绕过这个问题来说工作中如何解决类似的问题。