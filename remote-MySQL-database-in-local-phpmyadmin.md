### How to access remote MySQL database in local phpMyAdmin
---
### Remote Process
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
---
### Localhost process
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
- then, we have to extract the zip file and rename the filename like phpmyadmin
- Then, use vsftpd to push the phpmyadmin file to the apache2 document root (/var/www/html)
  

_`configure the remote database details in my local phpmyadmin file`_
```sh
# sudo vim /etc/phpmyadmin/config.inc.php

$i++;
$cfg['Servers'][$i]['host']          = 'ec2-3-110-186-140.ap-south-1.compute.amazonaws.com'; # Public IPv4 DNS Name
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



![image](https://github.com/fourtimes/php/assets/91359308/f09a578f-a386-4a64-9d13-f91a2add4f69)

![image](https://github.com/fourtimes/php/assets/91359308/e509fb53-5de5-42f9-bd12-5feb05c32baf)

**Check the remote db details match into the phpmyadmin - if it is matched the configuration correctly configured.**
![image](https://github.com/fourtimes/php/assets/91359308/a041d6b4-8201-427b-90b9-4503bf91d157)


