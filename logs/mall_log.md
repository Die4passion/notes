# YII2商城项目 #

### DAY1 ###

----------

####需求

* 品牌的增删改查
	- 简单的增删改查
* 品牌删除使用逻辑删除 （status=>‐1）
* 图片上传使用uploadify插件,上传成功后回显图片 
	- 安装插件，在composer.josn里的`requir`里添加`"xj/yii2-uploadify-widget": "~2.0.0",`
	- 在命令行执行composer update，
	- 视图里添加

```
use yii\web\JsExpression;
//外部TAG
echo Html::fileInput('test', NULL, ['id' => 'test']);
echo Uploadify::widget([
    'url' => yii\helpers\Url::to(['s-upload']),
    'id' => 'test',
    'csrf' => true,
    'renderTag' => false,
    'jsOptions' => [
        'width' => 120,
        'height' => 40,
        'onUploadError' => new JsExpression(<<<EOF
function(file, errorCode, errorMsg, errorString) {
    console.log('The file ' + file.name + ' could not be uploaded: ' + errorString + errorCode + errorMsg);
}
EOF
),
        'onUploadSuccess' => new JsExpression(<<<EOF
function(file, data, response) {
    data = JSON.parse(data);
    if (data.error) {
        console.log(data.msg);
    } else {
        console.log(data.fileUrl);
 		$('#logos').hide();
        
        $('#img_logo').attr('src',data.fileUrl).show();
        $("#brand-logo").val(data.fileUrl);
    }
}
EOF
),
    ]
]);
```

- 控制器里添加

```
use xj\uploadify\UploadAction;

public function actions() {
    return [
        's-upload' => [
            'class' => UploadAction::className(),
            'basePath' => '@webroot/upload',
            'baseUrl' => '@web/upload',
            'enableCsrf' => true, // default
            'postFieldName' => 'Filedata', // default
            //BEGIN METHOD
            'format' => [$this, 'methodName'], 
            //END METHOD
            //BEGIN CLOSURE BY-HASH
            'overwriteIfExist' => true,
            /*'format' => function (UploadAction $action) {
                $fileext = $action->uploadfile->getExtension();
                $filename = sha1_file($action->uploadfile->tempName);
                return "{$filename}.{$fileext}";*/
            },
            //END CLOSURE BY-HASH
            //BEGIN CLOSURE BY TIME
            'format' => function (UploadAction $action) {
                $fileext = $action->uploadfile->getExtension();
                $filehash = sha1(uniqid() . time());
                $p1 = substr($filehash, 0, 2);
                $p2 = substr($filehash, 2, 2);
                return "{$p1}/{$p2}/{$filehash}.{$fileext}";
            },
            //END CLOSURE BY TIME
            'validateOptions' => [
                'extensions' => ['jpg', 'png'],
                'maxSize' => 1 * 1024 * 1024, //file size
            ],
            'beforeValidate' => function (UploadAction $action) {
                //throw new Exception('test error');
            },
            'afterValidate' => function (UploadAction $action) {},
            'beforeSave' => function (UploadAction $action) {},
            'afterSave' => function (UploadAction $action) {
                $action->output['fileUrl'] = $action->getWebUrl();
                $action->getFilename(); // "image/yyyymmddtimerand.jpg"
                $action->getWebUrl(); //  "baseUrl + filename, /upload/image/yyyymmddtimerand.jpg"
                $action->getSavePath(); // "/var/www/htdocs/upload/image/yyyymmddtimerand.jpg"
            },
        ],
    ];
}
```

* 图片上传到七牛云
	- 安装方法同上
	- 使用方法，在配置main里添加

```
'qiniu' => [
				'class' => \backend\components\Qiniu::className(),
				'accessKey' => 'WHXsr4075A1KrBf71ihzM4-eeDU-F4PoexU6uBga',
				'secretKey' => 'REb1eyWPvV9nd7pvc5IFpTW85pXCUsrp6slXkdKm',
				'bucket' => 'yiishop',
				'domain' => 'http://or9o8pc7h.bkt.clouddn.com/',
			]
```

- 控制器里在原来的uploadify里修改添加

```
'afterSave' => function (UploadAction $action) {
						$qiniu=\Yii::$app->qiniu;
						$qiniu->UploadFile($action->getSavePath(),$action->getWebUrl());
						$url=$qiniu->getLink($action->getWebUrl());
						$action->output['fileUrl']=$url;
```

- 自己写一个组件(因为插件自带的组件不好自定义属性)

```
<?php
	
	namespace backend\components;
	use yii\base\Component;
	use yii\web\HttpException;
	
	class Qiniu extends Component
	{
		public $up_host = 'http://up-z2.qiniu.com';
		const RS_HOST = 'http://rs.qbox.me';
		const RSF_HOST = 'http://rsf.qbox.me';
		
		public $accessKey;
		public $secretKey;
		public $bucket;
		public $domain;
		
		/*  function __construct($accessKey, $secretKey, $domain, $bucket = '')
		  {
			  $this->accessKey = $accessKey;
			  $this->secretKey = $secretKey;
			  $this->domain = $domain;
			  $this->bucket = $bucket;
		  }*/
		
		/**
		 * 通过本地文件上传
		 *
		 * @param $filePath
		 * @param null $key
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		
		public function uploadFile($filePath, $key = null, $bucket = '')
		{
			if (!file_exists($filePath)) {
				throw new HttpException(400, "上传的文件不存在");
			}
			$bucket = $bucket ? $bucket : $this->bucket;
			
			$uploadToken = $this->uploadToken(array ( 'scope' => $bucket ));
			$data = [];
			if (class_exists('\CURLFile')) {
				$data['file'] = new \CURLFile($filePath);
			} else {
				$data['file'] = '@' . $filePath;
			}
			$data['token'] = $uploadToken;
			if ($key) {
				$data['key'] = $key;
			}
			$ch = curl_init();
			curl_setopt($ch, CURLOPT_URL, $this->up_host);
			curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
			curl_setopt($ch, CURLOPT_POST, true);
			curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
			$result = curl_exec($ch);
			$status = curl_getinfo($ch, CURLINFO_HTTP_CODE);
			curl_close($ch);
			$result = $this->response($result);
			if ($status == 200) {
				return $result;
			} else {
				throw new HttpException($status, $result['error']);
			}
		}
		
		/**
		 * 通过图片URL上传
		 *
		 * @param $url
		 * @param null $key
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		public function uploadByUrl($url, $key = null, $bucket = '')
		{
			$filePath = tempnam(sys_get_temp_dir(), 'QN');
			copy($url, $filePath);
			$result = $this->uploadFile($filePath, $key, $bucket);
			unlink($filePath);
			return $result;
		}
		
		/**
		 * 获取资源信息 http://developer.qiniu.com/docs/v6/api/reference/rs/stat.html
		 *
		 * @param $key
		 * @param string $bucket
		 *
		 * @return array
		 */
		
		public function stat($key, $bucket = '')
		{
			$bucket = $bucket ? $bucket : $this->bucket;
			$encodedEntryURI = self::urlBase64Encode("{$bucket}:{$key}");
			$url = "/stat/{$encodedEntryURI}";
			return $this->fileHandle($url);
		}
		
		/**
		 * 移动资源 http://developer.qiniu.com/docs/v6/api/reference/rs/move.html
		 *
		 * @param $key
		 * @param $bucket2
		 * @param bool $key2
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		public function move($key, $bucket2, $key2 = false, $bucket = '')
		{
			$bucket = $bucket ? $bucket : $this->bucket;
			if (!$key2) {
				$key2 = $bucket2;
				$bucket2 = $bucket;
			}
			$encodedEntryURISrc = self::urlBase64Encode("{$bucket}:{$key}");
			$encodedEntryURIDest = self::urlBase64Encode("{$bucket2}:{$key2}");
			$url = "/move/{$encodedEntryURISrc}/{$encodedEntryURIDest}";
			return $this->fileHandle($url);
		}
		
		/**
		 * 将指定资源复制为新命名资源。http://developer.qiniu.com/docs/v6/api/reference/rs/copy.html
		 *
		 * @param $key
		 * @param $bucket2
		 * @param bool $key2
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		public function copy($key, $bucket2, $key2 = false, $bucket = '')
		{
			$bucket = $bucket ? $bucket : $this->bucket;
			if (!$key2) {
				$key2 = $bucket2;
				$bucket2 = $bucket;
			}
			$encodedEntryURISrc = self::urlBase64Encode("{$bucket}:{$key}");
			$encodedEntryURIDest = self::urlBase64Encode("{$bucket2}:{$key2}");
			$url = "/copy/{$encodedEntryURISrc}/{$encodedEntryURIDest}";
			return $this->fileHandle($url);
		}
		
		/**
		 * 删除指定资源  http://developer.qiniu.com/docs/v6/api/reference/rs/delete.html
		 *
		 * @param $key
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		public function delete($key, $bucket = '')
		{
			$bucket = $bucket ? $bucket : $this->bucket;
			$encodedEntryURI = self::urlBase64Encode("{$bucket}:{$key}");
			$url = "/delete/{$encodedEntryURI}";
			return $this->fileHandle($url);
		}
		
		
		/**
		 * 批量操作（batch） http://developer.qiniu.com/docs/v6/api/reference/rs/batch.html
		 *
		 * @param $operator [stat|move|copy|delete]
		 * @param $files
		 *
		 * @throws HttpException
		 */
		public function batch($operator, $files)
		{
			$data = '';
			foreach ($files as $file) {
				if (!is_array($file)) {
					$encodedEntryURI = self::urlBase64Encode($file);
					$data .= "op=/{$operator}/{$encodedEntryURI}&";
				} else {
					$encodedEntryURI = self::urlBase64Encode($file[0]);
					$encodedEntryURIDest = self::urlBase64Encode($file[1]);
					$data .= "op=/{$operator}/{$encodedEntryURI}/{$encodedEntryURIDest}&";
				}
			}
			return $this->fileHandle('/batch', $data);
		}
		
		/**
		 * 列举资源  http://developer.qiniu.com/docs/v6/api/reference/rs/list.html
		 *
		 * @param string $limit
		 * @param string $prefix
		 * @param string $marker
		 * @param string $bucket
		 *
		 * @throws HttpException
		 */
		public function listFiles($limit = '', $prefix = '', $marker = '', $bucket = '')
		{
			$bucket = $bucket ? $bucket : $this->bucket;
			$params = array_filter(compact('bucket', 'limit', 'prefix', 'marker'));
			$url = self::RSF_HOST . '/list?' . http_build_query($params);
			return $this->fileHandle($url);
		}
		
		protected function fileHandle($url, $data = array ())
		{
			if (strpos($url, 'http://') !== 0) {
				$url = self::RS_HOST . $url;
			}
			
			if (is_array($data)) {
				$accessToken = $this->accessToken($url);
			} else {
				$accessToken = $this->accessToken($url, $data);
			}
			
			$ch = curl_init();
			curl_setopt($ch, CURLOPT_HTTPHEADER, array (
				'Authorization: QBox ' . $accessToken,
			));
			
			curl_setopt($ch, CURLOPT_URL, $url);
			curl_setopt($ch, CURLOPT_POST, true);
			
			curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
			curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
			$result = curl_exec($ch);
			$status = curl_getinfo($ch, CURLINFO_HTTP_CODE);
			curl_close($ch);
			$result = $this->response($result);
			if ($status == 200) {
				return $result;
			} else {
				throw new HttpException($status, $result['error']);
			}
		}
		
		public function uploadToken($flags)
		{
			if (!isset($flags['deadline'])) {
				$flags['deadline'] = 3600 + time();
			}
			$encodedFlags = self::urlBase64Encode(json_encode($flags));
			$sign = hash_hmac('sha1', $encodedFlags, $this->secretKey, true);
			$encodedSign = self::urlBase64Encode($sign);
			$token = $this->accessKey . ':' . $encodedSign . ':' . $encodedFlags;
			return $token;
		}
		
		public function accessToken($url, $body = false)
		{
			$parsed_url = parse_url($url);
			$path = $parsed_url['path'];
			$access = $path;
			if (isset($parsed_url['query'])) {
				$access .= "?" . $parsed_url['query'];
			}
			$access .= "\n";
			if ($body) {
				$access .= $body;
			}
			$digest = hash_hmac('sha1', $access, $this->secretKey, true);
			return $this->accessKey . ':' . self::urlBase64Encode($digest);
		}
		
		/**
		 * 可以传输的base64编码
		 *
		 * @param $str
		 *
		 * @return mixed
		 */
		
		public static function urlBase64Encode($str)
		{
			$find = array ( "+", "/" );
			$replace = array ( "-", "_" );
			return str_replace($find, $replace, base64_encode($str));
		}
		
		/**
		 * 获取文件下载资源链接
		 *
		 * @param string $key
		 *
		 * @return string
		 */
		public function getLink($key = '')
		{
//         $url = "http://{$this->domain}/{$key}";
			$url = rtrim($this->domain, '/') . "/{$key}";
			return $url;
		}
		
		
		/**
		 * 获取响应数据
		 *
		 * @param  string $text 响应头字符串
		 *
		 * @return array        响应数据列表
		 */
		private function response($text)
		{
			return json_decode($text, true);
		}
		
		/* public function __destruct()
		 {
		 }*/
	}
```

### DAY2 ###

----------

##文章分类管理

#### 需求

+ 文章表使用垂直分表,分两张表保存,文章表﴾article﴿和文章分类表﴾article_category﴿ 
	- 建两个表分别储存
+ 文章表的增删改查﴾多模型同时输入﴿
	- 同时储存

### DAY3 ###

----------

####需求 

+ 商品分类增删改查 
	- 删除时判断分类下是否有商品，有就不能删除
+ 使用嵌套集合保存分类数据 
	- 安装嵌套集合插件`"creocoder/yii2‐nested‐sets": "^0.9.0"`, 方法同上
	- 使用方法

```
use creocoder\nestedsets\NestedSetsQueryBehavior;
	use yii\db\ActiveQuery;
	
	
	class GoodCategoryQuery extends ActiveQuery
	{
		public function behaviors()
		{
			return [
				NestedSetsQueryBehavior::className(),
			];
		}
	}
```

+ 使用ztree插件展示商品分类
	- ztree插件官网 http://www.ztree.me

```
echo '<ul id="treeDemo" class="ztree"></ul>';
$this->registerCssFile('@web/zTree/css/zTreeStyle/zTreeStyle.css');
	$this->registerJsFile('@web/zTree/js/jquery.ztree.core.js', [ 'depends' => \yii\web\JqueryAsset::className() ]);
	$zNodes = yii\helpers\Json::encode($goodscategory);
	$js = new yii\web\JsExpression(
		<<<JS
 var zTreeObj;
   // zTree 的参数配置，深入使用请参考 API 文档（setting 配置详解）
   var setting = {
   	data: {
		simpleData: {
			enable: true,
			idKey: "id",
			pIdKey: "parent_id",
			rootPId: 0
		}
	},
	callback: {
		onClick: function(event, treeId, treeNode) {
		  	$('#goodscategory-parent_id').val(treeNode.id);
		}
	}
   };
   // zTree 的数据属性，深入使用请参考 API 文档（zTreeNode 节点数据详解）
   var zNodes = {$zNodes};
   /*$(document).ready(function(){
	   zTreeObj = $.fn.zTree.init($("#treeDemo"), setting, zNodes);
   });*/
   zTreeObj=$.fn.zTree.init($("#treeDemo"),setting,zNodes);
   zTreeObj.expandAll(true);
   var node=zTreeObj.getNodeByParam('id',$('#goodscategory-parent_id').val(),null);
   zTreeObj.selectNode(node);
  
JS
	);
	$this->registerJs($js);
```

### DAY4 ###

----------

####需求

+ 商品增删改查 
	- 逻辑删除
+ 商品相册图片添加,删除和列表展示 
	- 新增一个数据库，利用uploadify来上传

```
<?php
	use xj\uploadify\Uploadify;
	use yii\bootstrap\Html;
	use yii\web\JsExpression;
	
	echo Html::fileInput('test', NULL, [ 'id' => 'test' ]);
	echo Uploadify::widget([
							   'url' => yii\helpers\Url::to([ 's-upload' ]),
							   'id' => 'test',
							   'csrf' => true,
							   'renderTag' => false,
							   'jsOptions' => [
								   'formData' => [ 'goods_id' => $goods->id ],//上传文件的同时传参goods_id
								   'width' => 120,
								   'height' => 40,
								   'onUploadError' => new JsExpression(<<<EOF
function(file, errorCode, errorMsg, errorString) {
    console.log('The file ' + file.name + ' could not be uploaded: ' + errorString + errorCode + errorMsg);
}
EOF
								   ),
								   'onUploadSuccess' => new JsExpression(<<<EOF
function(file, data, response) {
    data = JSON.parse(data);
    if (data.error) {
        console.log(data.msg);
    } else {
        console.log(data);
        //$("#brand-logo").val(data.fileUrl);
        //$("#img").attr("src",data.fileUrl);
        var html='<tr data-id="'+data.goods_id+'" id="gallery_'+data.goods_id+'">';
        html += '<td><img src="'+data.fileUrl+'" /></td>';
        html += '<td><button type="button" class="btn btn-danger del_btn">删除</button></td>';
        html += '</tr>';
        $("table").append(html);
    }
}
EOF
								   ),
							   ]
						   ]);

?>
<?= \yii\bootstrap\Html::a('返回', [ 'goods/index' ], [ 'class' => 'btn btn-primary' ]); ?>

	<table class="table">
		<tr>
			<th>图片</th>
			<th>操作</th>
		</tr>
		<?php foreach ($goods->goodsLogo as $gallery): ?>
			<tr id="gallery_<?= $gallery->id ?>" data-id="<?= $gallery->id ?>">
				<td><?= Html::img($gallery->logo) ?></td>
				<td><?= Html::button('删除', [ 'class' => 'btn btn-danger del_btn' ]) ?></td>
			</tr>
		<?php endforeach; ?>
	</table>
<?php
	$url = \yii\helpers\Url::to([ 'del-logo' ]);
	$this->registerJs(new JsExpression(
						  <<<EOT
    $("table").on('click',".del_btn",function(){
        if(confirm("确定删除该图片吗?")){
        var id = $(this).closest("tr").attr("data-id");
            $.post("{$url}",{id:id},function(data){
                if(data=="success"){
                    //alert("删除成功");
                    $("#gallery_"+id).remove();
                }
            });
        }
    });
EOT
	
					  ));
控制器：
public function actionLogo($id)
		{
			$goods = Goods::findOne([ 'id' => $id ]);
			if ($goods == null) {
				throw new NotFoundHttpException('商品不存在');
			}
			return $this->render('logo', [ 'goods' => $goods ]);
			
		}
		
		/*
		 * AJAX删除图片
		 */
		public function actionDelLogo()
		{
			$id = \Yii::$app->request->post('id');
			$model = GoodsLogo::findOne([ 'id' => $id ]);
			if ($model && $model->delete()) {
				return 'success';
			} else {
				return 'fail';
			}
			
		}
```

+ 商品列表页可以进行搜索 
	- 封装一个表单

```
namespace backend\models;
	
	
	use yii\base\Model;
	use yii\db\ActiveQuery;
	
	class GoodsSearchForm extends Model
	{
		public $name;
		public $sn;
		public $minPrice;
		public $maxPrice;
		
		public function rules()
		{
			return [
				[ 'name', 'string', 'max' => 50 ],
				[ 'sn', 'string' ],
				[ 'minPrice', 'double' ],
				[ 'maxPrice', 'double' ],
			
			];
		}
		
		public function search(ActiveQuery $query)
		{
			//加载表单提交的数据
			$this->load(\Yii::$app->request->get());
			if ($this->name) {
				$query->andWhere([ 'like', 'name', $this->name ]);
			}
			if ($this->sn) {
				$query->andWhere([ 'like', 'sn', $this->sn ]);
			}
			if ($this->maxPrice) {
				$query->andWhere([ '<=', 'shop_price', $this->maxPrice ]);
			}
			if ($this->minPrice) {
				$query->andWhere([ '>=', 'shop_price', $this->minPrice ]);
			}
		}
	}
```

+ 新增商品自动生成sn,规则为年月日+今天的第几个商品,比如2016053000001
	- 商品保存成功时生成sn

```
$goodsSn = GoodsDayCount::findOne([ 'day' => date('Y-m-d') ]);
//					var_dump($goodsSn);exit;
				if (!$goodsSn) {
					$goodsSn = new GoodsDayCount();
					$goodsSn->count = 1;
				} else {
					$goodsSn->count += 1;
				}
				$goodsSn->day = date('Ymd');
				$model->sn = date('Ymd') . str_pad($goodsSn->count, '5', '0', STR_PAD_LEFT);
				
```

+ 商品详情使用ueditor文本编辑器 
	- 安装插件，同上

```
视图添加：
echo $goodsform->field($contents, 'content')->widget('kucha\ueditor\UEditor', []);
```

+ 记录每天创建商品数
	- 使用函数time()实现

### DAY5 ###

----------

>需求 

1. 管理员增删改查 
	- 简单的增删改查
	- 密码添加使用generatePasswordHash
	- 验证密码使用validatePassword
2. 管理员登录和注销 
	- 调用user实现

```
public function actionLogin()
		{
			$model = new Auth();
			if ($model->load(\Yii::$app->request->post())) {
				if ($model->validate()) {
					\Yii::$app->session->setFlash('success', '登录成功');
					return $this->redirect([ 'admin/index' ]);
				}
			}
			return $this->render('login', [ 'model' => $model ]);
		}

public function actionLogout()
		{
			\Yii::$app->user->logout();
			\Yii::$app->session->setFlash('success', '注销成功');
			return $this->redirect([ 'admin/login' ]);
		}

```

+ 基于cookie的自动登录 
	- 添加保存一个	`$this->auth_key = \Yii::$app->security->generateRandomString(32);`
	- 定义一个public $rememberMe;和实现接口IdentityInterface

```
 public function getAuthKey()
		{
			return $this->auth_key;
		}
public function validateAuthKey($authKey)
		{
			return $this->getAuthKey() === $authKey;
		}
```

+ 管理员登陆后需要保存最后登录时间和最后登陆ip
	- 在配置里添加事件或者登陆成功时保存数据

```
'on beforeLogin' => function ($event) {
					$admin = $event->identity;
					$admin->updated_time = time();
					$admin->updated_ip = \Yii::$app->request->userIP;
					$admin->save(false);
				},
```

### DAY7 ###

----------

####需求

+ 使用RBAC管理用户和权限
	- 难点：
	- 角色关联用户


```
//清除之前数据ID
$authManger=\Yii::$app->authManager;
					$authManger->revokeAll($id);

			//重新赋值
			$authManger->getRoles();
			foreach ($this->permissions as $permission){
				$role=$authManger->getRole($permission);
				$authManger->assign($role,$id);
			}
			$this->permissions=$authManger->getRolesByUser($id);
			return true;
```

> 权限增删改查 

```
//创建 create 和 view 权限
        $createPermission = $authManager->createPermission('day4/create');
        $viewPermission = $authManager->createPermission('day4/view');
        $authManager->add($createPermission);//将权限添加到数据表
        $authManager->add($viewPermission);//将权限添加到数据表
        //管理员角色关联create 和 view     普通会员 关联view权限
```

> 角色增删改查 

```
//创建角色 修改角色  删除角色
        $adminRole = $authManager->getRole('admin');//获取已存在的角色
        if($adminRole==null){
            $adminRole = $authManager->createRole('admin');
            $authManager->add($adminRole);
        }
        $adminRole->name = '管理员';
        $authManager->update('admin',$adminRole);//更新角色

        $authManager->remove($adminRole);//删除角色
 //创建管理员角色  创建普通会员角色
        $adminRole = $authManager->createRole('admin');
        $memberRole = $authManager->createRole('member');
        $authManager->add($adminRole);//将角色添加到数据表
        $authManager->add($memberRole);//将角色添加到数据表
 //admin用户关联管理员角色  zhangsan用户关联普通会员角色
        $authManager->assign($adminRole,1);
        $authManager->assign($memberRole,2);
```

> 角色和权限关联 

```
 //管理员角色关联create 和 view     普通会员 关联view权限
        $authManager->addChild($adminRole,$createPermission);//角色 权限
        $authManager->addChild($adminRole,$viewPermission);
        $authManager->addChild($memberRole,$viewPermission);
        echo '设置完成';
```

- 用户和角色关联 

```
 //admin用户关联管理员角色  zhangsan用户关联普通会员角色
        $authManager->assign($adminRole,1);
        $authManager->assign($memberRole,2);
```

### DAY8 ###

----------

####需求

1. 菜单增删改查 
	- 注意事项
	- 要显示父类名称
	- 显示权限路由
2. 菜单和权限关联 
	- 判断是否有权限，有权限显示无权限不显示
3. 根据权限显示菜单
	- 判断这个用户是否有这个权限，有就显示没有就不显示

```
if (\Yii::$app->user->isGuest) {
				$menuItems[] = [ 'label' => '登录', 'url' =>\Yii::$app->user->loginUrl ];
				
			} else {
				
				$menus=\backend\models\Menu::find()->where(['parent_id'=>0])->orderBy('sort')->all();
				foreach ($menus as $menu){
					$item=['label'=>$menu->label,'items'=>[]];
					foreach ($menu->children as $child){
						if(\Yii::$app->user->can($child->url)){
							$item['items'][]=['label'=>$child->label,'url'=>[$child->url]];
							
						};
					}
					if(!empty($item['items'])){
						$menuItems[]=$item;
					}
				}
				$menuItems[] = [ 'label' => '修改密码', 'url' =>['admin/passwd'] ];
				$menuItems[] = [ 'label' => '注销(' . \Yii::$app->user->identity->username . ')', 'url' =>['admin/logout'] ];
			}
```

---

>####以下是前台模块

### Day9 

#### 用户模块

> 用户注册

1. 用户注册
1. 用户登录（允许自动登录，登录成功记录最后登录时间和ip）
	+ 和后台一样，实现接口IdentityInterface
	+ 登陆成功在beforesave或config/main里面配置事件on beforeLogin
1. 收货地址管理
	+ 难点在于三级联动(我用三种方式分别实现了)
		* 第一种是建一个省市县的数据表，level 字段123分别代表3级，parent_id为0代表省，其余parent_id代表上级的关联数据，然后通过ajax读取数据，并将省市县的id分别保存到address表中。缺点是每次都需要读取数据库，性能不是很好。
		* 第二种是新建并引入一个保存所有省市县数据的js文件，用原生js代码来实现数据的交互，优点是速度快灵活，缺点是不是很好修改数据，数据表保存省市县的时候保存的是字符串，如果海量数据将会影响数据库性能，不过到时候可以采用分表分区解决这个问题，所以最后我还是选用了这种方法。

> 用到的插件

+ 阿里大于
	* 用来自动发送手机短信
	* 避免短信被刷：
		- 一个手机号每天只允许收到3条短信
		- 短信间隔时间一分钟或更久，阿里大于已经做了
		- 先通过随机验证码方可发送短信
	* 网络不好解决方案：
		- 短时间内第二条短信验证码不变，可设为5分钟以内
		- 每条短信内容加入一个编号，让用户根据编号输入验证
+ 发送邮件

````php
    public function actionMail()
    {
        //通过邮箱重设密码
        $result = \Yii::$app->mailer->compose()
            ->setFrom('18780228768@163.com')//谁的邮箱发出的邮件
            ->setTo('18780228768@163.com')//发给谁
            ->setSubject('六月感恩季，七牛献豪礼')//邮件的主题
            //->setTextBody('Plain text content')//邮件的内容text格式
            ->setHtmlBody('<b style="color: red">创办于2011年的七牛云，一直聚焦以数据为核心的云服务市场，为企业提供数据存储、CDN加速、富媒体处理、直播云等服务。成立六年来，在绝大多数人的印象中，七牛云这个名词是和技术与服务紧紧相连的。在云服务这个竞争激烈、变化巨大的领域中，七牛云已经成长为国内领先的企业级云服务商，其中为客户称道的无疑是前瞻的技术与用心的服务。<a href="http://www.xxx.com/user/resetpass?token=sdfhkjshdfjsdf1243">点此重设密码</a></b>')//邮件的内容 html格式
            ->send();
        var_dump($result);
    }
````

### Day10

---

- 首页
	+ 将前台页面先显示出来
	+ 将头部顶栏封装到widget
	+ 将尾部版权封装到widget
	+ 将尾部帮助列表封装到widget
	+ 页面都通过widget调用
- 展示商品分类列表
	+ 将商品导航列表封装到widget中
	+ 三次遍历输出3层分类展示到页面
- 展示商品列表
	+ 通过嵌套集合插件的方法`children`找到选中分类的所有子分类并将id遍历出来和选中分类的id合并为数组
	+ 根据id的数组找到所有`good_category_id`等于这个数组的商品
	+ 面包屑的显示
		* 通过嵌套集合插件的方法`parents` 找到选中分类的所有上级分类id并和选中分类id合并为数组
		* 然后循环遍历显示即可
- 商品详情
- 展示商品详情

````php
//商品列表页
    public function actionList($id)
    {
        $ids[] = $id;
        $ids2[] = $id;
        $goods_category = GoodsCategory::findOne(['id' => $id]);
        $children = $goods_category->children()->all();
        //找出所有下级分类的id
        foreach ($children as $child) {
            $ids[] = $child->id;
        }
        $models = Goods::findAll(['goods_category_id' => $ids]);
        //找到当前分类的所有上级分类
        $parents = $goods_category->parents()->all();
        foreach ($parents as $parent) {
            $ids2[] = $parent->id;
        }
        $categories = GoodsCategory::findAll(['id' => $ids2]);
        return $this->render('list', ['models' => $models, 'categories' => $categories]);
    }

    //商品详情页
    public function actionGoods($id)
    {
        $model = Goods::findOne(['id' => $id]);
        $goods_category = GoodsCategory::findOne(['id' => $model->goods_category_id]);
        $ids[] = $model->goods_category_id;
        $parents = $goods_category->parents()->all();
        foreach ($parents as $parent) {
            $ids[] = $parent->id;
        }
        $categories = GoodsCategory::findAll(['id' => $ids]);
        return $this->render('goods', ['model' => $model, 'categories' => $categories]);
    }
````

### Day11

---

- 购物车设计
- 参照主流的购物车设计方案(京东),完成购物车的数据存放方式的设计
- 如果没有登录就存放在cookie中
- 如果已经登录,就存放在数据表中
- 当用户登录的时候,将cookie中的数据自动同步到数据表中
- 如果已经有了这个商品,就使用cookie中的数量
- 如果没有这个商品,就添加这个商品到数据表

- 难点在于控制器的流程
	- 如果未登陆将数据保存到cookie
	- 如果用户已经登陆
		+ 如果cookie中有数据，将数据和用户登陆后的数据合并保存到数据表，然后清除cookie
		+ 如果cookie中没用数据，直接保存到数据表
	- 读取数据的时候
		+ 如果未登陆，直接读取cookie中的数据
		+ 如果已登录，读取数据表中的数据

>直接上控制器中的代码以作参考:

````php
  //加入购物车
    public function actionAddCart()
    {
        //改变数据
        self::changeCart();
        //将当前地址get传过去
        $name = \Yii::$app->request->post('name');
        $name = urlencode($name);
        //跳转到购物车页面
        return $this->redirect(['index/cart?name=' . $name]);
    }

    //更新购物车
    public function actionUpdateCart()
    {
        $this->update = true;
        self::changeCart();
    }

    //购物车页面
    public function actionCart()
    {
        if (\Yii::$app->user->isGuest) {
            //取出cookie中的商品id和数量
            $cookies = \Yii::$app->request->cookies;
            $cookie = $cookies->get('cart');
            if ($cookie == null) {
                $cart = [];
            } else {
                $cart = unserialize($cookie->value);
            }
            $models = [];
            foreach ($cart as $goods_id => $amount) {
                $goods = Goods::findOne(['id' => $goods_id])->attributes;
                $goods['amount'] = $amount;
                $models[] = $goods;
            }
        } else {
            //先将缓存同步到数据库
            self::cookie2data();
            $models = self::getCarts();
        }
        $name = \Yii::$app->request->get('name');
        return $this->render('cart', ['models' => $models, 'name' => $name]);
    }

    //购物车回退
    public function actionBack2Goods()
    {
        $name = \Yii::$app->request->get('name');
        //跳到上一个页面
        if ($name) {
            return $this->redirect($name);
        }
        return $this->redirect(\Yii::$app->request->getReferrer());
    }

    //将购物车cookie同步到数据表
    private function cookie2data()
    {
        //获取cookie中的购物车数据
        $cookies = \Yii::$app->request->cookies;
        $cookie = $cookies->get('cart');
        $member_id = \Yii::$app->user->getId();
        //将所有的缓存读出来，分别判断数据表是否有值，有则合并到数据表，没有则新增到数据表
        if ($cookie) {
            $cart = unserialize($cookie->value);
            foreach ($cart as $goods_id => $amount) {
                $old_cart = Cart::findOne(['member_id' => $member_id, 'goods_id' => $goods_id]);
                if ($old_cart != null) {
                    $old_cart->amount += $amount;
                    $old_cart->save();
                } else {
                    $model = new Cart();
                    $model->setAttributes(['member_id' => $member_id, 'goods_id' => $goods_id, 'amount' => $amount]);
                    $model->save();
                }
            }
        }
        //清空cookie
        $cookie = new Cookie([
            'name' => 'cart',
            'value' => '',
            'expire' => time() - 1,
        ]);
        \Yii::$app->response->cookies->add($cookie);
    }

    //更新购物车的方法
    private function changeCart()
    {
        //接收数据
        $goods_id = \Yii::$app->request->post('goods_id');
        $amount = \Yii::$app->request->post('amount');
        $goods = Goods::findOne(['id' => $goods_id]);
        //如果商品不存在，提示错误信息
        if ($goods == null) {
            throw new NotFoundHttpException('商品不存在');
        }
        //如果未登陆 保存到cookie
        if (\Yii::$app->user->isGuest) {
            //获取cookie中的购物车数据
            $cookies = \Yii::$app->request->cookies;
            $cookie = $cookies->get('cart');
            //如果cookie中没用数据，定义为空数组
            if ($cookie == null) {
                $cart = [];
            } else {
                $cart = unserialize($cookie->value);
            }
            //将商品id和数量保存到cookie
            $cookies = \Yii::$app->response->cookies;
            //更新
            if ($this->update) {
                //如果不是0这直接改为这个数，如果小于0这删掉这一条
                if ($amount > 0) {
                    $cart[$goods_id] = $amount;
                } else {
                    if (key_exists($goods['id'], $cart)) unset($cart[$goods_id]);
                }
                //修改
            } else {
                //检查购物车中是否有该商品，有则累加
                if (key_exists($goods->id, $cart)) {
                    $cart[$goods_id] += $amount;
                } else {
                    $cart[$goods_id] = $amount;
                }
            }
            //保存到cookie
            $cookie = new Cookie([
                'name' => 'cart',
                'value' => serialize($cart),
                'expire' => time() + 24 * 3600,
            ]);
            $cookies->add($cookie);
            //如果登陆 保存到数据表
        } else {
            //登陆先查cookie里是否有商品
            //没有直接保存到数据库
            $member_id = \Yii::$app->user->getId();
            //判断数据库是否已经有这个商品和对应的memberid数据
            $old_cart = Cart::findOne(['member_id' => $member_id, 'goods_id' => $goods_id]);
            //如果是更新直接修改，如果amount 小于 0 直接删除
            if ($this->update) {
                if ($old_cart == null) {
                    throw new NotFoundHttpException('你的购物车还没有该商品');
                }
                if ($amount > 0) {
                    $old_cart->amount = $amount;
                    $old_cart->save();
                } else {
                    $old_cart->delete();
                }
            } else {
                //如果有则加上之前的数据
                if ($old_cart != null) {
                    $old_cart->amount += $amount;
                    $old_cart->save();
                } else {
                    //其他情况直接保存这条数据
                    $model = new Cart();
                    $model->setAttributes(['member_id' => $member_id, 'goods_id' => $goods_id, 'amount' => $amount]);
                    $model->save();
                }
            }
            self::cookie2data();
        }
    }

    //找到用户所有购物车的方法
    private function getCarts()
    {
        //定义一个空数组接收数据
        $models = [];
        //找到该用户所有的商品
        $member_id = \Yii::$app->user->getId();
        $carts = Cart::find()->where(['member_id' => $member_id])->andWhere(['>', 'amount', 0])->all();
        //将所有的购物车数据遍历出来并保存到数组里
        foreach ($carts as $cart) {
            $goods = Goods::findOne(['id' => $cart->goods_id])->attributes;
            $goods['amount'] = $cart->amount;
            $models[] = $goods;
        }
        return $models;
    }
````

### Day12 订单

- 在提交订单的时候:
- 判断库存是否足够
- 如果不够回滚
- 如果足够执行后续流程
- 执行成功清除购物车
- 晚上7组讲解模块内容

>控制器中代码如下，主要用的技术是数据库的回滚，但是必须数据表的类型要支持事务，如innodb

````php
 //将购物车加入订单
    public function actionOrder()
    {
        //未登录跳到登陆页面
        if (\Yii::$app->user->isGuest) {
            return $this->redirect(['user/login', 'name' => \Yii::$app->request->getUrl()]);
        } else {
            $member_id = \Yii::$app->user->getId();
            //找到所有收货地址
            $addresses = Address::find()->where(['member_id' => $member_id])->asArray()->all();
            //找到所有购物车
            $carts = self::getCarts();
            //如果没有购物车跳转到首页
            if ($carts == null) {
                return $this->goHome();
            }
            return $this->render('order', ['addresses' => $addresses, 'carts' => $carts]);
        }
    }

    //订单提交
    public function actionPayment()
    {
        if (\Yii::$app->request->isPost) {
            $address_id = \Yii::$app->request->post('address_id');
            $payment_id = \Yii::$app->request->post('payment_id');
            $delivery_id = \Yii::$app->request->post('delivery_id');
            $price = \Yii::$app->request->post('price');
            $member_id = \Yii::$app->user->getId();
            //找到所有地址
            $address = Address::findOne(['id' => $address_id, 'member_id' => $member_id]);
            if ($address == null) {
                throw new NotFoundHttpException('地址不存在,请添加地址');
            }
            $order = new Order();
            //字段赋值
            $order->member_id = $member_id;
            $order->name = $address->name;
            $order->province = $address->province;
            $order->city = $address->city;
            $order->area = $address->area;
            $order->address = $address->address;
            $order->tel = $address->tel;
            $order->delivery_id = $delivery_id;
            $order->delivery_name = Order::$delivery_style[$delivery_id]['name'];
            $order->delivery_price = Order::$delivery_style[$delivery_id]['price'];
            $order->payment_id = $payment_id;
            $order->payment_name = Order::$payment_style[$payment_id]['name'];
            $order->total = $price;
            //开启事务
            $transaction = \Yii::$app->db->beginTransaction();
            try {
                $order->save();
                //订单商品详情表
                $carts = Cart::findAll(['member_id' => $member_id]);
                foreach ($carts as $cart) {
                    $goods = Goods::findOne(['id' => $cart->goods_id, 'status' => 1]);
                    //商品没有了，抛出异常
                    if ($goods == null) {
                        throw new Exception($goods->name . '--已售完');
                    }
                    //库存不足抛出异常
                    if ($goods->stock < $cart->amount) {
                        throw new Exception($goods->name . '--库存不足');
                    }
                    //构造生成订单详情表
                    $order_goods = new OrderGoods();
                    $order_goods->member_id = $member_id;
                    $order_goods->order_id = $order->id;
                    $order_goods->goods_id = $cart->goods_id;
                    $order_goods->goods_name = $goods->name;
                    $order_goods->logo = $goods->logo;
                    $order_goods->price = $goods->shop_price;
                    $order_goods->amount = $cart->amount;
                    $order_goods->total = $order_goods->amount * $order_goods->price;
                    //保存订单详情表
                    $order_goods->save();
                    $goods->stock -= $cart->amount;
                    $goods->save();
                }
                //清空购物车
                Cart::deleteAll(['member_id' => $member_id]);
                //一切正常，事务提交
                $transaction->commit();
            } catch (Exception $e) {
                //回滚
                $transaction->rollBack();
            }
            return $order->id;
        }
        return null;
    }

    //提交成功页面
    public function actionOrderSave()
    {
        return $this->render('order-success');
    }

    //订单状态页面
    public function actionOrderStatus()
    {
        //显示该用户的所有订单
        $member_id = \Yii::$app->user->getId();
        $orders = OrderGoods::findAll(['member_id' => $member_id]);
        return $this->render('order-status', ['orders' => $orders]);
    }
````






> 更多功能和bug修复我会及时更新~