# Module programming

A general tutorial common for all modules. For specific `features` and `functions` read module documentation.  

## Accessing specific module

Module API can be accessed by declaring its class. It will work right away, without a need for initialization. Multiple instances can be created.  
If module is not physically connected, all functions calls will be ignored.  

```arduino
TotemModule11 module; // Declare use of distance sensor [11]
TotemModule11 module2(25151); // Declare use of distance sensor [11] with serial 25151
```
`TotemModuleXX` - each module hash class implementation name of `TotemModule` + _number_.  
`module` - a variable name of `TotemModule11`. Can be any (sensor, controller, lineReader, ...). This instance is used to call module functions.  
`(25151)` - if there are multiple modules with same number - serial is used to specify exact one. Using instance `module2` will send commands to specific module only. Otherwise - all connected [11] modules will receive a command. Check [Module scanner](#module-scanner) on how to get serial.  

## General functions

Each module library contains an API to access its functionality.

```arduino title="Communicating with module"
// Read distance from sensor
int distance = module.getMM();
// Set RGB color to green
module.rgb.color(0, 255, 0);
```

## Events

```arduino
TotemModule15 mod15;
TotemModule22 mod22;

void moduleEvt(int evt) {
  if (evt == TotemModule22::evtTemp) printf("Temp: %fC\n", mod22.getTempC());
  if (evt == TotemModule15::evtButtonA) printf("ButtonA: %d\n", mod15.getButtonA());
  if (evt == TotemModule15::evtButtonB) printf("ButtonB: %d\n", mod15.getButtonB());
  if (evt == TotemModule15::evtButtonC) printf("ButtonC: %d\n", mod15.getButtonC());
  if (evt == TotemModule15::evtKnobA) printf("KnobA: %d\n", mod15.getKnobA());
  if (evt == TotemModule15::evtKnobB) printf("KnobB: %d\n", mod15.getKnobB());
  if (evt == TotemModule15::evtKnobC) printf("KnobC: %d\n", mod15.getKnobC());
}

void setup() {
  mod15.addEvent(moduleEvt);
  mod22.addEvent(moduleEvt);
  // Call required functions once to receive
  // values and start broadcasting
  mod22.getTempC();
  mod15.getButtonA();
  mod15.getButtonB();
  mod15.getButtonC();
  mod15.getKnobA();
  mod15.getKnobB();
  mod15.getKnobC();
}
void loop() {

}
```

Reading module functions like `#!arduino module.get()` will always return cached value without blocking the code. This value is constantly updated in background with broadcast messages from module.  
_Code blocks shortly whem calling "get" function first time - to register an event and return value._

Using `addEvent` function is only useful if you need a callback when latest value update from module is received. Events are called only if value has changed compared to last one.

#### module.addEvent(`function`) { #addEvent data-toc-label='addEvent()' }
: Register module event function. Will be called when new data is available.  
Event data can be received simply by accessing `module`. Data is cached and won't initiate read request, but must be called inside registered function.  
**Parameter:**  
`function` - event function name (`distanceEvent`).  

## Multiple modules example

Simple code example to show usage of Line Follower + Potentiometer modules and interaction between them.  
• Knob A - controls LED position on Line Follower  
• Knob B - controls RGB color  
• Knob C - controls RGB color  
```arduino
// Declare modules
TotemModule14 line; // Line follower module
TotemModule15 pot;  // Potentiometer module
// Start program
void setup() {
  // Empty
}
// Loop program
void loop() {
  // Read Potentiometer module knob positions
  int knobA = pot.getKnobA();
  int knobB = pot.getKnobB();
  int knobC = pot.getKnobC();
  // Display Knob A position on Line Follower module
  line.led.off();
  line.led[map(knobA, 0, 255, 0, 7)].on(); // map [0:255] -> [0:7]
  // Change RoboBoard X4 RGB color depending on Knob B, C positions
  RGB.color(
    255-knobB, knobC, 255-knobC // red, green, blue
  );
  // Delay 20 milliseconds
  delay(20);
}
```

## Module scanner

API to scan for TotemBUS modules. Can be used to get list of connected TotemBUS modules and their information (number and serial).  

```arduino
void setup() {
  Serial.begin(115200);
}
void loop() {
  // Try to find any module on the TotemBUS
  if (TotemX4Module::find()) {
    // Print found module
    Serial.printf("Found. Number: %d, Serial: %d\n", TotemX4Module::foundNumber, TotemX4Module::foundSerial);
  }
  else {
    Serial.println("Not found");
  }
  delay(1000); // Wait 1s
}
```

***

#### `found` TotemX4Module::find(`number`, `serial`) { #TotemX4Module.find data-toc-label='find()' }
: Try to find module on TotemBUS network.  
Available request types:  
• all modules  
• modules matching specified number  
• module matching specified number and serial  
Result can be acquired with `TotemX4Module::foundNumber` and `TotemX4Module::foundSerial`.  
**Parameter:**  
`number` - module number [`0`:`255`]. `0` - all modules  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial  
**Returns:**
`found` - `true` if module has been found

#### (`number`) TotemX4Module::foundNumber { #TotemX4Module.foundNumber data-toc-label='foundNumber' }
: Get found module number.  

#### (`serial`) TotemX4Module::foundSerial { #TotemX4Module.foundSerial data-toc-label='foundSerial' }
: Get found module serial.  
