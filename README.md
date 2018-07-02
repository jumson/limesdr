"# limesdr"

See: https://github.com/jumson/limesdr/wiki

These Dockerfiles / containers are designed to provide pre-built packages to run LimeSDR and LimeSDR mini for particular applications. Generally, these are meant to be run on Raspberry Pi 3 and/or Raspberry Pi Zero W

The goal is to provide images for: LTE eNodeB GSM BTS Basic GNURadio & LimeSuite application Basic GQRX with LimeSDR

For access to the SDR(privileged) and GUI -- most of these images should be run like:

For GNURadio Companion:

docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:latest-grc

docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix [CONTAINER-NAME] [OPTIONAL COMMAND(like bash)]

based on: http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
