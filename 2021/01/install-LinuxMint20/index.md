# Install Linux Mint 20.1 Ulyssa Cinnamon

## Install Linux Mint 20.1 Ulyssa Cinnamon
<!--more--> 
### Mint

- [Linux Mint Home](https://linuxmint.com/)
- [Linux Mint Documentation](https://linuxmint.com/documentation.php)
- [Linux Mint Forum](https://forums.linuxmint.com/)
- [Linux Mint Blog](https://blog.linuxmint.com/)
```
$ cat /etc/os-release 
NAME="Linux Mint"
VERSION="20.1 (Ulyssa)"
ID=linuxmint
ID_LIKE=ubuntu
PRETTY_NAME="Linux Mint 20.1"
VERSION_ID="20.1"
HOME_URL="https://www.linuxmint.com/"
SUPPORT_URL="https://forums.linuxmint.com/"
BUG_REPORT_URL="http://linuxmint-troubleshooting-guide.readthedocs.io/en/latest/"
PRIVACY_POLICY_URL="https://www.linuxmint.com/"
VERSION_CODENAME=ulyssa
UBUNTU_CODENAME=focal

$ cat /etc/lsb-release
DISTRIB_ID=LinuxMint
DISTRIB_RELEASE=20.1
DISTRIB_CODENAME=ulyssa
DISTRIB_DESCRIPTION="Linux Mint 20.1 Ulyssa"

```

---

### Disk Partitions
```
$ sudo fdisk -l /dev/sda
Disk /dev/sda: 698.65 GiB, 750156374016 bytes, 1465149168 sectors
Disk model: WDC WD7500BPKT-7
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x4057e768

Device     Boot    Start        End    Sectors  Size Id Type
/dev/sda1           2048     999423     997376  487M  b W95 FAT32
/dev/sda2  *      999424    3000319    2000896  977M 83 Linux
/dev/sda3        3000320   35000319   32000000 15.3G 82 Linux swap / Solaris
/dev/sda4       35000320 1465147391 1430147072  682G 83 Linux

$ df |grep -e Mount -e sda
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sda4      702801160 12192708 654838392   2% /
/dev/sda2         968312   309968    591940  35% /boot
/dev/sda1         497696        4    497692   1% /boot/efi
```

### Git
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
```bash
sudo apt-get install terminator
```

For the latest, add the official PPA, update the repository and then install Terminator.

```bash
sudo add-apt-repository ppa:mattrose/terminator
sudo apt-get update
sudo apt install terminator
```

Reference:
- [Installing Terminator](https://github.com/gnome-terminator/terminator/blob/master/INSTALL.md#installing-terminator)
- [Terminator - The robot future of terminals](https://gnometerminator.blogspot.com/)
---

### Google Chrome
First, download the Google signing key and install it.
```bash
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
Set up the Google Chrome repository.
```bash
echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
```
Update the repository index, install the stable version.
```bash
sudo apt-get update
sudo apt-get install -y google-chrome-stable
```
Reference:
[How to Install Google Chrome in Linux Mint 20](https://linuxhint.com/install_google_chrome_linux_mint-2/)


---

### Hugo 
Reference:
[Install Hugo from Tarball](https://gohugo.io/getting-started/installing/#install-hugo-from-tarball)

---

### Ibus-pinyin
***ibus-pinyin*** is an input method for traditional Chinese.
Here is the installation procedure.
```
$ sudo apt-get install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-doc
$ sudo im-config -s ibus
$ sudo apt install ibus-pinyin
```
To set ibus as preference by adding these environment variables into ~/.bashrc .

```bash
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
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
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install evolution
```

---

### Fonts

- 文泉驛正黑
  ```bash
  sudo apt-get install fonts-wqy-zenhei
  ```

- 文泉驛微米黑
  ```bash
  sudo apt-get install fonts-wqy-microhei
  ```

- 文泉驛點陣宋體
  ```bash
  sudo apt-get install xfonts-wqy
  ```

- 文鼎字型
  ```bash
  sudo apt-get install fonts-arphic-uming 
  ```

  如要安裝全部文鼎中文字型，可輸入以下指令：
  ```bash
  sudo apt-get install fonts-arphic*
  ```

- 教育部標準楷書
  ```bash
  sudo apt-get install fonts-moe-standard-kai
  ```

- cwTex圓體
  ```bash
  sudo apt-get install fonts-cwtex-yen
  ```

- Noto Sans CJK (思源黑體) 

  1. 先上官網(http://www.google.com/get/noto/)
  複製DOWNLOAD ALL FONTS按鈕的網址，格式大概如下：

      https://noto-website-2.storage.googleapis.com/pkgs/Noto-unhinted.zip

  2. 進入一個新增的資料夾：
      ```bash
      sudo mkdir /usr/share/fonts/truetype/Noto-unhinted
      cd /usr/share/fonts/truetype/Noto-unhinted
      ```
  3. 下載字型壓縮檔：
      ```bash
      sudo wget https://noto-website-2.storage.googleapis.com/pkgs/Noto-unhinted.zip
      ```
  4. 解壓縮
      ```bash
      sudo unzip Noto-unhinted.zip
      ```
  5. update font cache and reboot
      ```bash
      sudo fc-cache -f -v
      fc-cache -f -v
      sudo reboot
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
sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
sudo apt update
sudo apt install remmina remmina-plugin-rdp remmina-plugin-secret
sudo apt install freerdp2-x11 remmina-plugin-nx remmina-plugin-exec remmina-plugin-kwallet remmina-plugin-xdmcp remmina-plugin-www fam qt5-qmltooling-plugins
```

Make sure Remmina is not running. Either close it, reboot, or kill it by pasting this in a terminal:
```
sudo killall remmina
```
List available plugins with apt-cache search remmina-plugin. By default RDP, SSH and SFTP are installed. 
```
apt-cache search remmina-plugin
```
Reference:
[How to install Remmina](https://remmina.org/how-to-install-remmina/)

### Samba Client
Access network SAMBA share from Pi Client

Install required packages
```
sudo apt-get install samba-common smbclient samba-common-bin cifs-utils
```

Create directory for mount point
```
sudo mkdir /mnt/ds420p
sudo mkdir /mnt/ds420p/video
sudo mkdir /mnt/ds420p/music
sudo mkdir /mnt/ds420p/photo
```

Create a credential file for saving the user name and password.
```
sudo cat /root/smb.cred
username=xyz
password=password_of_xyz
domain=workgroup
```

Save this credential file and change its permissions so it is not readable by others. 
```
sudo chown root:root /root/smbcred
sudo chmod 600 /root/smbcred
```

Create entries in /etc/fstab
```
grep cifs /etc/fstab
//ds420p/video  /mnt/ds420p/video cifs credentials=/root/smb.cred  0  0
//ds420p/music  /mnt/ds420p/music cifs credentials=/root/smb.cred  0  0
//ds420p/photo  /mnt/ds420p/photo cifs credentials=/root/smb.cred  0  0

```
Test it,
```
sudo mount -a
df | grep ds420p
//ds420p/video 7742705752 7561820676 180885076  98% /mnt/ds420p/video
//ds420p/music 7742705752 7561820676 180885076  98% /mnt/ds420p/music
//ds420p/photo 7742705752 7561820676 180885076  98% /mnt/ds420p/photo
```
### Typora
[Typora](https://typora.io/) is a truly minimal markdown editor.

Add repository server's key by
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
```
or
```
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
```

Add Typora repository
```
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update
```

Install Typora
```
sudo apt-get install typora
```

### Reference:
- [25 Things to Do After Installing Linux Mint 20 Ulyssa](https://averagelinuxuser.com/linuxmint20-after-install/)
- [Important Things to Do After Installing Linux Mint 20](https://linuxhint.com/to_do_after_install-_linux_mint_20/)
- [10 Things to Do First in Linux Mint 20.1 Cinnamon](https://easylinuxtipsproject.blogspot.com/p/first-mint-cinnamon.html)




