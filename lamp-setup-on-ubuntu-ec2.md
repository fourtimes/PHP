LAMP Setup for ubuntu:
---------------------------------
```
LAMP - Linux, Apache2, MySQL, PHP
```
php installation
---------------------

```sh
sudo apt install php libapache2-mod-php php-mcrypt php-mysql
```
---
vsftpd
-------
_Firstly , we have to implement the vsftpd installation and configuration. we need to follow the below command_

```sh
# Install the vsftpd packages
sudo apt update 
sudo apt install vsftpd -y

# create the new user
sudo adduser ashli

# Create a directory for user and document root
sudo mkdir -p /etc/vsftpd/users /var/www/html/demo

# give the owner permission of new user
sudo chown -R ashli:ashli /var/www/html/demo

# give the local root
echo "local_root=/var/www/html/demo" | sudo tee -a /etc/vsftpd/users/ashli

# restrict the users at root level
echo "ashli" | sudo tee /etc/vsftpd.chroot_list
```
_Add the configuration file for vstfpd_
```sh
# sudo vim /etc/vsftpd.conf
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_file=/var/log/vsftpd.log
xferlog_std_format=NO
idle_session_timeout=600
data_connection_timeout=120
ascii_upload_enable=YES
ascii_download_enable=YES
ftpd_banner=Welcome to RADIANT FTP service.
chroot_local_user=YES
chroot_list_enable=NO
user_config_dir=/etc/vsftpd/users
allow_writeable_chroot=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
chroot_list_file=/etc/vsftpd.chroot_list
userlist_enable=YES
userlist_file=/etc/vsftpd.chroot_list
userlist_deny=NO
pasv_enable=YES
pasv_min_port=64000
pasv_max_port=64321
port_enable=YES
pasv_address=3.6.88.147 #aws-instance-ip
pasv_addr_resolve=NO
```
_After creating the configuration file, we need to start the service and check the status of the service_
```sh
#  start the vsftpd service
sudo systemctl start vsftpd

#  status of vsftpd service
sudo systemctl status vsftpd

# verify the configuration
sudo ss -tulpn | grep vsftpd

```
---

Apache2 configuration
----------------------------
_apache2 installation:_
```sh
sudo apt update
sudo apt install apache2 -y

sudo service apache2 start
sudo service apache2 enable
sudo service apache2 status
```
_After installing the apche2 package, the we need to create a document root._

```sh
sudo mkdir -p /etc/apache2/sites-available/demo
cd /etc/apache2/sites-available/demo
pwd
```
_Then we need to create a configuration file of apache2_
```sh
<VirtualHost *:80>

        ServerAdmin webmaster@januo.io

        # domain name - if we use a domain, enable the below line and change the domain name as per your wish. without domain, we have to use pulic ip for accessing.
        # Servername  (domain-name)
        # mention the document root 
        DocumentRoot /var/www/html/demo

        ErrorLog ${APACHE_LOG_DIR}/api.januo.io.error.log
        CustomLog ${APACHE_LOG_DIR}/api.januo.io.access.log combined
        
</VirtualHost>
```

MySQL
----------
```sh
sudo apt install mysql -y
sudo service mysql start
sudo service mysql enable
sudo service mysql status
```

phpmyadmin configuration:
-------------------------

- firstly, we have to download a phpmyadmin configuation file in my local using this link - https://www.phpmyadmin.net/
- then, we have to extract the zip file and rename the filename like `phpmyadmin`
- Then, use `vsftpd` to push the `phpmyadmin` file to the `apache2 document root `(/var/www/html/demo).


OUTPUT:
-----------

**vsftpd** - _push to index.php file and phpmyadmin file_ 
![Image](https://github.com/januo-org/proof-of-concepts/assets/91359308/2b23db47-045c-43c6-9c9c-71dee90e51d5)

**php demo page** - _http://(your-public-ip)/index.php_
![Image](https://github.com/januo-org/proof-of-concepts/assets/91359308/61c0cf5e-39d4-459d-95ad-94a7756509b2)

**phpmyadmin** - _http://(your-public-ip)/phpmyadmin_
![Image](https://github.com/januo-org/proof-of-concepts/assets/91359308/85435b84-3869-4e52-a8da-2caa26ee5200)
---

## Note:

_Suppose if we need to install the LAMP using single command. please use the below command_
```sh
sudo apt update
sudo apt install apache2 php php-gd php-zip php-mysql mysql-server -y
```

reference - https://www.youtube.com/watch?v=kHZKI27jFnc&t=399s
