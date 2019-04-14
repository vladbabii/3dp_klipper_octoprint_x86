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

# Read & apply [bed leveling guide](https://github.com/KevinOConnor/klipper/blob/master/docs/Bed_Level.md) especially the [probe calibration](https://github.com/KevinOConnor/klipper/blob/master/docs/Probe_Calibrate.md)

* probe calibration

First, heat up the bed and nozzle to normal printing temperature, and home the printer.

Then run this to start probe z calibration
```
PROBE_CALIBRATE 
```
then use the testz command and a piece of paper to calibrate nozzle height, either raising the printhead
```
TESTZ Z=+.1
```
or lowering it
```
TESTZ Z=-.1
```
Then, when you're satisfied, send 
```
ACCEPT
```

* extruder calibration

Use any marlin calibration tutorial, and you get a value H. You need to do (1/H) and put the value in your extruder config on
```
[extruder]
step_distance: .01
```
math:
```

todo: move 100
left: 16.17
done: 103.83

klipper ex steps: .01
marlin steps per unit: 100

100 X 100 = 10000

y x 103.83 = 10000

y = 10000 / 103.83

y = 96.31127805065973


marlin steps per unit: y 96.31127805065973

klipper ex steps: 1/y = 1 / 96.31127805065973
```


# Read [slicers related document](https://github.com/KevinOConnor/klipper/blob/master/docs/Slicers.md)
