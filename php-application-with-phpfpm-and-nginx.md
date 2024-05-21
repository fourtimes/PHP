# Application with PHP-FPM using NGINX
_1. Install php-fpm_
```sh
# Install PHP 
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.1-fpm
sudo systemctl start php8.1-fpm
sudo systemctl enable php8.1-fpm
```
_2. Install nginx_
```sh
# Install nginx
sudo apt update
sudo apt install nginx
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```
_3. add the custom nginx configuration_
```conf
# sudo vim /etc/nginx/site-available/nginx.conf
server {
    listen       80;
    server_name  nginx.fourcodes.net;

    return 301 https://$host$request_uri;
}

server {
    listen               443 ssl;
    server_name          nginx.fourcodes.net;

    ssl_certificate      /etc/nginx/ssl-certificate/certificate.crt;
    ssl_certificate_key  /etc/nginx/ssl-certificate/private.key;
    
    root   /var/www/html;
    index  index.html index.php;

    access_log   	 /var/log/nginx/access.log;
    error_log    	 /var/log/nginx/error.log;

    location / {
        #try_files $uri $uri/ /index.php?$query_string;
	try_files $uri $uri/ =404;
    }
    
    location ~ \.php$ {
	    include snippets/fastcgi-php.conf;
	    fastcgi_pass unix:/run/php/php8.1-fpm.sock;
	    #fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            #include fastcgi_params;
    }

    location ~ /\.ht {
            deny all;
    }

}
```
_4. Link the configuration file from sites-available to sites-enabled:_
```sh
sudo ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/
```
_5. Test Nginx Configuration:_
```sh
sudo nginx -t
```
_6. Restart Nginx:_
```sh
sudo systemctl restart nginx
```
_7. Install mysql packages and configure_
```sh
# Install the mysql packages
sudo apt update
sudo apt install mysql-server -y
sudo service mysql start
sudo service mysql status

# create the mysql user to connect the phpmy admin
sudo mysql
CREATE USER 'ashli'@'%' IDENTIFIED BY 'Password@123';
GRANT ALL PRIVILEGES ON *.* to 'ashli'@'%';
FLUSH PRIVILEGES;
```
_8. Put the source code to the document root. And change the required changes to that code._

_9. Check the domain in the browser._


