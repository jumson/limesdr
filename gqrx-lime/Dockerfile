FROM jumson/limesdr:latest-grc
MAINTAINER jon
ARG DEBIAN_FRONTEND=noninteractiv
# this container uses my grc-lime as a base, so GNURadio is already built-in.
#-------------everything below is application specific ----------------#
#-- the above layers should be common to all doe LimeSDR applications--#
#----GQRX application below----#
# https://github.com/csete/gqrx

# gr-iqbalance
WORKDIR /
RUN git clone https://github.com/osmocom/gr-iqbal.git
#this creates directory too
WORKDIR gr-iqbal/build
# RUN mkdir build
RUN cmake ..
RUN make
RUN sudo make install
RUN sudo ldconfig

# not compiling pulseaudio unless required....
# https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/Developer/PulseAudioFromGit/

# install GQRX: http://gqrx.dk/download/install-ubuntu
RUN sudo apt-get purge --auto-remove gqrx
RUN sudo apt-get purge --auto-remove gqrx-sdr
RUN sudo apt-get purge --auto-remove libgnuradio*

RUN sudo add-apt-repository -y ppa:bladerf/bladerf
RUN sudo add-apt-repository -y ppa:myriadrf/drivers
RUN sudo add-apt-repository -y ppa:myriadrf/gnuradio
RUN sudo add-apt-repository -y ppa:gqrx/gqrx-sdr
RUN sudo apt-get update

RUN sudo apt-get install gqrx-sdr

RUN sudo ldconfig

## It is recommended that users also install the libvolk1-bin package and 
## run the volk_profile tool to optimize GNU Radio performance on the 
## specific computer it is being used on.
RUN sudo apt-get install libvolk1-bin
RUN volk_profile

CMD gqrx

#for testing/debug
EXPOSE 22

# to build
# docker build --rm -f gqrx-lime/Dockerfile -t jumson/limesdr:gqrx-lime gqrx-lime
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:gqrx-lime
