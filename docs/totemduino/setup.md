# Arduino IDE setup

![Mini Lab power scheme](../assets/images/arduino-ide-blink.jpg)

Arduino IDE stands out with its simple design, allowing to write a code, compile a project and upload it to the TotemDuino. This is one of the reasons why it's so popular among beginners. Aside from basic functionality, it has thousands of third-party libraries and code examples to create a project even faster.  

!!! attention "Windows 11 driver"
    TotemDuino were manufactured with PL2303TA chip that is no more supported in Windows 11 OS.  
    Follow [Driver install](driver-install.md) tutorial to solve this issue.

## Step 1. Download Arduino IDE

Go to Arduino website to download application for your operating system.  
*Install guide (1.8):* [Windows](https://docs.arduino.cc/software/ide-v1/tutorials/Windows){target="_blank"} | [macOS](https://docs.arduino.cc/software/ide-v1/tutorials/macOS){target="_blank"} | [Linux](https://docs.arduino.cc/software/ide-v1/tutorials/Linux){target="_blank"}  
*Install guide (2.2):* [Windows / MacOS / Linux](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing){target="_blank"}

[:octicons-download-16: Download Arduino](https://www.arduino.cc/en/software){ .md-button .md-button--primary target="_blank"}

## Step 2. Select Board

TotemDuino is represented as "Arduino UNO".

1. Select `Tools` → `Board` → `Arduino AVR Boards` → `Arduino UNO`.  

## Step 3. Compile and upload code

![Arduino IDE](../assets/images/arduino-ide-image3.gif)

Load example code and upload it to TotemDuino.  

1. Select `File` → `Examples` → `01.Basics` → `Blink`.  
1. Connect TotemDuino to PC over USB cable.  
1. Select `Tools` → `Port` and click on the port displayed there.  
    _If there are multiple ports, disconnect USB cable and check which one is gone. Reconnect and select it._  
1. Select `Sketch` → `Upload` and wait till it completes.  
1. LED will start to blink in 1 second interval.  

![TotemDuino LED blinking](../assets/images/mini-lab/totemduino-blink.png)

## Step 4. Using Arduino IDE

For more information about getting started with Arduino, read following topics:  

**User interface:**

- [Arduino IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started-ide-v2){target="_blank"} | [Arduino IDE 1.8](https://docs.arduino.cc/software/ide-v1/tutorials/Environment){target="_blank"} - walk around Arduino IDE UI
- [Install libraries IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-installing-a-library){target="_blank"} | [Install libraries IDE 1.8](https://docs.arduino.cc/software/ide-v1/tutorials/installing-libraries){target="_blank"} - install third-party libraries
- [Upload sketch IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-uploading-a-sketch){target="_blank"} - upload compiled code to TotemDuino
- [Autocomplete feature IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-autocomplete-feature){target="_blank"} - displays code suggestions while typing

**Monitor:**

- [Serial Monitor IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-serial-monitor){target="_blank"} - view `Serial.print()` output
- [Serial Plotter IDE 2.2](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-serial-plotter){target="_blank"} - view plotted graphs

**Code documentation:**

- [Arduino code documentation](https://www.arduino.cc/reference/en/){target="_blank"} - Arduino functions documentation

**Code examples:**

- [Arduino examples / projects](https://docs.arduino.cc/built-in-examples/){target="_blank"} - general Arduino example projects
- [Mini Lab projects](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab){target="_blank"} - Mini Lab specific example projects

!!! question
    **Visit :information_source: [Support page](../support.md) to find more information or help from our community.**