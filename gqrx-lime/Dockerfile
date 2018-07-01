FROM ubuntu:latest
MAINTAINER jon

# enable ssh
RUN apt-get update; exit 0
RUN apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd
RUN echo 'root:toor' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# from: https://discourse.myriadrf.org/t/howto-install-gqrx-gnuradio-soapysdr-pothosware-limesuite-on-ubuntu/2402
RUN apt -y install software-properties-common
RUN add-apt-repository -y ppa:bladerf/bladerf
RUN add-apt-repository -y ppa:ettusresearch/uhd
RUN add-apt-repository -y ppa:gqrx/gqrx-sdr
RUN add-apt-repository -y ppa:myriadrf/drivers
RUN add-apt-repository -y ppa:myriadrf/gnuradio
RUN add-apt-repository -y ppa:pothosware/framework
RUN add-apt-repository -y ppa:pothosware/support
RUN apt-get update; exit 0
RUN apt-get -y upgrade; exit 0

RUN apt-get -y install gqrx-sdr

RUN apt-get -y install soapysdr-module-airspy soapysdr-module-redpitaya soapysdr-module-rfspace soapysdr-module-bladerf soapysdr-module-rtlsdr soapysdr-module-remote soapysdr-module-hackrf soapysdr-module-lms7 soapysdr-module-uhd soapysdr-module-mirisdr soapysdr-module-osmosdr
RUN ldconfig

# Make this export GUI components
# using: https://github.com/rogaha/docker-desktop
# http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/

RUN apt -y install mousepad
# docker run -t -i --privileged limesdr-oai bash
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix limesdr-oai bash
# CMD LimeSuiteGUI
CMD gqrx

#for testing/debug
EXPOSE 22

