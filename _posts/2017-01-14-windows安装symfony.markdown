---
title:  windows下搭建symfony环境
layout: post
guid: urn:uuid:b87da13a-a4dd-402f-b06a-cef7eeee2d80
tags:
    - symfony
---

1.下载安装php,并将php添加到环境变量中

2.进入到项目根目录 

    php -r "readfile('http://symfony.com/installer');" > symfony.phar

3.等待下载symfony.phar文件

4.下载完成之后，在目录下面执行 

    php symfony.phar

5.创建新工程

    php symfony.phar new projectName

6.如果php版本是5.6及以上可能会遇到cUrl error 60的错误

7.访问https://curl.haxx.se/ca/cacert.pem 将页面的内容拷贝复制到本地路径“路径/cacert.pem”

8.修改php.ini 配置文件

    curl.cainfo = "路径\cacert.pem"