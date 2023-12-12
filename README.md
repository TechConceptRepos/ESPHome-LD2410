# ESPHome LD2410 Human Presence Sensor for Home Assistant
## Introduction
This repository contains the setup for a human presence sensor designed for ESPHome, utilizing the WeMos D1 Mini, HLK-LD2410, and BH1750 modules.

## Hardware
+ **WeMos D1 Mini** (ESP8266)
+ **HLK-LD2410** -  Human Presence Sensor
+ **BH1750** - Ambient Light Sensor

#### Schematic
![](https://raw.githubusercontent.com/TechConceptRepos/ESPHome-LD2410/media/schema.png)

#### Pinout

WeMos D1 Mini |  HLK-LD2410 | BH1750
------------- | ----------- | ------
5V            | VCC         | VCC
GND           | GND         | GND
TX *(GPIO01)* | RX          | 
RX *(GPIO03)* | TX          | 
D1 *(GPIO05)* |             | SCL 
D2 *(GPIO04)* |             | SDA 

#### Flashing
- [ESPHome Web](https://web.esphome.io/) - You can flash the Wemos D1 Mini using a web browser.

## Software
### BH1750 - Ambient Light Sensor
The BH1750 is a digital ambient light sensor which uses I2C communication protocol.
``` yaml
i2c:
  sda: 4
  scl: 5
  scan: true
  
sensor:
  - platform: bh1750
    name: BH1750 Illuminance
    address: 0x23
```
### HLK-LD2410 - Human Presence Sensor
Sensor is connected to default Wemos D1 mini RX/TX pins. You should fine tune the sensor parameters like max distances or thresholds to your needs.
``` yaml
uart:
  tx_pin: TX
  rx_pin: RX
  baud_rate: 256000
  parity: NONE
  stop_bits: 1

ld2410:
  timeout: 30s
  max_move_distance : 6m
  max_still_distance: 0.75m
  g0_move_threshold: 50
  g0_still_threshold: 20
  g1_move_threshold: 50
  g1_still_threshold: 20
  g2_move_threshold: 40
  g2_still_threshold: 40
  g3_move_threshold: 40
  g3_still_threshold: 40
  g4_move_threshold: 40
  g4_still_threshold: 40
  g5_move_threshold: 40
  g5_still_threshold: 40
  g6_move_threshold: 30
  g6_still_threshold: 15
  g7_move_threshold: 30
  g7_still_threshold: 15
  g8_move_threshold: 30
  g8_still_threshold: 15

sensor:
  - platform: ld2410
    moving_distance:
      name : Moving Distance
    still_distance:
      name: Still Distance
    moving_energy:
      name: Move Energy
    still_energy:
      name: Still Energy
    detection_distance:
      name: Detection Distance

binary_sensor:
  - platform: ld2410
    has_target:
      name: Presence
    has_moving_target:
      name: Moving Target
    has_still_target:
      name: Still Target
```

### Demo ([human-presence-sensor.yaml](human-presence-sensor.yaml))
#### YAML
``` yaml
uart:
  tx_pin: TX
  rx_pin: RX
  baud_rate: 256000
  parity: NONE
  stop_bits: 1

i2c:
  sda: 4
  scl: 5
  scan: true

ld2410:
  timeout: 30s
  max_move_distance : 6m
  max_still_distance: 0.75m
  g0_move_threshold: 50
  g0_still_threshold: 20
  g1_move_threshold: 50
  g1_still_threshold: 20
  g2_move_threshold: 40
  g2_still_threshold: 40
  g3_move_threshold: 40
  g3_still_threshold: 40
  g4_move_threshold: 40
  g4_still_threshold: 40
  g5_move_threshold: 40
  g5_still_threshold: 40
  g6_move_threshold: 30
  g6_still_threshold: 15
  g7_move_threshold: 30
  g7_still_threshold: 15
  g8_move_threshold: 30
  g8_still_threshold: 15

sensor:
  - platform: ld2410
    moving_distance:
      name : Moving Distance
    still_distance:
      name: Still Distance
    moving_energy:
      name: Move Energy
    still_energy:
      name: Still Energy
    detection_distance:
      name: Detection Distance

  - platform: bh1750
    name: BH1750 Illuminance
    address: 0x23
    update_interval: 60s

binary_sensor:
  - platform: ld2410
    has_target:
      name: Presence
    has_moving_target:
      name: Moving Target
    has_still_target:
      name: Still Target
```

### Lovelace Card ([lovelace-card.yaml](lovelace-card.yaml))
![Lovelace Card](https://raw.githubusercontent.com/TechConceptRepos/ESPHome-LD2410/media/lovelace_card.png)

### ESPHome integration for Home Assistant 
[![Add Integration to your Home Assistant
instance.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=esphome)

### Enclosure
- https://www.thingiverse.com/thing:6357902
- https://cults3d.com/en/3d-model/gadget/esphome-ld2410-human-presence-sensor

You can choose between a long length case (with hidden micro USB connector), a short length case or a wall-mount type.

![3D Parts](https://raw.githubusercontent.com/TechConceptRepos/ESPHome-LD2410/media/3D_parts.png)

#### Links
- [ESPHome LD2410 documentation](https://esphome.io/components/sensor/ld2410.html)
