# GaladrielMap Demo image [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

The two images available:

* The .ova virtual machine image with Ubuntu -- for free, and
* Raspberry Pi bootable image with Raspbian -- for payment.

Both includes ready to use [GaladrielMap](https://github.com/VladimirKalachikhin/Galadriel-map), [GaladrielCache](https://github.com/VladimirKalachikhin/Galadriel-cache) and [netAIS](https://github.com/VladimirKalachikhin/netAIS) installed. 

Import GaladrielMap Demo image to any virtualization player (VirtualBox, VMware), and run machine. Open a browser and try GaladrielMap.  
Or boot you Raspberry Pi

YandexDisk:  
[GaladrielMap_ubuntu_20.04.ova](https://is.gd/hucCPX)  


## Usage
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

### Raspberry Pi 
1. Flash the image to SD card as described on [Raspberry Pi documentation](https://www.raspberrypi.org/documentation/installation/installing-images/README.md). The Image assumed a 32G SD card: 16G to OS and 16G to tile cache.  
Connect Raspberry Pi to LAN by cable.  
2. Boot machine
3. After starting, find _ip_address_of_the_machine_ as described in [this document](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).  
Or connect to Raspberry Pi by ssh: `$ ssh pi@raspberry.local`   
password: raspberry

Open http://_ip_address_of_the_machine_/map/ on you browser.

## More usage
### Use GNSS reciever
1. Connect external GNSS receiver to USB port.
2. For .ova: Allow guest machine access to this USB port.

### TOR http proxy
To avoid banning GaladrielCache tile loader use TOR as anonymize proxy (see OpenTopoMap source config, for example). As TOR is not a http proxy -- privoxy app included. But as loading tiles via proxy are mostly disabled -- privoxy daemon disabled too. Enable it if you need:  
`sudo systemctl enable privoxy`

### Dashboard
Open http://_ip_address_of_the_machine_/map/dashboard.php on you browser.  
Dashboard optimized to eInk devices.

### netAIS
Do update _/GaladrielMap/netAIS/params.php_ to place to $onion variable address of your TOR hidden service. This address located in _/var/lib/tor/hidden_service_netAIS/hostname_ file, and will be created at first start virtual machine.  
To get address run `cat /var/lib/tor/hidden_service_netAIS/hostname`  
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

Open http://_ip_address_of_the_machine_/map/ on you browser.

## Server administration -- in .ova only
Open _http://_ip_address_of_the_virtual_machine_:10000/_ on you browser.

## Update
You may update the software in images from [GaladrielMap Emergency Kit](https://github.com/VladimirKalachikhin/Galadriel-map/tree/master/emergencykit). Unzip archive as described in README.txt, but prevent from overwriting _boatInfo.ini_ file and, if necessary, files _/GaladrielMap/netAIS/params.php_, _/GaladrielMap/tileproxy/params.php_,_/GaladrielMap/map/params.php_ and _/GaladrielMap/tileproxy/mapsources/C-MAP.json_.


## Contains
* Apache2
* PHP7
* TOR
* gpsd
* privoxy, disabled on default. Enable it by `systemctl enable privoxy`
* mc
* other

## Payment
The Raspberry Pi bootable image available for $15 [via PayPal](https://paypal.me/VladimirKalachikhin) or [YandexMoney](https://yasobe.ru/na/galadrielmap) at [galadrielmap@gmail.com](mailto:galadrielmap@gmail.com). You cal also order burn image to SD card the capacity you need. The cost will be $15 + the SD card cost + shipping cost.  
The Raspberry Pi image is fully configured and ready for operation.