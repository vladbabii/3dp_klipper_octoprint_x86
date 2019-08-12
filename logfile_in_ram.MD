Create a folder on the pi
```
sudo mkdir /opt/klipper.log
sudo chmod -R 777 /opt/klipper.log
```

Add the folder as a temp drive by adding this line in /etc/fstab

Modify 128m to what value you like
```
tmpfs /opt/klipper.log tmpfs defaults,noatime,nosuid,size=128m 0 0
```


Edit klipper to write logs to new path: edit /etc/default/klipper and change the -l parameter in the KLIPPY_ARGS
```
KLIPPY_ARGS="/opt/klipper/klippy/klippy.py /opt/klipper.hill/printer.cfg -l /opt/klipper.log/klippy.log"
```

Mount the new filesystem
```
sudo mount -a
```

Restart klipper
```
sudo systemctl restart klipper
```
