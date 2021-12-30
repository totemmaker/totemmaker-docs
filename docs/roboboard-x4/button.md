# Programmable button

## Description

API to read integrated button state. By default it has no functionality and can be used for any custom scenario.  
NOTE: Reset button is dedicated to reload ESP32 and can't be reused.  

***

## Code examples

**Arduino projects:** [RoboBoardX4/BUTTON](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/BUTTON){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    // Is button currently pressed "false"
    bool isIn = X4.button.isPressed();
    // Is button currently released "false"
    bool isOut = X4.button.isReleased();
    // Is button pressed and holding for 1000 milliseconds "false"
    bool isInFor = X4.button.isPressedFor(1000);
    // Is button released for 1000 milliseconds "false"
    bool isOutFor = X4.button.isReleasedFor(1000);
    // Was button pressed before "false"
    bool wasIn = X4.button.wasPressed();
    // Was button released before "false"
    bool wasOut = X4.button.wasReleased();
    // Was button pressed and holding for 500 milliseconds "false"
    bool wasInFor = X4.button.wasPressedFor(500);
    // Was button released for 500 milliseconds "false"
    bool wasOutFor = X4.button.wasReleasedFor(500);
    // Was button double clicked "false"
    bool wasDouble = X4.button.wasDoubleClick();
    // Last timestamp when button changed it's state "1564661" (millis())
    int lasTime = X4.button.lastChange();
    ```

    ```arduino
    // Function called when button pressed or released
    void buttonTriggered() {
      if (X4.button.isPressed()) {
        // Button was pressed
      }
    }
    void setup() {
      // Register button state change event 
      X4.button.addEvent(buttonTriggered);
    }
    ```

***

## Functions

### Button read

#### (`status`) X4.button.isPressed() { data-toc-label='isPressed()' }
: Check if button is pressed.  
**Returns:**  
`status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.isReleased() { data-toc-label='isReleased()' }
: Check if button is released.  
**Returns:**  
`status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.isPressedFor(`time`) { data-toc-label='isPressedFor()' }
: Check if button is pressed in for a certain amount of `time`.  
**Parameter:** `time` - amount of milliseconds button is pressed [`0`:`65535`]ms  
**Returns:** `status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.isReleasedFor(`time`) { data-toc-label='isReleasedFor()' }
: Check if button is released in for a certain amount of `time`.  
**Parameter:** `time` - amount of milliseconds button is released [`0`:`65535`]ms  
**Returns:** `status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.wasPressed() { data-toc-label='wasPressed()' }
: Check if button was pressed earlier. Will reset if `true` is returned, until next press.  
**Returns:**  
`status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.wasReleased() { data-toc-label='wasReleased()' }
: Check if button was released earlier. Will reset if `true` is returned, until next release.  
**Returns:**  
`status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.wasPressedFor(`time`) { data-toc-label='wasPressedFor()' }
: Check if button was held for a certain amount of `time`.  
**Parameter:** `time` - amount of milliseconds button was held [`0`:`65535`]ms  
**Returns:** `status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.wasReleasedFor(`time`) { data-toc-label='wasReleasedFor()' }
: Check if button was released for a certain amount of `time`.  
**Parameter:** `time` - amount of milliseconds button was released [`0`:`65535`]ms  
**Returns:** `status` - yes / no [`true`:`false`]  

#### (`status`) X4.button.wasDoubleClick() { data-toc-label='wasDoubleClick()' }
: Check if button was was double clicked earlier. Will reset if `true` was returned, until next double click.  
**Returns:**  
`status` - yes / no [`true`:`false`]  

#### (`timestamp`) X4.button.lastChange() { data-toc-label='lastChange()' }
: Get time of last button state change.  
**Returns:**  
`timestamp` - last button state change (millis()) [`0`:`4294967295`]ms  

#### X4.button.addEvent(`function`) { data-toc-label='addEvent()' }
: Register an event function called on button state change (press, release).  
**Parameter:**  
`function` - function name [`buttonCallback`]  
