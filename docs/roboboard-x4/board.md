# Board information

## Description

API to read and control miscellaneous board functionality.

***

## Code examples

**Arduino projects:** [RoboBoardX4/X4](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/X4){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    /* Battery info */
    // Battery voltage "11.1"
    float voltage = X4.getBatteryVoltage();
    // Battery state "false"
    bool isLow = X4.isBatteryLow();
    // Battery charging state "false"
    bool isCharging = X4.isBatteryCharging();
    // Last battery charging timestamp "1326485" (millis())
    int lastChTime = X4.getBatteryChargingTime();
    ```
    ```arduino
    /* Board info */
    // Is DC cable plugged in "false"
    bool isInDC = X4.isDC();
    // Is USB cable plugged in "false"
    bool isInUSB = X4.isUSB();
    // X4 serial number
    int serial = X4.getSerial();
    // Board revision "v1.1"
    char *revision = X4.getRevision();
    // Board driver firmware version "v1.52"
    char *driver = X4.getDriverVersion();
    // Board software version "v2.00"
    char *software = X4.getSoftwareVersion();
    ```
    ```arduino
    /* Board control */
    // Force to enable connectivity to Totem App. Normally should be called inside setup()
    X4.enableAppControl();
    // Reboot X4 board
    X4.restart();
    X4.reset();
    ```

***

## Functions

### Battery info

#### (`voltage`) X4.getBatteryVoltage() { data-toc-label='getBatteryVoltage()' }
: Get connected battery voltage.  
**Returns:**  
`voltage` - battery voltage (_float_) [`8.40`:`12.60`]V  

#### (`time`) X4.getBatteryChargingTime() { data-toc-label='getBatteryChargingTime()' }
: Get amount of time battery is charging.  
**Returns:**  
`time` - charging time [`0`:`16777215`]ms  

#### (`state`) X4.isBatteryCharging() { data-toc-label='isBatteryCharging()' }
: Check if battery is currently charging.  
**Returns:**  
`state` - is charging yes / no [`true`:`false`]  

#### (`state`) X4.isBatteryLow() { data-toc-label='isBatteryLow()' }
: Check if battery is depleted and requires charging.  
**Returns:**  
`state` - is low yes / no [`true`:`false`]  

***

### Board info

#### (`state`) X4.isDC() { data-toc-label='isDC()' }
: Check if DC power adapter jack is plugged in.  
**Returns:**  
`state` - is DC plugged in yes / no [`true`:`false`]  

#### (`state`) X4.isUSB() { data-toc-label='isUSB()' }
: Check if USB cable is plugged in.  
**Returns:**  
`state` - is USB plugged in yes / no [`true`:`false`]  

#### (`serial`) X4.getSerial() { data-toc-label='getSerial()' }
: Get TotemBUS network serial number of RoboBoard X4 (as [04] module).  
**Returns:**  
`serial` - serial number [`0`:`32767`]  

#### (`revision`) X4.getRevision() { data-toc-label='getRevision()' }
: Get board revision string (text) (printed on top of the board `v.1.x`). Identifies hardware changes between different RoboBoard X4 releases.  
**Returns:**  
`revision` - board revision string (text) [`v1.0`,`v1.1`]  

#### (`version`) X4.getDriverVersion() { data-toc-label='getDriverVersion()' }
: Get board peripheral driver firmware version. Peripheral driver is an additional STM32 branded MCU taking care of controlling board functionality.  
**Returns:**  
`version` - driver version string (text) (`v1.00`)  

#### (`version`) X4.getSoftwareVersion() { data-toc-label='getSoftwareVersion()' }
: Get RoboBoard X4 software version string (text). This is an additional part of code running alongside Arduino program. It takes care of whole board functionality.  
**Returns:**  
`version` - software version string (text) (`v1.00`)  

***

### Board control

#### (`success`) X4.enableAppControl() { data-toc-label='enableAppControl()' }
: Force to start App connectivity. Will make RoboBoard X4 discoverable for Totem smartphone app. This will override Tools -> App control setting.  
**Returns:**  
`success` - returns `false` if internal error occurred during initialization  

#### X4.reset() { data-toc-label='reset()' }
#### X4.restart() { data-toc-label='restart()' }
: Reset ESP32 MCU. Will reload running program. Same functionality as physical Reset button.  
