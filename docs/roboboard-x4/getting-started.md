# Getting Started with X4

RoboBoard X4 is a versatile product and can be used in multiple ways:  

* Remote Bluetooth controller using Totem App  
* Program custom operations using Arduino  
* Extend functionality with Totem modules  
* Extend functionality with Qwiic® modules
* Control X4 from external devices

## Connecting with smartphone

RobotBoard X4 is designed to run user programmed code while still maintain controlling with a smartphone. Connection is similar as with Totem Mini Control Board X3:

1. Run [Android](https://play.google.com/store/apps/details?id=lt.aldrea.karolis.totemandroid&hl=en&gl=US){target=_blank} | [iOS](https://apps.apple.com/us/app/totemmaker/id1440494243){target=_blank} Totem app.
1. Connect battery (or DC adapter) to X4 and set power switch to on.
1. Click Connect button and select "Totem X4" from the list.

Now, you can control your robot right away. If you bought one of the kits, you will find it in the list of Totem App menu. In case of making your own robot - click + button to create a new model. You can follow tutorial [Setting up Totem robot controls](https://totemmaker.net/blog/setting-up-totem-robot-controls/){target=_blank} to learn creating custom controls in Totem App.

## Programming X4

![Arduino IDE](/assets/images/arduino_ide_2.png)

To start programming, you need to install Arduino environment on your PC. It will help to write a code, compile it and upload to X4 board. Follow tutorial [Arduino setup](/tutorials/01.ArduinoSetup) to do so. In this guide we will use Arduino IDE as example.

By following setup tutorial, you should have working X4 board with blinking LEDs. If something doesn't work, create a topic at [forum.totemmaker.net](https://forum.totemmaker.net){target=_blank} and we will help you to resolve the issue.

![Module X4 LedBlink](/assets/images/module_04_LedBlink.gif)

## Writing your own program

Lets write a simple program to jump LEDs between red, green, blue colors. The code required to do so:

```arduino linenums="1"
#include <Totem.h>
// Arduino setup function.
void setup() {
    // Initialize RoboBoard X4
    Totem.X4.begin();
}
// Arduino loop function
void loop() {
    Totem.X4.write("rgbAll", 255, 255, 0, 0);
    delay(700);
    Totem.X4.write("rgbAll", 255, 0, 255, 0);
    delay(700);
    Totem.X4.write("rgbAll", 255, 0, 0, 255);
    delay(700);
}
```

Take a look at each line:

> 1 | `#!arduino #include <Totem.h>`

Including Totem Library into code. By doing this, we tell compiler what to to with `Totem.X4` functions. For all available functions read [TotemModule](/remote-control/arduino/TotemModule/) reference.

> 3 | `#!arduino void setup() {`

Arduino initialization function. This code block will only run once, after each powerup or reset of the X4 board. After finishing, it goes to `#!arduino loop()`.

> 5 | `#!arduino Totem.X4.begin();`

Function required to initialize RoboBoard X4. It starts Totem Library and background processes required for the `Totem.X4.write` function to work.

> 8 | `#!arduino void loop()`

Arduino processing function. It executes code inside this block infinitely until board is powered off. If `#!arduino loop()` reaches end of the block, it repeats again from the top (line 9).

> 9 | `#!arduino Totem.X4.write("rgbAll", 255, 255, 0, 0);`

Function to set value of RGB LED.  
`Totem.X4.write` - function to send a command to X4 board.  
`"rgbAll"` - command to change color of all 4 RGB LED. For the list of all available commands see [RoboBoard X4 reference](/modules/04).  
`255, 255, 0, 0` - parameters setting LED brightness and color. Values 0-255 corresponds to 0-100%. The meaning is in order: Brightness, Amount of Red color, Amount of Green color, Amount of Blue color.  

> 10 | `#!arduino delay(700);`

`#!arduino loop()` runs very fast. This function waits 700 milliseconds until next line of code is executed. WIthout this function colors would change so fast we couldn't even see.

Now paste code example to Arduino IDE and press Upload.

![Arduino IDE LedBlink](/assets/images/arduino_ide_3.gif)

LEDs should start blinking. You can try changing parameters and see what happens.

![Module X4 LedBlink](/assets/images/module_04_LedBlink2.gif)
