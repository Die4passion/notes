##有用的方法总结

>Model方法in YII2

````php

<?php
/**
 * Created by PhpStorm.
 * User: hj
 * Date: 4/27/17
 * Time: 9:34 AM
 */

namespace app\services;

use yii\db\ActiveRecord;
use yii\db\Query;

class ModelService
{
    const SEPARATOR_SORT = ':';
    const SORT_DIRECTIONS = [
        SORT_DESC => 'DESC',
        SORT_ASC => 'ASC',
    ];
    const SEPARATOR_ERRCODE_ERRMSG = ':';
    const PAGE_SIZE_DEFAULT = 12;
    const ORDER_BY_DEFAULT = ['id' => SORT_DESC];
    const SUFFIX_FIELD_DESCRIPTION = '_desc';
    const FORMAT_DATA_METHOD = 'formatData';
    const EXTRA_DATA_METHOD = 'extraData';
    const PAGINATION_RETURN_ARRAY_KEY_TOTAL = 'total';
    const PAGINATION_RETURN_ARRAY_KEY_DETAILS = 'details';
    const FIELD_IDENTITY = 'id';

    /**
     * Generate sorting statements for query
     *
     * @param  ActiveRecord $model active record model
     * @param  array $sort sorting fields with direction
     * @return bool|string
     */
    public static function sortFields(ActiveRecord $model, array $sort = ['id:' . SORT_DESC])
    {
        $attributes = $model->getAttributes();
        $orderBy = [];

        foreach ($sort as $v) {
            list($field, $direction) = explode(self::SEPARATOR_SORT, $v);
            !$direction && $direction = SORT_DESC;
            if (!$field) {
                return false;
            }

            if (!isset(self::SORT_DIRECTIONS[$direction])) {
                return false;
            }

            if (in_array($field, array_keys($attributes))) {
                $orderBy[$field] = self::SORT_DIRECTIONS[$direction];
            } else {
                return false;
            }
        }

        $orderByStr = '';
        foreach ($orderBy as $filed => $direction) {
            $orderByStr .= ',' . $filed . ' ' . $direction;
        }

        return trim($orderByStr, ',');
    }

    /**
     * Get custom error code by error message
     *
     * @param string $errMsg error message
     * @return bool|int
     */
    public static function customErrCode($errMsg)
    {
        if (preg_match('/\d+' . self::SEPARATOR_ERRCODE_ERRMSG . '/', $errMsg, $matches)) {
            return str_replace(self::SEPARATOR_ERRCODE_ERRMSG, '', $matches[0]);
        }

        return false;
    }

    /**
     * Select model fields
     *
     * @param ActiveRecord $model model
     * @param array $fields default empty
     * @return array
     */
    public static function selectModelFields(ActiveRecord $model, array $fields = [])
    {
        $model->refresh();

        $data = [];

        $modelAttrs = array_keys($model->attributes);

        if (!$fields) {
            $fields = $modelAttrs;
        } else {
            if (array_diff($fields, $modelAttrs)) {
                return $data;
            }
        }

        foreach ($fields as $field) {
            $data[$field] = $model->$field;
        }

        return $data;
    }

    /**
     * View model by fields
     *
     * @param ActiveRecord $model model
     * @param array $fields model fields
     * @return array
     */
    public static function viewModelByFields(ActiveRecord $model, array $fields)
    {
        $viewData = [];

        foreach ($fields as $field) {
            $viewData[$field] = $model->$field;
        }

        return $viewData;
    }

    /**
     * Check if some attribute value has been updated
     *
     * @param array $attrs attributes to be checked
     * @param ActiveRecord $model model
     * @return bool
     */
    public static function hasChangedAttr(array $attrs, ActiveRecord $model)
    {
        return count(array_intersect($attrs, $model->getDirtyAttributes())) != count($attrs);
    }

    /**
     * Check if unique error
     *
     * @param ActiveRecord $model model
     * @param $attr attribute name
     * @return bool
     */
    public static function uniqueError(ActiveRecord $model, $attr)
    {
        $errors = $model->errors;
        return isset($errors[$attr]) && false !== stripos($errors[$attr][0], 'has already been taken');
    }

    /**
     * Get model list
     *
     * @param Query $query query object
     * @param array $select select fields default all fields
     * @param array $extraFields extra fields default empty
     * @param ActiveRecord $model model
     * @param string $formatMethod format method default 'formatData'
     * @param string $extraMethod extra method default 'extraData'
     * @param int $page page number default 1
     * @param int $size page size default 12
     * @param array $orderBy order by fields default id desc
     * @return array
     */
    public static function pagination(Query $query, array $select = [], array $extraFields = [], ActiveRecord $model, $page = 1, $size = self::PAGE_SIZE_DEFAULT, $formatMethod = self::FORMAT_DATA_METHOD, $extraMethod = self::EXTRA_DATA_METHOD, $orderBy = self::ORDER_BY_DEFAULT)
    {
        !in_array(self::FIELD_IDENTITY, $select) && $select[] = self::FIELD_IDENTITY;
        $query->select($select)->from($model->tableName());
        $offset = ($page - 1) * $size;
        $data = [
            self::PAGINATION_RETURN_ARRAY_KEY_TOTAL => $query->count(),
            self::PAGINATION_RETURN_ARRAY_KEY_DETAILS => $query
                ->orderBy($orderBy)
                ->offset($offset)
                ->limit($size)
                ->all()
        ];

        foreach ($data[self::PAGINATION_RETURN_ARRAY_KEY_DETAILS] as &$row) {
            if ($extraFields && method_exists($model, $extraMethod)) {
                $row = array_merge($model::$extraMethod($row[self::FIELD_IDENTITY], $extraFields), $row);
            }
            if (method_exists($model, $formatMethod)) {
                $model::$formatMethod($row);
            }
        }

        return $data;
    }

    /**
     * 得到开始和结束时间 戳
     * @param $time_type
     * @param $time_start
     * @param $time_end
     * @return array
     */
    public static function timeDeal($time_type, $time_start, $time_end)
    {
        if ($time_type == 'custom' && $time_start && $time_end) {
            $time_start = strtotime($time_start);
            $time_end = strtotime($time_end);
        } else {
            $time_area = StringService::startEndDate($time_type, 1);
            $time_start = $time_area[0];
            $time_end = $time_area[1];
        }
        return [$time_start, $time_end];
    }

    /**
     * 简单的分页处理
     * @param array $arr 数据数组
     * @param $count
     * @param $page_size
     * @param $page
     * @return array
     */
    public static function pageDeal(array $arr, $count, $page, $page_size = self::PAGE_SIZE_DEFAULT)
    {
        $total_page = ceil($count / $page_size);
        $page = $page < 1 ? 1 : $page;
        if ($page > $total_page) {
            $sd = array(
                'list' => '',
                'total_page' => $total_page,
                'count' => $count,
                'page' => $page
            );
        } else {
            $sd = array(
                'list' => $arr,
                'total_page' => $total_page,
                'count' => $count,
                'page' => $page
            );
        }
        return $sd;
    }
}
````

>string相关方法

````php

<?php
/**
 * Created by PhpStorm.
 * User: hj
 * Date: 4/27/17
 * Time: 9:34 AM
 */

namespace app\services;

use app\models\UploadForm;
use app\models\User;
use dosamigos\qrcode\QrCode;
use Yii;

class StringService
{
    const SEPARATOR_BIRTHDAY = '-';

    /**
     * Get basename of fully qualified class
     *
     * @param string $classname fully qualified class name
     * @return string
     */
    public static function classBasename($classname)
    {
        $classname = trim($classname, '\\');
        if ($pos = strrpos($classname, '\\')) {
            return substr($classname, $pos + 1);
        }
        return $classname;
    }

    /**
     * Check if mobile number
     *
     * @param int $mobile mobile
     * @return bool
     */
    public static function isMobile($mobile)
    {
        $mobile = trim($mobile);
        if (!$mobile) {
            return false;
        }

        return preg_match('/^1[34578]{1}\d{9}$/', $mobile);
    }

    /**
     * Check if birthday
     *
     * @param string $birthday birthday
     * @param string $separator separator default empty
     * @return bool
     */
    public static function isBirthday($birthday, $separator = '')
    {
        $birthday = trim($birthday);
        if (!$birthday) {
            return false;
        }

        $pattern = '/^\d{4}' . $separator . '\d{2}' . $separator . '\d{2}$/';
        return preg_match($pattern, $birthday);
    }

    /**
     * Check if class const exists
     *
     * @param string $constName const name
     * @param string $className class name
     * @return bool
     */
    public static function constExists($constName, $className)
    {
        if (!$constName || !$className) {
            return false;
        }

        return array_key_exists($constName, self::classConstants($className));
    }

    /**
     * Get class constants
     *
     * @param $className
     * @return array class constants list
     */
    public static function classConstants($className)
    {
        if (!$className) {
            return [];
        }

        $reflect = new \ReflectionClass($className);
        return $reflect->getConstants();
    }

    /**
     * Get start and ent time of some time type defined in params['timeTypes']
     *
     * @param string $timeType time type
     * @param bool $returnTimestamp if start_time and end_time of timestamp
     * @return array
     */
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

    /**
     * Check if valid date, both YYYY-mm-dd and YYYY-m-d are valid
     *
     * @param string $date date
     * @return bool
     */
    public static function checkDate($date)
    {
        return filter_var($date, FILTER_VALIDATE_REGEXP,
            [
                'options' => [
                    'regexp' => '/^\d{4}-\d{1,2}-\d{1,2}$/',
                ]
            ]
        );
    }

    /**
     * Get district names
     *
     * @param  array $codes strict code list
     * @return array
     */
    public static function districtNamesByCodes($codes)
    {
        foreach ($codes as &$districtCode) {
            $districtCode = (string)self::checkDistrict($districtCode);
        }

        return $codes;
    }

    /**
     * Check if valid district code
     *
     * @param  string $code strict code
     * @return bool|int
     */
    public static function checkDistrict($code)
    {
        $codeLen = 6;
        $parentCodeLastCodes = '0000';

        $code = trim($code);
        if (!$code || strlen($code) != $codeLen) {
            return false;
        }

        $parentCode = substr($code, 0, 2) . $parentCodeLastCodes;
        $codes = Yii::$app->params['districts'][0];
        if (empty($codes[$parentCode][$code])) {
            return false;
        }

        return $codes[$parentCode][$code];
    }

    /**
     * Check if has repeated element in a list
     *
     * @param array $elements elments to check
     * @return bool
     */
    public static function checkRepeatedElement(array $elements)
    {
        return count($elements) != count(array_unique($elements));
    }

    /**
     * Check if has empty element in a list
     *
     * @param array $elements elments to check
     * @return bool
     */
    public static function checkEmptyElement(array $elements)
    {
        if (!$elements) {
            return true;
        }

        foreach ($elements as $element) {
            if (!$element) {
                return true;
            }
        }

        return false;
    }

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
     * Check if identity card no
     *
     * @param string $cardNo identity card no
     * @return bool
     */
    public static function checkIdentityCardNo($cardNo)
    {
        return filter_var($cardNo, FILTER_VALIDATE_REGEXP,
            [
                'options' => [
                    'regexp' => '/^\d{17}[0-9xX]$/',
                ]
            ]
        );
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

    /**
     * Format birthday
     *
     * @param int $birthday birthday
     * @param string $separator separator default -
     * @return string
     */
    public static function formatBirthday($birthday, $separator = self::SEPARATOR_BIRTHDAY)
    {
        $birthday = trim($birthday);
        $pattern = '/(\d{4})(\d{2})(\d{2})/';
        if (!$birthday || strlen($birthday) != User::BIRTHDAY_LEN || !preg_match($pattern, $birthday, $matches)) {
            return '';
        }

        unset($matches[0]);
        return implode($separator, $matches);
    }
}
````