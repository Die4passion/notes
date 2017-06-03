###Yii2 常用语法

- 控制器
    + 文件保存在`controllers`目录下面
    + 文件名`XxxController.php` 大驼峰
    + 命名空间必须和当前路径对应`namespace fronted\controllers`
    + 控制器类名必须和文件名一致
    + web应用的控制器必须继承 `yii\web\Controller`

- 操作
    + 必须是`public function`
    + `actionXxx()` 必须以`action`开头 小驼峰
    + 浏览器访问 `r=控制器名/操作名` 都是小写
    + 选择视图并分配数据`return $this->render('视图名',[key=>变量])`

- 视图
    + 存放到`views`目录下面，以控制器命名的文件夹里
    + 视图文件以`.php`结尾

- 表单模型
	+ 存放到`models`目录下
	+ 文件名 `XxxForm.php`
	+ 命名空间必须和当前路径对应`namespace fronted\models`
	+ 类名必须和文件名一致
	+ 表单模型类必须继承`yii\base\Model`

