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

# Configure octoprint
```

```

# Octoprint automatic startup
```
wget https://github.com/foosel/OctoPrint/raw/master/scripts/octoprint.init && mv octoprint.init /etc/init.d/octoprint
wget https://github.com/foosel/OctoPrint/raw/master/scripts/octoprint.default && mv octoprint.default /etc/default/octoprint
chmod +x /etc/init.d/octoprint
update-rc.d octoprint defaults
```

