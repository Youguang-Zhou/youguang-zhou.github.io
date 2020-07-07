---
title: 在Ubuntu 18.04版本上正确配置防火墙
date: 2020-04-29 17:21:09
tags:
- Ubuntu
categories: Operations
---

*这篇博客将介绍如何在Ubuntu 18.04版本上配置防火墙。*

----------------------------------------

# **UFW介绍**
UFW是Uncomplicated Firewall的缩写，是Ubuntu系统上默认的防火墙组件。常用的命令有：
- 打开防火墙：

        ufw enable

- 关闭防火墙：

        ufw disable

- 显示防火墙状态：

        ufw status

- 查看防火墙详细状态：

        ufw status verbose

<!-- more -->

# **使用IPv6**
打开配置文件：

        sudo nano /etc/default/ufw

确保IPV6的值为yes。

# **重置防火墙**
默认情况下，防火墙的设置是拒绝所有传入连接并允许所有传出连接。

想要重置防火墙为默认，可使用命令：

        sudo ufw default deny incoming
        sudo ufw default allow outgoing

# **允许SSH连接**
SSH监听的端口是22，可通过以下命令来允许SSH连接：

        sudo ufw allow ssh

或：

        sudo ufw allow 22

# **允许HTTP/HTTPS连接**
通常情况下，可以直接通过允许Apache Full（一种Apache的应用程序配置文件）来配置HTTP/HTTPS端口。

查看UFW是否具有Apache Full：

        sudo ufw app list

如果在`Available applications`里显示有Apache Full，则通过以下命令查看Apache Full的详情：

        sudo ufw app info "Apache Full"

然后允许HTTP/HTTPS的连接：

        sudo ufw allow in "Apache Full"

*如果没有Apache Full，则可以通过允许HTTP和HTTPS各自监听的端口配置连接：*

- HTTP监听的端口为80:

        sudo ufw allow 80

- HTTPS监听的端口为443:

        sudo ufw allow 443

# **删除规则（Rules）**
以删除HTTP为例：

        sudo ufw delete allow http

或：

        sudo ufw delete allow 80

# **防火墙是如何知道HTTP的端口的呢？**
防火墙是如何知道命令：

        sudo ufw allow http

里http的端口就是80的呢？

<br>

是因为在`/etc/services`文件里已经把http的端口申明为80了。

# **Reference**
[1] B. Boucheron, “How To Set Up a Firewall with UFW on Ubuntu 18.04,” *DigitalOcean*, 05-Jul-2018. [Online]. Available: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04.