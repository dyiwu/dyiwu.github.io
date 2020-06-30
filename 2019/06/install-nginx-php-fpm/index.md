# Install Nginx and PHP-FPM on Linux Mint 19

## _Install Nginx_

Download the Nginx signing key from Nginx official site, and them add it into system.
<!--more-->
```
$ wget http://nginx.org/keys/nginx_signing.key | sudo apt-key add -
```

Add Nginx repository to the system.
```
$ echo "deb [arch=amd64] http://nginx.org/packages/ubuntu/ bionic nginx" | sudo tee /etc/apt/sources.list.d/nginx.list

$ echo "deb-src [arch=amd64] http://nginx.org/packages/ubuntu/ bionic nginx" | sudo tee -a /etc/apt/sources.list.d/nginx.list
```

Update repository index.
```
$ sudo apt update
```

Install Nginx
```
$ sudo apt install -y nginx
```

Set Site’s Root DirectoryPermalink
```
$ cd /etc/nginx/sites-available
$ sudo vi default

-       root /var/www/html;
+       root /var/www/public;
```

Ownership and permissions for nginx local webserver
```
$ sudo vi /etc/nginx/nginx.conf

$ grep user /etc/nginx/nginx.conf
-	user www-data;
+	user dyiwu cosmos;
```

## _Install PHP-FPM_
PHP-FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation.

Use apt to install php-fpm
```
$ sudo apt install -y php-fpm php-mysql php-cli
```

Edit the respective php.ini depending on the PHP version you have installed on the system.
```
$ sudo vi /etc/php/7.2/fpm/php.ini

-	cgi.fix_pathinfo=1
+	cgi.fix_pathinfo=0
```

Edit the /etc/php/7.2/fpm/pool.d/www.conf file, change _user_, _group_, _listen_,_listen.owner_ and _listen.group_.
```
$ sudo vi /etc/php/7.2/fpm/pool.d/www.conf
-	user = www-data
-	group = www-data
+	user = dyiwu
+	group = cosmos
 
-	listen = 127.0.0.1:9000
+	listen = /run/php/php7.2-fpm.sock
 
-	listen.owner = www-data
-	listen.group = www-data
+	listen.owner = dyiwu
+	listen.group = cosmos
```

Fix for Nginx PID problem in Ubuntu 16.04
```
$ sudo systemctl stop nginx
$ sudo mkdir /etc/systemd/system/nginx.service.d

# build the file in root where no sudo is needed
$ printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" > override.conf

$ sudo cp override.conf /etc/systemd/system/nginx.service.d/override.conf
$ sudo systemctl daemon-reload
$ sudo systemctl start nginx
```

Correct the permission of directory files.
```
$ sudo chown -R dyiwu:cosmos /var/www/public
$ ls -ld /var/www/public
```

## Test Nginx
Restart nginx and php-fpm services.
```
$ sudo systemctl restart nginx
$ sudo systemctl status nginx
$ ps aux | grep nginx

$ sudo systemctl restart php7.2-fpm
$ sudo systemctl status php7.2-fpm
$ ps aux | grep php-fpm
```

Check the version of nginx
```
$ sudo nginx -v
nginx version: nginx/1.16.0
$ sudo nginx -V

# create the index.php page to test PHP.
$ cat /var/www/public/index.php
<?php
phpinfo();
?>

# Open a web browser to http://127.0.0.1/, the phpinfo() page should be shown out.
```
## Deployment
Deploy the hugo pages to local public directory by rsync
```
$ cat rsync-local.sh 
#!/bin/bash
cd ~/Hugo/Sites/chaos
hugo
rsync -avh --delete --progress public/ /var/www/public/
```

Deploy the hugo pages to Raspberry Pi by rsync
```
$ cat rsync-raspberry.sh 
#!/bin/bash
cd ~/Hugo/Sites/chaos
hugo
rsync -avh --delete --progress public/ 192.168.1.164:/var/www/public/
```
---

## Reference

  - https://www.itzgeek.com/how-tos/linux/linux-mint-how-tos/how-to-install-linux-nginx-mariadb-php-lemp-stack-on-linux-mint-19.html
  - [Ngix/PHP-FPM configuration optimization](https://blog.gtwang.org/linux/nginx-php-fpm-configuration-optimization/)
  - [Nginx official documentation](http://nginx.org/en/docs/)
  - [nginx-announce mailing list](http://nginx.org/en/support.html)
  - [Commercial subscriptions for nginx](http://nginx.com/products/)


