# Install Klipper and Octoprint On Lubuntu

- all commands are run starting with root in each section

# Create user octopus
```
useradd octopus
mkdir /home/octopus
chmod -R 755 /home/octopus
chown -R octopus:octopus /home/octopus
usermod -s /bin/bash octopus
usermod -a -G tty octopus
usermod -a -G dialout octopus
```

# Set password to octopus user
```
passwd octopus
```

# Install octoprint
```
apt update
apt install python-pip python-dev python-setuptools python-virtualenv git libyaml-dev build-essential
mkdir /opt/octoprint
chmod -R 777 /opt/octoprint
cd /opt/octoprint
su octopus
virtualenv venv
source venv/bin/activate
pip install pip --upgrade
pip install https://get.octoprint.org/latest
```

# Octoprint automatic startup files
```
wget https://github.com/foosel/OctoPrint/raw/master/scripts/octoprint.init && mv octoprint.init /etc/init.d/octoprint
wget https://github.com/foosel/OctoPrint/raw/master/scripts/octoprint.default && mv octoprint.default /etc/default/octoprint
chmod +x /etc/init.d/octoprint
```

# Configure octoprint default settings
```
cp /etc/default/octoprint /home/octopus/etc.default.octoprint
sed -i "s/pi/octopus/g" /etc/default/octoprint
sed -i "s/#BASEDIR=/BASEDIR=/g" /etc/default/octoprint
sed -i "s/#CONFIGFILE=/CONFIGFILE=/g" /etc/default/octoprint
sed -i "s/#DAEMON=\/home\/octopus\/OctoPrint\/venv\/bin\/octoprint/DAEMON=\/opt\/octoprint\/venv\/bin\/octoprint/g" /etc/default/octoprint
sed -i "s/UMASK=022/UMASK=000/g" /etc/default/octoprint
```

# Octoprint automatic enable
```
update-rc.d octoprint defaults
```

In Settings > Commands, configure the following commands:
- Restart OctoPrint: sudo service octoprint restart
- Restart system: sudo shutdown -r now
- Shutdown system: sudo shutdown -h now

# Octoprint allow access to shutdown command
```
echo "octopus ALL=NOPASSWD: /sbin/shutdown" > /etc/sudoers.d/octoprint-shutdown
```

# Octoprint allow access to service command
```
echo "octopus ALL=NOPASSWD: /usr/sbin/service" > /etc/sudoers.d/octoprint-service
```

# Prepare klipper files
```
mkdir /opt/klipper
chmod -R 777 /opt/klipper
chown -R octopus:octopus /opt/klipper
cd /opt/klipper
su octopus
git clone https://github.com/KevinOConnor/klipper .
exit
cp ./scripts/install-ubuntu-18.04.sh ./scripts/install-custom.sh
sed -i 's/KLIPPER_USER="octoprint"/KLIPPER_USER="octopus"/g' ./scripts/install-custom.sh
./scripts/install-custom.sh
```

# Build micro-controller firmware
```
make menuconfig 
make
```

# Flash micro-controller (manual step)
```
service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/...
service klipper start
```

# Copy printer config file to home folder and restart klipper
# !! CHANGE generic-ramps.cfg to whatever is best for you !!
```
su octopus
cp /opt/klipper/config/generic-ramps.cfg /home/octopus/printer.cfg
exit
service klipper restart
```

# Octoprint install klipper plugin (manual step)
- go to settings > plugin manager > search for klipper > install octoklipper
- restart octoprint
- go to settings > octoklipper > basic > klipper config path = "/home/octopus/printer.cfg"
- restart octoprint

# Setup the serial port to always mount to the same path (less hassle when plugging/unplugging stuff)

# - create automation on serial connect, if needed (optional)
```
mkdir -p /opt/action-onplug
touch /opt/action-onplug/auto.sh
chmod +x /opt/action-onplug/auto.sh
chmod -R 755 /opt/action-onplug
chown -R octopus:octopus /opt/action-onplug
```

# - map serial to static /dev/-something- (in this example printerserial) by usb device id
use
```
lsusb
```
and you'll see a list, then plugin device then run that again. A new device should have popped up, something like (but different names)
```
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```
copy the id (1d6b:0002 in this case and replace it in the command below)
```
echo "SUBSYSTEM==\"tty\", ATTRS{idVendor}==\"1d6b\", ATTRS{idProduct}==\"0002\", SYMLINK+=\"printerserial\", RUN+=\"/opt/action-onplug/auto.sh\"" > /etc/udev/rules.d/99-zwave-stick.rules
```
- unplug device
- plug device back in and you should see the serial with
```
ls /dev/printerserial
```
now modify */home/octopus/printer.cfg* and set 
```
serial: /dev/printerserial
```
then restart klipper
```
service klipper restart
```

# - map multiple serial devices to multiple fixed /dev/-something-s (TODO)


