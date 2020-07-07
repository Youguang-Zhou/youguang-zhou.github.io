---
title: 在Ubuntu 18.04版本上安装LAMP
date: 2020-04-29 04:50:51
tags:
- Ubuntu
- Apache
categories: Operations
---

*这篇博客将介绍如何搭建基于LAMP的web服务器。*

----------------------------------------

# **什么是LAMP？**
LAMP是运用了Linux，Apache，MySQL，PHP技术的统称，被广泛用于搭建web服务器。

在Ubuntu系统上，可以通过apt（一种Ubuntu的包管理工具）来安装所需要的软件。

<!-- more -->

# **Apache**
- 更新apt并安装Apache：

        sudo apt update
        sudo apt install apache2


# **MySQL**
- 安装MySQL服务器：

        sudo apt install mysql-server

- 安装MySQL安全脚本：

        sudo mysql_secure_installation


# **PHP**
- 安装PHP及其相关mysql包：

        sudo apt install php libapache2-mod-php php-mysql

- 查看服务器是否支持index.php界面：

        sudo nano /etc/apache2/mods-enabled/dir.conf

- 重启服务器：

        sudo systemctl restart apache2


# **配置虚拟主机（Virtual Hosts）**
如果想在一个服务器上设置多个域名的话，推荐使用设置虚拟主机来实现。

默认情况下，服务器会返回储存在`/var/www/html`文件夹里的网页文件，但是对于多个域名的话，显然对每一个域名都各配置一个文件夹来储存他们各自的文件是一个比较合理的办法。

### 以[67373upup.com](https://www.67373upup.com)为例：
- 首先建立对应目录：

        sudo mkdir /var/www/67373upup.com

- 接下来使用$USER环境变量分配目录的所有权：

        sudo chown -R $USER:$USER /var/www/67373upup.com

- 使用chmod 755命令设置权限：

        sudo chmod -R 755 /var/www/67373upup.com

    *chmod 755 (chmod a+rwx,g-w,o-w) means:*

    |         |  Read | Write | Execute |
    |---------|-------|-------|---------|
    | User    |   ☑️   |   ☑️   |    ☑️    |
    | Group   |   ☑️   |   🚫  |    ☑️    |
    | Others  |   ☑️   |   🚫  |    ☑️    |

- 建立相关的配置文件：

        sudo nano /etc/apache2/sites-available/67373upup.com.conf

    并且填入对应信息：

        <VirtualHost *:80>
            ServerName 67373upup.com
            ServerAlias www.67373upup.com
            DocumentRoot /var/www/67373upup.com
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>

- 然后使这个文件生效（enable）：
        
        sudo a2ensite 67373upup.com.conf
    
    并且使默认配置失效（disable）：

        sudo a2dissite 000-default.conf
    
- 检查是否有错误：

        sudo apache2ctl configtest
    
    如果显示`Syntax OK`则表示没有错误。

- 最后重启：

        sudo systemctl restart apache2

# **测试：**
访问http://www.67373upup.com

# **Reference**
[1] M. Drake, “How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 18.04,” *DigitalOcean*, 27-Apr-2018. [Online]. Available: https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04.