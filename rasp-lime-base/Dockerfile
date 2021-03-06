FROM resin/rpi-raspbian:latest
LABEL maintainer="munsonj@gmail.com"
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

#take care of tzdata, etc breaking things?
RUN apt-get install tzdata -y --no-install-recommends apt-utils

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

#extra things to quickly add to the image base
RUN sudo apt-get -y install screen

# Download board firmware (only run when hardware! is connected!)
# RUN LimeUtil --update; exit 0

# to build
# docker build --rm -f lime-base/Dockerfile -t jumson/limesdr:lime-base lime-base
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:lime-base
EXPOSE 22
# must run with -p to actually use it ssh insterface on the port

#-------------everything below is application specific ----------------#
#-- the above layers should be common to all doe LimeSDR applications--#
