# Prepare SD card

## Edit boot/config.txt
> \# See /boot/overlays/README for all available options
> 
> gpu_mem=128
> start_file=start_x.elf
> fixup_file=fixup_x.dat
> dtparam=sd_overclock=100
> initramfs initramfs-linux.img followkernel


## Edit boot/cmdline.txt
> root=/dev/mmcblk0p2 rw rootwait console=ttyAMA0,115200 
console=tty1 selinux=0 plymouth.enable=0 
smsc95xx.turbo_mode=N dwc_otg.lpm_enable=0 
kgdboc=ttyAMA0,115200 elevator=noop cgroup_enable=memory 
cgroup_memory=1


# On the Raspberry Pi
## First steps
> \# passwd
> \# pacman -Syu
> \# pacman -S sudo git base-devel ttf-liberation

Edit _/etc/sudoers_ and remove _#_ before
> \#\# Same thing without a password
> \# %wheel ALL=(ALL) NOPASSWD: ALL

## Prepare users and home
> \# userdel alarm
> \# rm -rf /home/alarm

Update _/etc/fstab_ and add:
> /dev/mmcblk0p3	/home	ext4	defaults	0	
0

> \# mount -a
> \# groupadd alban
> \# useradd -g alban -G wheel -m alban

## Install wireless

> \# pacman -S netctl
> \# cd /etc/netctl
> \# install -m640 examples/wireless-wpa wireless-home
> \# nano wireless-home \# patch network name and key
> \# netctl enable wireless-home

## Prepare X

> $ sudo pacman -S xorg-server xf86-video-fbdev 
xorg-xrefresh xorg-util-macros xorg-server-devel
> $ make gcc git-core automake autoconf pkg-config 
libtool
> $ git clone https://github.com/robclark/libdri2.git
> $ git clone https://github.com/ssvb/xf86-video-fbturbo
> $ cd libdri2
> $ ./autogen.sh --prefix=/usr
> $ sudo make install
> $ cd ..
> $ cd xf86-video-fbturbo
> $ autoreconf -vi
> $ ./configure --prefix=/usr
> $ make
> $ nano src/fbdev.c # replace #if <whatever> by #if 0
> $ make
> $ sudo make install
> $ sudo cp xorg.conf 
/usr/share/X11/xorg.conf.d/99-fbturbo.conf

## Install xfce

> $ sudo pacman -S xfce4 xfce4-goodies xarchiver 
pavucontrol sddm
> $ sudo sh -c "sddm --example-config > /etc/sddm.conf"
> $ packer -S archlinux-themes-sddm
> $ sudo systemctl enable sddm

## Install work stuff

> $ sudo pacman -S qtcreator midori 
raspberrypi-firmware-tools

## Install cgroups tools

> $ git clone https://git.code.sf.net/p/libcg/libcg 
libcg-libcg
> $ cd libcg-libcg/
> $ autoreconf -vi
> $ ./configure 
> $  make
> $ sudo make install


