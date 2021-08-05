# API information

*[API]: Application Programming Interface

This section contains documentation for Totem Library API. It's a set of functions allowing to control Totem Modules connected to TotemBUS network.  

- [ModuleData](/API/ModuleData) - Object containing data received from module.  
- [MotorDriver](/API/MotorDriver) - Object helping to control DC and Servo motors.  
- [TotemModule](/API/TotemModule) - Instance of Totem module, allowing to send and receive commands.  
- [TotemRobot](/API/TotemRobot) - Instance of connection to a wireless or remote access capable Totem module.  

## Example

An example of bare minimum code required to control [RoboBoard X4](/modules/04):

```arduino
// Include Totem Library
#include <Totem.h>
// Arduino initialization function
void setup() {
  Totem.X4.begin(); // Initialize RoboBoard X4
}
// Arduino infinite loop function
void loop() {
  Totem.X4.write("rgbAll", 0xFFFF0000); // Set RED color
  delay(1000); // Wait 1 second
  Totem.X4.write("rgbAll", 0xFF00FF00); // Set GREEN color
  delay(1000); // Wait 1 second
  Totem.X4.write("rgbAll", 0xFF0000FF); // Set BLUE color
  delay(1000); // Wait 1 second
}
```
