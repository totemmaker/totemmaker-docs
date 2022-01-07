# Module programming

A general tutorial common for all modules. For specific `features` and `functions` read module documentation.  

## Accessing specific module

Module API can be accessed by declaring it's class. It will work right away, without a need for initialization. Multiple instances can be created.  
If module is not physically connected, all functions calls will be ignored.  

```arduino
Module11 module; // Declare use of distance sensor [11]
Module11 module2(25151); // Declare use of distance sensor [11] with serial 25151
```
`ModuleXX` - each module hash class implementation name of `Module` + _number_.  
`module` - a variable name of `Module11`. Can be any (sensor, controller, lineReader, ...). This instance is used to call module functions.  
`(25151)` - if there are multiple modules with same number - serial is used to specify exact one. Using instance `module2` will send commands to specific module only. Otherwise - all connected [11] modules will receive a command. Check [X4.module](/roboboard-x4/module/) on how to get serial.  

## General functions

Module functionality is objective oriented. Format `module.<feature>.<function>()` is used to group all it's features and call functions.  
`module` - instance of specific module.  
`<feature>` - features that are supported by module (distance, rgb, ...).  
`<function>` - functionality that is available for each feature (get, set, color, ...).  

```arduino title="Communicating with module"
// Read distance from sensor
int distance = module.distance.get();
// Set RGB color to green
module.rgb.color(0, 255, 0);
```

## Events

```arduino title="Using event functionality"
// Distance module instance
Module11 sensor;
// Event function of distance feature
void distanceEvent() {
  // Check if it's a "distance" feature event
  if (sensor.distance.isEvent()) {
      // Read cached distance data from event (non-blocking)
      int distance = sensor.distance.getCm(); // Takes no time to execute
  }
}
void setup() {
  // Register event function for Distance module
  sensor.addEvent(distanceEvent);
  // Enable events for distance parameter
  sensor.distance.event(); // When value is changed
}
void loop() {
  // Read distance by sending request and waiting for response (blocking)
  int distance = sensor.distance.getCm(); // Takes ~1ms to execute
  // Wait 1 seconds
  delay(1000);
}
```

Reading with functions like `#!arduino module.distance.get()` works by blocking code until response from module is received (same as I2C). For non-blocking read or subscribe functionality - event system is used. _Blocking function calls are executed very fast (around ~1ms. As do I2C). So it's fine to use them most of the times. It's much easier than managing events._  

Event system works by registering specific module parameter and it will be sent when value is changed or in time intervals.  


#### module.<feature\>.addEvent(`function`) { #feature.addEvent data-toc-label='feature.addEvent()' }
: Register event function for specified feature. Will be called when new data is available.  
Event data can be received simply by using functions from registered `<feature>`. Data is cached and won't initiate read request, but must be called inside registered function.  
**Parameter:**  
`function` - event function name (`distanceEvent`).  

#### (`state`) module.<feature\>.isEvent() { #feature.isEvent data-toc-label='feature.isEvent()' }
: Check if event is received. Used inside event function. Useful if same function is registered for multiple features.  
**Returns:**  
`state` - is event received [`true`:`false`]  

#### module.<feature\>.event() { #feature.event data-toc-label='feature.event()' }
: Start event for specified feature. Will be triggered when value is changed.  

#### module.<feature\>.event(`interval`) { #feature.event-interval data-toc-label='feature.event(interval)' }
: Start events for specified feature. Will be triggered at `inverval` rate (milliseconds).  
Example: setting `event(100)` will call registered event function every 100ms.  
**Parameter:**  
`interval` - event update time (milliseconds). `0` - disable event.  

#### module.<feature\>.eventOnce() { #feature.eventOnce data-toc-label='feature.eventOnce()' }
: Call event handler only once. Won't start broadcasting.  
