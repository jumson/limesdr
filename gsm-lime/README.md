this is not my own....yet -- but eventually i'll want to modify this image and make my own
for now, simply re-using a public base from: 
 https://hub.docker.com/r/cswiger/limeosmogsm01/

 The notes from the webpage are below:
----------------------------------------------------------------------------
LimeSDR utilities (LimeUtil and drivers), SoapySDR and UHD with 
osmocom GSM stack tested basic functionality with osmo-trx, 
osmo-niiitb and osmo-bts-trx. Launch with LimeSDR device attached:

```LimeUtil --find```
```lsusb``` <-- look for matching device on usb bus xxx yyy

 LimeSDR utilities (LimeUtil and drivers), SoapySDR and UHD with 
 osmocom GSM stack tested basic functionality with osmo-trx, osmo-nitb 
 and osmo-bts-trx. 

 Launch with LimeSDR device attached:
 or matching device on usb bus xxx yyy

 then launch this image with:
 ```docker run -it limeosmogsm01 --device=/dev/bus/usb/xxx/yyy bash```
 and in 3 screen sessions start in root /
 ```osmo-trx```
 ```osmo-nitb```
 ```osmo-bts-trx```

```telnet 0 4242``` to add subscribers per the osmo-nitb vty reference manual

some more info - perhaps for building my own: 
* https://github.com/cswiger/openbts
* https://github.com/cswiger/osmocom_cfg1
