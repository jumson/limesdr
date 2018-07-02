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

```docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix [IMAGE-NAME] [OPTIONAL COMMAND(like bash)]```

based on: http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/

** Pro-tip so --privileged may not be necessary gleaned from: https://hub.docker.com/r/cswiger/limeosmogsm01/ **

LimeUtil --find
lsusb <-- look for matching device on usb bus xxx yyy

then launch image with:

```docker run -ti --rm --device=/dev/bus/usb/xxx/yyy -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix [IMAGE-NAME] [COMMAND]```


** SOme cheatsheet for using screen: http://aperiodic.net/screen/quick_reference.**

| Window Management           | command|
|:----------------------------------------|:------------------------|
|create new window	| C-a c|
|change to last-visited active window |	C-a C-a (commonly used to flip-flop between two windows)|
|change to window by number |	C-a <number> (only for windows 0 to 9)|
|change to window by number or name |	C-a ' <number or title> |
|change to next window in list	| C-a n or C-a <space>|
|change to previous window in list	|C-a p or C-a <backspace>|
|see window list	|C-a " (allows you to select a window to change to)|
|show window bar	|C-a w (if you don't have window bar)|
|close current window	| Closes all applications in the current window (including shell)|
|kill current window|	C-a k (not recommended)|
|kill all windows	|C-a \ (not recommended)|
|rename current window	|C-a A|

| Split screen        | command|
|:----------------------------------------|:------------------------|
|split display horizontally|	C-a S|
|split display vertically|	C-a \| or C-a V (for the vanilla vertical screen patch)|
|jump to next display region|	C-a tab|
|remove current region|	C-a X|
|remove all regions but the current one	|C-a Q|
