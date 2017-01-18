# Установка LAMP-SERVER, настройка среды LINUX для битрикс

1. sudo tasksel install lamp-server
2. sudo apt-get install phpmyadmin
3. sudo a2enmod vhost_alias - включаем модуль для создания виртуального хоста
4. sudo a2enmod rewrite - Включаем  mod_rewrite.
5. sudo apt-get install libapache2-mpm-itk
6. Настройка хоста
```
<VirtualHost *:80>

    ServerName workspace

    AssignUserID programmer programmer

    VirtualDocumentRoot /home/programmer/workspace/vhosts/%2/%1/www

    LogFormat "%v %l %u %t \"%r\" %>s %b" comonvhost
    CustomLog /home/programmer/workspace/vhosts/logs/access_log comonvhost
    ErrorLog  /home/programmer/workspace/vhosts/logs/error_log    

    <Directory /home/programmer/workspace/vhosts/>
        Allowoverride all
        Require all granted
    </Directory>

    <Directory /home/programmer/workspace/vhosts/utf8>
        php_admin_value mbstring.func_overload 2
           php_admin_value mbstring.internal_encoding utf-8    
    </Directory>

    <Directory /home/programmer/workspace/vhosts/cp1251>
               php_admin_value mbstring.func_overload 0
       </Directory>
</VirtualHost>

```
project.utf8.dev
/home/programmer/workspace/vhosts/utf8/projectname/www

7. a2ensite *.conf - активация конфигурации
8. sudo apt-get install dnsmasq
9. sudo vim /etc/dnsmasq.conf найти строчку с 127.0.0.1   заменить на address=/dev/127.0.0.1

## Конфигурация mysql 
- в /etc/mysql/conf.d  создать файл bitrix.cnf
- поместить в него следующее
```
[mysqld]
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT
transaction-isolation = READ-COMMITTED
query_cache_size = 128M
query_cache_limit = 2M
sort_buffer_size = 10M
read_rnd_buffer_size = 10M
table_open_cache = 512
innodb_buffer_pool_size = 512M
innodb_buffer_pool_instances = 2
tmp_table_size = 1024M
max_heap_table_size = 1024M
max-connect-errors = 10000
table_definition_cache = 2048
thread_cache_size = 4

```
- sudo systemctl restart mysql.service

## Конфигурация php для битрикса
- в /etc/php/5.6/mods-available создать файл bitrix.ini и поместить 
```

[PHP]
realpath_cache_size=4096K
magic_quotes_gpc=0
max_input_vars=10000
memory_limit=512M
upload_max_filesize=64M
post_max_size=64M
short_open_tag=On

[Date]
date.timezone = 'Europe/Moscow'

```
phpenmod bitrix

sudo apt-get install php5.6-mcrypt
phpenmod mcrypt
sudo apt-get install php-mbstring

## Установка почты
sudo apt-get install postfix


## Если необходимо сменить версию php
```
sudo a2dismod php7.0
a2enmod php5.6
```

## Настройка xdebug
- установить модуль sudo apt-get install php-xdebug
- изменить  /etc/php/5.6/mods-available файл xdebug.ini
- поместить в него 
```
xdebug.remote_enable=1
xdebug.remote_port=9000
xdebug.idekey=PHPSTORM
xdebug.remote_autostart=0
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp

```
Настроить phpstorm ![alt text](../phpstorm_settings_xdebug.jpg)
![alt text](../phpstorm_settings_php.jpg)

Настройки плагина в хроме
![alt text](../phpstorm_settings_xdebug.jpg)

- sudo phpenmod xdebug.ini 
- sudo systemctl status apache2.service
