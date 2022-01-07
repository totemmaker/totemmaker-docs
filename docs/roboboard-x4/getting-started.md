# Getting Started with X4

RoboBoard X4 is a versatile product and can be used in multiple ways:  

* Remote Bluetooth controller using Totem App  
* Program custom operations using Arduino  
* Control X4 from external devices

## Connecting with smartphone

RobotBoard X4 is designed to run user programmed code while still maintain controlling with a smartphone. Connection is similar as with Totem Mini Control Board X3:

1. Run [Android](https://play.google.com/store/apps/details?id=lt.aldrea.karolis.totemandroid&hl=en&gl=US){target=_blank} | [iOS](https://apps.apple.com/us/app/totemmaker/id1440494243){target=_blank} Totem app.
1. Connect battery (or DC adapter) to X4 and set power switch to on.
1. Click Connect button and select "Totem X4" from the list.

Now, you can control your robot right away. If you bought one of the kits, you will find it in the list of Totem App menu. In case of making your own robot - click + button to create a new model. For more information read [Totem App](/remote-control/app/) section.

## Programming X4

![Arduino IDE](/assets/images/arduino-ide-image1.png)

```arduino
// Structure to hold generated color values
struct Color {
    int R, G, B;
};
// Function to generate random color
Color getRandomColor() {
    Color color;
    color.R = random(255); // Get random red color
    color.G = random(255); // Get random green color
    color.B = random(255); // Get random blue color
    return color;
}
// Function to update RGB leds with provided color array
void updateLeds(Color colors[4]) {
    X4.rgbA.color(colors[0].R, colors[0].G, colors[0].B);
    X4.rgbB.color(colors[1].R, colors[1].G, colors[1].B);
    X4.rgbC.color(colors[2].R, colors[2].G, colors[2].B);
    X4.rgbD.color(colors[3].R, colors[3].G, colors[3].B);
}
// Function called on module event
void buttonPressEvent() {
    // Check if received button event and it's pressed
    if (X4.button.isPressed()) {
        // Update all leds with random color
        Color color = getRandomColor();
        X4.rgb.color(color.R, color.G, color.B);
    }
}
// Array holding current color of all leds
Color colors[4];
// Arduino setup function.
void setup() {
    // put your setup code here, to run once:
    // Register button event function "buttonPressEvent"
    X4.button.addEvent(buttonPressEvent);
}
// Arduino loop function
void loop() {
    // Skip leds updating while button is pressed
    if (X4.button.isPressed())
        return;
    // Shift all colors in array forward
    for (int i=3; i>0; i--) {
        colors[i] = colors[i-1];
    }
    // Insert a new color
    colors[0] = getRandomColor();
    // Update leds with new colors
    updateLeds(colors);
    // Repeat each 100 milliseconds
    delay(100);
}
```

To start programming, you need to install Arduino environment on your PC. It will help to write a code, compile it and upload to X4 board. Follow tutorial [Arduino setup](/setup) to do so.

By following setup tutorial, you should have working X4 board with blinking LEDs. If something doesn't work, create a topic at [forum.totemmaker.net](https://forum.totemmaker.net){target=_blank} and we will help you to resolve the issue.

![Module X4 LedBlink](/assets/images/module_04_LedBlink.gif)

## Writing your own program

Lets write a simple program to jump LEDs between red, green, blue colors. The code required to do so:

```arduino linenums="1"
// Arduino setup function.
void setup() {  
  X4.rgb.color(255, 255, 255);
  delay(1000);
}
// Arduino loop function
void loop() {
  X4.rgb.color(255, 0, 0);
  delay(700);
  X4.rgb.color(0, 255, 0);
  delay(700);
  X4.rgb.color(0, 0, 255);
  delay(700);
}
```

> 2 | `#!arduino void setup() {`

Arduino initialization function. This code block will only run once, after each powerup or reset of the X4 board. After finishing, it goes to `#!arduino loop()`.

> 5 | `#!arduino X4.rgb.color(255, 255, 255);`

Set white color to RGB LED. This will be executed only once after power on (because called inside `setup()`).

> 4 | `#!arduino delay(1000);`

Wait 1 second before going to next line of code.

> 7 | `#!arduino void loop()`

Arduino processing function. It executes code inside this block infinitely until board is powered off. If `#!arduino loop()` reaches end of the block, it repeats again from the top (line 8).

> 8 | `#!arduino X4.rgb.color(255, 0, 0);`

Function to set color of RGB LED.  
`X4` - X4 board class containing all functionality.  
`"rgb"` - functions group that affects all 4 RGB LED at once. For the list of all available commands see [X4.rgb](/roboboard-x4/rgb/).  
`255, 0, 0` - parameters setting LED color. Values 0-255 corresponds to 0-100%. The meaning is in order: Amount of Red color, Amount of Green color, Amount of Blue color.  

> 9 | `#!arduino delay(700);`

`#!arduino loop()` runs very fast. This function waits 700 milliseconds until next line of code is executed. Without this function colors would change so fast we couldn't even see.

Now paste code example to Arduino IDE and press Upload.

![Arduino IDE LedBlink](/assets/images/arduino-ide-led-example.gif)

LEDs should start blinking. You can try changing parameters and see what happens.

![Module X4 LedBlink](/assets/images/module_04_LedBlink2.gif)
