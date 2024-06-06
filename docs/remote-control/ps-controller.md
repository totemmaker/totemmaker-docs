# Using PS3/PS4 controller

*[ESP32]: Espressif ESP32 microcontroller

![PS3 Controller](../assets/images/ps3.png){style="width:48%;padding:20px;"}
![PS4 Controller](../assets/images/ps4.png){style="width:48%;padding:20px;"}

Totem robots can be controller using PS3 or PS4 wireless controllers.
This tutorial contains set-up instructions required to use with RoboBoard.

## How it works

Controller communicates directly with ESP32 MCU using Arduino libraries [PS3 Controller Host](https://github.com/jvpernis/esp32-ps3){target=_blank}, [PS4Controller](https://github.com/aed3/PS4-esp32){target=_blank}. They provide API to read controller buttons, control leds, vibration, etc.

When ++"PS"++ button is pressed, controller tries to connect device with MAC address stored in its memory. ESP32 presents itself with this address and accepts connection. For this reason, a setup of same MAC address for controller and ESP32 is required.

## Install library

A third party Arduino library is required to compile the code to use with controller.  
Select install instructions depending on controller. Both of them can be installed also.  

??? example "PS3 Controller Host (click to expand)"
    === "Arduino IDE"
        1. Select `Sketch` → `Include Library` → `Manage Libraries..`.  
        1. In search field type `PS3 Controller Host`.  
        1. Select library `PS3 Controller Host` from the list and click ++"Install"++.  
        1. Close the window when installation is finished.  

    === "PlatformIO"
        1. Click PlatformIO icon in sidebar.  
        1. Select `Miscellaneous` → `New Terminal`.  
        1. Run command inside terminal:  
            `pio lib --global install https://github.com/jvpernis/esp32-ps3`  

??? example "PS4Controller (click to expand)"
    === "Arduino IDE"
        1. Download file [PS4-esp32-master.zip](https://github.com/aed3/PS4-esp32/archive/master.zip).
        1. Select `Sketch` → `Include Library` → `Add .ZIP Library..`.  
        1. Select downloaded **PS4-esp32-master.zip** file.  
        1. Message _"Library added to your libraries"_ should appear.  

    === "PlatformIO"
        1. Click PlatformIO icon in sidebar.  
        1. Select `Miscellaneous` → `New Terminal`.  
        1. Run command inside terminal:  
            `pio lib --global install https://github.com/aed3/PS4-esp32`  

## Change MAC

Stored MAC address can be changed with [Sixaxis Pairer Tool](https://github.com/user-none/sixaxispairer){target=_blank} using PC.  
This is required only once. If controller was connected to other device, this step should be repeated.
Follow instructions to change controller address:

=== "Windows"
    Download link: [SixaxisPairToolSetup-0.3.1.exe](../assets/files/sixaxis/SixaxisPairToolSetup-0.3.1.exe) – [Size: 25.6 MB]  
    Run downloaded file and install.  

=== "Mac OSX"
    Download link: [sixpair](../assets/files/sixaxis/sixpair) - [Size: 9.4 KB]  
    This utility requires libusb, which can be downloaded from [http://www.ellert.se/twain-sane/](http://www.ellert.se/twain-sane/){target=_blank}. Choose the binary for your version of OSX.

=== "Linux"
    Download link: [SixaxisPairTool](../assets/files/sixaxis/SixaxisPairTool) – [Size: 55.7 KB]  

    Required dependencies: qt5, libusb-1.0  

    You will also need to install the following udev rule file under /etc/udev/rules.d/ to give the app access to the controller: [51-sixaxispairtool.rules](../assets/files/sixaxis/51-sixaxispairtool.rules)  

![Sixaxis Pair Tool](../assets/images/sixaxispairtool.png)  

1. Run SixaxisPairTool.  
1. Connect controller to PC using USB cable.  
1. SixaxisPairTool should detect connected controller and display current stored MAC address at "Current Master".
1. In "Change Master" enter `00:02:03:04:05:06`
1. Click ++"Update"++
1. "Current Master" will change to "00:02:03:04:05:06"
1. Address is set. Unplug controller and close the tool.  

## Run example code

Interaction with controller features can be found inside library examples.

To use controllers with RoboBoard and Totem robotic kits, check [Totem Arduino examples](https://github.com/totemmaker/arduino-examples){target=_blank}.

**Ensure that adddress entered in "Change Master" matches with one provided in `#!arduino PS4.begin("00:02:03:04:05:06")`.**  
**Press ++"PS"++ button on the controller and after few seconds it should start working.**  

!!! bug
    Sometimes remote does not connect even if MAC address is correct.  
    Erase ESP32 flash and upload it again to resolve issue:  
    Select Arduino IDE → `Tools` → `Erase All Flash Before Sketch Upload`.  
    Select PlatformIO → Project Tasks → Platform → `Erase Flash`.  
    Command line: `esptool.py erase_flash`.
