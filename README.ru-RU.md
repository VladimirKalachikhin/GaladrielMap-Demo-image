# GaladrielMap Demo image [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

Имеется два загрузочных образа:

* Виртуальной машины в формате Открытого формата виртуализации .ova с операционной системой Ubuntu; и
* Загрузочного накопителя Raspberry Pi.

Каждый вариант содержит готовые к использованию картплотер [GaladrielMap](https://vladimirkalachikhin.github.io/Galadriel-map/README.ru-RU) с  [GaladrielCache](https://github.com/VladimirKalachikhin/Galadriel-cache/blob/master/README.ru-RU.md) и средство обмена информацией о местоположении [netAIS](https://github.com/VladimirKalachikhin/netAIS/blob/master/README.ru-RU.md).

Образ .ova может быть запущен в любой системе виртуальных машин (VirtualBox, VMware или другой). Образ Raspberry Pi может быть запущен в виртуальной машине QEMU или с него может быть загружена реальная машина.

Поскольку для пафосного GitHub размеры образов чрезвычайно велики, они расположены в другом месте --  
YandexDisk:  
[GaladrielMap_ubuntu_20.04.ova](https://is.gd/ZmMTBN) 3.23 ГБ  
[GaladrielMap-raspios-buster-armhf-lite.img.zip](https://is.gd/Tan6ZB) 1.21 ГБ  
GoogleDrive:  
[GaladrielMap_ubuntu_20.04.ova](https://is.gd/pFC31t) 3 ГБ (3 468 575 744 bytes)  
[GaladrielMap-raspios-buster-armhf-lite.img.zip](https://is.gd/PYDPqJ)  1 ГБ (1 303 943 853 байт)  

## Использование
### .ova 
1. Загрузите образ виртуальной машины в используемую вами систему виртуальных машин (образ сделан в VirtualBox).
2. Запустите виртуальную машину образа (она называется "гостевая машина").
3. После того, как машина стартует, войдите в систему:  
имя пользователя: gm  
пароль: gm
4. Определите _ip_адрес_машины_, введя:  
`ifconfig`

### Raspberry Pi
1. Запишите образ Raspberry Pi на флеш-карту как это указано [в документации](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). Необходима карта ёмкостью не менее 32G.
2. Запустите Raspberry Pi.
3. После загрузки определите _ip_адрес_машины_, можно воспользоваться [документацией](https://www.raspberrypi.org/documentation/remote-access/ip-address.md). Raspberry Pi доступна по найденному адресу по ssh:  ` ssh pi@_ip_адрес_машины_`.  
пароль: raspberry


Откройте адрес  
http://_ip_адрес_машины_/map/  
в браузере на любом компьютере локальной сети.

## Ещё возможности
### Использование приёмника ГПС
1. Подсоедините приёмник спутниковой геопозиционной системы в USB порт.
2. Если это виртуальная машина -- разрешите виртуальной машине доступ к этому USB порту.

### TOR http proxy
Во избежание препятствования скачиванию со стороны держателей карт можно использовать TOR как анонимизирующий прокси. Пример такой конфигурации есть для OpenTopoMap. Но, поскольку TOR не умеет быть http proxy, в составе программного обеспечения есть приложение, дающее такую возможность -- прокси-сервер privoxy. Однако, поскольку обычно для скачивания карт прокси не используется, privoxy не запускается при загрузке системы для экономии ресурсов. Включить загрузку можно, сказав:  
`sudo systemctl enable privoxy`

### Приборная панель
Откройте адрес  
http://_ip_адрес_машины_/map/dashboard.php  
в браузере.  
Панель оптимизтрована для слабых устройств с экраном на электронных чернилах (eInk).

### netAIS
Укажите в переменной $onion в файле _/GaladrielMap/netAIS/params.php_ адрес скрытого сервиса TOR. Адрес находится в файле _/var/lib/tor/hidden_service_netAIS/hostname_ и будет сгенерировани при первом запуске виртуальной машины.  
Увидеть адрес можно, сказав `cat /var/lib/tor/hidden_service_netAIS/hostname`  
Заполните файл информации о судне _boatInfo.ini_ чем-нибудь.  
Управление netAIS находится по адресу _http://_ip_адрес_машины_/netAIS/_

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

Откройте в браузере _http://_ip_адрес_машины_/map/_.

## Администрирование сервера -- имеется только в .ova
Откройте адрес  
_http://_ip_адрес_виртуальной_машины_:10000/_  
в браузере.

## Обновление программного обеспечения
Обновить программное обеспечение можно, воспользовавшись архивом [GaladrielMap Emergency Kit](https://github.com/VladimirKalachikhin/Galadriel-map/tree/master/emergencykit). Скачайте свежий архив в домашний каталог и разархивируйте как указано в README.txt внутри архива. Нужно позаботиться о сохранении от перезаписи файла _boatInfo.ini_ и, возможно, файлов параметров _/GaladrielMap/netAIS/params.php_, _/GaladrielMap/tileproxy/params.php_ и _/GaladrielMap/map/params.php_.

## Образ содержит
* Ubuntu 20.04 LTE mini 
* Apache2
* PHP7
* TOR
* gpsd
* privoxy -- отключен по умолчанию
* mc
* другие обычные утилиты и приложения