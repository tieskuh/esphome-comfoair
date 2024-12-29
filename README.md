# Zendher WHR 950
Port of ComfoAir protocol to ESPHome.io firmware originally by @wichers modified by @nyxnyx
to be installed as external_components. Modified for the Zehnder WHR 950 by @tieskuh.
Also confirmed working for the Zehnder WHR 930.

The version of @wichers (and other forks) contained an omission in the processing of the data. Double occurances of 0x07 in the data should be converted to single occurances. This also fixes the broken crc check and incorrect processing of fan values < 1000 rpm.
The coding is not as beautiful as the original code but is is now working according to the standard described in: http://www.see-solutions.de/sonstiges/Protokollbeschreibung_ComfoAir.pdf

# Configuration
Add to your yaml configuration the definition of `external_components`:
```
external_components:
  - source:
      type: git
      url: https://github.com/bammetje/esphome-comfoair
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


![image](https://github.com/user-attachments/assets/6a9b99cd-60ad-49be-b203-c1fd42381794)
