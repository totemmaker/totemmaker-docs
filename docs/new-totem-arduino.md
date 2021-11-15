# NEW Totem Arduino Framework

We have released Arduino implementation built for RoboBoard X4. Now you can simply install Totem framework to and start programming without any additional configuration or setup.
Whole environment is built around X4 board to bringing better user experience, full functionality and extensive objective API to control modules and board itself. TotemLibrary is no more required!

```arduino title="Example of new programming API"
// Initialize modules
Module14 line; // Line follower module
Module15 pot;  // Potentiometer module
// Initialize program
void setup() {
  // Empty
}
// Loop program
void loop() {
  // Receive Potentiometer module knob positions
  int knobA = pot.knobA.get();
  int knobB = pot.knobB.get();
  int knobC = pot.knobC.get();
  // Display Knob A position on Line Follower module
  line.led.off();
  line.led.onX(
    map(knobA, 0, 255, 0, 7) // map [0:255] -> [0:7]
  );
  // Change RoboBoard X4 RGB color depending on Knob A, B positions
  X4.rgb.color(
    knobB, knobC, 255-knobC, 0 // alpha, reg, green, blue
  );
  // Delay 20 milliseconds
  delay(20);
}
```

## See what changed

- [x] Integration with Arduino IDE (*install trough Board Manager*)  
- [x] Select Totem App connectivity in Arduino IDE context menu (*Tools -> App control*)  
- [x] `Totem.X4.begin()` is no more required. Can compile empty sketch  
- [x] `#include <Totem.h>` is no more required.  
- [x] Easy to use objective API. Perfect with autocomplete feature  
- [x] New code examples for each module and their functionality  
- [x] New events system  
- [x] Add more loop() functions with `addLoop()`  
- [x] RoboBoard X4 driver chip updater (*Burn Bootloader*)  
- [x] New RoboBoard X4 features:
    * [x] Config DC motor to play tones (sounds)
    * [x] Change DC motor frequency
    * [x] Control Servo motor speed
    * [x] Create RGB fade animations
- [ ] Integration with PlatformIO (*just select RoboBoard X4 from the list*)  
- [ ] Setup guide for Arduino IDE and PlatformIO
- [ ] Documentation for new objective API
- [ ] Fix MEMS example crashing

This is pre-release version, so few things are still missing out. Later this documentation site will adapt new changes.  

## Setup guide

**Step 1:** Install Arduino IDE.  
: Go to [download page](https://www.arduino.cc/en/main/software){target=_blank} to install Arduino IDE.

**Step 2:** Install Totem Boards.  

1. In Arduino IDE select `File` → `Preferences`.  
1. Locate `Additional Boards Manager URLs` and add:  
    `https://totemmaker.github.io/TotemArduinoBoards/package_totemmaker_index.json`  
1. Click ++"OK"++.  
1. Select `Tools` → `Board` → `Boards Manager..`  
1. In search field type `totem` (find `Totem Boards`).  
1. Click ++"Install"++ and wait for it to complete. It can take a few minutes.  
1. Close Boards Manager window.  

**Step 5:** Select board.  
:   Select `Tools` → `Board` → `Totem Boards` → `RoboBoard X4`.  

**Step 7:** Connect X4 board.  
:   Connect X4 board to PC over USB cable and power it on.  
    _Make sure the battery or DC jack is connected and power switch is turned on._  
    _X4 is not powered from USB alone!_  
**Step 8:** Select port.  
:   Select `Tools` → `Port` and click on the port displayed there.  
    _If `Port` is not available - check if X4 is on and plugged to PC with USB cable._  
    _If there are multiple ports, disconnect USB cable and check which one is gone. Reconnect and select it._  
**Step 6:** Update internal driver to latest version.  
:   Select `Tools` → `Burn Bootloader`.  
    _Wait till board updates itself and turns RGB LED green._  
    _**Only required once to access new functionality!**_  
**Step 6:** Load example code.  
:   Select `File` → `Examples` → `> Board` → `LED` → `LedBlink`.  
    _A new window with example code will open._  
**Step 9:** Compile and upload code.  
:   Select `Sketch` → `Upload` and wait till it completes (can take a few minutes first time).  
    _After completion LED should start blinking (different patterns)._  
    
**If encountered any issue - [create a topic](https://forum.totemmaker.net/c/questions/10){target=_blank} to resolve it.**  

## Getting started

Until documentation is ready, you can find some information inside header files of repository:

- _API_ [Module11](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemBUS/include/module/Module11.h) Distance sensor.  
- _API_ [Module14](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemBUS/include/module/Module14.h) Line follower.  
- _API_ [Module15](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemBUS/include/module/Module15.h) Potentiometer module.  
- _API_ [Module22](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemBUS/include/module/Module22.h) Environment sensor.  
- _API_ [RoboBoard X4 base](https://github.com/totemmaker/TotemArduinoBoards/blob/master/variants/roboboard_x4/RoboBoardX4.h)  
- _API_ [RoboBoard X4 all features](https://github.com/totemmaker/TotemArduinoBoards/blob/master/variants/roboboard_x4/RoboBoardX4Features.h)  
- _examples_ [Modules example code](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemBUS/examples)  
- _examples_ [Qwiic I2C scanner](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemQwiic/examples/ListConnectedBoards/ListConnectedBoards.ino)  
- _examples_ [RoboBoard X4 examples](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples)  

