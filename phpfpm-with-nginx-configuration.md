# PHP-FPM configuration with `socket` using nginx

_Install nginx and php-fpm_
```sh
# nginx
sudo apt update
sudo apt install nginx
```
```sh
# php-fpm
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.1-fpm
sudo systemctl start php8.1-fpm
sudo systemctl enable php8.1-fpm
```
_We should mention the configuration in the below content_
```sh
cd /etc/php/8.1/fpm/pool.d
sudo vim www.conf

# add the below content in the configuration file
listen = /run/php/php8.1-fpm.sock
```
_Restart the php-fpm service_
```sh
sudo systemctl restart php8.1-fpm
```
_Nginx configuration file shoule be like this the below format_
```conf
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	index index.html index.php;

	server_name _;

	location / {
		try_files $uri $uri/ =404;
	}

	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
	  fastcgi_pass unix:/run/php/php8.1-fpm.sock;
	}

	location ~ /\.ht {
		deny all;
	}
}
```
_Reload the nginx configuration_
```sh
sudo systemctl reload nginx.service
```
_Create the document root file_
```sh
cd /var/www/html
sudo rm index.nginx-debian.html
sudo mkdir index.php
echo "<?php echo phpinfo();?>" > index.php
```
_Load the content in web browser_
```sh
http://server_domain_or_IP
```
<img width="1460" alt="image" src="https://github.com/fourtimes/PHP/assets/91359308/ba21b1d9-e5b8-414e-b221-c3b247f4a240">

# PHP-FPM configuration with `port` using nginx

_Install nginx and php-fpm_
```sh
# nginx
sudo apt update
sudo apt install nginx
```
```sh
# php-fpm
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.1-fpm
sudo systemctl start php8.1-fpm
sudo systemctl enable php8.1-fpm
```
_We should mention the configuration in the below content_
```sh
cd /etc/php/8.1/fpm/pool.d
sudo vim www.conf

# add the below content in the configuration file
listen = 127.0.0.1:9000
```
_Restart the php-fpm service_
```sh
sudo systemctl restart php8.1-fpm
```
_Listen the port_
```sh
sudo ss -tulpn | grep 9000
```
_Nginx configuration file shoule be like this the below format_
```conf
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	index index.html index.php;

	server_name _;

	location / {
		try_files $uri $uri/ =404;
	}

	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass 127.0.0.1:9000;
	}

	location ~ /\.ht {
		deny all;
	}
}
```
_Reload the nginx configuration_
```sh
sudo systemctl reload nginx.service
```
_Create the document root file_
```sh
cd /var/www/html
sudo rm index.nginx-debian.html
sudo mkdir index.php
echo "<?php echo phpinfo();?>" > index.php
```
_Load the content in web browser_
```sh
http://server_domain_or_IP
```
<img width="1456" alt="image" src="https://github.com/fourtimes/PHP/assets/91359308/1e98af1f-2683-4ff0-b271-bbfdcf779db4">
