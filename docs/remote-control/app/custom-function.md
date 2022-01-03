# Custom functions

!!! info
    Only available in Totem Android application version

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
1. Click ++"Next"++ → ++"Done"++ → ++"Save"++.
1. Select ++"Play"++ mode.

Now, when we click this button, a value `100` will be sent to command `functionA`. On release - `-100`. These values can be changed by adjusting the slider. X4 board can intercept this command and read it's value. By doing so, we can program a certain tasks to execute. There are 4 auxiliary commands in total: `functionA`, `functionB`, `functionC`, `functionD` and each one can be used.

## Moving servo motor

To get started with X4 programming, read [Getting started with RoboBoard X4](/tutorials/GettingStartedX4/){target=_blank} first.

We will write a code to wave servo arm. The motor itself is connected to servo channel A. Arm can move 180 degrees, and parameter values corresponds in range from `-100` to `100`. Look at the image below to see how it's related:

![Servo arm angles](/assets/images/servo_arm_angles.png)

So, we will try to move arm in this sequence: -45°, 0°, 45°, 0°. The code required to do so:

```arduino linenums="1"
void moveServo() {
  X4.servoA.pos(-50);
  delay(300);
  X4.servoA.pos(0);
  delay(300);
  X4.servoA.pos(50);
  delay(300);
  X4.servoA.pos(0);
  delay(300);
}

void setup() {

}

void loop() {
  moveServo();
  delay(2000);
}
```

**Line 1:** `#!arduino void moveServo() {` defines a function, containing instructions to wave a servo arm.  
**Line 2:** `#!arduino X4.servoA.pos(-50);` function call to set servo channel A to position `-50` (-45°).  
**Line 3:** `#!arduino delay(300);` delays code executions for 300 milliseconds.  

Inside function `#!arduino loop()` we are calling `#!arduino moveServo()` to execute sequence and it is repeated every 2 seconds. The result should look like this:

![Module X4 LedBlink](/assets/images/module_04_servo.gif)

## Detecting App button press

Now let's move this servo arm only when in-App button is pressed. As bonus, we will indicate button press with on-board LED. To detect command events we need to add a little bit more code:

```arduino linenums="1"
void moveServo() {
  X4.servoA.pos(-50);
  delay(300);
  X4.servoA.pos(0);
  delay(300);
  X4.servoA.pos(50);
  delay(300);
  X4.servoA.pos(0);
  delay(300);
}

bool buttonPressed = false;

void eventFunctionA() {
  if (X4.functionA.get() == 100) {
    X4.led.on();
    buttonPressed = true;
  }
  else {
    X4.led.off();
    buttonPressed = false;
  }
}

void setup() {
  X4.functionA.addEvent(eventFunctionA);
}

void loop() {
  if (buttonPressed) {
    moveServo();
  }
}
```

**Line 12:** `#!arduino bool buttonPressed = false;` creating variable to hold the state of button press.  
**Line 14:** `#!arduino void eventFunctionA() {` define a function to receive `functionA` events.  
**Line 15:** `#!arduino if (X4.functionA.get() == 100) {` check if command `functionA` received value 100 (button is pressed in).  
**Line 16:** `#!arduino X4.led.on();` light up on-board LED.  
**Line 17:** `#!arduino buttonPressed = true;` set variable `buttonPressed` state to `true`.  
**Line 19:** `#!arduino else {` code block executed when `functionA` receives value that isn't `100` (button was released). Code inside this block turns off on-board LED and sets `buttonPressed` variable to `false`.  
**Line 26:** `#!arduino X4.functionA.addEvent(eventFunctionA);` register function `eventFunctionA` to receive `functionA` events from App.  
**Line 30:** `#!arduino if (buttonPressed) {` wait till `buttonPressed` variable state becomes `true` and then execute `moveServo()` function.  

When uploaded this code to X4 board, connect with Totem App and try pressing created button. Servo arm should move and LED light up.  
