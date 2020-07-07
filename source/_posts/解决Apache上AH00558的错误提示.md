---
title: 解决Ubuntu上AH00558的错误提示
date: 2020-04-29 22:03:01
tags:
- Ubuntu
- Apache
- Bug
categories: Operations
---

*这篇博客将介绍如何解决Ubuntu上AH00558的错误提示。*

----------------------------------------

有时候当重启服务器的时候会遇到AH00558的错误：

        AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message

这条错误提示是因为ServerName并没有设置为全局变量，解决方法为：
- 打开`apache2.conf`文件：

        sudo nano /etc/apache2/apache2.conf

- 在合适的位置添加以下代码：

        ServerName localhost

- 重启即可解决该问题。

<!-- more -->

# **Reference**
[1] B. Boucheron, “How To Create a Self-Signed SSL Cert for Apache in Ubuntu 18.04,” *DigitalOcean*, 05-Jul-2018. [Online]. Available: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04.

[2] A. Clemence, “‘Could not reliably determine the server's fully qualified domain name, using 127.0.1.1 for ServerName’ Error on Ubuntu,” *DEV Community*, 11-Jan-2020. [Online]. Available: https://dev.to/ayekpleclemence/could-not-reliably-determine-the-server-s-fully-qualified-domain-name-using-127-0-1-1-for-servername-error-on-ubuntu-4gc9.