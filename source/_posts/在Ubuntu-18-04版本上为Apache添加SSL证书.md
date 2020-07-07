---
title: 在Ubuntu-18-04版本上为Apache添加SSL证书
date: 2020-04-29 19:52:49
tags:
- Ubuntu
- Apache
categories: Operations
---

*这篇博客将介绍如何在Ubuntu 18.04版本上为Apache添加SSL证书以及将HTTP的访问重定向到HTTPS。*

----------------------------------------

# **SSL介绍**
SSL是Secure Sockets Layer的缩写，是一种用于安全加密通信的网络协议。

<!-- more -->

# **为Apache添加强加密设置**
首先在`/etc/apache2/conf-available`目录下新建一个名为`ssl-params.conf`的文件：

        sudo nano /etc/apache2/conf-available/ssl-params.conf

并在该文件里添加相应配置：

        SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
        SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
        SSLHonorCipherOrder On
        # Disable preloading HSTS for now.  You can use the commented out header line that includes
        # the "preload" directive if you understand the implications.
        # Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
        Header always set X-Frame-Options DENY
        Header always set X-Content-Type-Options nosniff
        # Requires Apache >= 2.4
        SSLCompression off
        SSLUseStapling on
        SSLStaplingCache "shmcb:logs/stapling-cache(150000)"
        # Requires Apache >= 2.4.11
        SSLSessionTickets Off

然后保存。

# **修改默认的Apache SSL虚拟主机文件**
*如果你有自己的虚拟主机配置文件，只需把下面的`default`替换成相应的文件名。*

- 首先备份原来的配置文件：

        sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak

- 然后打开并配置新的文件：

        sudo nano /etc/apache2/sites-available/default-ssl.conf

- 修改并保存相应配置：

        <IfModule mod_ssl.c>
            <VirtualHost _default_:443>

                ServerName server_domain_or_IP

                DocumentRoot /var/www/html

                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined

                SSLEngine on

                SSLCertificateFile      /etc/ssl/certs/domain.pem
                SSLCertificateKeyFile   /etc/ssl/private/domain.key

                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

            </VirtualHost>
        </IfModule>

    *1. 其中`SSLCertificateFile`和`SSLCertificateKeyFile`是相应的SSL证书和密钥。*
    *2. 如果是为虚拟主机配置SSL，则需要修改`DocumentRoot`为该虚拟主机的网页文件目录。*

# **将HTTP重定向到HTTPS**
*同样，对于虚拟主机，需要把下面的`default`替换成相应的文件名。*

首先打开相应配置文件：

        sudo nano /etc/apache2/sites-available/000-default.conf

并添加重定向指令`Redirect`：

        <VirtualHost *:80>
                . . .

                Redirect "/" "https://your_domain_or_IP/"

                . . .
        </VirtualHost>

最后保存。

如果想设置成**永久重定向**，则需要将命令`Redirect`改为`Redirect permanent`。

# **调整防火墙使其接受HTTPS连接**

        sudo ufw allow 'Apache Full'

# **使所有配置生效**
- 使`mod_ssl``和mod_headers`生效：

        sudo a2enmod ssl
        sudo a2enmod headers

- 使配置SSL的虚拟主机生效：
*视个人情况更改default文件名*

        sudo a2ensite default-ssl

- 使`ssl-params.conf`文件生效：

        sudo a2enconf ssl-params

- 检查有无句法错误：

        sudo apache2ctl configtest

- 最后重启Apache：

        sudo systemctl restart apache2

# **测试**
以HTTP或HTTPS访问该域名即可。

# **Reference**
[1] B. Boucheron, “How To Create a Self-Signed SSL Cert for Apache in Ubuntu 18.04,” *DigitalOcean*, 05-Jul-2018. [Online]. Available: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04.