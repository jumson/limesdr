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

# follow these isntructions! https://wiki.gnuradio.org/index.php/InstallingGRFromSource
RUN apt-get -y install git sudo
RUN git config --global user.name "jumson"
RUN git config --global user.email "munsonj@gmail.com"

RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo
RUN su docker

#compile gnuradio from source
RUN sudo apt-get -y install python-pip
RUN sudo pip install pybombs
RUN sudo pip install git+https://github.com/gnuradio/pybombs.git
RUN pybombs auto-config
RUN pybombs recipes add-defaults
RUN mkdir ~/gnurad
RUN pybombs prefix init ~/gnurad -a myprefix -R gnuradio-default
RUN ls -al ~/gnurad


# go back to root
RUN exit
# instlal SoapySDR
RUN git clone https://github.com/pothosware/SoapySDR.git
RUN cd SoapySDR; git pull origin master; mkdir build && cd build; cmake ..; make -j4; make install
RUN ldconfig

# install LimeSDR
RUN git clone https://github.com/myriadrf/LimeSuite.git
RUN cd LimeSuite; cd build; cmake ..; make -j4; sudo make install
RUN ldconfig
RUN apt install -y udev
RUN chmod a+x LimeSuite/udev-rules/install.sh; sudo LimeSuite/udev-rules/install.sh

# Download board firmware (only run when hardware! is connected!)
RUN LimeUtil --update; exit 0


# Make this export GUI components
# using: https://github.com/rogaha/docker-desktop
# http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/

# this is to help debug and check on things, edit config files, etc.
RUN apt -y install mousepad
# docker run -t -i --privileged limesdr-oai bash
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:late-grc
# CMD LimeSuiteGUI
CMD bash -c 'pybombs run gnuradio-companion'
EXPOSE 22

