```
[output_pin lockstatus]
pin: PA2
value: 1

[gcode_macro TOOLUNLOCK]
gcode:

  {% if printer["output_pin lockstatus"].value == 0.0 %}
  
  M118 Tool Unlock - Skipped, already unlocked  
  
  {% else %}
  
  ...
  ;; do stuff that takes some time (like unlocking)
  ...
  SET_PIN PIN=lockstatus VALUE=0
  ...
  
  {% endif %}
  
  ...
  UPDATE_DELAYED_GCODE ID=toollockprintstatus DURATION=1
  ....
  

[delayed_gcode toollockprintstatus]
gcode:
  M118 Tool Lock Status {printer["output_pin lockstatus"].value}  
  ```
