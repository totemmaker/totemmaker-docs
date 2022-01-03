# ModuleData

Object containing data received from module.

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
| `bool` | [is(`command`)](#iscommand) | Check if data came of specified command |
| `bool` | [isInt()](#isint) | Check if there is valid integer data |
| `bool` | [isString()](#isstring) | Check if there is valid string data |
| `int`  | [getInt()](#getint) | Read integer from data |
|`string`| [getString()](#getstring) | Read string from data |
| `bool` | [getData(`array`)](#getdataarray) | Read array pointer of data |
| `bool` | [getData(`array`, `length`)](#getdataarray-length) | Read array pointer and data length |
| `int`  | [getCmdHash()](#getcmdhash) | Get hash of received command string |

## Example

```arduino
#include <Totem.h>
// Declare that we are willing to communicate with RoboBoard X4
TotemModule module(04);
// Receiver function for result of read() function
void onModuleData(ModuleData data) {
  if (data.is("cfg/robot/name")) { // cfg/robot/name received
    Serial.print("'cfg/robot/name' is ");
    Serial.println(data.getString()); // Prints: 'cfg/robot/name' is Totem Robot
  }
  else if (data.is("version")) { // version received
    Serial.print("'version' is");
    Serial.println(data.getInt()); // Prints: 'version' is 100
  }
}
// Arduino initialization function
void setup() {
  // put your setup code here, to run once:
  Totem.X4.begin(); // Initialize RoboBoard X4
  // Register module read() data receiver function
  module.attachOnData(onModuleData);
  // Read cfg/robot/name from module X4. Result will be passed to onModuleData
  module.read("cfg/robot/name");
  // Read firmware version from module X4. Result will be passed to onModuleData
  module.read("version");
}
// Arduino loop function
void loop() {
  // put your main code here, to run repeatedly:
}
```

## API

### Functions

#### is(`command`)

: Check if received data is from specified `command`.  
_Returns:_ [`true`:`false`]  

```arduino
void onModuleData(ModuleData data) {
  if (data.is("cfg/robot/name"))
    Serial.println("Data came from 'cfg/robot/name' command");
  if (data.is("version"))
    Serial.println("Data came from 'version' command");
}
```

#### isInt()

: Check if data can be read with [`getInt()`](#getint).  
_Returns:_ [`true`:`false`]  

```arduino
ModuleData data;
module.readWait("cfg/robot/name", data);
data.isInt(); // returns false. "cfg/robot/name" data contains string value
module.readWait("version", data);
data.isInt(); // returns true. "version" data contains integer value
```

#### isString()

: Check if data can be read with [`getString()`](#getstring).  
_Returns:_ [`true`:`false`]  

```arduino
ModuleData data;
module.readWait("cfg/robot/name", data);
data.isString(); // returns true. "cfg/robot/name" data contains string value
module.readWait("version", data);
data.isString(); // returns false. "version" data contains integer value
```

#### getInt()

: Read integer value from received data.  
_Returns:_ signed 32bit `int` [`-2147483648`:`2147483647`]  

```arduino
ModuleData data;
// Read firmware version. Read result will be stored to "data".
if (module.readWait("version", data)) {
  Serial.print("Value of 'version': ");
  Serial.println(data.getInt()); // Prints: Value of 'version': 100
}
```

#### getString()

: Read string value from received data.  
_Returns:_ string array.  

```arduino
ModuleData data;
// Read robot name. Read result will be stored to "data".
if (module.readWait("cfg/robot/name", data)) {
  Serial.print("'cfg/robot/name' is ");
  Serial.println(data.getString()); // Prints: 'cfg/robot/name' is Totem X4
}
```

#### getData(`array`)

: Get pointer to data array.  
Value is stored in passed parameter `array`.  
_In case of printing received data, TotemBUS protocol always adds NULL terminator at the end of the data, so it's safe to use print and string manipulation functions._  
_Returns:_ data stored in `array` [`true`:`false`]  

```arduino
ModuleData data;
if (module.readWait("cfg/robot/name", data)) { // Read robot name
  char *name;
  data.getData(name); // Information is pointed to name variable
  Serial.print("'cfg/robot/name' is ");
  Serial.println(name); // Prints: 'cfg/robot/name' is Totem X4
}
```

#### getData(`array`, `length`)

: Get pointer to data array and it's length.  
Value is stored in passed parameters `array`, `length`.  
_In case of printing received data, TotemBUS protocol always adds NULL terminator at the end of the data, so it's safe to use print and string manipulation functions._  
_Returns:_ data stored in `array`, `length` [`true`:`false`]  

```arduino
ModuleData data;
if (module.readWait("cfg/robot/name", data)) {// Read robot name
  char *name;
  int length;
  data.get(name, length); // Information is pointed to name and length variables
  Serial.print("'cfg/robot/name' is ");
  Serial.print(name);
  Serial.print(", length: ");
  Serial.println(length); // Prints: 'cfg/robot/name' is Totem X4, length: 8
}
```

#### getCmdHash()

: Get hash of result command.
Will return command identifier that can be generated with `#!arduino TotemModule::hashCmd("command")`.  
_Returns:_ unsigned 32bit hash [`0`:`0xFFFFFFFF`]  

```arduino
void onModuleData(ModuleData data) {
  if (data.getCmdHash() == TotemModule::hashCmd("cfg/robot/name")) {
    // Got command "cfg/robot/name"
  }
  else if (data.getCmdHash() == TotemModule::hashCmd("version")) {
    // Got command "version"
  }
}
```
