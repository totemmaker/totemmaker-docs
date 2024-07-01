# TotemModule

Module instance object, providing to send and receive data from module.

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
|  | [TotemModule(`number`)](#totemmodulenumber) | Create module with number |
|  | [TotemModule(`number`, `DataReceiver`)](#totemmodulenumber-datareceiver) | Create module with number and data receiver |
|  | [TotemModule(`number`, `serial`)](#totemmodulenumber-serial) | Create module with number and serial |
|  | [TotemModule(`number`, `serial`, `DataReceiver`)](#totemmodulenumber-serial-datareceiver) | Create module with number, serial and data receiver |
| `bool` | [write(`command`)](#writecommand) | Send command to module |
| `bool` | [write(`command`, `int`)](#writecommand-int) | Send command with integer value |
| `bool` | [write(`command`, `string`)](#writecommand-string) | Send command with string value |
| `bool` | [write(`command`, `array`, `length`)](#writecommand-array-length) | Send command with data |
| `bool` | [write(`command`, `int`, `int`, `int`, `int`)](#writecommand-int-int-int-int) | Send command with multiple values |
| `bool` | [read(`command`)](#readcommand) | Non-blocking read from module |
| [ModuleData](ModuleData.md) | [readWait(`command`)](#readwaitcommand) | Blocking inline read from module |
| `bool` | [readWait(`command`, `ModuleData`)](#readwaitcommand-moduledata) | Blocking read from module |
| `bool` | [subscribe(`command`, `interval`)](#subscribecommand-interval) | Subscribe command read |
| `bool` | [unsubscribe(`command`)](#unsubscribecommand) | Unsubscribe command read |
| `bool` | [attachOnData(`DataReceiver`)](#attachondatadatareceiver) | Register data receive function |
| _none_ | [setNumber(`number`)](#setnumbernumber) | Change number of TotemModule object |
| _none_ | [setSerial(`serial`)](#setserialserial) | Change serial of TotemModule object |
| `int`  | [getNumber()](#getnumber) | Read number of TotemModule object |
| `int`  | [getSerial()](#getserial) | Read serial of TotemModule object |
| _none_ | [ping()](#ping) | Send ping request to module |
| `int`  | [hashCmd(`command`)](#hashcmdcommand) | Encode command to integer |
| `int`  | [hashModel(`model`)](#hashmodelmodel) | Encode robot model (type) to integer |

## API

### Constructor

`TotemModule` object allows to exchange commands with attached module. A number of module and serial (optional) should be passed to specify which module on the network should respond to it. Module number is printed in a white rectangle. Serial number has to be acquired programmatically.  
If number `0` is passed - object will communicate with all modules connected to TotemBUS.  

#### TotemModule(`number`)

: Create object by specifying module number.  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  

```arduino
TotemModule module(04); // Create object for module 04
```

#### TotemModule(`number`, `DataReceiver`)

: Create object by specifying number and data receiver function.  
`DataReceiver` is a user defined function to get all incoming data from the module.  
Format: `#!arduino void onModuleData(ModuleData data)`. Received data is stored in [`ModuleData`](ModuleData.md) parameter. Also can be registered with function [attachOnData()](#attachondatadatareceiver).  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  
`DataReceiver` - data receiver function name `onModuleData`.  

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

#### TotemModule(`number`, `serial`)

: Create object by specifying number and serial.  
Serial is required when there is two or more modules with the same number.  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial.  

```arduino
// Create object for module 04 with serial number 25485.
// This will send commands only to module with matching serial.
TotemModule module(04, 25485);
```

#### TotemModule(`number`, `serial`, `DataReceiver`)

: Create object by specifying number, serial and data receiver function.  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial.  
`DataReceiver` - data receiver function name `onModuleData`.  

```arduino
void onModuleData(ModuleData data) {
  // Received command result from module 04 with serial number 25485
}
// Create object for module 04 with serial 25485 and register data receiver
TotemModule module(04, 25485, onModuleData);
```

***

### Functions

Calling `writeWait(...)` will block code execution until response from module received or timeouts. Will return `true` only if module successfully executed write command request. Used when confirmation of value update is required.  
Calling simple `write(...)` only sends command to network without acknowledgment.  
For more information read [Wait functions](#wait-functions).

#### write(`command`) { data-toc-label='write(command)' }

: Call command.  
_Parameter:_  
`command` - module command string or hash.  
_Returns:_  command is sent [`true`:`false`].

```arduino
module.write("indicate"); // Blink LED on module
```

#### write(`command`, `int`) { data-toc-label='write(command. int)' }

: Write integer to command.  
_Parameter:_  
`command` - module command string or hash.  
`int` - 32bit value [`-2147483648`:`2147483647`] or [`0`:`4294967295`].  
_Returns:_  command is sent [`true`:`false`].

```arduino
module.write("motorA", 50); // Set motor channel A power to 50%
```

#### write(`command`, `string`) { data-toc-label='write(command, string)' }

: Write string to command.  
_Parameter:_  
`command` - module command string or hash.  
`string` - any string array.  
_Returns:_  command is sent [`true`:`false`].

```arduino
module.write("cfg/robot/name", "My robot"); // Set robot name to "My robot"
```

#### write(`command`, `array`, `length`) { data-toc-label='write(command, array, length)' }

: Write fixed length array to command.  
_Parameter:_  
`command` - module command string or hash.  
`array` - 8bit data array.  
`length` - size of array.  
_Returns:_  command is sent [`true`:`false`].

```arduino
char data[] = {'N', 'a', 'm', 'e'};
module.write("cfg/robot/name", data, sizeof(data)); // Set robot name from data array
```

#### write(`command`, `int`, `int`, `int`, `int`) { data-toc-label='write(command, int, int, int, int)' }

: Write four 8bit values to command. These values are merged into one 32bit value and forwarded to [write(command, int)](#writecommand-int). Also accepts 3 int parameters.  
_Parameter:_  
`command` - module command string or hash.  
`int` - 8bit value 1.  
`int` - 8bit value 2.  
`int` - 8bit value 3.  
`int` - 8bit value 4.  
_Returns:_  command is sent [`true`:`false`].

```arduino
module.write("motorABCD", 50, 0, -60, 20); // Write all 4 motor channels at once
module.write("servoABC", -100, 0, 100); // Write all 3 servo motor channels at once
module.write("rgbA", 255, 0, 255, 0); // Set rgbA led to GREEN color with max brightness
// module.write("rgbA", 0xFF00FF00); // Equivalent to previous command
```

#### read(`command`) { data-toc-label='read(command)' }

: Read command from module. This function sends a read request. It will not block program execution and result of the read will be returned in the function attached with [attachOnData()](#attachondatadatareceiver).  
 _Parameter:_  
`command` - module command string or hash.  
_Returns:_  command is sent [`true`:`false`].

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

#### readWait(`command`) { data-toc-label='readWait(command)' }

: Read command from module. This function sends a read request and waits for the result. It will block program execution until result is received or timeouts (100 milliseconds).  
This function returns [`ModuleData`](ModuleData.md) to make inline read request.  
If read is failed, values will be set to NULL and 0.  
 _Parameter:_  
`command` - module command string or hash.  
_Returns:_  [`ModuleData`](ModuleData.md).

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

#### readWait(`command`, [`ModuleData`](ModuleData.md)) { data-toc-label='readWait(command, data)' }

: Read command from module. This function sends a read request and waits for the result. It will block program execution until result is received or timeouts (100 milliseconds).  
If returned true - read operation was successful and [`ModuleData`](ModuleData.md) contains received data.  
 _Parameter:_  
`command` - module command string or hash.  
[`ModuleData`](ModuleData.md) - received data.  
_Returns:_  is requested data received [`true`:`false`].  

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

#### subscribe(`command`, `interval`) { data-toc-label='subscribe(command, interval)' }

: Subscribe to periodical data receive. Result will be returned in `DataReceiver` function registered with [attachOnData()](#attachondatadatareceiver).  
If no interval provided - event will be sent only when value is changed (eg. button press).
_Parameter:_  
`command` - module command string or hash.  
`interval` - interval in milliseconds. `0` - value change event.  
_Returns:_  request is sent [`true`:`false`].

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

#### unsubscribe(`command`) { data-toc-label='unsubscribe(command)' }

: Unsubscribe from receiving command data.  
_Parameter:_  
`command` - module command string or hash.  
_Returns:_  request is sent [`true`:`false`].

```arduino
module.subscribe("battery"); // Request to receive "battery" on value change
module.unsubscribe("battery"); // Stop receiving "battery" on value change
```

#### attachOnData(`DataReceiver`)

: Register module data receiver function.  
`DataReceiver` is a user defined function to get all incoming data from the module.  
Format: `#!arduino void onModuleData(ModuleData data)`. Received data is stored in [`ModuleData`](ModuleData.md) parameter.  
_Note:_ A command has to be [subscribed](#subscribecommand-interval) or [read](#readcommand) to appear in receiver function.  
_Parameter:_  
`DataReceiver` - data receiver function name `onModuleData`.  

```arduino
void onModuleData(ModuleData data) {
  // Receiving all data from 'module'
  if (data.is("command")) { /* received "command" */ }
}
void setup() {
  module.attachOnData(onModuleData); // Register data receiver for module
}
```

#### setNumber(`number`)

#### setSerial(`serial`)

: Change number or serial of defined module.  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial.  

```arduino
TotemModule module(0); // All modules
module.setNumber(04); // Reconfigure to 04 modules only.
module.setSerial(5468); // Reconfigure to module with 5468 serial only.
```

#### getNumber() { data-toc-label='getNumber()' }

#### getSerial() { data-toc-label='getSerial()' }

: Get number or serial of defined module.  
_Returns:_  
`number` - module number [`0`:`255`].  
`serial` - module serial [`0`:`32767`].  

```arduino
TotemModule module(11, 2657); // Distance sensor
int number = module.getNumber(); // Get module number "11"
int serial = module.getSerial(); // Get module serial "2657"
```

#### ping()

: Sends a request for module to respond. If TotemModule object is initialized with number `0`, all modules will receive ping request.  
This can be used to discover modules connected to TotemBUS system or check if they are still connected.  
_Returns:_  request is sent [`true`:`false`].

```arduino
TotemModule module(0); // All modules
module.ping(); // Request module to respond
```

#### hashCmd(`command`) { data-toc-label='hashCmd(command)' }

: Encode string command to 32bit integer value.  
This is done internally when calling any write() function, providing a string command.  
_Note:_ This command is `static`, so requires to be called trough a class name.  
_Parameter:_  
`command` - module command string.  
_Returns:_ unsigned 32bit hash [`0`:`0xFFFFFFFF`]  

```arduino
// Convert command "indicate" to hash
int commandHash = TotemModule::hashCmd("indicate");
// Call write command with encoded hash
module.write(commandHash);
```

#### hashModel(`model`) { data-toc-label='hashModel(model)' }

: Encode Totem robot model name to 16bit integer value.  
Used with [`cfg/robot/model`](../../modules/roboboard-x4.md#cfgrobotmodel-model) and [getModel()](TotemRobot.md#getmodel).  
_Note:_ This command is `static`, so requires to be called trough a class name.  
_Parameter:_  
`command` - model name string.  
_Returns:_ unsigned 16bit hash [`0`:`0xFFFF`]  

```arduino
// Convert model name "MiniTrooper" to hash
int modelHash = TotemModule::hashModel("MiniTrooper");
// Set robot model to "MiniTrooper"
module.write("cfg/robot/model", modelHash);
```

## Wait functions

Simple [write](#writecommand) functions sends data without any acknowledgement and only returns false if command is failed to send. The reasons of failure could be:  

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

Same rules applies to [subscribe](#subscribecommand-interval) and [unsubscribe](#unsubscribecommand) functions.  
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
