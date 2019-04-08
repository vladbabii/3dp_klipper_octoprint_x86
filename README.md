# Install Klipper and Octoprint On Lubuntu

create user named octopus
```
useradd octopus
mkdir /home/octopus
chmod -R 755 /home/octopus
chown -R octopus:octopus /home/octopus
```

install octoprint
```
apt update
apt install python-pip python-dev python-setuptools python-virtualenv git libyaml-dev build-essential
mkdir /opt/octoprint
chmod -R 777 /opt/octoprint
cd /opt/octoprint
virtualenv venv
source venv/bin/activate
pip install pip --upgrade
pip install https://get.octoprint.org/latest
```
