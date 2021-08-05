# Interfaces information

*[BLE]: Bluetooth Low Energy

[Totem Library](https://github.com/totemmaker/TotemArduino){target=_blank} is designed to support multiple ways to connect TotemBUS network. Each one has different capabilities and may be build for multiple development platforms. The main goal - passthrough for Totem messages.  
Each required interface should be initialized at `setup()` function. Once `begin()` function is called, all `TotemModule` objects are assigned to initialized interface and can communicate with Totem modules.

## BLE

`Totem.BLE.begin()`  
Provides remote communication with BLE capable Totem modules. This interface can be used whit any ESP32 development board to control Totem modules, even from multiple connections. Interface provides all required functionality to discover Totem robots and connect them.  
Modules are able to represent themselves as "robot", providing name, color, model (type of robot). This information is available before BLE connection is established. Same information is visible when connecting with mobile Totem app.  
For full interface documentation read [BLE Interface](/interfaces/BLE/).

```arduino
// Include Totem Library
#include <Totem.h>
// Declare communication with any connected Totem module
TotemModule module(0);
// Arduino initialization function
void setup() {
  Serial.begin(115200);
  Totem.BLE.begin(); // Start Bluetooth Low Energy interface
  Serial.println("Searching for Totem robot...");
  // Wait until connected to first found Totem robot
  TotemRobot robot = Totem.BLE.findRobot();
  // At this point further execution of setup() is blocked
  // until library connects to the robot
  Serial.print("Connected to: ");
  Serial.println(robot.getName());
  // Continue to loop()
}
// Arduino infinite loop function
void loop() {
  // Control motor of connected robot
  module.write("motorA", 100); // Set motor A channel to 100% power
  delay(1000); // Wait 1 second
  module.write("motorA", 0); // Stop motor on A channel
  delay(1000); // Wait 1 second
}
```

## X4

`Totem.X4.begin()`  
Used **only** with Totem [RoboBoard X4](/modules/04) to enable all functionality and on-board features:

* Implements all RoboBoard X4 functionality.
* Makes RoboBoard X4 discoverable over Bluetooth.
* Provides direct access to `TotemModule` functions (`Totem.X4.write("indicate")`).
* Enables passthrough to communicate with external modules connected over TotemBUS.

Can be used concurrently with `Totem.BLE.begin()`.  
For full interface documentation read [X4 Interface](/interfaces/X4/).

```arduino
// Include Totem Library
#include <Totem.h>
// Arduino initialization function
void setup() {
  Totem.X4.begin(); // Start X4 interface
  // Continue to loop()
}
// Arduino infinite loop function
void loop() {
  // Blink RGB leds
  Totem.X4.write("rgbAll", 0xFF, 0xFF, 0, 0); // Set RGB leds to RED
  delay(500); // Wait 500 milliseconds
  Totem.X4.write("rgbAll", 0xFF, 0, 0xFF, 0); // Set RGB leds to GREEN
  delay(500); // Wait 500 milliseconds
  Totem.X4.write("rgbAll", 0xFF, 0, 0, 0xFF); // Set RGB leds to BLUE
  delay(500); // Wait 500 milliseconds
}
```
