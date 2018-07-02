FROM jumson/limesdr:latest-grc
MAINTAINER jon
ARG DEBIAN_FRONTEND=noninteractiv
# this container uses my grc-lime as a base, so GNURadio is already built-in.
#-------------everything below is application specific ----------------#
#-- the above layers should be common to all doe LimeSDR applications--#
#----GQRX application below----#
# https://github.com/csete/gqrx

RUN pybombs install gqrx

CMD bash -c 'pybombs run gqrx'

#for testing/debug
EXPOSE 22

# to build
# docker build --rm -f gqrx-lime/Dockerfile -t jumson/limesdr:gqrx-lime gqrx-lime
# to run with GUI capabilities!
# docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:gqrx-lime
