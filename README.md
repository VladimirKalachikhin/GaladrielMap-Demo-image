# GaladrielMap Demo image [![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

The demo image is a .ova virtual machine image. It includes Ubuntu with [GaladrielMap](https://github.com/VladimirKalachikhin/Galadriel-map) & [GaladrielCache](https://github.com/VladimirKalachikhin/Galadriel-cache) installed. Import GaladrielMap Demo image to any virtualization player (VirtualBox, VMware), and run machine. Open a browser and try GaladrielMap.

So GitHub does not allow big files - get this image from [GoogleDrive](https://drive.google.com/file/d/1zcrP-swscdtCANFZk2ZGlFW3MZPUHfeC/view?usp=sharing)  

## Usage
1. Import GaladrielMap Demo image to any virtualization player (Image created in VirtualBox).
2. Run machine (it's called a guest machine).
3. After starting, login:  
username: gm  
password: gm
4. Determine _ip_address_of_the_virtual_machine_:  
`ifconfig`

Open http://_ip_address_of_the_virtual_machine_/map/ on you browser.

## More usage
### Use GNSS reciever
1. Connect external GNSS receiver to USB port of the host (where the guest machine is running) machine.
2. Allow guest machine access to this USB port.

### Dashboard
Open http://_ip_address_of_the_virtual_machine_/map/dashboard.php on you browser.  
Dashboard optymised to eInk devices.

### netAIS
Do update _/GaladrielMap/netAIS/params.php_ to place to $onion variable address of your TOR hidden service. This address located in _/var/lib/tor/hidden_service_netAIS/hostname_ file, and will be created at first start virtual machine.  
To get address run `cat /var/lib/tor/hidden_service_netAIS/hostname`  
Fit vehicle info in _boatInfo.ini_ file.  
Open _http://_ip_address_of_the_virtual_machine_/netAIS/_ on you browser.

### Trip simulation
In _/GaladrielMap/map/gpsdAISd_ contains naiveNMEAdaemon.php -- a tool to simulate NMEA streams from instruments to use with gpsd. Three logs include: _sample1.log_ -- the record of AIS situation on port; _Suomi_2018.nmea_ and _Suomi_2019.nmea_ -- records of two tracks on Saimaa lake, Finland.  
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

Open http://_ip_address_of_the_virtual_machine_/map/ on you browser.

## Server administration
Open _http://_ip_address_of_the_virtual_machine_:10000/_ on you browser.

## Contains
* Ubuntu 20.04 LTE mini 
* Apache2
* PHP7
* gpsd
* mc
* webmin
* other