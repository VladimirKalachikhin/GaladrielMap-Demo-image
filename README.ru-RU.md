# GaladrielMap Demo image [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

Demo image представляет собой образ диска виртуальной машины в формате Открытого формата виртуализации .ova

Образ содержит веб-сервер под операционной системой Ubuntu с установленным приложением [GaladrielMap](https://github.com/VladimirKalachikhin/Galadriel-map), и может быть запущен в любой системе виртуальных машин (VirtualBox, VMware, другой).

Поскольку для пафосного GitHub 1.8Gb -- это чрезвычайно большой файл, образ расположен на  [GoogleDrive](https://drive.google.com/file/d/1zcrP-swscdtCANFZk2ZGlFW3MZPUHfeC/view?usp=sharing)  

## Использование
1. Загрузите GaladrielMap Demo image в используемую вами систему виртуальных машин (образ сделан в VirtualBox).
2. Запустите виртуальную машину образа (она называется "гостевая машина").
3. После того, как машина стартует, войдите в систему:  
имя пользователя: gm  
пароль: gm
4. Определите _ip_адрес_виртуальной_машины_, введя:  
`ifconfig`

Откройте адрес  
http://_ip_адрес_виртуальной_машины_/map/  
в браузере на любом компьютере локальной сети.

## Ещё возможности
### Использование приёмника ГПС
1. Подсоедините приёмник ГПС в USB порт того компьютера, на котором запущена виртуальная машина.
2. Разрешите виртуальной машине доступ к этому USB порту.

### Приборная панель
Откройте адрес  
http://_ip_адрес_виртуальной_машины_/map/dashboard.php  
в браузере.  
Панель оптимизтрована для слабых устройств с экраном на электронных чернилах (eInk).

### netAIS
Укажите в переменной $onion в файле _/GaladrielMap/netAIS/params.php_ адрес скрытого сервиса TOR. Адрес находится в файле _/var/lib/tor/hidden_service_netAIS/hostname_ и будет сгенерировани при первом запуске виртуальной машины.  
Увидеть адрес можно, сказав `cat /var/lib/tor/hidden_service_netAIS/hostname`  
Заполните файл информации о судне _boatInfo.ini_ чем-нибудь.  
Управление netAIS находится по адресу _http://_ip_address_of_the_virtual_machine_/netAIS/_

### Имитация движения
В каталоге _/GaladrielMap/map/gpsdAISd_ имеется naiveNMEAdaemon.php -- средство имитации потока сообщений NMEA для gpsd. Там же есть три файла с записью потока сообщений: _sample1.log_ -- запись AIS обстановки в порту, _Suomi_2018.nmea_ и _Suomi_2019.nmea_ -- две записи пути по озеру Сайма в Финляндии.  
Для запуска имитации:  
1. Остановите gpsd:
`sudo systemctl stop gpsd.socket gpsd.service`
2. Создайте две сессии screen:
`screen`, CtrlA-D для выхода из сессии  
`screen`, CtrlA-D для выхода из сессии  
`screen -ls` для получения списка сессий
3. Запустите имитацию потока NMEA 
`screen -r НомерПервойСессии`подключитесь к первой сессии  
Внутри сессии:  
`cd /GaladrielMap/map/gpsdAISd/'  
'php naiveNMEAdaemon.php -iSuomi_2018.nmea -btcp://localhost:2222` для воспроизведения Suomi_2018.nmea  
CtrlA-D для выхода из сессии
4. Запустите gpsd
`screen -r НомерВторойСессии`подключитесь к второй сессии  
Внутри сессии:  
`gpsd -N -n tcp://localhost:2222`
CtrlA-D для выхода из сессии
5. Убедитесь, что воспроизведение началось
`screen -r НомерПервойСессии`подключитесь к первой сессии  

Откройте в браузере _http://_ip_address_of_the_virtual_machine_/map/_.

## Администрирование сервера
Откройте адрес  
_http://_ip_адрес_виртуальной_машины_:10000/_  
в браузере.

## Образ содержит
* Ubuntu 20.04 LTE mini 
* Apache2
* PHP7
* gpsd
* mc
* webmin
* другие обычные утилиты и приложения