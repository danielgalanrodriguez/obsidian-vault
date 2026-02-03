https://blog.devops.dev/how-to-install-apache-mysql-phpmyadmin-server-on-macos-ventura-c65e68387776


# allow login without password
Modify the file `config.inc.php` and set the `AllowNoPassword` to `true` :
phpMyAdmin/config.inc.php
`$cfg['Servers'][$i]['AllowNoPassword'] = true;`

# Get user name
`mariadb -e "SELECT USER()"`
