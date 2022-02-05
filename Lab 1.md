Первым этапом скачиваем и устанавливаем сервер Apache24. Для этого необходимо в командной строке от имени администратора перейти в папку установки 
```
C:\Apache24\bin
```
И выполнить команду установки 
```
>httpd.exe -k install
``` 
После установки переходим к настройке сервера. Для этого открываем файл *hhtpd.conf*.

С помощью поиска файла ищем строку с закомментированным кодом: 
```
#ServerName www.example.com:80
```

Меняем название сервера, не меняя при этом порт. Получаем следующую строку: 
```
ServerName www.aneverova.com:80
```

В этом же файле необходимо указать путь к файлу *httpd-vhosts.conf*. 
Для этого необходимо раскомментировать строку:
```
#Include conf/extra/httpd-vhosts.conf
```

Далее необходимо отредактировать этот файл. Меняем
```
DocumentRoot "${SRVROOT}/docs/dummy-host.example.com"
ServerName dummy-host.example.com
ServerAlias dummy-host.example.com
```
на
```
DocumentRoot "${SRVROOT}/htdocs"
ServerName www.aneverova.com
ServerAlias www.aneverova.com
```
Чтобы завершить настройку сервера необходимо указать локальный хост в системном файле *hosts*. 
Для этого добавляем строку:
```
127.0.0.1  www.aneverova.com
```
Поменяем стандартный код в документе *index.html* на свой.
После этого можем в браузере перейти по адресу www.aneverova.com. 
Результат выглядит следующим образом:

<img src="https://res.cloudinary.com/dlrmdokvi/image/upload/v1644091468/picture1_wbq2k5.png" />
 
Добавим в папку *htdocs* страницы с кодом 403 и 404 (*403.html* и *404.html*).
Создадим файл *.htaccess*, в котором укажем следующие инструкции:
```
ErrorDocument 404 /404.html
ErrorDocument 403 /403.html
```

В файл *httpd.conf* добавим следующий код:
```
<Location "/">
    Options +IncludesNoExec -ExecCGI    
</Location>
```
Далее с помощью поиска по документу меняем все *AllowOverride none* на *AllowOverride all*. Это позволит переопределить стандартные файлы с ошибками на те, которые были созданы ранее в папке *htdocs*. 
Перезапустим сервер с помощью команды 
```
httpd.exe -k restart
```
Результат перенаправления на страницу с ошибкой 403 выглядит так:

<img src="https://res.cloudinary.com/dlrmdokvi/image/upload/v1644091469/picture2_ouukch.png" />
 
Результат перенаправления на страницу с ошибкой 404 выглядит так:

<img src="https://res.cloudinary.com/dlrmdokvi/image/upload/v1644091469/picture3_pjwgoa.png" />
 





