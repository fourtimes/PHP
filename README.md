# phpmyadmin

If you want to reinstall phpMyAdmin on Ubuntu, run:

```sh
sudo apt-get update
sudo apt-get install phpmyadmin
```

un-installation

```sh
sudo apt-get remove phpmyadmin -y
sudo apt-get purge phpmyadmin -y
sudo apt-get autoremove -y
```

Disable phpMyAdmin without uninstalling

```sh
sudo a2disconf phpmyadmin
sudo systemctl reload apache2
```

To re enable phpMyAdmin, run:

```sh
sudo a2enconf phpmyadmin
sudo systemctl reload apache2
```


Password@1234567890
Sharik@12345
