# this error comes from phpmyadmin page
![image](https://github.com/fourtimes/php/assets/91359308/595bf348-c914-436a-8bca-5c9ed051cdc2)

**Solution:**

`sudo nano /usr/share/phpmyadmin/libraries/sql.lib.php`
```sh
(count($analyzed_sql_results['select_expr'] == 1)
((count($analyzed_sql_results['select_expr']) == 1)
```

`sudo nano /etc/phpmyadmin/config.inc.php`
```sh
$cfg['SendErrorReports'] = 'never';
```
reference - https://www.nicesnippets.com/blog/error-solved-some-errors-have-been-detected-on-the-server-please-look-at-the-bottom-of-this-window-in-phpmyadmin
