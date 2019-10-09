---
layout: main
title: 基于LAMP安装EduSoho在线学习管理系统(CMS，LMS)
categories: [在线学习平台]
tags: [EduSoho, CMS, LMS]
---

# 基于LAMP安装EduSoho在线学习管理系统(CMS，LMS)

开始本篇中步骤之前，确保有一台Ubuntu 16.04的主机或虚拟机。

## 第1步：安装LAMP

安装LAMP的具体步骤可以参考文章[如何在Ubuntu 16.04安装Linux，Apache，MySQL和PHP-LAMP](https://7788wangzi.github.io/install-linux-apache-mysql-php-on-ubuntu-16-04/)，在此，只简略描述要运行的命令。

### 安装更新
```
sudo apt-get update
```

### 安装Apache2
```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-xsendfile
```

查看Apache版本，确保Apache是2.4版本
```
apachectl -v
```
```output
Server version: Apache/2.4.18 (Ubuntu)
Server built:   2019-04-03T13:34:47
```

配置Apache启用rewrite 和xsendfile模块
```
sudo a2enmod rewrite
sudo a2enmod xsendfile
```

修改/etc/apache2/apache2.conf的文件来抑制一个警告消息。
```
sudo apache2ctl configtest
```
输出结果如下
```
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
```
使用文本编辑打开主配置文件：
```
sudo nano /etc/apache2/apache2.conf
```
在内部，在文件的底部，添加一个ServerName指令，指向您的主域名。 如果您没有与服务器关联的域名，则可以使用服务器的公共IP地址： NOTE: 如果你不知道你的服务器的IP地址，跳过了一节如何找到您的服务器的公网IP地址找到它。

ServerName server_domain_or_IP

当你完成后，保存并按下Ctrl-X关闭文件。 你必须确认键入y保存，然后按Enter键确认该文件的保存位置。 接下来，通过键入以检查语法错误：
```
sudo apache2ctl configtest
```
由于我们增加了全球ServerName指令，你应该看到的是：
```
Syntax OK
```
重新启动Apache以实现您的更改：
```
service apache2 restart
```
**调整防火墙以允许Web流量。**
确保UFW具有Apache的应用程序配置文件
```
sudo ufw app list
```
输出
```
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```
查看Apache Full信息
```
sudo ufw app info "Apache Full"
```
输出应该是80, 443端口
```
Profile: Apache Full
Title: Web Server (HTTP,HTTPS)
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Ports:
  80,443/tcp
```
设置配置文件，允许传入流量
```
sudo ufw allow in "Apache Full"
```
现在，可以在浏览器中使用如下网站来访问apache网页。
```
http://your_server_IP_address
```

### 安装MySQL
安装MySQL
```
sudo apt-get install mysql-server
```
**设置密码**

在这个过程过程中会要求您输入MySQL数据库的root密码，请认真填写。

### 安装PHP

安装PHP7.0版本
```
sudo apt-get install php-pear php7.0-cli php7.0-common php7.0-curl php7.0-dev php7.0-fpm php7.0-json php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-opcache php7.0-zip php7.0-intl php7.0-gd php7.0-xml
```
修改配置文件，设置文件大小限制
```
sudo nano /etc/php/7.0/fpm/php.ini
```
修改这三个值的大小
```
post_max_size = 1024M
memory_limit = 1024M
upload_max_filesize = 1024M
```
当你完成后，保存并按下Ctrl-X关闭文件。 你必须确认键入y保存，然后按Enter键确认该文件的保存位置。

重启PHP7.0-fpm
```
sudo service php7.0-fpm restart
```

安装PHP另外2个组件
```
sudo apt-get install libapache2-mod-php7.0 php7.0-mcrypt php7.0-mysql
```
修改服务器目录中文件优先顺序
```
sudo nano /etc/apache2/mods-enabled/dir.conf
```
它将如下所示：
```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
将PHP索引文件调整到最前
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

当你完成后，保存并按下Ctrl-X关闭文件。 你必须确认键入y保存，然后按Enter键确认该文件的保存位置。

重启Apache
```
service apache2 restart
```

在Web服务器上测试PHP处理

新疆info.php文件
```
sudo touch /var/www/html/info.php
```
打开文件并修改内容
```
sudo nano /var/www/html/info.php
```
将以下内容拷贝进去
```
<?php
phpinfo();
?>
```

完成后，保存并关闭文件。
使用如下地址测试PHP被正确处理。
```
http://your_server_IP_address/info.php
```

## 第2步：安装edusoho
进入服务器目录
```
cd /var/www/html
```
下载edusoho压缩包文件
```
sudo wget http://download.edusoho.com/edusoho-8.0.16.tar.gz
```
**注意**：如果下载很慢，可以从本机下载文件再拷贝到Linux虚拟机中。拷贝会有权限问题，可以先拷贝到/tmp下，在使用sudo命令移动到/var/www/html下。
```
cd /tmp
sudo mv edusoho-8.0.16.tar.gz /var/www/html/
```
确保edusoho-8.0.16.tar.gz文件在/var/www/html/目录中，解压文件。
```
cd /var/www/html/
sudo tar -zxvf edusoho-8.0.16.tar.gz
```
使用`ls`命令查看当前路径中已释放出文件夹edusoho。

给edusoho文件夹进行www授权
```
sudo chown www-data:www-data edusoho/ -Rf
```

## 修改Apache配置文件，修改文件服务器入口
进入/etc/apache2/sites-enabled目录
```
cd /etc/apache2/sites-enabled
```
创建edusoho.conf文件
```
sudo touch edusoho.conf
```
打开edusoho.conf文件，拷入内容:
```
sudo nano edusoho.conf
```
文件内容中，修改ServerName和DocumentRoot中的路径，一定要修改到/web下：
```
<VirtualHost *:80>
    ServerName your_server_IP_address

    DocumentRoot /var/www/html/edusoho/web
    <Directory /var/www/html/edusoho/web>
        # enable the .htaccess rewrites
        AllowOverride All
        Require all granted   
    </Directory>
    ErrorLog /var/log/apache2/project_error.log
    CustomLog /var/log/apache2/project_access.log combined
</VirtualHost>
```
当你完成后，保存并按下Ctrl-X关闭文件。 你必须确认键入y保存，然后按Enter键确认该文件的保存位置。

打开000-default.conf文件:
```
sudo nano 000-default.conf
```
同样修改DocumentRoot到/web下：
```
ServerAdmin webmaster@localhost
DocumentRoot /var/www/html/edusoho/web
```
保存并按下Ctrl-X关闭文件。 你必须确认键入y保存，然后按Enter键确认该文件的保存位置。

重启Apache。
```
sudo service apache2 restart
```

## 第3步：配置edusoho
在浏览器中，访问
```
http://your_server_IP_address/app.php/
```
按照引导，完成edusoho的配置。
