# Using Qwiic modules

![Qwiic](https://cdn.sparkfun.com/assets/custom_pages/2/7/2/qwiic-logo-registered.jpg){width=150px}

[Qwiic®](https://www.sparkfun.com/qwiic){target=_blank} is a connection system introduced by SparkFun. It is designed to simply plug and use multiple modules from different manufacturers. With over 100 third-party modules available, you can easily extend RoboBoard X4 functionality without any soldering or jumper wires. With daisy-chain feature - multiple modules can be connected just trough single port.  

!!! info "Note"
    Feature is only available in board revision v1.1

## Connector

It uses standard 4-pin JST connector with I2C communication and 3.3V power line. Modules usually contains 2 connectors for joining multiple of them in a single line.  

![Qwiic connector](/assets/images/x4-1.1-back-desc.jpg){style="width: 350px; height: 80px; object-fit: cover; object-position: 0% 68%;"}

**Wire signals:**  
• Black = **GND**  
• Red = <span style="color:red">3.3V</span>  
• Blue = <span style="color:blue">SDA</span>  
• Yellow = <span style="color:#FFC300">SCL</span>  

## Using with RoboBoard X4

![X4 qwiic](/assets/images/x4-qwiic.jpg){width=500px}

To setup Qwiic® module programming, follow these steps:

1. Plug in module using Qwiic cable.
1. Identify module and find it in manufacturer website (e.g. [SparkFun Micro OLED](https://www.sparkfun.com/products/14532){target=_blank}).
1. Select ++"DOCUMENTS"++ tab.  
![X4 qwiic](/assets/images/sparkfun-documents.png){width=500px}  
Look for `Arduino Library` link. If it doesn't exist - select `GitHub`. There should be information where to find required library.  
1. Inside GitHub website you should find Arduino library that is built for this specific Qwiic module. In case for `SparkFun Micro OLED` it is [SparkFun Micro OLED Arduino Library](https://github.com/sparkfun/SparkFun_Micro_OLED_Arduino_Library){target=_blank}.
1. Click on `library.properties` file, copy library name, paste it to Arduino IDE Library Manager and install it.  
_If you can't find it in Library Manager, download github repository and [install manually](https://docs.arduino.cc/software/ide-v1/tutorials/installing-libraries#importing-a-zip-library){target=_blank}._  
![X4 qwiic](/assets/images/sparkfun-library.jpg)
1. Load example by selecting `File` → `Examples` and upload to the board. No additional editing is required. You should see module start to work.  
Of course you can use all X4 features along with Qwiic modules or TotemBUS.  
![X4 qwiic](/assets/images/arduino-example-qwiic-oled.jpg)

## Totem example

Code example to display text as in picture:  
![X4 qwiic](/assets/images/x4-qwiic.jpg){style="width: 300px; height: 50px; object-fit: cover; object-position: 0% 55%;"}
```arduino
#include <Wire.h>
#include <SFE_MicroOLED.h> // SparkFun Micro OLED Breakout library

#define PIN_RESET GPIOA    // Not used
MicroOLED oled(PIN_RESET); // I2C declaration

void setup() {
  Wire.begin(); // Initialize I2C
  
  oled.begin(0x3D, Wire); // Initialize OLED
  
  oled.clear(ALL); // Clear display internal memory
  oled.clear(PAGE); // Clear buffer

  oled.setFontType(1); // Change font for bigger letters
    
  oled.setCursor(0, 0); // Set cursor to top-left
  
  oled.println("Robo"); // Print text
  oled.println("Board");
  oled.println("X4");
  
  oled.display();  // Display what's in the buffer
}

void loop() {
  
}
```
## Comparison to TotemBUS

|    | TotemBUS | Qwiic® |
| :- | :------: | :----: |
| **Connector** | 6-pin micro match  | 4-pin JST |
| **Wire length** | 100M | 1M |
| **Voltage** | 5V and 12V | 3.3V   |
| **Current** | 1A | 226mA |
| **BUS name** | CAN | I2C |
| **BUS type** | peer-to-peer | master-slave |
| **Speed**    | 500 kb/s | 100 kb/s or 400 kb/s |
| **Protocol** | TotemBUS | module specific |
| **API** | included | library per module |
| **Daisy-chain** | yes | yes |
| **Data pooling** | yes | yes |
| **Data braodcast** | yes | no |
| **Events (interrupt)** | yes | no |
| **X4 as slave** | yes | no |
| **Use same modules** | yes | no* |

\* Identical modules on single line can be used if they are able to change I2C address.
