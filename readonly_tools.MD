Install gpio halt
```
apt-get install -y --force-yes wiringpi
cd /tmp
curl -LO --insecure https://github.com/adafruit/Adafruit-GPIO-Halt/archive/master.zip
unzip master.zip
cd Adafruit-GPIO-Halt-master
make
mv gpio-halt /usr/local/bin
cd /tmp
rm -rf Adafruit-GPIO-Halt-master
```

```
  HALT_PIN="21"
  grep gpio-halt /etc/rc.local >/dev/null
	if [ $? -eq 0 ]; then
		# gpio-halt already in rc.local, but make sure correct:
		sed -i "s/^.*gpio-halt.*$/\/usr\/local\/bin\/gpio-halt $HALT_PIN \&/g" /etc/rc.local >/dev/null
	else
		# Insert gpio-halt into rc.local before final 'exit 0'
		sed -i "s/^exit 0/\/usr\/local\/bin\/gpio-halt $HALT_PIN \&\\nexit 0/g" /etc/rc.local >/dev/null
	fi
```
