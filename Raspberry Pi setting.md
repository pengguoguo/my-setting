2014-08-06: I learn markdown also here: https://help.github.com/articles/github-flavored-markdown

Purpose of Pi: (2014-08)
========
1. Help to maintace the daily operation of hobototes (e.g. When we travel around the world, we need a small-enough computer for operation.).
2. As a device to operate all-day-long with low power consumption. (verse the power a notebook needs)
3. Runs the hobototes-data-centric apps on it. (To free my notebook from always-turning-on. Moving django project to it.)
4. Runs as a host to share files. (BTSync, verse Dropbox)
5. Needs no maintance once after setup. Once it's broken, it is brain-less to be rebuilt the software. (0-day recovery.) (verse PC, which the storage, power suppile, display unit are easy to be broken, and making heat. PC needs to be setup again once a hardawre is replaced. Time consuming.)
6. Plug and play,


----
Raspberry for hobototes.
====
### 1st stage:
* The official Raspbian image with SSH, git, VNC, btsync, dropbox and other system-admin-related stuffs.
* Setup a dynamic dns. (Therefore, I can connect the web apps when I am not on the local network.)
* Make a backup of the SD card before going to the 2nd stage.

### 2nd stage:
* Preparation for running [Django] (http://djangoproject.com). Upgrade [python] to version 3.4 when need.
* pip, virtualenv & virtualenvwrapper (I need to learn both of them.)
* Setup a web server (nginx), database (MySQL), Django, phpmyadmin

### 3rd stage:
* Write the apps for the business.
** hobototes-data-centric apps.
** Automate the calculation.
* Doing some automation, e.g. automate the database backup.
* Doing some experiment, such as Rails4, node.js.
----
Service starts after power on
====
VNC

BTSync

Django

----
2014-08-06:
je09
In the 1st boot up:

Expand the disk, set locale, turn on SSH, reboot.

    sudo apt-get install update && apt-get install upgrade
    dpkg

### History
2014-08-06: Initial the system.

2014-08-09: Initial the system with a few sys admin tools setup. Preparing to be a python web server next time. Backup as an image. Learning.

### Custom software on Pi:
tightvncserver, vim-gtk,  git, transmission, chromium,
htop, nmap, tree

(No Dvorak keyboard layout setup is needed, as VNC transfer my key layout to the pi. :D)

#### not yet:
[sublime, utorrent, btsync, Dropbox
python & virtualenv, Django, MySQL, rails, nginx, phpmyadmin
]

### Lookup IP
    ifconfig
    hostname -I

ref: http://www.raspberrypi.org/documentation/troubleshooting/hardware/networking/ip-address.md

Setup ssh
raspi-config
reserve a fixed ip for pi, e.g. 192.168.0.101, for always on connection
ssh pi@192.168.0.101

ref: http://www.raspberrypi.org/documentation/remote-access/ssh/unix.md

Setup VNC
sudo apt-get install tightvncserver
tightvncserver
vncserver :1 -geometry 1920x1080 -depth 24

Creating default startup script /home/pi/.vnc/xstartup
Starting applications specified in /home/pi/.vnc/xstartup
Log file is /home/pi/.vnc/raspberrypi:1.log

vncviewer, 192.168.0.101:1

[[NoIP for accessing Pi over internet. In fact, You no need to set up static IP at your Raspberry. Almost all home routers has option to bind MAC number of you network card with IP. If you do so, you'll get the same IP adress after every reboot.]]

ref: http://www.raspberrypi.org/documentation/remote-access/vnc/README.md
http://computers.tutsplus.com/tutorials/take-control-of-your-raspberry-pi-using-your-mac-pc-ipad-or-phone--mac-54603

Setup GVim
sudo apt-get install vim-gtk (or vim-gnome)

gvim (vim can run over ssh)

Setup Sublime
http://www.sublimetext.com/

Setup BTSync
cd ~/BTSync
./btsync (or ./btsync --config ./sync.conf)
127.0.0.1:8888

sudo nano /etc/apt/sources.list.d/btsync.list
past following lines: 
deb http://debian.yeasoft.net/btsync wheezy main contrib non-free
deb-src http://debian.yeasoft.net/btsync wheezy main contrib non-free
Control + x to close/save the file.
sudo apt-get update
sudo apt-get install btsync
Config: Default Sync instance? Yes
http://raspberrypihelp.net/tutorials/46-raspberry-pi-bittorrent-sync-bitsync

ref: 
http://blog.bittorrent.com/2013/08/20/sync-hacks-how-to-sync-without-data-loss-using-btsync-raspberry-pi/ <-- this one useful
http://blog.bittorrent.com/2014/08/05/sync-stories-dual-backup-with-a-beaglebone-black-and-virtual-private-server/ shows using BTSync with BeagleBone in a founded business.
	https://gist.github.com/johnantoni/8199088
http://blog.bittorrent.com/2013/05/23/how-i-created-my-own-personal-cloud-using-bittorrent-sync-owncloud-and-raspberry-pi/
http://reustle.io/blog/btsync-pi

Setup Python for dev (enhance it) http://docs.python-guide.org/en/latest/
pip
sudo apt-get install python-pip

Setup virtualenv
Google: raspberry pi virtualenv
gets you some common Python tools (distribute for packaging, pip for easy installation/removal of packages & virtualenv for nice isolated environments)
# sudo apt-get install python-dev
# curl -O http://python-distribute.org/distribute_setup.py
# python distribute_setup.py
# curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
# python get-pip.py
sudo pip install virtualenv
http://raspberry.io/wiki/how-to-get-python-on-your-raspberrypi/
http://flask.pocoo.org/docs/installation/#virtualenv

http://www.raspberrypi.org/forums/viewtopic.php?t=7208&p=403771

To disable virtualenv
Google: virtualenv disable
deactivate
https://www.hackerschool.com/blog/14-there-is-no-magic-virtualenv-edition
http://docs.python-guide.org/en/latest/dev/virtualenvs/

Setup virtualenvwrapper
http://docs.python-guide.org/en/latest/dev/virtualenvs/

Setup Django

Setup Rails

http://computers.tutsplus.com/tutorials/how-to-install-ruby-on-rails-on-raspberry-pi--cms-21421

Setup Nginx
sudo apt-get install nginx


ref: http://www.raspberrypi.org/documentation/remote-access/web-server/nginx.md


Backup

Backup disk image
sudo dd bs=4M if=/dev/sdb of=raspbian.img (or dcfldd)
dd if=/path/to/image of=/dev/sdb
ref: Google: raspberry backup
http://raspberrypi.stackexchange.com/questions/311/how-do-i-backup-my-raspberry-pi

or combian use of bzip to compress the image!
dd if=/dev/sdx | gzip > /path/to/image.gz
gzip -dc /path/to/image.gz | dd of=/dev/sdx 

or dump the SD card, and pipe it with ssh:
dd bs=1M if=/dev/mmcblk0 | ssh user@host 'dd of=/remote/path/to/sdcard.img'
with compression:
dd bs=1M if=/dev/mmcblk0 | ssh user@host 'gzip -9 > /remote/path/to/sdcard.img.gz'
http://www.raspberrypi.org/forums/viewtopic.php?p=118519

Set Fonts
Google: linux install font

Source Code Pro
Google: Source code pro
http://blog.typekit.com/2012/09/24/source-code-pro/
https://github.com/adobe-fonts/source-code-pro
http://sourceforge.net/projects/sourcecodepro.adobe/


http://www.playpcesor.com/2014/07/source-han-sans-cht-adobe-google.html
http://blog.typekit.com/alternate/source-han-sans-cht/
http://www.playpcesor.com/2014/07/google-chrome-font.html
Fonts:
http://sourceforge.net/adobe/source-han-sans/
http://github.com/adobe-fonts/source-han-sans/
http://www.google.com/get/noto/#/family/noto-sans-hant