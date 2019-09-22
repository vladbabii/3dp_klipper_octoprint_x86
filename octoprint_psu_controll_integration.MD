```
#!/bin/bash

echo "Octoprint: connect"

OCTOAPI="http://localhost:5000/api/"
APIKEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
CONNECTION="connection"
PRINTERCMD="printer/command"

/usr/bin/curl -v -k -H "X-API-KEY: $APIKEY" -H "Content-Type: application/json" -d '{"command":"connect","port":"/tmp/printer"}' "$OCTOAPI$CONNECTION"

sleep 3

/usr/bin/curl -v -k -H "X-API-KEY: $APIKEY" -H "Content-Type: application/json" -d '{"command":"FIRMWARE_RESTART"}' "$OCTOAPI$PRINTERCMD"
```

script to toggle pin on and off to toggle a relay state if the printer is not on
```
#!/bin/bash
FILE=/dev/printerserial
if test -e "$FILE"; then
  echo "already on"
  #systemctl start klipper
  exit 0
else
  echo "powering on"
  gpio -g mode 5 out
  sleep 5
  gpio -g mode 5 in
fi
```

to turn off

```
#!/bin/bash

FILE=/dev/printerserial
if test -e "$FILE"; then
  echo "powering off"
  gpio -g mode 5 out
  sleep 5
  gpio -g mode 5 in
  sleep 2
  systemctl stop klipper
else
  echo "already off"
  systemctl stop klipper
fi
```

and udev rule script

```
#!/bin/bash
/bin/chmod 777 /dev/printerserial
/bin/systemctl start klipper
sleep 3
/bin/bash /path/to/connectoctoprint.sh
```

using the psu controll octoprint plugin

when you turn off - it run script to toggle relay then stops klipper

when you turn on - it runs script to toggle relay

then, the udev rule will run when serial is connected and start klipper then run connect command to octoprint

```
SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", SYMLINK+="printerserial", RUN+="/path/to/udevscript.sh"
```
^- this goes into /etc/udev/rules.d/99-printer.rules
