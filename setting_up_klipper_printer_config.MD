# MANDATORY! Read [Config Checks Page](https://github.com/KevinOConnor/klipper/blob/master/docs/Config_checks.md) and do all checks

Check at least
- movement
- homing
- z probe
- move distances


Then optimisations:
- PID for hotend
```
PID_CALIBRATE HEATER=extruder TARGET=220
```
- PID for bed
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
```

If homing is outside print area, fix this by adding for X and Y
```
[stepper_<direction>]
position_min: -20
position_endstop: -20
```
