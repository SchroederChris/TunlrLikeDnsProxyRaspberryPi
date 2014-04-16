NetflixDnsProxyRaspberryPi
==========================
t3slider describes how to set up a tunlr clone on a generic linux machine [here](https://github.com/t3slider/Tunlr-Clone-Non-SNI). 

This tutorial describes the specifics when using a RaspberryPi for the local proxy server.

###Basic Setup
* [Download arch linux for the pi](http://www.raspberrypi.org/downloads/)
* Put it on an SD card, boot the pi and log in.
* Update arch: 
```bash
pacman -Syu
```
* install sudo
```bash
pacman -S sudo
```
