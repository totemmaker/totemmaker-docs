# 0. Setup

![Mini Lab LabBoard menu display](/assets/images/mini-lab/labboard-menu-display.png)

Menu to access various LabBoard configuration options.  
These setting are stored in memory and used during LabBoard operation.  
To exit selected option - press ++"Left SELECT"++ button.  
Settings are saved when value is changed. No need to press a button for confirmation. Some menu options required to press and hold `run` button (++"Right SELECT"++) to execute specified action.

1. `dISP` - [Display brightness](#display)
1. `dAC` - [DAC output manual adjust](#dac-offset)
1. `AdC` - [Voltage measure manual adjust](#adc-offset)
1. `CALIb` - [Auto calibration](#calibration)
1. `SERIAL` - [Default Serial mode baud rate](setup/#serial)
1. `UERSI` - [Firmware version](#firmware)
1. `rESEt` - [Factory reset](#reset)
1. `boot` - [Enter boot mode](#boot)

## Setup menu options

### 1. Display brightness { #display }

Change display and LED brightness.  

`diSP    0` will be shown and LED will light up to indicate current brightness level.  

Using ++"SET\+"++ and ++"SET\-"++ increase or decrease brightness level between [`-8`:`7`].  

### 2. DAC manual offset { #dac-offset }

Manually adjust DAC offset. Can be used to fine tune DAC channel output if value displayed on screen differs from multimeter reading.  

`dAC  0.000` will be shown and selected DAC channel LED will light up.  

Click ++"Middle SELECT"++ to switch active DAC channel to configure.  
Using ++"SET\+"++ and ++"SET\-"++ increase or decrease offset.  

It is suggested to rerun calibration if changed DAC1 offset.

### 3. ADC manual offset { #adc-offset }

Manually adjust ADC offset. Can be used to fine tune voltage measurements if value displayed on screen differs from multimeter readings.  
These values are auto generated during calibration, but can be manually adjusted if required.  

`AdC      0` will be shown and selected voltage channel LED will light up.  

Click ++"Middle SELECT"++ to switch active voltage channel to configure.  
Using ++"SET\+"++ and ++"SET\-"++ increase or decrease offset.  

### 4. Auto calibration { #calibration }

![Mini Lab LabBoard calibration wiring](/assets/images/mini-lab/labboard-calibration-wiring.png)

This feature will calibrate ADC channels for more precise voltage measurements.  

`CALIb run` will be shown. It is in  waiting state until "run" button is pressed.

1. Connect wires as specified:
    - ±50v -> GND
    - ±5v -> GND
    - ±0.5v -> DAC1
1. Press and hold ++"Right SELECT"++ for 1s until calibration starts
1. Wait for display `donE`
1. Press ++"Left SELECT"++ to exit

### 5. Serial default baud rate { #serial }

Configure default Serial mode baud rate selection.

`baud 9600` will be shown indicating current selected default baud rate.  

Using ++"SET\+"++ and ++"SET\-"++ change selection.  

### 6. Firmware version { #firmware }

Display current firmware version.  

`UERSI x.xx` will be shown indicating installed firmware version.

### 7. Factory reset { #reset }

Factory reset option will reset all settings to default state. Calibration will be requested after reset is done.  

`rESEt run` will be shown. It is in waiting state until "run" button is pressed.

1. Press and hold ++"Right SELECT"++ for 1s until reset starts
1. `donE` will be displayed briefly
1. `CALIb run` will be shown. Follow [calibration section](#calibration) to proceed

### 8. Enter boot mode { #boot }

![Mini Lab LabBoard boot wiring](/assets/images/mini-lab/labboard-boot-wiring.png)

Enter system bootloader for firmware update.

`boot  run` will be shown. It is in waiting state until "run" button is pressed.

1. Press and hold ++"Right SELECT"++ for 1s
1. `boot   on` will be displayed
1. Run firmware download. Follow [firmware update section](/labboard/firmware-update/)

_Note: pins `D0` -> `TXD` and `D1` -> `DIG2` has to be connected for revision v.2.1 and v.2.2 boards._