<h1 align="center"> LaraCMS </h1>

<p align="center">  基于 Laravel 开发的内容管理系统。</p>


[![Build Status](https://travis-ci.org/wanglelecc/laracms.svg?branch=master)](https://travis-ci.org/wanglelecc/laracms)
[![StyleCI build status](https://github.styleci.io/repos/152737755/shield)](https://github.styleci.io/)

## 环境需求
- PHP 7.1+
- Mysql 5.7+

## 安装说明

### 安装 Laravel 框架

```bash
$ composer create-project --prefer-dist laravel/laravel laracms
```

### 添加 composer 包来源
```json
    {
      "repositories": [{
        "type": "composer",
        "url": "https://packagist.56br.com/"
      }]
    }
```
> 因 LaraCMS 扩展包属于私有包，所以需要手动添加来源到项目 `composer.json` 文件.

### 安装 LaraCMS 扩展包
```bash
$ cd laracms
$ composer require wanglelecc/laracms:dev-master -vvv
```

### 发布资源
```bash
# 配置文件
$ php artisan vendor:publish --tag=config

# 静态资源
$ php artisan vendor:publish --tag=public

# 错误模板
$ php artisan vendor:publish --tag=laracms-view-errors
```

### 新增 `.ENV` 配置项并配置好数据库
```ini
FILESYSTEM_DRIVER=public

ALIYUN_ACCESS_ID=1w7GGueV8xMLqHMw
ALIYUN_ACCESS_KEY=0ttIn6T5aJhSYtaY1yxrUyPdWPj0XD
ALIYUN_OSS_BUCKET=wanglelecc
ALIYUN_OSS_ENDPOINT=oss-cn-beijing.aliyuncs.com
ALIYUN_OSS_PREFIX=
ALIYUN_OSS_URL=https://wanglelecc.oss-cn-beijing.aliyuncs.com
ALIYUN_VOD_URLOAD_URL=outin-53989bccd12711e88630163e1c60dc.oss-cn-beijing.aliyuncs.com

AZURE_STORAGE_NAME=
AZURE_STORAGE_KEY=
AZURE_STORAGE_CONTAINER=

BAIDU_TRANSLATE_APPID=
BAIDU_TRANSLATE_KEY=

API_STANDARDS_TREE=prs
API_SUBTYPE=laracms
API_DOMAIN=
API_PREFIX=api
API_VERSION=v1
API_DEBUG=true

WEIXIN_KEY=wx3531f6
WEIXIN_SECRET=d4624c6795d1d9
WEIXIN_REDIRECT_URI="${APP_URL}/login/weixin/callback"

WEIXINWEB_KEY=
WEIXINWEB_SECRET=
WEIXINWEB_REDIRECT_URI=

QQ_KEY=1074656
QQ_SECRET=9e05ee41efb0f7a583e1ef95c8a
QQ_REDIRECT_URI="${APP_URL}/login/qq/callback"

WEIBO_KEY=3949566
WEIBO_SECRET=142dae5c3509d77679c252f778dd
WEIBO_REDIRECT_URI="${APP_URL}/login/weibo/callback"

GITHUB_KEY=d6b462c46be4257bd
GITHUB_SECRET=584c5327fcd1ed54497e2f47355ee33f151
GITHUB_REDIRECT_URI="${APP_URL}/login/github/callback"

JWT_SECRET=HSKxIUfdJj5gadbqfQo5im9zje95g9

SCOUT_DRIVER=tntsearch
```
> 追加到项目根目录下的 `.ENV` 文件。

### 修改 `app/User.php` 模型
```php
<?php

namespace App;

class User extends \Wanglelecc\Laracms\Models\User
{
    
}
```

### 修改 `routes/web.php` 文件
```php
# 删除以下代码
Route::get('/', funtion(){ 
	return vuew('welcome'); 
});
```

### 修改 `app/Console/Kernel.php` 文件，在 `schedule` 方法里新增以下内容
```php
# 每天午夜清除一次临时垃圾分片数据
$schedule->command('laracms:uploader')->daily();
```

### 修改 `config/app.php` 文件
```php
# 修改默认时区
'timezone' => 'Asia/Shanghai',

# 修改语言
'locale' => 'zh-CN',
```

### 数据迁移
```bash
$ php artisan migrate
```

### 创建 `storage` 软链接
```bash
$ php artisan storage:link
```

至此安装完成。记得修改 `.ENV` 里的 `APP_URL` 选项。

## 访问方式
```text
后台：http://example.com/administrator
前台：http://example.com/
用户：admin@56br.com / 123456
```

## 附加
如需要开发，建议再安装辅助扩展包。在项目根目录下的 `composer.json` 文件 `require-dev` 选项增加：
```bash
# 页面调试工具栏
$ composer require-dev barryvdh/laravel-debugbar

# 记录所有 Sql 语句
$ composer require-dev overtrue/laravel-query-logger

# 逆向数据迁移文件
$ composer require-dev orangehill/iseed
```
> 此项可选，根据需要添加。