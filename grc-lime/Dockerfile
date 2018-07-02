FROM ubuntu:latest      
MAINTAINER jon
ARG DEBIAN_FRONTEND=noninteractive


# worked on 17.04 -- when using older versions of ubuntu -- operations below are necessary
# from: https://ubuntuforums.org/showthread.php?t=2382832
# RUN echo "## EOL upgrade sources.list" > /etc/apt/sources.list
# RUN echo "# Required" >> /etc/apt/sources.list
# RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty main restricted universe multiverse" >> /etc/apt/sources.list
# RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty-updates main restricted universe multiverse" >> /etc/apt/sources.list
# RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty-security main restricted universe multiverse" >> /etc/apt/sources.list

# Setting up SSH login ability
RUN apt-get update; exit 0
RUN apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN echo 'root:toor' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

#Setup git, sudo and other user called docker -- do rest of build as docker with sudo, not root
RUN apt-get -y install git sudo python-pip udev mousepad software-properties-common 
RUN git config --global user.name "jumson"
RUN git config --global user.email "munsonj@gmail.com"
RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo
RUN su docker

# First, following instructions for Lime_Suite
# http://wiki.myriadrf.org/Lime_Suite#Installers_and_packages

#packages for soapysdr available at myriadrf PPA
RUN sudo add-apt-repository -y ppa:myriadrf/drivers
RUN sudo apt-get update

#install core library and build dependencies
RUN sudo apt-get -y install git g++ cmake libsqlite3-dev

#install hardware support dependencies
RUN sudo apt-get -y install libsoapysdr-dev libi2c-dev libusb-1.0-0-dev

#install graphics dependencies
RUN sudo apt-get -y install libwxgtk3.0-dev freeglut3-dev libboost-all-dev swig libboost-all-dev swig

# install SoapySDR, LimeSuite needs this
# github.com/pothosware/SoapySDR.git

WORKDIR /
RUN git clone https://github.com/pothosware/SoapySDR.git
WORKDIR SoapySDR
# RUN git pull origin master; 
# RUN mkdir build
WORKDIR build
RUN cmake ..
RUN make -j4
RUN sudo make install
RUN sudo ldconfig

# install LimeSDR
WORKDIR /
RUN git clone https://github.com/myriadrf/LimeSuite.git
WORKDIR LimeSuite/build 
RUN cmake ..
RUN make -j4
RUN sudo make install
RUN sudo ldconfig

WORKDIR /LimeSuite/udev-rules
RUN pwd; ls -al
RUN sudo chmod a+x install.sh; 
RUN sudo ./install.sh

# Download board firmware (only run when hardware! is connected!)
# RUN LimeUtil --update; exit 0

#-------------everything below is application specific ----------------#
#-- the above layers should be common to all doe LimeSDR applications--#
#----GNURadio application below----#
# follow these instructions! https://wiki.gnuradio.org/index.php/InstallingGRFromSource
# compile gnuradio from source
WORKDIR /
RUN sudo pip install pybombs
RUN sudo pip install git+https://github.com/gnuradio/pybombs.git
RUN pybombs auto-config
RUN pybombs recipes add-defaults
RUN mkdir gnurad
RUN pybombs prefix init ~/gnurad -a myprefix -R gnuradio-default
RUN ls -al gnurad

#Now Lime plugins for GNU: https://wiki.myriadrf.org/Gr-limesdr_Plugin_for_GNURadio
WORKDIR /
RUN git clone https://github.com/myriadrf/gr-limesdr
#this creates directory too
WORKDIR gr-limesdr/build
# RUN mkdir build
RUN cmake ..
RUN make
RUN sudo make install
RUN sudo ldconfig



# go back to root?
# RUN exit

# Make this export GUI components
# using: https://github.com/rogaha/docker-desktop
# http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/

# mousepad is to help debug and check on things, edit config files, etc.

# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:late-grc
# CMD LimeSuiteGUI
CMD bash -c 'pybombs run gnuradio-companion'
EXPOSE 22

# to build
# docker build --rm -f grc-lime/Dockerfile -t jumson/limesdr:grc-lime grc-lime
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:grc-lime