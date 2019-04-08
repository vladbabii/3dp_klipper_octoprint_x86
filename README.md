# Install Klipper and Octoprint On Lubuntu

- all commands are run starting with root in each section

# Create user octopus
```
useradd octopus
mkdir /home/octopus
chmod -R 755 /home/octopus
chown -R octopus:octopus /home/octopus
usermod -s /bin/bash octopus
passwd -d octopus
usermod -a -G tty octopus
usermod -a -G dialout octopus
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
