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
`$ ifconfig`

Open http://_ip_address_of_the_virtual_machine_/map/ on you browser.

## More usage
1. Connect GNSS receiver to USB port of the host (where the guest machine is running) machine.
2. Allow guest machine access to this USB port.

Open http://_ip_address_of_the_virtual_machine_/map/dashboard.php on you browser.

## Server administration
Open http://_ip_address_of_the_virtual_machine_:10000/ on you browser.

##Contains
* Ubuntu 20.04 LTE mini 
* Apache2
* PHP7
* gpsd
* mc
* webmin
* other