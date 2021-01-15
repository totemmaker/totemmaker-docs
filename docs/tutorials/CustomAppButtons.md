# Custom App buttons

Normally the buttons inside Totem app are mapped to motor outputs directly. If we press a button - motor starts to spin, and stops on release. This is quite limited functionality. X4 board allows to have custom actions programmed and we will make an example of waving a servo motor arm.

## Adding control button

<video width="30%" controls autoplay loop>
  <source src="/assets/videos/totemfunctionbuttonapp.m4v" type="video/mp4">
Your browser does not support the video tag.
</video>

For more about buttons editing you can read [Setting up Totem robot controls](https://totemmaker.net/blog/setting-up-totem-robot-controls/){target=_blank}.

1. Power on X4 board.
1. Connect with Totem App.
1. Create a new model.
1. Add button.
1. Go to button edit.
1. Select X4 board and add `functionA`.
1. Click ++"Next"++ > ++"Done"++ > ++"Save"++.
1. Select ++"Play"++ mode.

Now, when we click this button, a value `100` will be sent to command `functionA`. On release - `-100`. These values can be changed by adjusting the slider. X4 board can intercept this command and read it's value. By doing so, we can program a certain tasks to execute. There are 4 auxiliary commands in total: `functionA`, `functionB`, `functionC`, `functionD` and each one can be used.

## Moving servo motor

To get started with X4 programming, read [Getting started with RoboBoard X4](https://totemmaker.net/blog/setting-up-totem-robot-controls/){target=_blank} first.

We will write a code to wave servo arm. The motor itself is connected to servo channel A. Arm can move 180 degrees, and parameter values corresponds in range from `-100` to `100`. Look at the image below to see how it's related:

![Servo arm angles](/assets/images/servo_arm_angles.png)

So, we will try to move arm in this sequence: -45°, 0°, 45°, 0°. The code required to do so:

```arduino linenums="1"
#include <Totem.h>

void moveServo() {
  Totem.X4.write("servoA", -50);
  delay(300);
  Totem.X4.write("servoA", 0);
  delay(300);
  Totem.X4.write("servoA", 50);
  delay(300);
  Totem.X4.write("servoA", 0);
  delay(300);
}

void setup() {
  Totem.X4.begin();
}

void loop() {
  moveServo();
  delay(2000);
}
```

**Line 3:** `#!arduino void moveServo() {` defines a function, containing instructions to wave a servo arm.  
**Line 4:** `#!arduino Totem.X4.write("servoA", -50);` function call to set servo channel A to position `-50` (45°).  
**Line 5:** `#!arduino delay(300);` delays code executions for 300 milliseconds.  

Inside function `#!arduino loop()` we are calling `#!arduino moveServo()` to execute sequence and it is repeated every 2 seconds. The result should look like this:

![Module X4 LedBlink](/assets/images/module_04_servo.gif)

## Detecting App button press

Now let's move this servo arm only when in-App button is pressed. As bonus, we will indicate button press with on-board LED. To detect command events we need to add a little bit more code:

```arduino linenums="1"
#include <Totem.h>

void moveServo() {
  Totem.X4.write("servoA", -50);
  delay(300);
  Totem.X4.write("servoA", 0);
  delay(300);
  Totem.X4.write("servoA", 50);
  delay(300);
  Totem.X4.write("servoA", 0);
  delay(300);
}

bool buttonPressed = false;

void onModuleData(ModuleData data) {
  if (data.is("functionA")) {
    if (data.getInt() == 100) {
      Totem.X4.write("led", 1);
      buttonPressed = true;
    }
    else {
      Totem.X4.write("led", 0);
      buttonPressed = false;
    }
  }
}

void setup() {
  Totem.X4.begin();
  Totem.X4.attachOnData(onModuleData);
  Totem.X4.subscribe("functionA");
}

void loop() {
  if (buttonPressed) {
    moveServo();
  }
}
```

**Line 14:** `#!arduino bool buttonPressed = false;` creating variable to hold the state of button press.  
**Line 16:** `#!arduino void onModuleData(ModuleData data) {` define a function to receive events from X4 board.  
**Line 17:** `#!arduino if (data.is("functionA")) {` check if received command is `functionA`.  
**Line 18:** `#!arduino if (data.getInt() == 100) {` check if command `functionA` received value 100 (button is pressed in).  
**Line 19:** `#!arduino Totem.X4.write("led", 1);` light up on-board LED.  
**Line 20:** `#!arduino buttonPressed = true;` set variable `buttonPressed` state to `true`.  
**Line 22:** `#!arduino else {` code block executed when `functionA` receives value that isn't `100` (button was released). Code inside this block turns off on-board LED and sets `buttonPressed` variable to `false`.  
**Line 31:** `#!arduino Totem.X4.attachOnData(onModuleData);` register function `onModuleData` to receive X4 board events.  
**Line 32:** `#!arduino Totem.X4.subscribe("functionA");` subscribe to receive `functionA` command events.  
**Line 36:** `#!arduino if (buttonPressed) {` wait till `buttonPressed` variable state becomes `true` and then execute `moveServo()` function.  

When uploaded this code to X4 board, connect with Totem App and try pressing created button. Servo arm should move and LED light up.  
If something isn't working as expected, create a topic on our forum at [forum.totemmaker.net](https://forum.totemmaker.net){target=_blank}. Will be happy to help!
