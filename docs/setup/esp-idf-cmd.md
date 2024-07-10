---
icon: simple/espressif
---

# Setup ESP-IDF

[ESP-IDF](https://github.com/espressif/esp-idf){target=_blank} is an official Espressif SDK for ESP32 development. It contains latest features and provides full access to chip functionality and configuration. Almost any ESP32 framework (like ESP32 Arduino) is based on ESP-IDF, which provides required low-level drivers.  
In combination, Totemmaker provides [totem-bsp](https://github.com/totemmaker/totem-bsp){target=_blank} (Board Support Package) for RoboBoard development using ESP-IDF. It contains required drivers to control on board features (like motor drivers).

## Step 1. Setup ESP-IDF

Visit [Getting Started](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/){target=_blank} to setup ESP-IDF toolchain.

??? "Manual setup (click to expand)"
    Clone esp-idf project:  
    ```
    mkdir ~/esp
    cd ~/esp
    git clone --recursive https://github.com/espressif/esp-idf.git
    cd esp-idf
    ```
    Install required tools (`install.ps1` for PowerShell):  
    ```
    ./install.sh
    ```
    Load esp-idf environment (`export.ps1` for PowerShell):
    ```
    ./export.sh
    ```
    Go to your project and run:  
    ```
    idf.py build
    ```

## Step 2. Create a project

1. Create new ESP-IDF project or use command:  
```
idf.py create-project roboboard_project
```
2. Inside project create manifest file `main/idf_component.yml` or use command:  
```
idf.py create-manifest
```
3. Add `roboboard_x3` or `roboboard_x4` as dependency:  
=== "RoboBoard X3"
    ```yaml
    ## IDF Component Manager Manifest File
    dependencies:
      # Include roboboard_x3 component
      roboboard_x3:
        path: roboboard_x3
        git: https://github.com/totemmaker/totem-bsp.git
    ```
=== "RoboBoard X4"
    ```yaml
    ## IDF Component Manager Manifest File
    dependencies:
      # Include roboboard_x4 component
      roboboard_x4:
        path: roboboard_x4
        git: https://github.com/totemmaker/totem-bsp.git
    ```
    !!! tip "RoboBoard X4"
        Update motor driver firmware to latest version. See [driver update](../roboboard-x4/index.md#driver-update) guide.

## Step 3. Write firmware

Include `totem-bsp.h` header and code into project `main/roboboard_project.c` file:  
```c
#include "bsp/totem-bsp.h"

void app_main(void)
{
    // Initialize board
    bsp_board_init();
    // Spin motor A at 100% power
    bsp_dc_spin(BSP_PORT_A, 100);
    // Spin servo A to 500us pulse
    bsp_servo_spin(BSP_PORT_A, 500);
    // RoboBoard X4 LED on
    // bsp_board_set_led(1);
    // bsp_rgb_color(BSP_PORT_ALL, 0xFF00FF00);
}
```

## Step 4. Build project

Build project:  
```
idf.py build
```

Flash to board:  
```
idf.py flash monitor
```

## Step 5. Using ESP-IDF

For more information read following topics:  

**Code documentation:**

- RoboBoard X3
    - [roboboard_x3.h header file](https://github.com/totemmaker/totem-bsp/blob/master/roboboard_x3/include/bsp/roboboard_x3.h){target="_blank"} - RoboBoard X3 low-level API
    - [RoboBoard X3 imu.h](https://github.com/totemmaker/totem-bsp/blob/master/roboboard_x3/include/bsp/imu.h){target="_blank"} - RoboBoard X3 accelerometer & gyroscope API
    - [RoboBoard X3 rgb.h](https://github.com/totemmaker/totem-bsp/blob/master/roboboard_x3/include/bsp/rgb.h){target="_blank"} - RoboBoard X3 RGB lights API
- RoboBoard X4
    - [roboboard_x4.h header file](https://github.com/totemmaker/totem-bsp/blob/master/roboboard_x4/include/bsp/roboboard_x4.h){target="_blank"} - RoboBoard X4 low-level API
    - [RoboBoard X4 imu.h](https://github.com/totemmaker/totem-bsp/blob/master/roboboard_x4/include/bsp/imu.h){target="_blank"} - RoboBoard X4 accelerometer & gyroscope API
- [ESP-IDF API Reference](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/index.html){target="_blank"} - ESP32 API Reference
- [ESP-IDF API Guides](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/index.html){target="_blank"} - More details about ESP-IDF

**Code examples:**

- [RoboBoard X3](https://github.com/totemmaker/totem-bsp/tree/master/examples/x3_board/main){target="_blank"} - example of board feature control
- [RoboBoard X4](https://github.com/totemmaker/totem-bsp/tree/master/examples/x4_board/main){target="_blank"} - example of board feature control

!!! question
    **Visit :information_source: [Support page](../support.md) to find more information or help from our community.**