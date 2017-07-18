# Настройка среды на MAC

1. Установка homebrew - https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions 
2. Настройка конфигурации apache2
- Открыть файл конфигурации
```
open -e /usr/local/etc/apache2/2.4/httpd.conf
```
- Указать в нем
```
User programmer
Group programmer
```
- Задать DocumentRoot - папка в которой будут все проекты
```
DocumentRoot "/Users/lb/workspase/projects"
<Directory "/Users/lb/workspase/projects">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```
- Подключить mod_rewrite раскоментировав :
```
LoadModule rewrite_module libexec/mod_rewrite.so
```
- Изменить serverName на  localhost
- Добавить :
```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
- Подключить папку с конфигами хостов. Путь к любой папке в которой планируется хранить настройки хостов
```
Include /Users/lb/etc/apache2/vhosts/*.conf
```
3. Настройка виртуального хоста
- в конфиге апача раскомментировать строки:
```
LoadModule vhost_alias_module libexec/apache2/mod_vhost_alias.so
```
- Создать конфиграционный файл для виртуального хоста. Пример:
```
<VirtualHost *:80>

    ServerName projects

    VirtualDocumentRoot /Users/lb/workspase/projects/%1/www/%2/web

    LogFormat "%v %l %u %t \"%r\" %>s %b" comonvhost
    CustomLog /Users/lb/Desktop/workspase/logs/access_log comonvhost
    ErrorLog  /Users/lb/Desktop/workspase/logs/error_log    

    <Directory /Users/lb/Desktop/projects/>
        Allowoverride all
        Require all granted
    </Directory>

   <Directory "/Users/lb/workspase/projects/%1/www/%2/web»>
       DirectoryIndex index.php
   </Directory>
</VirtualHost>
```
4. Настроить dns маски - https://gist.github.com/ogrrd/5831371
5. Пример установки модулей php:
```
brew install php55-intl
```
6. Установка mysql 
```
https://gist.github.com/nrollr/a8d156206fa1e53c6cd6
```
7. Настройка xdebug
```
brew install php55-xdebug
```
```
open -e  /usr/local/etc/php/5.5/conf.d/ext-xdebug.ini
```
Добавить следующее :
```
xdebug.remote_enable=1
xdebug.remote_port=9000
xdebug.idekey=PHPSTORM
xdebug.remote_autostart=0
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp
```
8. Установка composer
```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
9. Установка node.js и npm - https://github.com/creationix/nvm
```
brew install nvm
npm i 
```
10. SSH ключи - https://help.github.com/articles/connecting-to-github-with-ssh/

