FROM ubuntu:latest      
MAINTAINER jon
# from: https://discourse.myriadrf.org/t/howto-install-gqrx-gnuradio-soapysdr-pothosware-limesuite-on-ubuntu/2402
ARG DEBIAN_FRONTEND=noninteractive
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

#Setup git, sudo and other user called docker
# follow these isntructions! https://wiki.gnuradio.org/index.php/InstallingGRFromSource
RUN apt-get -y install git sudo python-pip udev mousepad software-properties-common 
RUN git config --global user.name "jumson"
RUN git config --global user.email "munsonj@gmail.com"

RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo
RUN su docker

# Starting instructions from main reference
RUN sudo apt-get purge --auto-remove gqrx
RUN sudo apt-get purge --auto-remove gqrx-sdr
RUN sudo apt-get purge --auto-remove libgnuradio*

# Add ppa archives:
RUN sudo add-apt-repository -y ppa:bladerf/bladerf
RUN sudo add-apt-repository -y ppa:ettusresearch/uhd
RUN sudo add-apt-repository -y ppa:gqrx/gqrx-sdr
RUN sudo add-apt-repository -y ppa:myriadrf/drivers
RUN sudo add-apt-repository -y ppa:myriadrf/gnuradio
RUN sudo add-apt-repository -y ppa:pothosware/framework
RUN sudo add-apt-repository -y ppa:pothosware/support
RUN sudo apt-get update; exit 0
RUN sudo apt-get upgrade -y

# Gqrx: (Also installs Gnuradio)
RUN sudo apt-get -y install gqrx-sdr

# Soapysdr:
RUN sudo apt-get -y install soapysdr
RUN sudo apt-get -y install python-soapysdr
RUN sudo apt-get -y install python3-soapysdr

# Soapysdr driver modules: (installs all drivers)
RUN sudo apt-get -y install soapysdr-module-airspy soapysdr-module-redpitaya soapysdr-module-rfspace soapysdr-module-bladerf soapysdr-module-rtlsdr soapysdr-module-remote soapysdr-module-hackrf soapysdr-module-lms7 soapysdr-module-uhd soapysdr-module-mirisdr soapysdr-module-osmosdr

# Pothosware:
RUN sudo apt-get -y install pothos-all
# Either python2 bindings:
RUN sudo apt-get -y install python-pothos
# Or for python3 bindings:
# RUN sudo apt-get install python3-pothos

# Limesuite:
RUN sudo apt-get -y install limesuite liblimesuite-dev limesuite-udev limesuite-images

# Volk: (Create hardware profile) !!! Do this when using the hardware, not container building
# RUN volk_profile

# to build
# docker build --rm -f full-lime/Dockerfile -t jumson/limesdr:lime-full full-lime
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:lime-full
# CMD LimeSuiteGUI
CMD bash -c 'bash'
EXPOSE 22