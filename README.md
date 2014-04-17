Setting up a DNS Proxy for watching Netflix outside the US on the Raspberry Pi
==============================================================================
t3slider describes how to set up a tunlr clone on a generic linux machine [here](https://github.com/t3slider/Tunlr-Clone-Non-SNI). 

This tutorial describes the specifics when using a Raspberry Pi for the local proxy server.

###Basic Setup
[Download arch linux for the pi](http://www.raspberrypi.org/downloads/)

Put it on an SD card, boot the pi and log in as root.

Update arch: 
```
pacman -Syu
```

Install sudo:
```
pacman -S sudo
```

Install adduser:
```
pacman -S adduser
```

Add a user:
```
adduser
```

Enable sudo for the new user:
```
EDITOR=nano visudo
add the following line: YOUR_USER_NAME ALL=(ALL) ALL
```


###17 IP adresses
Create an rc.local file:
```bash
nano /etc/rc.local
```

Insert the script from the [original tutorial](https://github.com/t3slider/Tunlr-Clone-Non-SNI).

Make the file executable:
```
chmod a+x /etc/rc.local
```

Create a service for that file:
```bash
nano /etc/systemd/system/rc-local.service
```

Insert the following content:
```
[Unit]
Description=/etc/rc.local Compatibility
ConditionPathExists=/etc/rc.local

[Service]
Type=oneshot
ExecStart=/etc/rc.local start
StandardOutput=tty
RemainAfterExit=yes
SysVStartPriority=99

[Install]
WantedBy=multi-user.target
```

Enable the service:
```
systemctl enable rc-local.service
```

Reboot and log in with the new user account.


###Install haproxy
haproxy is not available from pacman, so we need to install it from aur.

First, install the base-devel package:
```bash
sudo pacman -S base-devel
```

Create a directory for aur builds:
```
mkdir aur
cd aur
```

Download the archive from aur and unzip it:
```
wget https://aur.archlinux.org/packages/ha/haproxy-devel/haproxy-devel.tar.gz
tar xvzf haproxy-devel.tar.gz
cd haproxy-devel
```

The pi's armv6h architecture is not officially supported, but it will build, so we simply add it:
```
nano PKGBUILD
add 'armv6h' to the arch list
```

Build haproxy:
```
makepkg -sic
```

Modify haproxy.cfg as described in the [original tutorial](https://github.com/t3slider/Tunlr-Clone-Non-SNI).

Test haproxy by starting it manually:
```
haproxy -f /etc/haproxy/haproxy.cfg -d
```

If it works, enable and start the service:
```
sudo systemctl enable haproxy
sudo systemctl start haproxy
```

###Remote proxy and DNSmasq
See the [original tutorial](https://github.com/t3slider/Tunlr-Clone-Non-SNI)
