# TotemModule

Module instance object, providing to send and receive data from module.

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
|  | [TotemModule(`number`)](#totemmodulenumber) | Create module with number |
|  | [TotemModule(`number`, `DataReceiver`)](#totemmodulenumber-datareceiver) | Create module with number and data receiver |
|  | [TotemModule(`number`, `serial`)](#totemmodulenumber-serial) | Create module with number and serial |
|  | [TotemModule(`number`, `serial`, `DataReceiver`)](#totemmodulenumber-serial-datareceiver) | Create module with number, serial and data receiver |
| `bool` | [write(`command`)](#bool-writecommand) | Send command to module |
| `bool` | [write(`command`, `int`)](#bool-writecommand-int) | Send command with integer value |
| `bool` | [write(`command`, `string`)](#bool-writecommand-string) | Send command with string value |
| `bool` | [write(`command`, `array`, `length`)](#bool-writecommand-array-length) | Send command with data |
| `bool` | [write(`command`, `int`, `int`, `int`, `int`)](#bool-writecommand-int-int-int-int) | Send command with multiple values |
| `bool` | [read(`command`)](#bool-readcommand) | Non-blocking read from module |
| [ModuleData](/API/ModuleData) | [readWait(`command`)](#moduledata-readwaitcommand) | Blocking inline read from module |
| `bool` | [readWait(`command`, `ModuleData`)](#bool-readwaitcommand-moduledata) | Blocking read from module |
| `bool` | [subscribe(`command`, `interval`)](#bool-subscribecommand-interval) | Subscribe command read |
| `bool` | [unsubscribe(`command`)](#bool-unsubscribecommand) | Unsubscribe command read |
| `bool` | [attachOnData(`DataReceiver`)](#attachondatadatareceiver) | Register data receive function |
| _none_ | [setNumber(`number`)](#setnumbernumber) | Change number of TotemModule object |
| _none_ | [setSerial(`serial`)](#setserialserial) | Change serial of TotemModule object |
| `int`  | [hashCmd(`command`)](#int-hashcmdstring) | Encode command to integer |
| `int`  | [hashModel(`model`)](#int-hashmodelstring) | Encode robot model (type) to integer |

## Example

```arduino
// Define use of X4 module
// This can be any module connected to Totem X4 board
TotemModule module(04);
// Arduino setup
void setup() {
  Serial.begin(115200);
  // Initialize X4 module
  Totem.X4.begin();
}
// Arduino loop
void loop() {
  // Blink on-board led
  module.write("indicate");
  delay(500); // Wait 500ms
  // Read robot (module) name
  Serial.println(module.readWait("cfg/robot/name").getString());
  delay(500); // Wait 500ms
}
```

## Description

> ### TotemModule(`number`)

**Description:** Create module object by number. It's printed on the module in a white rectangle. All modules with the same functionality has same identification number.  
Setting to 0 will communicate with all modules available on the TotemBUS.  
**Parameter:** Module identification number.  

```arduino
TotemModule module(04); // Create object for module 04
```

> ### TotemModule(`number`, `DataReceiver`)

**Description:** Create module object by number. It's printed on the module in a white rectangle. All modules with the same functionality has same identification number.  
Setting to 0 will communicate with all modules available on the TotemBUS.  
Specified `DataReceiver` function is used to get all incoming data from the module. It's a user defined function with any name, but must have [`ModuleData`](/API/ModuleData) parameter. Same as function [attachOnData()](#attachondatadatareceiver).  
**Parameter:** Module identification number and `DataReceiver` function.  

```arduino
// Function (DataReceiver) to receive data events from registered module
void onModuleData(ModuleData data) {
  // Received data from module 04
}
// Create object for module 04 and register data receiver
TotemModule module(04, onModuleData);
// Same functionality (adding data receiver) can be achieved with:
void setup() {
  module.attachOnData(onModuleData);
}
```

> ### TotemModule(`number`, `serial`)

**Description:** Create module object by number. It's printed on the module in a white rectangle. All modules with the same functionality has same identification number.  
Each module has a unique (serial) identifier and it is required to control multiple modules with the same number. This identifier isn't printed anywhere and has to be acquired programmatically.  
**Parameter:** Module identification number and serial number.  

```arduino
// Create object for module 04 with serial number 25485.
// This will send commands only to module with matching serial.
TotemModule module(04, 25485);
```

> ### TotemModule(`number`, `serial`, `DataReceiver`)

**Description:** Create module object by number. It's printed on the module in a white rectangle. All modules with the same functionality has same identification number.  
Each module has a unique (serial) identifier and it is required to control multiple modules with the same number. This identifier isn't printed anywhere and has to be acquired programmatically.  
Specified `DataReceiver` function is used to get all incoming data from the module. It's a user defined function with any name, but must have [`ModuleData`](/API/ModuleData) parameter. Same as function [attachOnData()](#attachondatadatareceiver).  
**Parameter:** Module identification number, serial and `DataReceiver` function.  

```arduino
void onModuleData(ModuleData data) {
  // Received command result from module 04 with serial number 25485
}
// Create object for module 04 with serial 25485 and register data receiver
TotemModule module(04, 25485, onModuleData);
```

> ### `bool` write(`command`) { data-toc-label='write(command)' }

**Description:** Call module command.  
**Parameter:** Command string.  

```arduino
module.write("indicate"); // Blink LED on module
```

> ### `bool` write(`command`, `int`) { data-toc-label='write(command. int)' }

**Description:** Write module command with integer value.  
**Parameter:** Command string and up to 32bit value.  

```arduino
module.write("motorA", 50); // Set motor channel A power to 50%
```

> ### `bool` write(`command`, `string`) { data-toc-label='write(command, string)' }

**Description:** Write module command with string value.  
**Parameter:** Command string and string value.  

```arduino
module.write("cfg/robot/name", "My robot"); // Set robot name to "My robot"
```

> ### `bool` write(`command`, `array`, `length`) { data-toc-label='write(command, array, length)' }

**Description:** Write module command with fixed length array data.  
**Parameter:** Command string and array data with fixed length.  

```arduino
char data[] = {'N', 'a', 'm', 'e'};
module.write("cfg/robot/name", data, sizeof(data)); // Set robot name from data array
```

> ### `bool` write(`command`, `int`, `int`, `int`, `int`) { data-toc-label='write(command, int, int, int, int)' }

**Description:** Write module command with four 8bit values. These values internally are merged into one 32bit value and forwarded to [single value write](#writecommand-int). Also accepts 3 int parameters.  
Used only for certain commands.  
**Parameter:** Command string and four 8bit values.  

```arduino
module.write("motorABCD", 50, 0, -60, 20); // Write all 4 motor channels at once
module.write("servoABC", -100, 0, 100); // Write all 3 servo motor channels at once
module.write("rgbA", 255, 0, 255, 0); // Set rgbA led to GREEN color with max brightness
// module.write("rgbA", 0xFF00FF00); // Equivalent to previous command
```

> ### `bool` read(`command`) { data-toc-label='read(command)' }

**Description:** Read command from module. This function sends a read request. It will not block program execution and result of the read will be returned in the function attached with [attachOnData()](#attachondatadatareceiver).  
If false is returned - DataReceiver function is not attached or failed to send read request.  
 **Parameter:** Command string.  

```arduino
void onModuleData(ModuleData data) {
  // Check if received data is from 'cfg/robot/name' command
  if (data.is("cfg/robot/name")) {
    Serial.println(data.getString()); // Print name
  }
}
void setup() {
  // Register onModuleData as data receive function
  module.attachOnData(onModuleData);
  // Send read request for 'cfg/robot/name'
  module.read("cfg/robot/name");
}
```

> ### [`ModuleData`](/API/ModuleData) readWait(`command`) { data-toc-label='readWait(command)' }

**Description:** Read command from module. This function sends a read request and waits for the result. It will block program execution until result is received or timeouts (100 milliseconds).  
This function returns [`ModuleData`](/API/ModuleData) to make inline read request. If read is failed, values will be set to NULL and 0.  
 **Parameter:** Command string and [`ModuleData`](/API/ModuleData) as return value.  

```arduino
// Print robot name
const char *name = module.readWait("cfg/robot/name").getString();
Serial.print("Robot cfg/robot/name: ");
Serial.println(name);
// Get battery voltage
int voltage = module.readWait("battery").getInt();
Serial.print("Robot battery: ");
Serial.println(voltage);
```

> ### `bool` readWait(`command`, [`ModuleData`](/API/ModuleData)) { data-toc-label='readWait(command, data)' }

**Description:** Read command from module. This function sends a read request and waits for the result. It will block program execution until result is received or timeouts (100 milliseconds).  
If returned true - read operation was successful and [`ModuleData`](/API/ModuleData) contains received data.  
 **Parameter:** Command string and [`ModuleData`](/API/ModuleData) as receiver.  

```arduino
// Prepare ModuleData variable
ModuleData data;
// Start read robot name. Will block until result is returned
if (module.readWait("cfg/robot/name", data)) {
  // Print received name
  Serial.print("Received cfg/robot/name: ");
  Serial.println(data.getString());
}
else { // Reading failed. No result returned
  Serial.println("Read cfg/robot/name failed");
}
```

> ### `bool` subscribe(`command`, `interval`) { data-toc-label='subscribe(command, interval)' }

**Description:** Subscribe to periodically receive a certain command data. Result will be returned in DataReceiver function registered with [attachOnData()](#attachondatadatareceiver).  
If no interval parameter is provided - will subscribe to value change (eg. button press).
**Parameter:** Command string and time interval in milliseconds.  

```arduino
void onModuleData(ModuleData data) {
  // Receiving "battery" value
  if (data.is("battery")) {
    Serial.print("Battery level: ");
    Serial.println(data.getInt());
  }
}
void setup() {
  // Register onModuleData as data receive function
  module.attachOnData(onModuleData);
  // Request to receive "battery" on value change
  module.subscribe("battery");
  // Request to receive "battery" command every 1 second
  module.subscribe("battery", 1000);
}
```

> ### `bool` unsubscribe(`command`) { data-toc-label='unsubscribe(command)' }

**Description:** Unsubscribe from receiving command data.  
**Parameter:** Command string

```arduino
module.subscribe("battery"); // Request to receive "battery" on value change
module.unsubscribe("battery"); // Stop receiving "battery" on value change
```

> ### attachOnData(`DataReceiver`)

**Description:** Register module data receiver function. Specified `DataReceiver` function is used to get all incoming data from the module. It's a user defined function with any name, but must have [`ModuleData`](/API/ModuleData) parameter.  
_Note: A command has to be [subscribed](#bool-subscribecommand-interval) or [read](#bool-readcommand) to appear in receiver function._  
**Parameter:** Data receiver function.  

```arduino
void onModuleData(ModuleData data) {
  // Receiving all data from 'module'
  if (data.is("command")) { /* received "command" */ }
}
void setup() {
  module.attachOnData(onModuleData); // Register data receiver for module
}
```

> ### setNumber(`number`)

> ### setSerial(`serial`)

**Description:** Change number or serial of defined module.  
**Parameter:** Valid module number or serial.  

```arduino
TotemModule module(0); // All modules
module.setNumber(04); // Reconfigure to 04 modules only.
module.setSerial(5468); // Reconfigure to module with 5468 serial only.
```

> ### `int` hashCmd(`command`) { data-toc-label='hashCmd(command)' }

**Description:** Encode string command to 32bit integer value.  
Allows to encode string commands manually.  
This is done internally when calling any write() function, providing a string command.  
_This command is `static`, so requires to be called trough class name._  
**Parameter:** Command string.  

```arduino
// Convert command "indicate" to hash
int commandHash = TotemModule::hashCmd("indicate");
// Call write command with encoded hash
module.write(commandHash);
```

> ### `int` hashModel(`model`) { data-toc-label='hashModel(model)' }

**Description:** Encode Totem robot model name to 16bit integer value.  
Allows to encode model names manually.  
Used with [`cfg/robot/model`](/modules/04/#cfgrobotmodel-int) and [getModel()](/API/TotemRobot/#int-getmodel).  
_This command is `static`, so requires to be called trough class name._  
**Parameter:** Model name string.  

```arduino
// Convert model name "MiniTrooper" to hash
int modelHash = TotemModule::hashModel("MiniTrooper");
// Set robot model to "MiniTrooper"
module.write("cfg/robot/model", modelHash);
```

## Wait functions

Simple [write](#bool-writecommand) functions sends data without any acknowledgement and only returns false if command is failed to send. The reasons of failure could be:  

* `module` object is not connected to any interface to send command over.
* Module number is larger than 255, or serial is larger than 32767.  
* Internal send queue is full (too much messages are being sent).

This allows to reach high communication speed, but doesn't guarantee that module received a command. To ensure that data was processed by receiving module, use functions with "Wait" suffix. These functions will send a command and block until response is received, indicating successful or failed transmission. Possible reasons of failure:  

* `module` object is not connected to any interface to send command over.
* Module number is larger than 255, or serial is larger than 32767.  
* Internal send queue is full (too much messages are being sent).
* Requested module is not available on TotemBUS or unresponsive.
* "command" is not available for requested module.
* Sending invalid data to "command" (e.g. sending int when command expects a string).
* "command" failed to perform a task it suppose to do.

Same rules applies to [subscribe](#bool-subscribecommand-interval) and [unsubscribe](#bool-unsubscribecommand) functions.  
Usage examples:

```arduino
TotemModule module04(04); // Define existing 04 module
TotemModule module03(03); // Define existing 03 module, but not connected
TotemModule module333(333); // Define non-existing (invalid) 333 module

void function() {
// Sending "indicate" to existing 04 module
module04.write("indicate"); // returns true -> module blinks a led
// Sending "indicate" to not connected 03 module
module03.write("indicate"); // returns true -> not expecting for response
// Sending "indicate" to non-existing 333 module
module333.write("indicate"); // returns false -> invalid number

// Sending invalid "indicate" to existing 04 module
module04.write("inDICAte"); // returns true -> module receives a command,
// but does not blink a led, because command spelling is incorrect.
// Sending invalid "indicate" to not connected 03 module
module03.write("inDICAte"); // returns true -> not expecting for response
// Sending invalid "indicate" to non-existing 333 module
module333.write("inDICAte"); // returns false -> invalid number

// Sending "indicate" to existing 04 module
module04.writeWait("indicate"); // returns true -> response received, led blink
// Sending "indicate" to not connected 03 module
module03.writeWait("indicate"); // returns false -> timeout,
// none of the 03 modules responded. Was blocked for 100ms
// Sending "indicate" to non-existing 333 module
module333.writeWait("indicate"); // returns false -> invalid number
// Setting name for existing 04 module
module04.writeWait("cfg/robot/name", 123); // returns false -> failure response,
// received integer while string data expected
}
```
