# RoboBoard X4

*[PC]: Personal Computer
*[DC]: Direct Current
*[DIY]: Do It Yourself
*[PWM]: Pulse-Width Modulation
*[RGB]: Red Green Blue
*[LEDS]: Light-Emitting Diodes
*[BLE]: Bluetooth Low Energy
*[GPIO]: General Purpose Input Output
*[WiFi]: Wireless technology that uses radio waves to provide network connectivity
*[Bluetooth]: Wireless technology that uses radio waves to connect devices (smartphones, headsets, ...)

## Programming

This section contains RoboBoard X4 functionality documentation and usage examples.  
An objective API is available to control the board - `X4.<feature>.<function>()`.  
`X4` - main board group containing all the features.  
`<feature>` - features that are supported by X4 (listed below).  
`<function>` - functionality that is available for each feature.  

For more information click on specific feature:  

* :material-information-outline: | [Board info](board) - General board information like version and battery status.  
* :material-screen-rotation: | [MEMS](mems) - Accelerometer and Gyroscope.  
* :material-google-circles-extended: | [GPIO](gpio) - General purpose input output pins. Similar like Arduino boards.  
* :material-wrench: | [Config](config) - RoboBoard X4 settings.  
* :material-gesture-tap-button: | [Button](button) - Programmable button.  
* :material-led-outline: | [Led](led) - Status LED.  
* :traffic_light: | [RGB](rgb) - Bright color LEDs.  
* :material-tire: | [DC](dc) - DC motor to spin wheels.  
* :material-angle-acute: | [Servo](servo) - Servo motor for positioning or steering.  
* :material-puzzle: | [Module](module) - Connected TotemBUS modules information.  
* :fontawesome-solid-mobile-alt: | [Function](function) - Events sent from mobile Totem App.  

## Feature details

It's our latest control board containing powerful processor and motor drivers. Just plug in battery, motors and everything will work without any soldering or customization. Look bellow for a brief list of available functionality.  

**Processor:**  
• ESP32 module (ESP32-D0WD)  
• Dual-core 240Mhz (Xtensa® LX6)  
• 520KB SRAM (328KB usable)  
• 8MB flash  

**Connectivity:**  
• Wi-Fi (up to 150 Mbps)  
• Bluetooth (BLE)  
• Totem BUS  
• USB port  

**Extensions:**  
• Totem BUS (attach [Totem modules](/modules))  
• Qwiic® (over 100 third-party modules available)  
• 4 programmable GPIO pins  

**On-board features:**  
• 3 Servo channels (5 Volts)  
• 4 DC channels (11.1 Volts, 1 Amp each)  
• 4 GPIO channels  
• 4 bright RGB LED  
• MEMS sensor (gyroscope and accelerometer)  
• Programmable button  
• Reset button  
• Status LED  
• On/off switch  
• DC & battery input, integrated charger  

**Power:**  
• DC adapter: 30W, 15V, 2A, 5.5/2.0mm center-positive connector  
• Battery: 3S Li-Ion 11.1V 2200mAh, JST-VH connector  

**Dimensions:**  
• 7x5x1.4 cm (L x W x H)  

=== "Front v1.1"
    ![X4 Board image front](/assets/images/x4-1.1-front-desc.jpg){style="display: block;margin-left: auto;margin-right: auto;width: 100%;"}
=== "Back v1.1"
    ![X4 Board image back](/assets/images/x4-1.1-back-desc.jpg){style="display: block;margin-left: auto;margin-right: auto;width: 100%;"}
=== "Front v1.0"
    ![X4 Board image front](/assets/images/x4-1.0-front-desc.jpg){style="display: block;margin-left: auto;margin-right: auto;width: 100%;"}  
    _Revision v1.0 is not sold anymore._
=== "Back v1.0"
    ![X4 Board image front](/assets/images/x4-1.0-back.jpg){style="display: block;margin-left: auto;margin-right: auto;width: 80%;"}  
    _Revision v1.0 is not sold anymore._

### :fontawesome-solid-microchip: ESP32 240Mhz processor

Board is powered by dual-core 240Mhz ESP32 processor with 8MB of flash. It's very popular among Arduino community and has built-in WiFi and Bluetooth technologies. This allows to communicate with a smartphone wirelessly and run user code inside. Not only that - it can connect to the Internet to unlock unlimited possibilities. You can find many example code and tutorials for this MCU.

### :material-angle-acute: 3 Servo channels

5 Volt servo motor channels marked with letter A, B, C. Can be controlled individually.  
Most servo motors can turn it's arm 180 degrees. 0° - center, 90° - left, -90° - right. Pulse duration is 500μs-2500μs (can be modified with [`setPulseMinMax()`](servo/#servox.setPulseMinMax)). It corresponds with percentage of turn: 0% - center, 100% - left, -100% - right. X4 functions accepts percentage ([`pos()`](servo/#servox.pos)), angle (0°-180°) ([`angle()`](servo/#servox.angle)) and microsecond ([`pulse()`](servo/#servox.pulse)) parameters.  
Servo angle visualization:  
![Servo arm angles](/assets/images/servo_arm_angles.png)

```arduino
X4.servoA.pos(100); // Set channel A to 90° angle
X4.servoB.pos(0);   // Set channel B to 0° angle
X4.servoC.pos(-50); // Set channel C to -45° angle
```

### :material-tire: 4 DC channels

11.1 Volt DC motor channels marked with letter A, B, C, D. Can be controlled individually.  
Allows to control motor spin direction, power (speed) and braking. Some more features like [modifying frequency](dc/#dcxx.setFreq) and [playing tones](dc/#dcxx.tone) are available. X4 passes current from connected battery and controls output voltage with PWM. The higher the voltage, the faster motor will spin. Maximum voltage may vary depending on battery state of charge (8.4V-12.6V).  
Power values corresponds with percentage:  
0% - (no power, 0.0V)  
100% - (full power forward, 11.1V)  
-100% - (full power backward, -11.1V)  

```arduino
X4.motorA.power(100); // Set channel A to spin forward (11.1V)
X4.motorB.power(0);   // Set channel B to power off (0.0V)
X4.motorC.power(-100); // Set channel C to spin backward (-11.1V)
X4.motorD.power(-25);  // Set channel D to spin backward (-2.78V)
```

### :material-google-circles-extended: 4 GPIO channels

4 integrated GPIO pins are available to hook up simple IO or communications (UART, I2C, SPI, ...). It can be used for custom implementations, modules and other creative things. Using standard Arduino programming they can be accessed same as with TotemDuino.  
Connector pinout: VCC - 3.3V source. GND - ground. A, B, C, D - GPIO pins.  
For more information read [X4 GPIO](gpio) section.  

![Module X4 GPIO](/assets/images/module_04_gpio.png)

=== "Output"
    ```arduino
    X4.gpioA.digitalWrite(HIGH); // Set Pin A to 3.3V
    X4.gpioA.digitalWrite(LOW);  // Set Pin A to 0V (GND)
    ```
=== "Input"
    ```arduino
    int state = X4.gpioA.digitalRead(); // Read Pin A state.
    // state == HIGH - connected to VCC, state == LOW - connected to GND
    Serial.println(state == HIGH ? "HIGH" : "LOW");
    ```
=== "Analog output"
    Values from 0 to 20. 50hz modulation.  
    ```arduino
    X4.gpioA.analogWrite(20); // Set Pin A to 3.30V
    X4.gpioA.analogWrite(10); // Set Pin A to 1.65V
    X4.gpioA.analogWrite(0);  // Set Pin A to 0.0V
    ```
=== "Analog input"
    Measure potentiometer position connected to pin C. Wiring:  
    ![Potentiometer connection](/assets/images/potentiometer_connection.png)
    ```arduino
    int value = X4.gpioC.analogRead(); // Read value of potentiometer
    Serial.println(value); // Print value
    ```

### :traffic_light: 4 RGB LED

Back side of the board contains individually controllable [RGB LED](rgb). Each one can be set to particular color and brightness. Also supports fade animations.  

![Module X4 RGB led](/assets/images/module_04_rgb.png)

```arduino
X4.rgbA.color(0, 255, 0); // Set LED A to GREEN color.
X4.rgbB.color(255, 255, 0); // Set LED B to YELLOW color (RED+GREEN).
X4.rgbC.color(255, 255, 0); // Set LED B to YELLOW color (RED+GREEN).
X4.rgbD.color(0, 0, 255); // Set LED D to BLUE color
```

### :material-puzzle: Totem BUS

Totem BUS connectors allows to expand X4 functionality by attaching Totem modules and control them using Arduino code. Each module is identified by it's number in a white square. For reference of each module check [Modules information](/modules/).  
Modules can be connected in any order and daisy-chained. Connector provides power and communication.  

```arduino
Module11 module; // Define connected module
void setup() {
  // Call function to change distance sensor RGB color to green
  module.rgb.color(0, 255, 0);
}
```

### :material-transit-connection-variant: Qwiic

Qwiic is a connection system introduced by SparkFun. It uses standard JST connector across all electronics, so it would work straight away, without a need to think about compatibility. There are many compatible boards from different manufacturers and RoboBoard X4 is one of them. Read [Qwiic section](qwiic) to find out how to use them.

### :material-screen-rotation: MEMS sensor

MEMS is a 3-axis accelerometer and 3-axis gyroscope allowing to measure board movement and angles. Very useful in many robotic projects. This chip is connected to I2C line (same as Qwiic connector) and can be used trough standard Arduino code. Read mode in [MEMS section](MEMS).

### :material-usb-port: USB port

miniUSB connector is present for code upload from PC to X4 board.  
X4 board cannot be powered from USB! It requires either DC or battery input.

### :material-bluetooth: Bluetooth

Uses BLE to advertise board appearance. This allows to connect it with smartphone using Totem App. This process is running in background, alongside your written Arduino code. Option `Tools` → `App control` → `Enabled` must be selected to enable this feature. Also ```#!arduino X4.enableAppControl()``` can be called inside ```#!arduino void setup()``` function to enable this option programmatically.  

Documentation and examples of BLE DIY solutions can be found in [TotemArduinoBoards repository](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/BLE){target=_blank}.

### :material-wifi: WiFi

ESP32 has integrated WiFi capability. It can be used to connect to home network and access Internet. From there possibilities are endless.  

Documentation and examples of WiFi DIY solutions can be found in [TotemArduinoBoards repository](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/WiFi/examples){target=_blank}.

### :material-led-on: LED indications

Red LED next to DC jack - indicated battery charging status:  
:material-white-balance-sunny:{ style="color: red" } Blinking - battery not detected.  
:octicons-dot-fill-24:{ style="color: red" } On - battery is charging.  
:octicons-dot-24: Off - battery is charged.  
_NOTE: sometimes LED can start blinking when battery is charged._  

Red LED next to user button for indicating board state:  
:material-white-balance-sunny:{ style="color: red" } Short rapid blink - board started or indicated.  
:material-white-balance-sunny:{ style="color: red" } Blinking constantly - driver initialization error (consult [forum](https://forum.totemmaker.net/c/questions/10){target=_blank}).  
:octicons-dot-fill-24:{ style="color: red" } On - running.  

Board state LED can be overridden:
```arduino
X4.led.on(); // Turn LED On
X4.led.off(); // Turn LED Off
```

### :material-gesture-tap-button: User button

By default this button is inactive and left for user implementation.  
Example code how to turn off LED on button press:  

```arduino
void setup() { }

void loop() {
  if (X4.button.wasPressed()) {
    X4.led.off();
  }
  else if (X4.button.wasReleased()) {
    X4.led.on();
  }
}
```

### :material-power: On/off switch

Used to turn X4 board power on or off without disconnecting DC jack or battery.  
Battery can be charged while button is set to OFF position.

### :material-power-plug: DC input

15V DC input for powering board and charging battery.  
**Recommended to use only supplied DC power adapter.**  
Specifications: 30W, 15V, 2A, 5.5/2.0mm center-positive connector.

### :material-battery-high: Battery input

Battery input for connecting 11.1V Li-Ion battery.  
**Recommended to use only supplied battery.**  
Specifications: 3S Li-Ion 11.1V 2200mAh, JST-VH connector.

Charging indication: [LED indications](#led-indications)  