# Setup Arduino IDE

![Arduino IDE](/assets/images/arduino-ide-image1.png)
Arduino IDE stands out with it's simple design, allowing to write a code, compile a project and upload it to the RoboBoard X4. This is one of the reasons why it's so popular among beginners. Aside from basic functionality, it has thousands of third-party libraries and code examples to create a project even faster.  


## Step 1. Download Arduino IDE

Go to Arduino website to download application for your operating system.  
*For this tutorial select version 1.8.x (not 2.0).*

[Download Arduino](https://www.arduino.cc/en/software){ .md-button .md-button--primary target=_blank}

## Step 2. Install Totem Boards

![Arduino IDE](/assets/images/arduino-ide-image2.png)
Install `Totem Boards` extension to add support for RoboBoard X4.  

1. In Arduino IDE select `File` → `Preferences`.  
1. Locate `Additional Boards Manager URLs` and paste in:  
    `https://totemmaker.github.io/TotemArduinoBoards/package_totemmaker_index.json`  
    *If you have multiple URLs, click a button next to input field and add this link to the bottom.*
1. Click ++"OK"++.  
1. Select `Tools` → `Board` → `Boards Manager..`  
1. In search field type in `totem`.  
1. Click ++"Install"++ on `Totem Boards` and wait for it to complete. It can take a few minutes.  
1. Close Boards Manager window.  
1. Select `Tools` → `Board` → `Totem Boards` → `RoboBoard X4`.  

_If you did not succeed read - [Installing additional cores](https://www.arduino.cc/en/Guide/Cores){target=_blank}._  

## Step 3. Compile and upload code

![Arduino IDE](/assets/images/arduino-ide-image3.gif)
Load example code and upload it to RoboBoard X4.  

1. Select `File` → `Examples` → `Board` → `RGB` → `SetColor`.  
1. Connect X4 board to PC over USB cable and power it on.  
    _Make sure the battery or DC jack is connected and power switch is turned on._  
    _X4 won't work from USB alone!_  
1. Select `Tools` → `Port` and click on the port displayed there.  
    _If `Port` is not available - check if X4 is on and plugged to PC with USB cable._  
    _If there are multiple ports, select one with (ESP32 USB) or disconnect USB cable and check which one is gone. Reconnect and select it._  
1. Select `Sketch` → `Upload` and wait till it completes (can take a few minutes first time).  
1. RoboBoard X4 RGB will start to blink in different colors. Try other examples of each available feature.  

## Step 4. Using Arduino IDE

For more information about getting started with Arduino, read following topics:  

* [Arduino IDE usage guide](https://www.arduino.cc/en/Guide/Environment){target=_blank}  
* [How to install libraries](https://www.arduino.cc/en/Guide/Libraries){target=_blank}  
* [Using Serial Monitor](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-serial-monitor){target=_blank}  
* [Arduino code documentation](https://www.arduino.cc/reference/en/){target=_blank}  
* [RoboBoard X4 code documentation](/roboboard-x4){target=_blank}  
* [RoboBoard X4 code examples](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples){target=_blank}  
* [General Arduino examples](https://www.arduino.cc/en/Tutorial/BuiltInExamples){target=_blank}  

!!! question
    **If you encountered any problem - [create a topic](https://forum.totemmaker.net/c/questions/10){target=_blank} on our community [forum.totemmaker.net](https://forum.totemmaker.net){target=_blank}.**