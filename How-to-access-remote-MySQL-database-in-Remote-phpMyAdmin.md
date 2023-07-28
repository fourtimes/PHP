### `How to access remote MySQL database in Remote phpMyAdmin`
### vsftpd
---
vsftpd
-------
_Firstly , we have to implement the vsftpd installation and configuration. we need to follow the below command_

```sh
# Install the vsftpd packages
sudo apt update 
sudo apt install vsftpd -y

# create the new user
sudo adduser ashli # password - Password@123

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

### phpmyadmin remote server (public)
---
_`Then, we have to install the phpmyadmin in my local`_

```sh
sudo apt update
sudo apt install apache2 php php-gd php-zip php-mysql -y

sudo service apache2 start
sudo service apache2 status
```
_`phpmyadmin configuration:`_

- firstly, we have to download a phpmyadmin configuation file in my local using this link - https://www.phpmyadmin.net/
- then, we have to extract the zip file and rename the filename like `phpmyadmin`
- Then, use vsftpd to push the `phpmyadmin` file to the apache2 document root (`/var/www/html/demo`)


**Output:**
`after the above phpmyadmin setup, we will get this page`
![image](https://github.com/fourtimes/php/assets/91359308/e0fb7f22-b1ab-4e3a-ad2b-0364c8a7e8b6)

### MySQL remote server (private)
---
_`Firstly, create the remote mysql database(public) in aws (ec2)`_
```sh
# install the mysql packages
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
_`Configure MySQL for remote connections`_
```sh
# Log into your MySQL database server and open the configuration file with the command:
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

# Change that line instead of bind-address = 127.0.0.1:
bind-address = 0.0.0.0

# Restart the MySQL service with:
sudo systemctl restart mysql
```

**output:** `after the above mysql setup, we will get this output.`
![image](https://github.com/fourtimes/php/assets/91359308/51d4574e-3323-44a5-86a5-3e318e4c601c)

_configure the remote database details in my remote phpmyadmin file_
```sh
# find the phpmyadmin configuration file
cd /var/www/html/demo/phpmyadmin
ll | grep config
mv config.sample.inc.php config.inc.php
```
_Add the mysql remote configuration in remote phpmyadmin server_
```sh
# sudo vim /var/www/html/demo/phpmyadmin/config.inc.php

$i++;
$cfg['Servers'][$i]['host']          = '172.31.1.142'; # Public IPv4 DNS Name
$cfg['Servers'][$i]['port']          = '3306';
$cfg['Servers'][$i]['socket']        = '';
$cfg['Servers'][$i]['connect_type']  = 'tcp';
$cfg['Servers'][$i]['extension']     = 'mysql';
$cfg['Servers'][$i]['compress']      = FALSE;
$cfg['Servers'][$i]['auth_type']     = 'config';
$cfg['Servers'][$i]['user']          = 'ashli';
$cfg['Servers'][$i]['password']      = 'Password@123';
$cfg['Servers'][$i]['AllowNoPassword'] = TRUE;

```
---

OUTPUT 
-------

![image](https://github.com/fourtimes/php/assets/91359308/ff72a39b-8519-40bb-9eff-6f0224abe92e)

![image](https://github.com/fourtimes/php/assets/91359308/6ee42a18-ebae-47b4-ae03-59cf117f7195)

**Check the remote db details match into the remote phpmyadmin - if it is matched the configuration correctly configured.**
![image](https://github.com/fourtimes/php/assets/91359308/81fdee7d-bbb5-4863-a2f0-79349bcb96dd)

![image](https://github.com/fourtimes/php/assets/91359308/85a313cf-7970-4c57-b72b-6a194527fe7a)



