---
layout: post
title: 微信公众号支付的php笔记
date: 2018-01-01
tag: 工作笔记
---

#### 缘由,坑爹的官方文档

之前一直觉得微信没什么意思,就一直没怎么去看,只会用easywechat现成的去做. 然而公司这个项目我想用easywechat确遇到了各种拓展的兼容性问题,因为项目的composer拓展和yii版本没有同步控制.故而在手动配置了很多扩展之后还是决定用微信官方的方法写一写,看上去也不难.结果写完才发现真的是巨坑无比.官方文档没有任何一个地方是清晰明了的,需要自己一步一个错解决之后才能慢慢做出来,里面的坑实在是太多.    故记下此篇,方面参考:


#### 小tips:

1. 可以将测试域名映射到本地,先在本地测试获取openid和prepay_id等,其他功能再去服务器测试,以加快开发效率.
2. 需在发起支付请求方法之前页面获得openid,支付同时获取openid我暂时还不知如何处理
3. 统一下单接口中参数`total_fee`单位为 分!
4. 注意`timestamp timeStamp noncestr nonceStr` 两种写法混用的
5. 所有获取签名或key均为参数按照字典顺序排序拼接说得字符串, php可先将所有参数存入数组`$data`中,用`sha1(urldecode(http_build_query($data)))`获得 

#### 开发步骤:

### 获取openid

1. 申请公众号
	- 此处技术主管的号,略

2. 微信公众号里相关配置
	- wx公众平台->登录->找到->网页授权->修改回调域名为`www.xxxx.com`
	- 我也记不清具体位置( ╯□╰ ),反正找到appid,secret,mch_id,写到你框架的配置文件,最好是构造方法检测必设置这些值
	- 必须设置商户支付密钥,这个在微信商户平台设置
	- 同上位置,设置支付的确定目录
	- 必须下载证书`MP_verify_xxxxxxxxxx.txt`到你项目入口文件同级目录
	- 设置js接口安全域名(最多3个)
	- 下载证书到项目目录,设置证书路径

```php
const SSLCERT_PATH = '../cert/apiclient_cert.pem';
const SSLKEY_PATH = '../cert/apiclient_key.pem';
```

3. 获取openid
	- 调用oauth 2.0授权获取code
	- 根据返回code获取openid
		+ 注意支付目录必须设置为微信开放平台设置的最后一个/的同级目录
			* `eg: 如果你需要调用支付的地址为www.xxxx.com/buy-xxxx.html 那么你设置的地址为 www.xxxx.com/buy/`
			* `如果为 www.xxxx.com/buy/goods/xxx.html 那么设为www.xxxx.com/buy/goods/`
	- 得到openid 必须保存, 目前我用的有两种方式,一种是入库,一种是存入session
		* 如果是后续还有公众号相关开发,建议入库
		* 如果仅仅是支付功能,可考虑存入session,过期时间目前我用的框架默认

4. 因为我只做支付功能,所以用的snsapi_base参数,不需要用户确认. 目前业务逻辑为,用户进入购买页面,如果判断用户为登录状态,则获取openid和本页完整url分别存入session. 这都是用户感受不到的.

5. 获取openid之后,工作基本做了一半,下面的就是根据用户openid和公众号相关信息以及订单信息进行支付. 订单信息可自去看微信文档,必填参数写入即可. 

### jsspi支付

1. 根据官方文档,和二维码支付网页跳转支付其他参数基本一样,我们只须填入上一步中得到用户对应此公众号的openid即可,另外如果之前使用其他账号支付,那么此处要单独配置appid和secret为此公众号的
2. 参照官方文档传入需要的参数调起公共支付接口
3. 返回得值再经过处理得到js支付需要的参数,prepay_id等
4. 根据官方demo改写获取js支付需要的参数,代码如下

```php
    /**
     *
     * 获取jsapi支付的参数
     * @param array $UnifiedOrderResult 统一支付接口返回的数据
     * @throws WxPayException
     *
     * @return string json数据，可直接填入js函数作为参数
     */
    public function GetJsApiParameters($UnifiedOrderResult)
    {
        if (!array_key_exists("appid", $UnifiedOrderResult)
            || !array_key_exists("prepay_id", $UnifiedOrderResult)
            || $UnifiedOrderResult['prepay_id'] == "") {
            throw new WxPayException("参数错误");
        }
        $jsapi = new WxPayJsApiPay();
        $jsapi->SetAppid($UnifiedOrderResult["appid"]);
        $timeStamp = time();
        $jsapi->SetTimeStamp("$timeStamp");
        $jsapi->SetNonceStr(WxPayApi::getNonceStr());
        $jsapi->SetPackage("prepay_id=" . $UnifiedOrderResult['prepay_id']);
        $jsapi->SetSignType("MD5");
        $jsapi->SetPaySign($jsapi->MakeSign());
        $parameters = $jsapi->GetValues();
        return $parameters;
    }
    /**
     * 官方demo有几处不明:
     * 1. 以上方法中WxPayJsApiPay,官方demo中并没有找到,我在`https://github.com/anruence/yii2-tech/blob/master/pay/wxsdk/WxPayJsApiPay.php`此页面找到的
     * 2. 此处nonceStr为32位(和下面js配置中的16位noncestr没有关系)
     * 3. 此方法关键为得到prepay_id, 待会需要prepay_id发起支付, 此处返回parameter格式中所有参数为发起支付时对应参数,如不需传入其他参数,js页面可直接将返回json值作为`WeixinJSBridge.invoke` 方法 `getBrandWCPayRequest`之后的参数,第二个参数的值.
     */
```

##### 页面js发起请求调起微信内支付

- 需要支付的页面引入js `http://res.wx.qq.com/open/js/jweixin-1.2.0.js`
- 页面js代码参考如下

```javascript
//js通过prepay调起支付
function callPay(data){

    WeixinJSBridge.invoke(
        'getBrandWCPayRequest', {
            "appId" : data.appId,
            "timeStamp" : data.timeStamp,
            "nonceStr" : data.nonceStr,
            "package" : data.package,
            "signType" : data.signType,
            "paySign" : data.paySign
        },
        function(res){
            if(res.err_msg == "get_brand_wcpay_request:ok" ) {
                alert("支付成功!");
                setTimeout(function(){
                    window.location.href = data.callback;
                },300)
            } else if (res.err_msg == "get_brand_wcpay_request:cancel") {
                alert("取消支付成功!");
            } else {
                alert("支付失败!请稍候重试");
            }
        }
    );
}
/**
 * 需要注意的几点:
 * 1. 微信开发者工具不能发起支付
 * 2. 注意package参数格式为prepay_id=xxxxxxx
 * 3. 注意此处的paySign为 以上参数字典排序生成的字符串+sha1算法生成
 */
```

##### 微信文档中完全没有提到的

- 必须在js中写入一个`wx.config`,形如

```javascript
wx.config({
    appId: data.appId,
    timestamp: data.timeStamp,
    nonceStr: data.noncestr,
    signature: data.signature,
    jsApiList: [
        "chooseWXPay"
    ]
});
/**
 * 注意:
 * 1. appId,timestamp,和发起支付的参数必须一样
 * 2. nonceStr 此处为16位随机字符串,和发起支付的32位区分开
 * 3. signature签名请看下面的笔记,又是一个烦人的东西...
 * 4. jsApiList 参考微信官方文档,做完整公众号开发,用到的都必须写到这里
 */
```

##### JS-SDK使用权限签名signature算法(即上面config中的)

- 通过`appid`和`secret`获取`access_token`
- `https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET`
- 生成`jsapi_ticket`
- 用第一步拿到的`access_token`采用http GET方式请求获得`jsapi_ticket`
- `https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=ACCESS_TOKEN&type=jsapi`
- 需要注意access_token 和jsapi_ticket有效期均为7200s，jsapi_ticket每天限制2000次请求，所以服务端必须进行缓存.
- 生成signature 签名需要的字段如下:

```php
$data = [
    'jsapi_ticket' => $this->getJsApiTicket(),
    'noncestr' => Yii::$app->getSecurity()->generateRandomString(16),
    'timestamp' => (int)YII_BEGIN_TIME,
    'url' => explode('#', Yii::$app->getRequest()->getAbsoluteUrl())[0]
    ];
```

##### 调用支付的另一种js写法(应该结果是一样的,没仔细测)

```javascript
wx.chooseWXPay({
    // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
    timestamp: 0, 
    nonceStr: '', // 支付签名随机串，不长于 32 位
    package: '', // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=***）
    signType: '', // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
    paySign: '', // 支付签名
    success: function (res) {
        //res返回信息只有errMsg并没有err_msg，都是自己开调试模式，log出来的！都是泪
        // 支付成功后的回调函数
    }
    cancel: function (res) {
        // 支付取消的回调函数
    }
    error: function (res) {
        // 支付失败的回调函数
    }
});
```

### 开发中可能用到的调试工具

- [微信公众平台接口调试工具](https://mp.weixin.qq.com/debug/)
- [微信 JS 接口签名校验工具](https://link.jianshu.com/?t=https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=jsapisign)

*ps:有任何错误之处还请大家指出*

