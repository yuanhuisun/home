## Ubuntu 20.04 下如何安装或更新至 PHP 8.1
原文作者：Summer  
转自链接：https://learnku.com/php/t/51997  
版权声明：著作权归作者所有。商业转载请联系作者获得授权，非商业转载请保留以上作者信息和原文链接。  

本指南让你了解如何安装最新的 php 版本 8.1，并在你的任何 VPS、云服务器、专用主机上的 Ubuntu 20.0 系统中升级到最新版本，并将其配置为 Apache 和 Nginx。

最新的 php 8.1 正式发布于 2022 年 1 月 22 日。它附带了一些新特性，并且在你升级旧版本之前应该注意到一些不兼容的问题。

### 开始
通过运行以下命令，确保你的 Ubuntu 服务器具有最新的软件包。

   sudo apt update
   sudo apt upgrade

这将更新软件包索引，并将已安装的软件包更新为最新版本。

### 为 PHP 8.1 添加 PPA
添加具有 PHP 8.1 软件包和其他必需的 PHP 扩展的 ondrej / php。

   sudo apt install software-properties-common
   sudo add-apt-repository ppa:ondrej/php
   sudo apt update

一旦你添加 PPA，你就可以安装 PHP 8.1 了。

### 为 Apache 安装 PHP 8.1
执行以下命令以安装 PHP 8.1

   sudo apt install php8.1

安装完成后，可以使用以下命令确认安装

   php -v

### 为 Nginx 安装 PHP 8 FPM
对于 Nginx，你需要安装 FPM，执行以下命令以安装 PHP 8 FPM

   sudo apt install php8.1-fpm

安装完成后，请使用以下命令确认 PHP 8.1 FPM 已正确安装

   php-fpm8.1 -v

### 安装 PHP 8.1 扩展
安装 php 扩展很简单，使用下面的命令可以安装任意扩展

   sudo apt install php8.1-extension_name

下面列出了常用的扩展，可以复制并直接安装

   sudo apt install php8.1-common php8.1-mysql php8.1-xml php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip -y

### 为 Apache 配置 PHP 8.1
现在我们配置 Web 应用的 PHP 版本，可以通过修改 php.ini 文件中的某些值来配置

对于使用 Apache 的 PHP 8.1，php.ini 位置一般在下面的目录中。

   sudo nano /etc/php/8.1/apache2/php.ini

推荐在编辑器中按 F6，使用搜索功能修改配置项，推荐更新以下值可以提高性能。

   upload_max_filesize = 32M 
   post_max_size = 48M 
   memory_limit = 256M 
   max_execution_time = 600 
   max_input_vars = 3000 
   max_input_time = 1000

修改 PHP 设置后，你需要重新启动 Apache 才能使更改生效。

### 为 Nginx 配置 PHP 8.1
对于使用 Nginx 的 PHP 8.1 FPM，php.ini 位置将在以下目录中。

   sudo nano /etc/php/8.1/fpm/php.ini

在编辑器中按 F6 键进行搜索，并更新以下值以获得更好的性能。

   upload_max_filesize = 32M 
   post_max_size = 48M 
   memory_limit = 256M 
   max_execution_time = 600 
   max_input_vars = 3000 
   max_input_time = 1000

修改 PHP 设置后，你需要重新启动 PHP FPM 才能使更改生效。

   sudo php-fpm8.1 -t 
   sudo service php8.1-fpm restart

### 配置 PHP 8.1 FPM 池
PHP 8.1 FPM 允许你为服务配置用户和组，并在其下运行。你可以使用以下命令修改它们

   sudo nano /etc/php/8.1/fpm/pool.d/www.conf

通过将 www-data 替换为你的 username 来更改以下几行。

   user = username 
   group = username 
   listen.owner = username
   listen.group = username

点击 CTRL+X 和 Y 保存配置，并检查配置是否正确，并重启 PHP。

### 重启 PHP 8.1 FPM
更新 PHP FPM 设置后，你需要重新启动它以应用更改。

   sudo php-fpm8.1 -t 
   sudo service php8.1-fpm restart

现在，你已经安装并配置了 PHP 8.1。

### 为 Apache 升级到 PHP 8.1
安装 PHP 8.1 之后，你需要升级到最新安装的 PHP 版本。

你需要禁用旧 PHP 版本并启用新 PHP 版本 8.1。

   sudo a2dismod php7.4

此命令将禁用 PHP 7.4 模块。

   sudo a2enmod php8.1

此命令将启用 PHP 8.1 模块。
必须要重新启动 Apache 才能使更改生效。使用下面的命令

   sudo service apache2 restart

### 将 Nginx 升级到 PHP 8.1
你需要在 Nginx 配置文件里修改 PHP-FPM 的版本，在 Nginx 安装目录下的 sites-available 文件中找到对应的配置文件，配置文件中 location 块下面的 location ~ \.php$ 里的内容就是你对 PHP 相关的配置

   sudo nano /etc/nginx/sites-available/your.conf

找到 fastcgi_pass 配置项，一般长这样

   fastcgi_pass unix:/run/php/php7.4-fpm.sock;

然后你需要将旧版本替换为新版本，修改成下面的样子

   fastcgi_pass unix:/run/php/php8.1-fpm.sock;

测试你的配置文件，并重启 Nginx

   sudo nginx -t
   sudo service nginx restart

### 结论
现在你学会了如何在 Ubuntu 上安装并配置 PHP 8，谢谢你的观看，如果你遇到了任何问题，可以在下面评论区讨论

本文中的所有译文仅用于学习和交流目的，转载请务必注明文章译者、出处、和本文链接
我们的翻译工作遵照 CC 协议，如果我们的工作有侵犯到您的权益，请及时联系我们。

原文地址：https://php.watch/articles/php-8.0-insta...

译文地址：https://learnku.com/php/t/51997

————————————————  
原文作者：Summer  
转自链接：https://learnku.com/php/t/51997  
版权声明：著作权归作者所有。商业转载请联系作者获得授权，非商业转载请保留以上作者信息和原文链接。  
