Lime-base is meant to be a basic image built on ubuntu:latest that can serve as a platform for any specific applications that are meant to use the LimeSDR.
Lime-base capabilites:
* SSH server is active, with root login (root:toor)
* docker:docker username with sudo working
* git installed, and configured for my own github
* all packages and dependancies needed for LimeSuiteGUI (based on: http://wiki.myriadrf.org/Lime_Suite#Installers_and_packages)
* udev rules are set as per recommendations
* additional things can be added at the end, as of now: 
    * screen
