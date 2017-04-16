# back-end

### 简单后台 环境搭建


### 配置选择

> xampp + lumen + bootstrap

#### xampp 安装

> 选择 xampp ,php5.3,5.6,7.0三版本切换


#### Composer 

> 下载 composer.phar 放到 php  /xampp/php/

> echo php "%~dp0composer.phar" %* > composer.bat

> composer config -l -g 查看 composer 全局配置信息

> 更换国内镜像 composer config -g repositories.packagist composer http://packagist.phpcomposer.com

```

[Lumen环境Windows下Xampp环境安装Composer](http://blog.m1910.com/archives/1481.html) 

[Windows 下离线安装  Composer](https://www.xfcy.me/win-install-composer.html) 

composer安装需要开启openssl ，所以 在php.ini 中 php_openssl 扩展要开启
```


#### php 开发框架

> [Laravel & Lumen 一键安装包下载](http://www.golaravel.com/download/)
  
> composer update 更新 lumen & laravel 版本（composer.json中修改依赖版本号）

> [lumen5.4整合dingo/api、jwt-auth](http://m.blog.csdn.net/article/details?id=55195207)

```
1、jwt-auth 稳定版本0.5，但不兼容lumen 5.4, 所以选择 jwt-auth开发版本 1.0@dev

[使用php轻框架－lumen搭建api服务](http://www.jianshu.com/p/bba4239c7c03) 

[lumen功能测试 ](https://lumen.laravel-china.org/docs/5.1/testing) 

[lumen-api-demo](https://github.com/liyu001989/lumen-api-demo)

[Lumen上使用Dingo/Api做API开发时用JWT-Auth做认证的实现](http://blog.csdn.net/hooloo/article/details/49714259) 

[Laravel & Lumen RESTFul API 扩展包：Dingo API](http://laravelacademy.org/post/3822.html)

[DingoAPI开发及访问方法](https://laravel-china.org/topics/1463/lumen-version-of-the-problem-using-dingo-api)

[Laravel (Lumen) 中使用JWT-Auth刷新token的问题](http://blog.csdn.net/hooloo/article/details/50649712)

[JWT（Json Web Token） 实现基于API的用户认证](http://laravelacademy.org/post/3640.html)
```

> [Lumen如何实现类Laravel5用户友好的错误页面](https://segmentfault.com/a/1190000003738977?utm_source=tuicool&utm_medium=referral)

```
/**
 * Render an exception into an HTTP response.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Exception  $e
 * @return \Illuminate\Http\Response
 */
public function render($request, Exception $e)
{
    if( !env('APP_DEBUG') and $this->isHttpException($e)) {
        return $this->renderHttpException($e);
    }
    return parent::render($request, $e);
}

/**
 * Render the given HttpException.
 *
 * @param  \Symfony\Component\HttpKernel\Exception\HttpException  $e
 * @return \Symfony\Component\HttpFoundation\Response
 */
protected function renderHttpException(HttpException $e)
{
    $status = $e->getStatusCode();

    if (view()->exists("errors.{$status}"))
    {
        return response(view("errors.{$status}", []), $status);
    }
    else
    {
        return (new SymfonyExceptionHandler(env('APP_DEBUG', false)))->createResponse($e);
    }
}

/**
 * Determine if the given exception is an HTTP exception.
 *
 * @param  \Exception  $e
 * @return bool
 */
protected function isHttpException(Exception $e)
{
    return $e instanceof HttpException;
}
```

> [Laravel 数据库之：数据库请求构建器](http://d.laravel-china.org/docs/5.4/queries)  


> xampp 虚拟主机的配置

```
<virtualhost *:80>  
ServerName template  
DocumentRoot "D:/phpweb/tpl"
<Directory D:/phpweb/tpl> 
Options FollowSymLinks IncludesNOEXEC Indexes
DirectoryIndex index.html index.htm index.php
    Order Deny,Allow
    Allow from all
    Require all granted
</Directory>
</virtualhost>
```











 



 



 

        

