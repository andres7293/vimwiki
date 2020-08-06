# PHP

## Instalar stack LAMP 

LAMP (Linux Apache Mysql PHP)

### Apache

```bash
pacman -S apache
sudo systemctl start httpd
# Descomentar
LoadModule rewrite_module modules/mod_rewrite.so 
# Comentar --> #LoadModule mpm_event_module modules/mod_mpm_event.so
# Descomentar --> LoadModule mpm_prefork_module modules/mod_mpm_prefork.so

Agregar al final de la lista de LoadModule 
LoadModule php7_module modules/libphp7.so
AddHandler php7-script .php

# Agregar al final de la lista de Includes
Include conf/extra/php7_module.conf
sudo systemctl restart httpd
```

### Mariadb (MySQL)

```bash
pacman -Syu mariadb
## Configurar mariadb
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
sudo systemctl start mariadb
sudo mysql_secure_installation
```

### PHP

```bash
sudo pacman -S php php-cgi php-apache php-sqlite php-pgsql php-odbc php-dblib
```

Configurar apache para ejecutar código php: 

```bash
vim /etc/httpd/conf/httpd.conf
```

Comprobar instalación: 

Crear archivo: /srv/http/test.php

```php
<?php phpinfo(); ?>
```

Abrir en el navegador : http://localhost/test.php
y debería renderizar una página web


#### Composer

Composer es un gestor de dependencias de php

```bash
pacman -S composer
```

#### Laravel

Laravel es un framework de php.
Laravel necesita composer para instalarse

```bash
composer global require laravel/installer
#Añadir al path el ejecutable de laravel. 
export PATH=$PATH:/home/andres/.config/composer/vendor/bin
```
