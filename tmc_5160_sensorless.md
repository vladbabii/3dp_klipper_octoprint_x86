Source: https://www.reddit.com/r/3Dprinting/comments/dp7skd/klipper_tmc5160_applies_to_others_aswell/

```
Basic motor settings:

Disabled double homing via "homing_retract_dist: 0"

[stepper_y]

step_pin: ar54

dir_pin: !ar47

enable_pin: !ar55

step_distance: .00625

endstop_pin: tmc5160_stepper_y:virtual_endstop

position_endstop: 465

homing_retract_dist: 0

position_max: 465

homing_positive_dir: true

homing_speed: 20

TMC Motor settings:

Next part, most important "driver_SGT" 2 never triggers for me 0 doesnt even move. So i went with 1 and tuned Torque ( A ) and accel until it worked reliably.

[tmc5160 stepper_y]

cs_pin: ar20

spi_speed: 1000000

spi_software_sclk_pin: ar52

spi_software_mosi_pin: ar51

spi_software_miso_pin: ar50

diag1_pin: !ar34

microsteps: 32

interpolate: True

run_current: 2

hold_current: 1

sense_resistor: 0.075

stealthchop_threshold: 0

driver_IHOLDDELAY: 6

driver_TPOWERDOWN: 10

driver_TBL: 2

driver_TOFF: 3

driver_HEND: 1

driver_HSTRT: 4

driver_tpfd: 0

driver_pwm_autoscale: True

driver_pwm_autograd: True

driver_pwm_freq: 2

driver_PWM_GRAD: 0

driver_PWM_OFS: 0

driver_PWM_REG: 0

driver_PWM_LIM: 0

driver_SGT: 1

[homing_override]

From tuning we got very low values for accel and torque ( A ) so we want to intercept G28 and adjust them live so the printer can retain its usual high power values for printing, we have SPI so thats possible.

[homing_override]

axes: x,y,z

gcode:

G91

G28 X0

SET_TMC_CURRENT STEPPER=stepper_y CURRENT=0.4 HOLDCURRENT=0.2

M204 S100

G28 Y0

G0 Y-10

M204 S2500

SET_TMC_CURRENT STEPPER=stepper_y CURRENT=2 HOLDCURRENT=1

G28 Z0

G90

Zero X ( in my current setup still classic switches )

Drive current and accel down for homing

Zero Y

Move y 10mm away from the stop so we can home multiple times in a row ( needs a bit of space to trigger reliably again )

Set full power again for printing.
```
