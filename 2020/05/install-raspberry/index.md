# Install Raspberry

## Install Raspberry
<!--more--> 

### ibus-pinyin
***ibus-pinyin*** is an input method for traditional Chinese.
Here is the installation procedure.
```
$ sudo apt install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-qt4
$ im-config -s ibus
$ sudo apt install ibus-pinyin
```

### Terminator
***Terminator*** is a GNOME based terminal.
Here is the installation procedure.

```
$ sudo apt-get install terminator
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





