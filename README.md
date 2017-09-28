# laravel-pingpp
pingxx基于laravel5的封装

[![Latest Stable Version](https://poser.pugx.org/tobess/laravel-pingpp/v/stable)](https://packagist.org/packages/tobess/laravel-pingpp) [![Total Downloads](https://poser.pugx.org/tobess/laravel-pingpp/downloads)](https://packagist.org/packages/tobess/laravel-pingpp) [![Latest Unstable Version](https://poser.pugx.org/tobess/laravel-pingpp/v/unstable)](https://packagist.org/packages/tobess/laravel-pingpp) [![License](https://poser.pugx.org/tobess/laravel-pingpp/license)](https://packagist.org/packages/tobess/laravel-pingpp)

# 配置方法
1. 在`composer.json`里添加如下内容，并运行`composer update`:
```json
{
    "require": {
        "tobess/laravel-pingpp": "dev-master"
    }
}
```
1. 在`app/config/app.php`文件里的providers变量下添加`Tobess\Pingpp\PingppServiceProvider::class,`
1. 在`app/config/app.php`文件里的aliases变量下添加`'Pingpp' => Tobess\Pingpp\Facades\Pingpp::class,`
1. 运行`php artisan vendor:publish`生成配置文件
1. 修改配置文件里面的2组key
1. 若需回调验证，请填写`public_key_path`，**注意该处是路径！**，这里参考官方已改为路径，同时当前会兼容旧版本配置文件
1. 若需要使用商户身份验证，请填写`private_key_path`，**注意该处是路径！**
线上生产环境建议配置`.evn`来配置生产环境

# 使用方法
```php
use Pingpp;

class SomeClass extends Controller {
    
    public function someFunction()
    {
    	Pingpp::Charge()->create([
            'order_no'  => '123456789',
		    'amount'    => '100',
		    'app'       => array('id' => 'app_xxxxxxxxxxxxxx'),
		    'channel'   => 'upacp',
		    'currency'  => 'cny',
		    'client_ip' => '127.0.0.1',
		    'subject'   => 'Your Subject',
		    'body'      => 'Your Body'
        ]);
    }
}
```

```php
use Pingpp;

class SomeClass extends Controller {
    
    public function someFunction()
    {
    	Pingpp::RedEnvelope()->create([
            'order_no'  => '123456789',
	        'app'       => array('id' => 'APP_ID'),
	        'channel'   => 'wx_pub',
	        'amount'    => 100,
	        'currency'  => 'cny',
	        'subject'   => 'Your Subject',
	        'body'      => 'Your Body',
	        'extra'     => array(
	            'nick_name' => 'Nick Name',
	            'send_name' => 'Send Name'
	        ),
	        'recipient'   => 'Openid',
	        'description' => 'Your Description'
        ]);
    }
}
```

# 错误调用
当Pingpp调用发生错误的时候会`return false`，此时调用`Pingpp::getError();`返回具体错误内容。

# 接收 Webhooks 通知
直接调用`Pingpp::notice()`，若验证成功,会返回通知的`array`结构数据,若失败直接弹出错误回Pingpp。如果需要记录错误，请自行监听系统错误，详见[Errors & Logging](https://laravel.com/docs/5.5/errors)

# 感谢
感谢几位forked的同学的改进，当时写的很简单，无入侵式的写法，其实在接收webhooks方面，可以用Laravel自己的[Events](https://laravel.com/docs/5.5/events)去监听处理，显得更Laravel风格。
