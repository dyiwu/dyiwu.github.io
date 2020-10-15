# Install Raspberry

## Install Raspberry
<!--more--> 

### ibus-pinyin
***ibus-pinyin*** is an input method for traditional Chinese.
Here is the installation procedure.
```
$ sudo apt install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-qt4
$ sudo im-config -s ibus
$ sudo apt install ibus-pinyin
```

### Terminator
***Terminator*** is a GNOME based terminal.
Here is the installation procedure.

```
$ sudo apt-get install terminator
```

### Add user
Scripe to add user
```
#!/bin/bash
groupadd -g 8888 cosmos
useradd -m -u 9009 -g 8888 -c ",," -s /bin/bash -G adm,dialout,cdrom,sudo,audio,
video,plugdev,games,users,input,netdev,spi,i2c,gpio dyiwu
```

### fonts

#### Reference:
- [在樹莓派上安裝中文字型 Install Chinese Fonts with Raspberry Pi](http://studyraspberrypi.blogspot.com/2015/12/install-chinese-fonts.html)
- [Raspberry Pi 筆記(38)：系統語系與中文輸入法](https://atceiling.blogspot.com/2017/03/raspberry-pi_26.html)

### Remmina
[Remmina](https://remmina.org/) is a remote desktop client written in GTK+, aiming to be useful for system administrators and travellers, who need to work with lots of remote computers in front of either large monitors or tiny netbooks.

Remmina supports multiple network protocols in an integrated and consistent user interface.

The profile options relax-order-checks and glyph-cache, which are under profile's advanced tab, need be enabled to make remmnia work in Raspberry Pi.

xfreerdp /w:1900 /v:MyDNSServername /relax-order-checks +glyph-cache


#### Reference:
- [How to install Remmina](https://remmina.org/how-to-install-remmina/)

### SAMBA Client
Access network SAMBA share from Pi Client

Install required packages
```
$ sudo apt-get install  samba-common smbclient samba-common-bin smbclient  cifs-utils
```

Create directory for mount point
```
$ sudo mkdir /mnt/DS420P
$ sudo mkdir /mnt/DS420P/video
$ sudo mkdir /mnt/DS420P/music
$ sudo mkdir /mnt/DS420P/photo
```
 
Create a credential file for saving the user name and password.
```
$ sudo cat /home/pi/.smbcred
username=xyz
password=password_of_xyz
```

Save this credential file and change its permissions so it is not readable by others. 
```
$ sudo chown xyz:xyz /home/pi/.smbcred
$ sudo chmod 600 /home/pi/.smbcred
```

Create entries on /etc/fstab
```
$ grep cifs /etc/fstab
//ds420p/video  /mnt/DS420P/video cifs credentials=/home/pi/.smbcred  0  0
//ds420p/music  /mnt/DS420P/music cifs credentials=/home/pi/.smbcred  0  0
//ds420p/photo  /mnt/DS420P/photo cifs credentials=/home/pi/.smbcred  0  0

```
Test it,
```
$ sudo mount -a
$ df | grep DS420P
//ds420p/video 7742705752 7561820676 180885076  98% /mnt/DS420P/video
//ds420p/music 7742705752 7561820676 180885076  98% /mnt/DS420P/music
//ds420p/photo 7742705752 7561820676 180885076  98% /mnt/DS420P/photo
```

### Pi 3B USB boot

#### Reference
- [How to Make Raspberry Pi 3 Boot From USB](https://www.makeuseof.com/tag/make-raspberry-pi-3-boot-usb/)




