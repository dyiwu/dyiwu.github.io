# Install Linux Mint 20 Ulyana Cinnamon

## Install Linux Mint 20 Ulyana Cinnamon
<!--more--> 
### git
Add the Official PPA by GIT, update the repository and then install Git.
```
$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt-get update
$ sudo apt-get install git
```
Reference:
[How to Install Git 2.28.0 in Ubuntu / Linux Mint & CentOS?](https://www.tipsonunix.com/2020/07/install-git-2-28-0-in-ubuntu-20-04-linux-mint-centos/)

---

### Terminator
Just do this for old version 1.91.
```
$ sudo apt-get install terminator
```
For the latest, add the official PPA, update the repository and then install Terminator.
```
$ sudo add-apt-repository ppa:gnome-terminator
$ sudo apt-get update
$ sudo apt-get install terminator
```
Reference:
[Terminator - The robot future of terminals](https://gnometerminator.blogspot.com/)

---

### Google Chrome
First, download the Google signing key and install it.
```
$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
Set up the Google Chrome repository.
```
$ echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
```
Update the repository index, install the stable version.
```
$ sudo apt-get update
$ sudo apt-get  install -y google-chrome-stable
```
Reference:
[How to Install Google Chrome in Linux Mint 20](https://linuxhint.com/install_google_chrome_linux_mint-2/)


---

### Hugo 
Reference:
[Install Hugo from Tarball](https://gohugo.io/getting-started/installing/#install-hugo-from-tarball)

---

### ibus-pinyin
***ibus-pinyin*** is an input method for traditional Chinese.
Here is the installation procedure.
```
$ sudo apt-get install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-doc
$ sudo im-config -s ibus
$ sudo apt install ibus-pinyin
```

---

### Add user
Script to add user
```
#!/bin/bash
groupadd -g 8888 cosmos
useradd -m -u 9009 -g 8888 -c "Dyi-Wu Liu,," -s /bin/bash -G adm,cdrom,sudo,dip,plugdev,lpadmin,sambashare,cosmos dyiwu
```

---

### Evolution
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install evolution
```

---

### fonts

- 文泉驛正黑
  ```
  $ sudo apt-get install fonts-wqy-zenhei
  ```

- 文泉驛微米黑
  ```
  $ sudo apt-get install fonts-wqy-microhei
  ```

- 文泉驛點陣宋體
  ```
  $ sudo apt-get install xfonts-wqy
  ```

- 文鼎字型
  ```
  $ sudo apt-get install fonts-arphic-uming 
  ```

  如要安裝全部文鼎中文字型，可輸入以下指令：
  ```
  $ sudo apt-get install fonts-arphic*
  ```

- 教育部標準楷書
  ```
  $ sudo apt-get install fonts-moe-standard-kai
  ```

- cwTex圓體
  ```
  $ sudo apt-get install fonts-cwtex-yen
  ```

- Noto Sans CJK (思源黑體) 

  1. 先上官網(http://www.google.com/get/noto/)
  複製DOWNLOAD ALL FONTS按鈕的網址，格式大概如下：

      https://noto-website-2.storage.googleapis.com/pkgs/Noto-unhinted.zip

  2. 進入一個新增的資料夾：
      ```
      $ sudo mkdir /usr/share/fonts/truetype/Noto-unhinted
      $ cd /usr/share/fonts/truetype/Noto-unhinted
      ```
  3. 下載字型壓縮檔：
      ```
      $ sudo wget https://noto-website-2.storage.googleapis.com/pkgs/Noto-unhinted.zip
      ```
  4. 解壓縮
      ```
      $ sudo unzip Noto-unhinted.zip
      ```
  5. update font cache and reboot
      ```
      $ sudo fc-cache -f -v
      $ fc-cache -f -v
      $ reboot
      ```

Reference:
- [在樹莓派上安裝中文字型 Install Chinese Fonts with Raspberry Pi](http://studyraspberrypi.blogspot.com/2015/12/install-chinese-fonts.html)
- [Raspberry Pi 筆記(38)：系統語系與中文輸入法](https://atceiling.blogspot.com/2017/03/raspberry-pi_26.html)

---

### Remmina
[Remmina](https://remmina.org/) is a remote desktop client written in GTK+, aiming to be useful for system administrators and travellers, who need to work with lots of remote computers in front of either large monitors or tiny netbooks.

Remmina supports multiple network protocols in an integrated and consistent user interface.

The profile options relax-order-checks and glyph-cache, which are under profile's advanced tab, need be enabled to make remmnia work in Raspberry Pi.

xfreerdp /w:1900 /v:MyDNSServername /relax-order-checks +glyph-cache

An official PPA with Remmina 1.4.7 release can be installed by copying and pasting this in a terminal:
```
$ sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
$ sudo apt update
$ sudo apt install remmina remmina-plugin-rdp remmina-plugin-secret
$ sudo apt install freerdp2-x11 remmina-plugin-nx remmina-plugin-exec remmina-plugin-kwallet remmina-plugin-xdmcp remmina-plugin-www fam qt5-qmltooling-plugins
```

Make sure Remmina is not running. Either close it, reboot, or kill it by pasting this in a terminal:
```
$ sudo killall remmina
```
List available plugins with apt-cache search remmina-plugin. By default RDP, SSH and SFTP are installed. 
```
$ apt-cache search remmina-plugin
```
Reference:
[How to install Remmina](https://remmina.org/how-to-install-remmina/)

### SAMBA Client
Access network SAMBA share from Pi Client

Install required packages
```
$ sudo apt-get install samba-common smbclient samba-common-bin cifs-utils
```

Create directory for mount point
```
$ sudo mkdir /mnt/ds420p
$ sudo mkdir /mnt/ds420p/video
$ sudo mkdir /mnt/ds420p/music
$ sudo mkdir /mnt/ds420p/photo
```
 
Create a credential file for saving the user name and password.
```
$ sudo cat /root/smb.cred
username=xyz
password=password_of_xyz
domain=workgroup
```

Save this credential file and change its permissions so it is not readable by others. 
```
$ sudo chown root:root /root/smbcred
$ sudo chmod 600 /root/smbcred
```

Create entries in /etc/fstab
```
$ grep cifs /etc/fstab
//ds420p/video  /mnt/ds420p/video cifs credentials=/root/smb.cred  0  0
//ds420p/music  /mnt/ds420p/music cifs credentials=/root/smb.cred  0  0
//ds420p/photo  /mnt/ds420p/photo cifs credentials=/root/smb.cred  0  0

```
Test it,
```
$ sudo mount -a
$ df | grep ds420p
//ds420p/video 7742705752 7561820676 180885076  98% /mnt/ds420p/video
//ds420p/music 7742705752 7561820676 180885076  98% /mnt/ds420p/music
//ds420p/photo 7742705752 7561820676 180885076  98% /mnt/ds420p/photo
```

Reference:
- [25 Things to Do After Installing Linux Mint 20 Ulyana](https://averagelinuxuser.com/linuxmint20-after-install/)
- [Important Things to Do After Installing Linux Mint 20](https://linuxhint.com/to_do_after_install-_linux_mint_20/)

