# BitPay SSPanel-v3-mod
BitPay - 更匿名、更安全、无需支付宝姓名和账户等个人私密信息。Bitpay利用数字货币的匿名性，保障商户的安全。
（注意，请必须遵守当地政策。建议去有拍照并且合法的交易。例如美国Coinbase、新加坡Gemini、马耳他Binance币安等交易所）。

你如果是老司机，应该经历过：
 * 剛繳註冊費，審核等了1天，审核不通过（审核費卒）
 * 支付平台跑路，无法提币（或暂停维护1个月）。
 * 自己实名身份被第三方记录（比如说支付平台，并且很有可能被上交)

请继续查看Bitpay数字货币匿名支付，其他[常见问题](https://github.com/bitpaydev/docs/blob/master/FAQ.md)。如果需要[在线客服聊天](https://bitpay.dev)。

如果你仍然希望你的用户使用微信、支付宝等原生体验，可以使用混合模式，下面用TomatoBitPay作为例子（支持TMT，以及BitPay）。

> 谨慎能捕千秋蝉 - 周子沐


## 获取授权

通过[邀请链接](https://merchants.mugglepay.com/user/register?ref=MP38158552) 注册 邀请码MP38158552，然后注册登录[商家后台](https://merchants.mugglepay.com)，然后选择"个人设置"->“API认证”->“后台服务器”。

我们推荐以USDT或者BTC结算，所以不需要实名认证审核（或者我们不希望存你的隐私数据，例如手机或者支付宝信息）。默认结算用USDT，数字货币相关问题请先查阅[常见问题](https://github.com/bitpaydev/docs/blob/master/FAQ.md)。

## 对接交流群

 * [技术在线对接群](https://t.me/joinchat/GLKSKhUnE4GvEAPgqtChAQ)

## 文件新增

需要新增4个文件，并且放入对应的目录下，不会与现有的代码冲突。[下载链接](https://github.com/bitpaydev/bitpayForSSPanelV3)。

 * resources/views/material/user/tomatobitpay.tpl
 * resources/views/material/user/bitpay.tpl
 * app/Services/Gateway/BitPay.php
 * app/Services/Gateway/TomatoBitPay.php



## 文件修改

需要修改2个配置文件。如果你只用数字货币，就选择bitpay。如果想同时兼容bitpay和tomatopay，那就用tomatobitpay。

 * config/.config.php
 * app/Services/Payment.php

```
# 修改： config/.config.php

# 取值 none | trimepay | yftpay |tomatopay | tomatobitpay | bitpay
$System_Config['payment_system']='bitpay';

# 商户后台获取授权码 https://merchants.mugglepay.com/
$System_Config['bitpay_secret']='XXXX';
```

```
# 修改： app/Services/Payment.php, 在对应位置添加几行代码
use App\Services\Gateway\{
  ..., BitPay, TomatoBitPay
};

# 加入 bitpay
class Payment
{
  public static function getClient()
  {
    $method = Config::get("payment_system");
    switch ($method) {
      case("tomatobitpay"):
        return new TomatoBitPay();
      case("bitpay"):
        return new BitPay(Config::get('bitpay_secret'));
      default:
        return NULL;
    }
  }
}
```
