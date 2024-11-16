# Zendher WHR 950
Port of ComfoAir protocol to ESPHome.io firmware originally by @wichers modified by @nyxnyx
to be installed as external_components. Modified for the Zehnder WHR 950 by @tieskuh.
Also confirmed working for the Zehnder WHR 930.

# Configuration
Add to your yaml configuration the definition of `external_components`:
```
external_components:
  - source:
      type: git
      url: https://github.com/tieskuh/esphome-comfoair
    components: [comfoair]
    refresh: 0s
```

and than use it:
```
uart:
  id: uart_bus
  rx_pin: 22
  tx_pin: 19
  baud_rate: 9600

comfoair:
  name: "WHR 950"
  uart_id: uart_bus
  fan_supply_air_percentage:
    name: "Supply fan"
  fan_exhaust_air_percentage:
    name: "Exhaust fan"
  fan_speed_supply:
    name: "Supply fan speed"
  fan_speed_exhaust:
    name: "Exhaust fan speed"
  is_bypass_valve_open:
    name: "Bypass"
  is_preheating:
    name: "Preheating"
  outside_air_temperature:
    name: "Outside temperature"
  supply_air_temperature:
    name: "Supply temperature"
  return_air_temperature:
    name: "Return temperature"
  exhaust_air_temperature:
    name: "Exhaust temperature"
  is_supply_fan_active:
    name: "Supply fan"
  is_filter_full:
    name: "Filter status"
  bypass_step:
    name: "Bypass valve"
  is_summer_mode:
    name: "Summer mode"

button:
  - platform: template
    name: "Reset filter"
    on_press:
      then:
        - lambda: |-
              id(comfoair_climate)->reset_filter();
```

# Sensors
The following sensors are created:

![image](https://github.com/tieskuh/esphome-comfoair/assets/115901851/7d733ddb-2106-4b77-b6f5-8dccbe4459c1)


# Hardware
I used the following hardware:
- RS-232 cable with DB9 D-SUB male connector
- M5Stack: https://nl.aliexpress.com/item/1005003299215808.html
- M5Stack RS-232 add-on: https://nl.aliexpress.com/item/1005005933403536.html
- 12v to 5v USB-C adapter: https://nl.aliexpress.com/item/1005004984244010.html

![image](https://github.com/tieskuh/esphome-comfoair/assets/115901851/30fac702-c32d-469d-85dd-78bd432e304a)

![image](https://github.com/tieskuh/esphome-comfoair/assets/115901851/fb6004f1-fa2d-49d7-8792-edb49e918043)

![image](https://github.com/tieskuh/esphome-comfoair/assets/115901851/41531d2f-ca43-4b86-b24f-a7311b0b5898)

Pin layout:
- RX (receive) = pin 2
- TX (transmit) = pin 3
- GND (ground) = pin 5

(If it doesn't work, try to switch RX and TX. The RX on one end should be the TX on the other end.)

![image](https://github.com/tieskuh/esphome-comfoair/assets/115901851/6c8e96ea-fa3e-4b4c-8c62-9d6a73201175)

To power the M5Stack, I used a 12v to 5v USB-C adapter and powered the M5Stack via USB-C. There are unused 12v connectors on the WHR's board.
I assume you can also connect the 12v of the WHR directly to the 12v of the M5Stack RS-232 addon. Or just use a normal 220v USB adapter.

![image](https://github.com/user-attachments/assets/6a9b99cd-60ad-49be-b203-c1fd42381794)
