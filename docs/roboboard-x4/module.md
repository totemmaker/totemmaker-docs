# Module scanner

## Description

API to scan for TotemBUS modules. Can be used to get list of connected TotemBUS modules and their information (number and serial).  

***

## Code examples

**Arduino projects:** [RoboBoardX4/TotemBUS](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemBUS/examples/ListConnectedModules/ListConnectedModules.ino){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    // Function called when ping response from module is received
    void moduleDiscovered() {
      int number = X4.module.getLastNumber();
      int serial = X4.module.getLastSerial();
    }
    void setup() {
      // Register module ping response event function
      X4.module.addEvent(moduleDiscovered);
    }
    void loop() {
      X4.module.ping(); // Ping all modules
      delay(1000);
      X4.module.ping(11); // Ping only distance module [11]
      delay(1000);
      X4.module.ping(11, 12654); // Ping only distance module [11] with serial 12654
      delay(1000);
    }
    ```

***

## Functions

### Discover modules

#### X4.module.ping(`number`, `serial`) { data-toc-label='ping()' }
: Request modules to send a response. All connected modules will be listed in function registered with `addEvent()`.  
Available request types:  
• all modules  
• modules matching specified number  
• module matching specified number and serial  
**Alternatives:**  
`ping(number)` - request only by module number  
`ping()` - request all modules  
**Parameter:**  
`number` - module number [`0`:`255`]. `0` - all modules  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial  

#### X4.module.addEvent(`function`) { data-toc-label='addEvent()' }
: Register module discovery event function. It will be called on each module ping response. Functions `getLastNumber()` and `getLastSerial()` can be used inside registered function to get module information.  
**Parameter:**  
`function` - function name [`moduleCallback`]  

#### (`number`) X4.module.getLastNumber() { data-toc-label='getLastNumber()' }
: Get last received module number (from ping response).  

#### (`serial`) X4.module.getLastSerial() { data-toc-label='getLastSerial()' }
: Get last received module serial (from ping response).  
