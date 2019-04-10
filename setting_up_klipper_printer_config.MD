# MANDATORY! Read [Config Checks Page](https://github.com/KevinOConnor/klipper/blob/master/docs/Config_checks.md) and do all checks

Check
- movement
- homing
- z probe


Then optimisations:
- PID for hotend
```
PID_CALIBRATE HEATER=extruder TARGET=220
```
- PID for bed
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
```
