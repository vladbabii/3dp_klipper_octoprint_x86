from: https://petr.io/en/blog/2015/11/09/read-only-raspberry-pi-with-jessie/

```
apt-get update
apt install -y htop nano screen
apt-get remove --purge -y wolfram-engine triggerhappy cron logrotate dbus dphys-swapfile xserver-common lightdm fake-hwclock
apt-get autoremove --purge
apt-get install -y busybox-syslogd; dpkg --purge rsyslog
```


cat /boot/cmdline.txt 

and add "fastboot noswap ro" at the end

example:

```
dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 root=... rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait fastboot noswap ro
```


```
rm -rf  /var/spool
ln -s /tmp /var/spool
```


vim /etc/ssh/sshd_config

```
UsePrivilegeSeparation no
```



Make edits to the fstab
root@raspberrypi:/home/pi# cat /etc/fstab 


```
proc            /proc           proc    defaults          0       0
/dev/mmcblk0p1  /boot           vfat    defaults,ro          0       2
/dev/mmcblk0p2  /               ext4    defaults,noatime,ro  0       1
tmpfs	/var/log	tmpfs   nodev,nosuid	0	0
tmpfs	/var/tmp	tmpfs	nodev,nosuid	0	0
tmpfs   /tmp        tmpfs   nodev,nosuid    0   0
```
Last 3 lines and RO flag for /dev/mmcblk0p* are new additions.


```
touch /tmp/dhcpcd.resolv.conf
rm /etc/resolv.conf
ln -s /tmp/dhcpcd.resolv.conf /etc/resolv.conf
```

place the below at the end of your /etc/bash.bashrc
```
# set variable identifying the filesystem you work in (used in the prompt below)
fs_mode=$(mount | sed -n -e "s/^.* on \/ .*(\(r[w|o]\).*/\1/p")
# alias ro/rw 
alias ro='mount -o remount,ro / ; fs_mode=$(mount | sed -n -e "s/^.* on \/ .*(\(r[w|o]\).*/\1/p")'
alias rw='mount -o remount,rw / ; fs_mode=$(mount | sed -n -e "s/^.* on \/ .*(\(r[w|o]\).*/\1/p")'
# setup fancy prompt
export PS1='\[\033[01;32m\]\u@\h${fs_mode:+($fs_mode)}\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
# aliases for mounting boot volume
alias roboot='mount -o remount,ro /boot'
alias rwboot='mount -o remount,rw /boot'
```

