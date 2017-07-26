
### ECS :
```
网页  控制台：ECS 中可通过网页控制台进入ECS主机(需要网页版密码-与主机密码不一样 - 记忆主机密码时，可在此设置)

Xshell控制台: ECS 中可通过 Xshell 控制台 进入ECS 主机（需要 用户名 与 主机密码）
```

### [FTP:](http://www.cnblogs.com/lidan/archive/2011/11/12/2246507.html)
用户名
密码
用户目录
>安装 vsftp
sudo apt-get update
sudo apt-get install vsftpd
sudo service vsftpd restart


> vsftpd 配置文件
vim /etc/vsftpd.conf

>修改
anonymouse_enable=NO

local_enable=YES
write_enable=YES
chroot_local_user=YES
local_umask=022
> 新添加
userlist_deny=NO
userlist_enable=YES
userlist_file=/etc/allowed_users
seccomp_sandbox=NO
local_root=/var/www

> 用户配置文件-添加用户名
vim /etc/allowed_users  #允许用户
vim /etc/ftpusers		#禁止用户，删除uftp

> 创建新添加
useradd testUser -m
passwd testUser 回车
输入密码


### Apache2
```
sudo apt-get install apache2
sudo /etc/init.d/apache2 start/stop/restart
```



### [https 安装](http://blog.csdn.net/Sky_qing/article/details/44303221)
sudo a2enmod ssl  启用 ssl 模块

vim /etc/apache2/apache2.conf

<Directory /var/www/>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

> 放置 ssl 证书
cd /etc/apache2/cert
/etc/apache2/cert/demo1
/etc/apache2/cert/demo2

> 启用其它端口
vim /etc/apache2/ports.conf
```
Listen 80

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

```

> vim /etc/hosts
```
192.168.1.250   unixmen1.local
192.168.1.250   unixmen2.local
```

> 添加多个虚拟主机绑定多个域名、每个域名都要开启https,启用 ssl 模块
cd /etc/apache2/sites-enabled
vim 000-default.conf
sudo a2dissite 000-default.conf
或 a2ensite demo1.local.conf
或 a2ensite demo2.local.conf
```
<VirtualHost *:80>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ServerAdmin huangzhengjie@dingtalk.com
        DocumentRoot  /var/www/xxx.cn
        ServerName xxx.cn
        RewriteEngine on
        RewriteCond %{HTTPS} !=on [NC]
        RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
        <Directory /var/www/xxx>
                Options FollowSymLinks MultiViews
                AllowOverride All
                Order allow,deny
                allow from all
                deny from 218.29.103.130 213.104.3.169 183.228.113.145 117.72.159.52

        </Directory>
</VirtualHost>

<VirtualHost *:80>
        ServerAdmin huangzhengjie@dingtalk.com
        DocumentRoot  /var/www/yyy
        ServerName yyy.cn
        RewriteEngine on
        RewriteCond %{HTTPS} !=on [NC]
        RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

        <Directory /var/www/yyy>
                Options FollowSymLinks MultiViews
                AllowOverride All
                Order allow,deny
                allow from all
                deny from 218.29.103.130 213.104.3.169 183.228.113.145 117.72.159.52

        </Directory>
</VirtualHost>
```

> vim 001-ssl.conf
```
<VirtualHost *:443>
        SSLEngine on
        # 添加 SSL 协议支持协议，去掉不安全的协议
        SSLProtocol TLSv1 TLSv1.1 TLSv1.2
        # 修改加密套件如下
        SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4

        #SSLCertificateFile /etc/apache2/cert/xxx/214081794140507.pem
        # 证书公钥配置
        SSLCertificateFile /etc/apache2/cert/xxx/public.pem
        # 证书私钥配置
        SSLCertificateKeyFile /etc/apache2/cert/xxx/214081794140507.key
        # 证书链配置，如果该属性开头有 '#'字符，请删除掉
        SSLCertificateChainFile /etc/apache2/cert/xxx/chain.pem


        ServerAdmin huangzhengjie@dingtalk.com
        DocumentRoot  /var/www/xxx
        ServerName xxx.cn

        <Directory /var/www/xxx>
                Options FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>
</VirtualHost>
<VirtualHost *:443>
        SSLEngine on
        # 添加 SSL 协议支持协议，去掉不安全的协议
        SSLProtocol TLSv1 TLSv1.1 TLSv1.2
        # 修改加密套件如下
        SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4

        #SSLCertificateFile /etc/apache2/cert/yyy/214081794140507.pem
        # 证书公钥配置
        SSLCertificateFile /etc/apache2/cert/yyy/public.pem
        # 证书私钥配置
        SSLCertificateKeyFile /etc/apache2/cert/yyy/214081794140507.key
        # 证书链配置，如果该属性开头有 '#'字符，请删除掉
        SSLCertificateChainFile /etc/apache2/cert/yyy/chain.pem


        ServerAdmin huangzhengjie@dingtalk.com
        DocumentRoot  /var/www/yyy
        ServerName yyy.cn

        <Directory /var/www/yyy>
                Options FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>
</VirtualHost>

```


#### 开启服务器静态缓存

```
1、
sudo a2enmod expires

2、
sudo /etc/init.d/apache2 restart

3、
在.htaccess文件中
#Now set the expires time for various type of contents
<IfModule mod_expires.c>
    ExpiresActive On
    
    #30 days
    ExpiresByType image/x-icon A2592000
    ExpiresByType application/x-javascript A2592000
    ExpiresByType application/javascript A2592000
    ExpiresByType text/javascript A2592000
    ExpiresByType text/ecmascript A2592000
    ExpiresByType text/css A2592000
    
    #7 Days
    ExpiresByType image/gif A604800
    ExpiresByType image/png A604800
    ExpiresByType image/jpeg A604800
    ExpiresByType text/plain A604800
    ExpiresByType application/x-shockwave-flash A604800
    ExpiresByType video/x-flv A604800
    ExpiresByType application/pdf A604800
    
    #ExpiresByType text/html A900
</IfModule>
```

4、
create a new file "expires.conf" under mods_available that contains the following:

```
<IfModule mod_expires.c>
# turn on the module for this directory
ExpiresActive on

# cache common graphics for 3 days
ExpiresByType image/jpg "access plus 3 days"
ExpiresByType image/gif "access plus 3 days"
ExpiresByType image/jpeg "access plus 3 days"
ExpiresByType image/png "access plus 3 days"

# cache CSS for 24 hours
ExpiresByType text/css "access plus 24 hours"

# set the default to 24 hours
ExpiresDefault "access plus 24 hours"
</IfModule>

5、create soft link under mods-enable
ln -s expires.conf ../mods-available/expires.conf


6、service apache2 restart
```