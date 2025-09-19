[In English](README.md)
# GaladrielMap Demo image [![License: CC BY-NC-SA 4.0](Cc-by-nc-sa_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en)

Имеется три готовых образа:

* Образ для Docker
* Образ виртуальной машины в формате Открытого формата виртуализации .ova с операционной системой Ubuntu
* Платный образ загрузочного накопителя для Raspberry Pi.

Каждый вариант содержит готовые к использованию:
- картплотер [GaladrielMap](https://vladimirkalachikhin.github.io/Galadriel-map/README.ru-RU) с [gpsdPROXY](https://github.com/VladimirKalachikhin/gpsdPROXY/blob/master/README.ru-RU.md)
- тайловый кеш [GaladrielCache](https://github.com/VladimirKalachikhin/Galadriel-cache/blob/master/README.ru-RU.md)
- средство обмена информацией о местоположении [netAIS](https://github.com/VladimirKalachikhin/netAIS/blob/master/README.ru-RU.md)
- навигационный указатель [e-inkDashboardModern](https://github.com/VladimirKalachikhin/e-inkDashboardModern/blob/master/README.ru-RU.md)

и средства имитации данных:
- [naiveNMEAdaemon](https://github.com/VladimirKalachikhin/naiveNMEAdaemon)
- [inetAIS](https://github.com/VladimirKalachikhin/inetAIS)

Образ .ova может быть запущен в любой системе виртуальных машин (VirtualBox, VMware или другой). Образ Raspberry Pi может быть запущен в виртуальной машине QEMU или с него может быть загружена реальная машина.

Ссылки для скачивания:  
[образ контейнера Docker](https://drive.google.com/file/d/1Oe_VyfIztDg3JwlMi3evI7eX3ucY19zd/view?usp=drive_link) [![magnet link](magnet.svg)](magnet:?xt=urn:btih:cd505726f57b0d7b6db10faa888af10bbd37529b&dn=galadrielmapimage.tar.gz&tr=http%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fttk2.nbaonlineservice.com%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce)  
[файлы для генерации образа контейнера Docker](https://drive.google.com/file/d/1KPGGgsR7eBQu0oKfwoj5Z6EKGL3BOWGo/view?usp=drive_link) [![magnet link](magnet.svg)](magnet:?xt=urn:btih:cd505726f57b0d7b6db10faa888af10bbd37529b&dn=galadrielmapimage.tar.gz&tr=http%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fttk2.nbaonlineservice.com%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce)  
[виртуальная машина](https://drive.google.com/file/d/1UfbppWCSY9iXSCohBU9FkXefE7TFbBLO/view?usp=drive_link) [![magnet link](magnet.svg)](magnet:?xt=urn:btih:4bac654f38808d9d03a4587a0a364ca754041e9a&dn=GaladrielMap%5Fubuntu%5F20.04.ova&tr=http%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fttk2.nbaonlineservice.com%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce)  

## Использование
### Образ Docker
Загрузите образ в Docker:
`gunzip -c galadrielmapimage.tar.gz | docker load`  
Запустите контейнер:
`docker run -p 80:80 -p 3838:3838 -d --name galadrielmap galadrielmap`  
Добавьте параметр ` -p 9050:9050` в команду запуска контейнера, если предполагается использовать сервер netAIS.   
Откройте 'http://YourDockerAddress(172.17.0.1?)/map' в браузере.  

Образ Docker запускает имитацию поступления данных NMEA, так что в на экране уже будет изменяющееся местоположение на движущейся карте.

### .ova 
1. Загрузите образ виртуальной машины в используемую вами систему виртуальных машин (образ сделан в VirtualBox).
2. Запустите виртуальную машину образа (она называется "гостевая машина").
3. После того, как машина стартует, войдите в систему:  
имя пользователя: gm  
пароль: gm
4. Определите _ip_адрес_машины_, введя:  
`ifconfig`  
Или просто подключитесь `$ ssh gm@galadrielmap.local`

Откройте адрес  
http://_ip_адрес_машины_/map/  
в браузере на любом компьютере локальной сети.  
Или просто откройте адрес http://galadrielmap.local/map

Вы должнв поменять по крайней мере имя судна в файле `\GaladrielMap\netAIS\boatinfo.ini`  
Если нужна имитация движения -- запустите её сами, выполнив `\GaladrielMap\map\samples\startSimulation`

### Raspberry Pi
1. Запишите образ Raspberry Pi на флеш-карту как это указано [в документации](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). Необходима карта ёмкостью не менее 32G.
2. Запустите Raspberry Pi.
3. После загрузки определите _ip_адрес_машины_, можно воспользоваться [документацией](https://www.raspberrypi.org/documentation/remote-access/ip-address.md). Raspberry Pi доступна по найденному адресу по ssh:  ` ssh pi@_ip_адрес_машины_`.  
Или просто `$ ssh gm@galadrielmap.local`  
пароль: raspberry

Откройте адрес  
http://_ip_адрес_машины_/map/  
в браузере на любом компьютере локальной сети.  
Или просто откройте адрес  
http://raspberrypi.local/map


## Ещё возможности
### Использование приёмника ГПС
Кроме образа Docker:  
1. Подсоедините приёмник спутниковой геопозиционной системы в USB порт.
2. Если это виртуальная машина -- разрешите виртуальной машине доступ к этому USB порту.

### Приборная панель
Откройте адрес  
http://_ip_адрес_машины_/map/dashboard.php  
в браузере.  
Панель оптимизтрована для слабых устройств с экраном на электронных чернилах (eInk).  

На более современных устройствах откройте адрес http://_ip_адрес_машины_/dash/


### netAIS
Заполните файл информации о судне _boatInfo.ini_ чем-нибудь.  
Управление netAIS находится по адресу _http://_ip_адрес_машины_/netAIS/_

В образе Docker уже всё настроено и запущено.

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

или просто выполните `\GaladrielMap\map\samples\startSimulation`

Откройте в браузере _http://_ip_адрес_машины_/map/_.

В образе Docker уже всё настроено и запущено.

## Администрирование сервера -- имеется только в .ova
Откройте адрес  
_http://_ip_адрес_виртуальной_машины_:10000/_  
в браузере.

## Обновление программного обеспечения
Обновить программное обеспечение можно, воспользовавшись архивом [GaladrielMap Emergency Kit](https://github.com/VladimirKalachikhin/Galadriel-map/tree/master/emergencykit). Скачайте свежий архив в домашний каталог и разархивируйте как указано в README.txt внутри архива. Нужно позаботиться о сохранении от перезаписи файла _boatInfo.ini_ и, возможно, файлов параметров _/GaladrielMap/netAIS/params.php_, _/GaladrielMap/tileproxy/params.php_, _/GaladrielMap/map/params.php_ и _/GaladrielMap/tileproxy/mapsources/C-MAP.json_.

## Образ содержит
* Apache2
* PHP7
* TOR
* gpsd
* mc
* другие обычные утилиты и приложения

## Оплата
Готовый к использованию загрузочный образ для Raspberry Pi может быть получен за 2 500 руб. через [ЮMoney](https://sobe.ru/na/galadrielmap), или при оплате другим способом.  
Можно заказать готовую карту SD требуемой ёмкости, с прошитым образом. Её нужно будет просто вставить в Raspberry Pi.
