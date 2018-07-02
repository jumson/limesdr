FROM jumson/limesdr:lime-base     
MAINTAINER jon
ARG DEBIAN_FRONTEND=noninteractive

#-------------everything below is application specific ----------------#
#-- the above layers should be common to all doe LimeSDR applications--#
#----GNURadio application below----#
# follow these instructions! https://wiki.gnuradio.org/index.php/InstallingGRFromSource
# compile gnuradio from source
WORKDIR /
RUN sudo apt install -y python-apt
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

# gr-osmosdr
WORKDIR /
RUN git clone git://git.osmocom.org/gr-osmosdr
#this creates directory too
WORKDIR gr-osmosdr/build
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
# CMD bash -c 'pybombs run gnuradio-companion'
# EXPOSE 22

# to build
# docker build --rm -f grc-lime/Dockerfile -t jumson/limesdr:grc-lime grc-lime
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:grc-lime