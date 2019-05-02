Install gpio halt
```
apt-get install -y --force-yes wiringpi
cd /tmp
curl -LO --insecure https://github.com/adafruit/Adafruit-GPIO-Halt/archive/master.zip
unzip master.zip
cd Adafruit-GPIO-Halt-master
make
mv gpio-halt /usr/local/bin
```
