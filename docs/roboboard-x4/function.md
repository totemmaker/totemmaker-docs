# Auxiliary functions

## Description

API to receive custom commands from smartphone using Totem App. There are 4 channels that can be used independently to receive widgets state. Using this functionality, RoboBoard X4 can be programed to perform specific tasks.  

***

## Read App events

This API is available for each auxiliary channel `X4.functionA`, `X4.functionB`, `X4.functionC`, `X4.functionD`.  

#### X4.functionA.addEvent(`function`) { data-toc-label='addEvent()' }
: Register Totem App event function. It will be called on each message sent. Value is received using `get()` function.  
**Parameter:**  
`function` - function name [`appCallback`]  

#### (`value`) X4.functionA.get() { data-toc-label='get()' }
: Get widget value sent from App.  
**Returns:**  
`value` - 32-bit value [`-2147483648`:`2147483647`]  

## Example

Arduino examples: [RoboBoardX4/FUNCTION](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/FUNCTION){target=_blank}

```arduino
// Function called when button is pressed, that maps to functionA
void eventFunctionA() {
  int value = X4.functionA.get(); // Value sent to functionA by Totem App
}
// Function called when button is pressed, that maps to functionB
void eventFunctionB() {
  int value = X4.functionB.get(); // Value sent to functionB by Totem App
}
void setup() {
  // Register event handlers for functions
  X4.functionA.addEvent(eventFunctionA);
  X4.functionB.addEvent(eventFunctionB);
}
```