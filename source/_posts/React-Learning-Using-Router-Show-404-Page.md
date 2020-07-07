---
title: 'React Learning: Using Router Show 404 Page'
date: 2020-06-08 23:47:54
tags:
- React
- Ubuntu
- Apache
- Bug
categories:
- [React, Notes]
---

*This bug happened when refreshing the page because the server is not configured.*

----------------------------------------

# Solution:
1. ## Enabling mod_rewrite
    enable mod_rewrite:

        sudo a2enmod rewrite

    then restart Apache:

        sudo systemctl restart apache2

2. ## Setting Up .htaccess
    first add rewrite rules:

        sudo nano /etc/apache2/sites-available/000-default.conf

    or if you have https configured:

        sudo nano /etc/apache2/sites-available/default-ssl.conf

    and add the following new block(port is 80 or 443):

        <VirtualHost *:80>
            <Directory /var/www/html>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride All
                Require all granted
            </Directory>

            . . .
        </VirtualHost>

    finally restart Apache:

        sudo systemctl restart apache2

3. ## Creating .htaccess
    Create the following .htaccess file in the root of web project:
        Options -MultiViews
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^ index.html [QSA,L]

<!-- more -->

# Refernce:
https://stackoverflow.com/questions/51357947/react-app-on-server-while-refreshing-the-page-shows-404-page