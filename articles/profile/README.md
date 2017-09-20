# Профилирование

## Xdebug
- Установить xdebug
```
sudo apt install php-xdebug
```
- Изменить файл конфигурации xdebug
```
sudo gedit /etc/php/7.0/mods-available/xdebug.ini
```
```
zend_extension=xdebug.so

xdebug.remote_enable=1
xdebug.remote_port=9000
xdebug.idekey=PHPSTORM
xdebug.remote_autostart=0
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp

xdebug.profiler_enable = 0
xdebug.profiler_enable_trigger = 1
xdebug.profiler_output_dir = /home/lilit/workspace/profiler
xdebug.profiler_output_name = cachegrind.out.%p.%r

```
- Перезагрузить  сервер
- Установить расширение https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc?hl=ru и настроить его согласно настройкам xdebug
- Включить профилирование и запустить проверяемый скрипт
- Открыть файлы профилирования с помощью PhpStorm в меню Tools пункт Analyze Xdebug Profiler Snapshot


## XHProf
[Пример](https://github.com/SmotrovaLilit/xhprof)
