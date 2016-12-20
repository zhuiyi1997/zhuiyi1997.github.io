---
title:  ubuntu安装lnmp环境
layout: post
guid: urn:uuid:b87da13a-a4dd-402f-b06a-cef7eeee2d80
tags:
    - linux
    - ubuntu
---

首先ctrl+alt+t打开终端

1.更新软件源:
    sudo apt-get update
2.安装nginx
    sudo apt-get install nginx
3.启动nginx
    sudo /etc/init.d/nginx start
4.安装php5和mysql
    sudo apt-get install php5-cli php5-cgi mysql-server php5-mysql

5.修改mysql用户权限
 1)从终端进入mysql
     mysql -u root -p123456
 2)更改用户权限: grant 权限 on 数据库对象 to 用户
   grant all on *.* to root@'%' identified by '123456';
 3)刷新让权限生效
   flush privileges;

6.修改mysql下配置文件
 1)修改mysql的配置文件
   sudo vi/etc/mysql/my.cnf
 2)将bind-address = 127.0.0.1前面加上#注释掉，这样就可以远程连接数据库了。

7.安装php5-fpm
  sudo apt-get install php5-fpm

8.配置nginx并重启服务
 1)然后同样进入vi编辑
    sudo vi /etc/nginx/sites-enabled/default
 2)将里面的内容修改成以下样子
  (把里面server的listen 80和location ~\.php${的注释打开，其他的看着改)

    server{
       listen 80;
       root /usr/share/nginx/www;
       index index.php index.html index.htm;
       server_name localhost;
       location / {
           try_files $uri $uri/ /index.html;
       }
       location ~ \.php$ {
           fastcgi_pass 127.0.0.1:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME /usr/share/nginx/www$fastcgi_script_name;
           include /etc/nginx/fastcgi_params;
       }
   }

 3).改完保存退出

9.启动fastcgi php
  sudo service php5-fpm start

10.重启nginx服务
  sudo service nginx restart

11.安装php相关扩展(xdebug,memcache,oauth等)
 1）安装curl
   sudo apt-get install php5-curl
 2)安装gettext:
   sudo apt-get install php-gettext
 3)安装gd库：
   sudo apt-get install php5-gd
 4)安装mcrypt:
   sudo apt-get install php5-mcrypt
 5)安装memcache
   a)安装服务器：
     sudo apt-get install memcached
     memcached -d -m 50 -p 11211 -u root
     -m指定使用多少兆的缓存空间（这里50） -p指定要监听的端口 (11211) -u指定哪个用户使用(root)
   b)安装php模块
     sudo apt-get install php5-memcache
 6)安装oauth:基于pecl的
   a)sudo apt-get install php5-dev php-pear libpcre3-dev
     sudo pecl install oauth(要确保linux系统里可以make)
   b)修改配置文件
     sudo vi /etc/php5/fpm/php.ini
     在最后添加

extension=oauth.so

  12)安装ssh2:
   sudo apt-get install libssh2-php
 13)安装xdebug
   1)sudo apt-get install php5-xdebug
   2)修改php配置文件
    sudo vi /etc/php5/fpm/php.ini
    将display_errors和html_errors都改为On
 14)重启php服务:
   sudo service php5-fpm restart


 这时环境搭配就成功了.服务器的文件路径是/usr/share/nginx/www。

  打开发现里面有个index.html文件。然后在浏览器输入localhost/index.html  就可以看到亲切的
Welcome to nginx!

使用终端常用命令:

查看文件权限
ls -l /dir/files
以管理员身份执行命令:
sudo .....
更改文件权限
sodu chmod 777 xxxx(777)是开放所有权限  644是管理员有读写，其他人只有读权限

vi编写常用命令:
从光标所在的地方插入
i
从光标之后插入
a
退出编辑模式
esc
删除光标在内的当前行及其下面的n-1行内容（退出编辑模式才可使用）
ndd
删除字符（退出编辑模式才可使用）
X(大写，删除光标前)    x(小写，删除光标后的)
保存并退出vi(退出编辑模式才可使用)
:x
不保存并退出vi(退出编辑模式才可使用)
:q!