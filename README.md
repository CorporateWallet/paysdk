歡迎使用 Corporate Wallet SDK for PHP 。

## 環境要求
1. Corporate Wallet SDK for PHP 需要 PHP 5.5 以上的開發環境。

2. 使用 Corporate Wallet SDK for  PHP 之前 ，您需要先联系Corporate Wallet管理员完成接入的一些準備工作，包括創建應用、為應用設置接口相關配置等。

3. 準備工作完成後，注意保存如下信息，後續將作為使用SDK的輸入。

* 加簽模式為公私鑰證書模式

`AppID`、`應用的私鑰`、`應用公鑰`


## 快速使用

1. Composer 安裝
```
composer require corporate/paysdk
```

2. 示例代碼
```php
<?php

require 'vendor/autoload.php';

use Corporate\PaySDK\Base\CorporateClient;
use Corporate\PaySDK\Base\Config;


//加载配置文件
$corporateClient = new CorporateClient(getOptions());


 //付款請求接口
$params = [
    "out_trade_no" => "20200326235526001",
    "public_chain" => "TRON",
    "digital_coin" => "USDT",
    "amount" => 1,
    "protocol" => "TRC-20",
    "receipt_address" => "TWK3vomsDgKNSwdvezcGGs24jztNjmKK99",
    "timestamp" => 1666245173,
    "note" => "test"
];

$result = $corporateClient->execute("/api/wallet/goPay",$params);
if($result->isSuccess()){

    if($result->verify()){

        print_r($result->dataMap());
    }else{
        throw new \Exception("驗簽失敗，請檢查Corporate應用公鑰或應用私鑰是否配置正確。");
    }

}else{
    throw new \Exception($result->ret() .":".$result->msg());
}


//异步回调通知處理示例
$json_string = '{"ret":1000,"msg":"\u8bf7\u6c42\u6210\u529f","data":"WDlwdnBoSkFDeS96bVdIYjg4WUNaaXVuV3NTQ......."}';
$result = $corporateClient->getApiResponse($json_string);
if($result->isSuccess()){

    if($result->verify()){

        print_r($result->dataMap());
    }else{
        throw new \Exception("驗簽失敗，請檢查Corporate應用公鑰或應用私鑰是否配置正確。");
    }
}


function getOptions()
{
    $options = new Config();

    $options->apiUrl = "<-- 請填寫應用分配的接口域名，例如：https://xxx.corporate.com/ -->";
    $options->appToken = "<-- 請填寫您的appToken，例如：377b26eb8c25bd... -->";

    //語系(參考文檔中最下方語系表，如:TC)
    $options->lang = "TC";

    $options->corporatePrivateCertPath = "<-- 請填寫您的應用私鑰路徑，例如：/foo/MyPrivateKey.pem -->";
    $options->corporatePublicCertPath = "<-- 請填寫Corporate應用公鑰證書文件路徑，例如：/foo/CorporatePublicKey.pem -->";

    return $options;
}

```
