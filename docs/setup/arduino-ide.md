---
icon: simple/arduino
---

# Setup Arduino IDE 1.8

![Arduino IDE](../assets/images/arduino-ide-large.png)
Arduino IDE stands out with its simple design, allowing to write a code, compile a project and upload it to the RoboBoard. This is one of the reasons why it's so popular among beginners. Aside from basic functionality, it has thousands of third-party libraries and code examples to create a project even faster.  


## Step 1. Download Arduino IDE

Go to Arduino website to download application for your operating system.  
*For this tutorial select version 1.8.x (not 2.2).*  
*Install guide:* [Windows](https://docs.arduino.cc/software/ide-v1/tutorials/Windows){target="_blank"} | [macOS](https://docs.arduino.cc/software/ide-v1/tutorials/macOS){target="_blank"} | [Linux](https://docs.arduino.cc/software/ide-v1/tutorials/Linux){target="_blank"}  

[:octicons-download-16: Download Arduino](https://www.arduino.cc/en/software#legacy-ide-18x){ .md-button .md-button--primary target="_blank"}

## Step 2. Install Totem Boards

![Arduino IDE](../assets/images/arduino-ide-board-manager.png)
Install `Totem Boards` extension to add support for Totem RoboBoard.  

1. In Arduino IDE select `File` → `Preferences`.  
1. Locate `Additional Boards Manager URLs` and paste in:  
    ![Arduino IDE](../assets/images/arduino-ide-board-url.png)  
    ```
    https://totemmaker.github.io/TotemArduinoBoards/package_totemmaker_index.json
    ```  
    *If you have multiple URLs, click a button next to input field and add this link to the bottom.*
1. Click ++"OK"++.  
1. Select `Tools` → `Board` → `Boards Manager..`  
1. In search field type in `totem`.  
1. Click ++"Install"++ and wait for it to complete. It can take a few minutes.  
1. Close Boards Manager window.  

## Step 3. Compile and upload code

![Arduino IDE](../assets/images/arduino-ide-image3.gif)
Load example code and upload it to RoboBoard.  

1. Select `Tools` → `Board` → `Totem Boards` → `RoboBoard X4` (or `RoboBoard X3`).  
1. Select `File` → `Examples` → `> RoboBoard` → `RGB` → `ColorRun`.  
1. Connect RoboBoard to PC over USB cable and power it on.  
1. Select `Tools` → `Port` and click on the port displayed there.  
_If `Port` is not available - check if RoboBoard is on and plugged to PC with USB cable._  
_If there are multiple ports, disconnect USB cable and check which one is gone. Reconnect and select it._  
1. Select `Sketch` → `Upload` and wait till it completes (can take a few minutes first time).  
_Note: in case upload fails - try selecting lower speed in `Tools` → `Upload speed`._  
1. RGB lights will start to blink in different colors. There is an example for each functionality, including a simple description.  

![RoboBoard X4 LedBlink](../assets/images/module_04_LedBlink.gif)

_For RoboBoard X4 make sure the battery or DC jack is connected and power switch is turned on._  
_It won't work from USB alone!_  

## Step 4. Project settings

![Arduino IDE settings](../assets/images/arduino-ide-settings.png)

Menu option `Tools` contains Board settings that can change code build and upload parameters.

- **Board** - board type selection
- **Upload speed** - firmware upload speed. Use highest, unless upload errors occur
- **Erase All Flash Before Sketch Upload** - fully erase ESP32 flash before new firmware upload
- **Remote control** - makes RoboBoard discoverable by [Totem App](../remote-control/app/index.md) when selected
- **Board status** - enable [RoboBoard status](../roboboard/api/board.md#setStatusRGB) with RGB lights and beeping
- **Charging mode** - enable RoboBoard X3 [charging mode](../roboboard/api/board.md#setChargingMode)
- **Port** - USB port name RoboBoard is connected to (required for Serial Monitor and upload)

## Step 5. Using Arduino IDE

For more information about getting started with Arduino, read following topics:  

**User interface:**

* [Getting Started with Arduino IDE](https://docs.arduino.cc/software/ide-v1/tutorials/Environment){target="_blank"} - walk around Arduino IDE UI
* [Installing libraries](https://docs.arduino.cc/software/ide-v1/tutorials/installing-libraries){target="_blank"}  - install third-party libraries

**Code documentation:**

- [RoboBoard board settings](../roboboard/index.md#board-settings){target="_blank"} - RoboBoard settings and default firmware
- [RoboBoard API documentation](../roboboard/api/index.md){target="_blank"} - RoboBoard functions documentation
- [Arduino code reference](https://www.arduino.cc/reference/en/){target="_blank"} - Arduino functions documentation

**Code examples:**

- [Connect GPIO and Qwiic](../roboboard/api/gpio-qwiic.md){target="_blank"} - interface GPIO pins and Qwiic modules
- [Interface with Totem App](../remote-control/app/custom-function.md){target="_blank"} - read commands from Totem App
- [RoboBoard code examples](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemRB/examples){target="_blank"} - RoboBoard specific code examples
- [ESP32 code examples](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries){target="_blank"} - ESP32 processor specific code examples
- [Arduino examples / projects](https://docs.arduino.cc/built-in-examples/){target="_blank"} - general Arduino example projects

!!! question
    **Visit :information_source: [Support page](../support.md) to find more information or help from our community.**