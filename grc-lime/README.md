This Dockerfile is built on top of the lime-base image (which is built on top of the ubuntu:latest).
lime-base image simply has SSH server setup and running (for remote administration)

**Lime-base capabilites:**
* SSH server is active, with root login (root:toor)
* docker:docker username with sudo working
* git installed, and configured for my own github
* all packages and dependancies needed for LimeSuiteGUI (based on: http://wiki.myriadrf.org/Lime_Suite#Installers_and_packages)
* udev rules are set as per recommendations
* additional things can be added at the end, as of now: screen

**grc-lime capabilites**
* GNU Radio (and PYBOMBs) as per:  https://wiki.gnuradio.org/index.php/InstallingGRFromSource
* via PYBOMBs, these GNU Radio modules have been installed:
    * gr-limesdr
    * gr-osmosdr
    * gr-rds
    * gr-tpms
    * 
    * gr-keyfob
    * gr-gsm
    * gr-cdma
    * gr-adsb
    * gr-burst
    * gr-ax25

To run:

docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:grc-lime

To do: how to I use some flowcharts and save them? Perhaps simply start with bash, and use github?

