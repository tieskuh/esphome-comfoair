# ComfoAir
esphome:
  name: zehnder-whr-950
  friendly_name: Zehnder WHR 950

esp32:
  board: esp32dev
  framework:
    type: arduino

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: ""

ota:
  password: ""

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Zehnder-Whr-950 Fallback Hotspot"
    password: ""

captive_portal:

external_components:
  - source:
      type: git
      url: https://github.com/tieskuh/esphome-comfoair
    components: [comfoair]
    refresh: 0s

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
                auto reset_filter = new esphome::comfoair::ComfoAirComponent();
                reset_filter->reset_filter();
