FROM cswiger/limeosmogsm01
LABEL maintainer="munsonj@gmail.com"
# this is not my own....yet -- but eventually i'll want to modify this image and make my own
# for now, simply re-using a public base from: 

# https://hub.docker.com/r/cswiger/limeosmogsm01/
# LimeSDR utilities (LimeUtil and drivers), SoapySDR and UHD with 
# osmocom GSM stack tested basic functionality with osmo-trx, osmo-nitb 
# and osmo-bts-trx. Launch with LimeSDR device attached:
# or matching device on usb bus xxx yyy
# then launch this image with:
# docker run -it limeosmogsm01 --device=/dev/bus/usb/xxx/yyy bash
# and in 3 screen sessions start in root /
# osmo-trx
# osmo-nitb
# osmo-bts-trx

# telnet 0 4242 to add subscribers per the osmo-nitb vty reference manual

# other reference: https://github.com/cswiger/osmocom_cfg1


RUN apt -y install mousepad


