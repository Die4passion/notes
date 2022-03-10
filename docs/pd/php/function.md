
> For every smile shall wither, the hopeful laughter fade, the cup of joys illusions bashed from the craving lips. And as all hopes are shattered, the last of passions scattered, the gale of bitter failure is all that shall remain.

!> 比较好的方法

```php
/** *利用google api生成二维码图片
 * $content：二维码内容参数
 * $size：生成二维码的尺寸，宽度和高度的值
 * $lev：可选参数，纠错等级
 * $margin：生成的二维码离边框的距离
 */
public static function create_erweima($content, $size = '200', $lev = 'L', $margin= '0')
{
    $content = urlencode($content);
    $image = '<img src="http://chart.apis.google.com/chart?chs='.$size.'x'.$size.'&cht=qr&chld='.$lev.'|'.$margin.'&chl='.$content.'"  widht="'.$size.'" height="'.$size.'" />';
    return $image;
}



/**
 * Make "year/month/day" directories
 *
 * @return bool|string
 */
public static function makeYmdDirs()
{
    $ymd = date('Y-m-d');
    list($year, $month, $day) = explode('-', $ymd);
    $imagePath = Yii::getAlias('@webroot') . '/' . UploadForm::DIR_PUBLIC;
    $ymdDirs = $year . '/' . $month . '/' . $day;
    $directory = $imagePath . '/' . $ymdDirs;

    if (!file_exists($directory)) {
        if (!mkdir($directory, 0777, true)) {
            return false;
        }
    }

    return $ymdDirs;
}



 return preg_match('/^1[34578]{1}\d{9}$/', $mobile);


/**
 * Check if list composed of int numbers
 *
 * @param array $elements elments to check
 * @return bool
 */
public static function checkIntList(array $elements)
{
    if (!$elements) {
        return false;
    }

    foreach ($elements as $element) {
        if (!is_numeric($element)) {
            return false;
        }
    }

    return true;
}




/**
 * Check if two lists have same elements
 *
 * @param array $arr1 one array to check
 * @param array $arr2 the other array check
 * @return bool
 */
public static function checkArrayIdentity(array $arr1, array $arr2)
{
    return empty(array_diff($arr1, $arr2)) && empty(array_diff($arr2, $arr1));
}





/**
 * Get user agent
 *
 * @return string
 */
public static function userAgent()
{
    $type = 'other';

    $userAgent = Yii::$app->request->headers->get('user-agent');
    if (stripos($userAgent, 'iPhone') !== false
        || stripos($userAgent, 'iPad') !== false
    ) {
        $type = 'ios';
    } elseif (stripos($userAgent, 'Android')) {
        $type = 'android';
    }

    return $type;
}




/**
 * Generate qr code image
 *
 * @param string $str string
 * @param string $filename filename to saved as
 */
public static function generateQrCodeImage($str, $filename)
{
    $dir = Yii::getAlias('@webroot') . '/' . UploadForm::DIR_PUBLIC . '/';
    QrCode::png($str, $dir . "{$filename}.png");
}




/**get  client ip
 * @return array|false|string
 */
public static function getClientIP()
{
    global $ip;
    if (getenv("HTTP_CLIENT_IP"))
        $ip = getenv("HTTP_CLIENT_IP");
    else if (getenv("HTTP_X_FORWARDED_FOR"))
        $ip = getenv("HTTP_X_FORWARDED_FOR");
    else if (getenv("REMOTE_ADDR"))
        $ip = getenv("REMOTE_ADDR");
    else $ip = "Unknow";
    return $ip;
}



/**
 * Format price
 *
 * @param int|float $price price
 * @param int $decimalDigits decimal digits default 2
 * @return string
 */
public static function formatPrice($price, $decimalDigits = 2)
{
    return sprintf('%.' . $decimalDigits . 'f', $price);
}


/**
 * 自定义过滤 yii http请求数据
 * @param  string $value   [请求数据变量名]
 * @param  string $type    [请求方式: 现在只写了get post, 默认post]
 * @param  string $default [过滤失败默认值, 默认'']
 * @return [type]          [返回过滤值或默认值]
 */
function filterResponse(string $value, $type = 'post', $default = '')
{
    $request = \Yii::$app->request;
    if ($type == 'get'
        && $request->get($value) 
        && trim($request->get($value)) != ''
    ) {
        return trim($request->get($value));
    } 

    if ($type == 'post'
        && $request->post($value) 
        && trim($request->post($value)) != ''
    ) {
        return trim($request->post($value));
    } 

    return $default;
}

/**
 * 将字符串分割为数组     包括中文
 * @param  string $str 
 * @return array      
 */
function str_split_utf8($str)  
{  
    $split = 1;  
    $array = array();  
    for ($i = 0; $i < strlen($str);) {  
        $value = ord($str[$i]);  
        if ($value > 127) {  
            if ($value >= 192 && $value <= 223) {  
                $split = 2;  
            } elseif ($value >= 224 && $value <= 239) {  
                $split = 3;  
            } elseif ($value >= 240 && $value <= 247) {  
                $split = 4;  
            }  
        } else {  
            $split = 1;  
        }  
        $key = null;  
        for ($j = 0; $j < $split; $j++, $i++) {  
            $key .= $str[$i];  
        }  
        array_push($array, $key);  
    }  
    return $array;  
}  

/**
 * 循环删除文件夹及下面所有文件
 * @param  {[type]} $dirName [文件夹绝对路径]
 * @return {[bool]}          [成功/失败]
 */
public static function deleteFiles($dirName)
{
    if (is_dir($dirName) && rmdir($dirName) == false) {

        //所有二级目录
        $files = scandir($dirName);

        foreach ($files as $key => $value) {
            //所有二级目录的路径
            $path = realpath($dirName . DIRECTORY_SEPARATOR . $value);

            //循环删除
            if (!is_dir($path)) {
                self::deleteFiles($path);
            } elseif ($value != "." && $value != "..") {
                unlink($path);
            }
        }
        return true;
    }

    return false;
}


/**
 * 检查密码复杂度
 */
public function checkPassword($pwd) 
{
    $pwd = trim($pwd);
    if (!strlen($pwd) >= 6) {//密码小于6个字符
        return 1;
    }
    if (preg_match("/^[0-9]+$/", $pwd)) { //密码全是数字
        return 1;
    }
    if (preg_match("/^[a-zA-Z]+$/", $pwd)) { //密码全是字母
        return 1;
    }
    if (preg_match("/^[0-9A-Z]+$/", $pwd)) {  //密码包含数字，字母大写
        return 2;
    }
    if (preg_match("/^[0-9a-z]+$/", $pwd)) {  //密码包含数字，字母小写
        return 2;
    }
    return 3;
}



public static function startEndDate($timeType, $returnTimestamp = false)
{
    $startTime = $endTime = '';

    $yearMonthDay = date('Y-m-d');
    list($year, $month, $day) = explode('-', $yearMonthDay);
    switch ($timeType) {
        case 'today':
            $startTime = date("Y-m-d H:i:s", mktime(0, 0, 0, $month, $day, $year));
            $endTime = date("Y-m-d H:i:s", mktime(23, 59, 59, $month, $day, $year));
            break;
        case 'week':
            $startTime = date("Y-m-d H:i:s", mktime(0, 0, 0, $month, $day - date('w') + 1, $year));
            $endTime = date("Y-m-d H:i:s", mktime(23, 59, 59, $month, $day - date('w') + 7, $year));
            break;
        case 'month':
            $startTime = date("Y-m-d H:i:s", mktime(0, 0, 0, $month, 1, $year));
            $endTime = date("Y-m-d H:i:s", mktime(23, 59, 59, $month, date('t'), $year));
            break;
        case 'year':
            $startTime = date("Y-m-d H:i:s", mktime(0, 0, 0, 1, 1, $year));
            $endTime = date("Y-m-d H:i:s", mktime(23, 59, 59, 12, 31, $year));
            break;
    }

    if ($returnTimestamp) {
        $startTime = (int)strtotime($startTime);
        $endTime = (int)strtotime($endTime);
    }
    return [$startTime, $endTime];
}



//二维数组去重
function assoc_unique($arr) {
    foreach ($arr as $k => $v) {
        if (!is_array($v)) {
            return false;
        }
        $v_t = implode(',', $v);
        $temp[] = $v_t;
    }
    $temp = array_unique($temp);
    foreach ($temp as $k => $v) {
        $temp[$k] = explode(',', $v);
    }
    return $temp;
}
```

!> 表情包接口

```bash
curl -X GET \
  'https://pic.sogou.com/pics/json.jsp?query=%E8%8B%8F%E5%A4%A7%E5%BC%BA%20%E8%A1%A8%E6%83%85&st=5&start=0&xml_len=24&reqFrom=wap_result' \
  -H 'Cookie: IPLOC=CN5101; SUV=00E65321ABD471BF5C8F41BF67D32411; JSESSIONID=aaaW4EHCX1rUV6QIMFMNw; ABTEST=1|1554350361|v1' \
  -H 'Postman-Token: a981f141-4b3d-4ff1-b15b-c6b194905adb' \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F 'query=苏大强 表情' \
  -F st=5 \
  -F start=0 \
  -F xml_len=24 \
  -F reqFrom=wap_result 
```
