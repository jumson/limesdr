"# limesdr"

See: https://github.com/jumson/limesdr/wiki

These Dockerfiles / containers are designed to provide pre-built packages to run LimeSDR and LimeSDR mini for particular applications. Generally, these are meant to be run on Raspberry Pi 3 and/or Raspberry Pi Zero W

The goal is to provide images for: LTE eNodeB GSM BTS Basic GNURadio & LimeSuite application Basic GQRX with LimeSDR

**Instructions for building and running -- modify these fit your specific setup and desired folder/Dockerfile above**

**For instance, I tag them with my hub.docker info, so that probably wont work for you.**

**Full Lime Suite, GQRX, GNURadio, Pothosware etc:**
* to build

``` docker build --rm -f full-lime/Dockerfile -t jumson/limesdr:lime-full full-lime```

* to run with GUI capabilities

``` docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:lime-full```

* to run with SSH capability....tbd, need to use the -p 22:22 argument though

For access to the SDR(privileged) and GUI -- most of these images should be run like:

```docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix [CONTAINER-NAME] [OPTIONAL COMMAND(like bash)]```

based on: http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
