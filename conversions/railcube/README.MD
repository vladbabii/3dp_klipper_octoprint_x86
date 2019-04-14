```
M503
```

```
echo:  G21    ; (mm)

echo:Filament settings: Disabled
echo:  M200 D1.75
echo:  M200 D0
echo:Steps per unit:
echo:  M92 X80.00 Y80.00 Z400.00 E100.00
echo:Maximum feedrates (units/s):
echo:  M203 X400.00 Y400.00 Z8.00 E50.00
echo:Maximum Acceleration (units/s2):
echo:  M201 X1000 Y1000 Z100 E10000
echo:Acceleration (units/s2): P<print_accel> R<retract_accel> T<travel_accel>
echo:  M204 P500.00 R1000.00 T500.00
echo:Advanced: B<min_segment_time_us> S<min_feedrate> T<min_travel_feedrate> X<max_x_jerk> Y<max_y_jerk> Z<max_z_jerk> E<max_e_jerk>
echo:  M205 B20000 S0.00 T0.00 X6.00 Y6.00 Z0.30 E5.00
echo:Home offset:
echo:  M206 X0.00 Y0.00 Z0.00
echo:Auto Bed Leveling:
echo:  M420 S0 Z0.00
echo:  G29 W I0 J0 Z0.83750
echo:  G29 W I1 J0 Z0.25417
echo:  G29 W I2 J0 Z-0.40333
echo:  G29 W I0 J1 Z0.58000
echo:  G29 W I1 J1 Z-0.07083
echo:  G29 W I2 J1 Z-0.79917
echo:  G29 W I0 J2 Z0.11667
echo:  G29 W I1 J2 Z-0.70583
echo:  G29 W I2 J2 Z-1.52750
echo:PID settings:
echo:  M301 P22.20 I1.08 D114.00
echo:Z-Probe Offset (mm):
echo:  M851 Z-3.90
```


servo stowed
```
M402
M280 P0

Recv: echo: Servo 0: 0
```

servo deployed
```
M402
M280 P0

Recv: echo: Servo 0: 160
```
