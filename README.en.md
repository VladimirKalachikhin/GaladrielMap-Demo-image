[Русское описание](README.md)  
# GaladrielMap Demo image [![License: CC BY-NC-SA 4.0](Cc-by-nc-sa_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en)

Three images are available:
* The Docker image - for free
* The .ova virtual machine image with Ubuntu - for free
* and Raspberry Pi bootable image with Raspbian - for payment.

All of them includes ready to use 
- [GaladrielMap](https://github.com/VladimirKalachikhin/Galadriel-map) v.2.9.8 witn [gpsdPROXY](https://github.com/VladimirKalachikhin/gpsdPROXY) v.0.6.14
- [GaladrielCache](https://github.com/VladimirKalachikhin/Galadriel-cache) v.2.7.5
- [netAIS](https://github.com/VladimirKalachikhin/netAIS) v.1.5.10

and simulation tools:
- [naiveNMEAdaemon](https://github.com/VladimirKalachikhin/naiveNMEAdaemon)
- [inetAIS](https://github.com/VladimirKalachikhin/inetAIS) v.0.2.5

Download:  
[Docker image](https://drive.google.com/file/d/1Oe_VyfIztDg3JwlMi3evI7eX3ucY19zd/view?usp=sharing) [![magnet link](magnet.svg)](magnet:?xt=urn:btih:cd505726f57b0d7b6db10faa888af10bbd37529b&dn=galadrielmapimage.tar.gz&tr=http%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fttk2.nbaonlineservice.com%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce)  
[Virtual machine image](https://drive.google.com/file/d/1UfbppWCSY9iXSCohBU9FkXefE7TFbBLO/view?usp=sharing) [![magnet link](magnet.svg)](magnet:?xt=urn:btih:4bac654f38808d9d03a4587a0a364ca754041e9a&dn=GaladrielMap%5Fubuntu%5F20.04.ova&tr=http%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce&tr=udp%3A%2F%2Fttk2.nbaonlineservice.com%3A6969%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce)  


## Usage
### Docker image
Load image to your Docker:  
`gunzip -c galadrielmapimage.tar.gz | docker load`  
Run container:  
`docker run -p 80:80 -p 3838:3838 -d --name galadrielmap galadrielmap`  
Add ` -p 9050:9050` option to command above if you plan to use the netAIS server.   
Open 'http://YourDocker/map' in browser.  
 
The Docker image already contains running NMEA flow simulation, so you will see a cursor displaying the position on the moving map. The netAIS is also running.

### .ova
1. Import GaladrielMap Demo image to any virtualization player (Image created in VirtualBox).
2. Run machine (it's called a guest machine).
3. After starting, login:  
username: gm  
password: gm
4. Determine _ip_address_of_the_machine_:  
`ifconfig`  
5. Open ip_address/map in browser

Or jast open http://galadrielmap.local/map in browser  

Another way is to connect by ssh: `$ ssh gm@galadrielmap.local`

You must set at least unique *shipname* in `\GaladrielMap\netAIS\boatinfo.ini`  
No simulation is running in this image. Run it yourself by run `\GaladrielMap\map\samples\startSimulation`

### Raspberry Pi 
1. Flash the image to SD card as described on [Raspberry Pi documentation](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). The Image assumed a 32G SD card: 16G to OS and 16G to tile cache.  
Connect Raspberry Pi to LAN by cable.  
2. Boot machine
3. After starting, find _ip_address_of_the_machine_ as described in [this document](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).  
Or connect to Raspberry Pi by ssh: `$ ssh gm@galadrielmap.local`   
password: gm

Open http://_ip_address_of_the_machine_/map/ on you browser.

## More usage
### Use GNSS reciever
Except Docker image:
1. Connect external GNSS receiver to USB port.
2. For .ova: Allow guest machine access to this USB port.

### Dashboard
Open http://_ip_address_of_the_machine_/map/dashboard.php on you browser.  
Dashboard optimized to eInk devices.

### netAIS
Except Docker image:
To enable your own netAIS server, edit _/GaladrielMap/netAIS/params.php_ to place to $onion variable address of your TOR hidden service. This address located in _/var/lib/tor/hidden_service_netAIS/hostname_ file, and will be created at first start virtual machine.  
To get address run  
`# cat /var/lib/tor/hidden_service_netAIS/hostname`  
Fit vehicle info in _boatInfo.ini_ file.  
Open _http://_ip_address_of_the_machine_/netAIS/_ on you browser.

### Trip simulation
In _/GaladrielMap/map/samples_ contains naiveNMEAdaemon.php -- a tool to simulate NMEA streams from instruments to use with gpsd. Three logs include: _sample1.log_ -- the record of AIS situation on port; _Suomi_2018.nmea_ and _Suomi_2019.nmea_ -- records of two tracks on Saimaa lake, Finland.  
To play this logs:  
1. Stop gpsd daemon:  
`sudo systemctl stop gpsd.socket gpsd.service`
2. Create two screen sessions:   
`screen`, CtrlA-D to quit session  
`screen`, CtrlA-D to quit session  
`screen -ls` to list session numbers 
3. Run simulate NMEA stream
`screen -r FirstSessionNumber`attach session  
In session:  
`cd /GaladrielMap/map/gpsdAISd/'  
'php naiveNMEAdaemon.php -iSuomi_2018.nmea -btcp://localhost:2222` to play Suomi_2018.nmea log.  
CtrlA-D to quit session
4. Run gpsd daemon 
`screen -r SecondSessionNumber`attach session  
In session:  
`gpsd -N -n tcp://localhost:2222`  
CtrlA-D to quit session
5. See as simulation running
`screen -r FirstSessionNumber`attach session

or just run `\GaladrielMap\map\samples\startSimulation`

Open http://_ip_address_of_the_machine_/map/ on you browser.

## Server administration -- in .ova only
Open _http://_ip_address_of_the_virtual_machine_:10000/_ on you browser.

## Update
You may update the software in images from [GaladrielMap Emergency Kit](https://github.com/VladimirKalachikhin/Galadriel-map/tree/master/emergencykit). Unzip archive as described in README.txt, but prevent from overwriting _boatInfo.ini_ file and, if necessary, files _/GaladrielMap/netAIS/params.php_, _/GaladrielMap/tileproxy/params.php_,_/GaladrielMap/map/params.php_ and _/GaladrielMap/tileproxy/mapsources/C-MAP.json_.


## Contains
.ova and Raspbery Pi images contains:
* Apache2
* PHP7
* TOR
* gpsd
* mc
* other

## On paid
The Raspberry Pi bootable image available for $25 by [ЮMoney](https://sobe.ru/na/galadrielmap) or in another way. You can also order burn image to SD card the capacity you need. The cost will be $15 + the SD card cost + shipping cost.  
The Raspberry Pi image is fully configured and ready for operation.
