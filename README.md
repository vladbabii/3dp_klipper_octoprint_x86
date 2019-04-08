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
