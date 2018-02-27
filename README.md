# Install Nginx
```ubuntu
1- sudo apt-get update

2- sudo apt-get install nginx

3- systemctl status nginx (optional)

4- sudo systemctl start nginx

5- sudo systemctl enable nginx (to start nginx on boot)
```
# Install PHP (7.1)
```ubuntu
1- sudo apt-get install software-properties-common

2- sudo add-apt-repository ppa:ondrej/php

3- sudo apt-get update

4- sudo apt-get install php7.1

5- sudo apt-get install php7.1-cli php7.1-json php7.1-mysql php7.1-xml php7.1-mbstring php7.1-mcrypt php7.1-zip php7.1-fpm composer unzip
```
# Install Mysql
```
1- sudo apt-get install mysql-server

2- mysql_secure_installation (optional)
```
# Remove apache (If you have installed it before - optional)
```
1- sudo service apache2 stop

2- sudo apt-get purge apache2 apache2-utils apache2.2-bin apache2-common

3- sudo apt-get autoremove

4- whereis apache2

5- sudo rm -rf /etc/apache2  
```

### Configurations:
##### PHP

```
1- sudo nano /etc/php/7.1/cli/php.ini

2- cgi.fix_pathinfo=0 (find cgi.fix_pathinfo and change from 1 to 0)

3- sudo systemctl restart php7.1-fpm.service
```
##### Nginx

```
sudo vim /etc/nginx/sites-available/website.com
```

```nginx
server {

        listen 8000; # on which port you want to serve API

        server_name website.com; #website or ip address

        root /var/www/website/API/public;

        index index.php;

        location / {

                try_files $uri $uri/ /index.php?$query_string;

        }

        add_header X-Frame-Options "SAMEORIGIN";

        add_header X-XSS-Protection "1; mode=block";

        add_header X-Content-Type-Options "nosniff";

        location = /favicon.ico { access_log off; log_not_found off; }

        location = /robots.txt  { access_log off; log_not_found off; }

        error_page 404 /index.php;

        location ~ \.php$ {

            fastcgi_pass unix:/run/php/php7.1-fpm.sock;

            include snippets/fastcgi-php.conf;

            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

        }

        location ~ /\.ht {

                deny all;

        }

}
```
```nginx
server {

        listen 80;

        server_name website.com;

        root /var/www/website/WEB;

        index index.html;

        location / {

               try_files $uri $uri/ /index.html;

        }

        location ~ /\.ht {

                deny all;

        }

}
```
#### Add file permision to laravel
```
cd /var/www/website/API
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache
```
#### Symlink nginx config 
```
sudo ln -s /etc/nginx/sites-available/website.com /etc/nginx/sites-enabled/website.com
```
#### Restart services
```
sudo nginx -t

sudo systemctl restart nginx.service

sudo systemctl enable nginx.service

sudo systemctl enable php7.1-fpm.service
```
