FROM ubuntu:17.04
MAINTAINER jon


# from: https://ubuntuforums.org/showthread.php?t=2382832
RUN echo "## EOL upgrade sources.list" > /etc/apt/sources.list
RUN echo "# Required" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://old-releases.ubuntu.com/ubuntu/ zesty-security main restricted universe multiverse" >> /etc/apt/sources.list

RUN apt-get update; exit 0
RUN apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN echo 'root:toor' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# follow these isntructions! https://open-cells.com/index.php/2017/05/10/limesdr-installation/
RUN apt-get -y install git
# also adapted from https://open-cells.com/index.php/2017/08/22/all-in-one-openairinterface-august-22nd/

RUN git config --global user.name "jumson"
RUN git config --global user.email "munsonj@gmail.com"
RUN echo -n | openssl s_client -showcerts -connect gitlab.eurecom.fr:443 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' | tee -a /etc/ssl/certs/ca-certificates.crt 

RUN apt-get install -y libboost-all-dev libusb-1.0-0-dev python-mako doxygen python-docutils python-requests cmake build-essential
RUN apt-get install -y cmake g++ libpython-dev python-numpy swig git libsqlite3-dev libi2c-dev libusb-1.0-0-dev libwxgtk3.0-dev freeglut3-dev
RUN ldconfig

# instlal SoapySDR
RUN git clone https://github.com/pothosware/SoapySDR.git
RUN cd SoapySDR; git pull origin master; mkdir build && cd build; cmake ..; make -j4; make install
RUN ldconfig

# install LimeSDR
RUN git clone https://github.com/myriadrf/LimeSuite.git
RUN cd LimeSuite; cd build; cmake ..; make -j4; make install
RUN ldconfig
RUN cd LimeSuite/udev-rules/; chmod a+x install.sh; ./install.sh

# Download board firmware (only run when hardware! is connected!)
RUN LimeUtil --update; exit 0

# download/compile OAI - OpenAirInterface
RUN git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
RUN cd openairinterface5g; git checkout develop
RUN ldconfig
# the install scripts use sudo so.....
RUN apt install sudo

# install SW packages from internet
RUN cd openairinterface5g; source ./oaienv; ./cmake_targets/build_oai -I; exit 0       
RUN ldconfig

# compile eNB
RUN openairinterface5g/cmake_targets/build_oai -c -w LMSSDR --eNB --UE; exit 0
RUN openairinterface5g/cmake_targets/lte_build_oai/build/lte-softmodem -d -O enb.conf --rf-config-file openairinterface5g/targets/ARCH/LMSSDR/LimeSDR_above_1p8GHz_1v4.ini; exit 0
RUN ldconfig

# Make this export GUI components
# using: https://github.com/rogaha/docker-desktop
# http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/

RUN apt -y install mousepad
# docker run -t -i --privileged limesdr-oai bash
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix limesdr-oai bash
# CMD LimeSuiteGUI

EXPOSE 22

