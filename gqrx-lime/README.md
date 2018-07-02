Built on top of grc-lime, which is built on lime-base, which is built on ubuntu:latest.

To Run:
docker run -ti --rm --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jumson/limesdr:gqrx-lime

Current issue: 
Not running in Kali 2018, pulseaudio issues.
Potential solution: https://gist.github.com/RuCodee/5482ca50b81d1379a35ce49d280344fa