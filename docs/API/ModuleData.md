# ModuleData

Object containing data received from module.

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
| `bool` | [is(`command`)](#bool-iscommand) | Check if data came of specified command |
| `bool` | [isInt()](#bool-isint) | Check if there is valid integer data |
| `bool` | [isString()](#bool-isstring) | Check if there is valid string data |
| `int`  | [getInt()](#int-getint) | Read integer from data |
|`string`| [getString()](#string-getstring) | Read string from data |
| `bool` | [getData(`array`)](#bool-getdataarray) | Read array pointer of data |
| `bool` | [getData(`array`, `length`)](#bool-getdataarray-length) | Read array pointer and data length |
| `int`  | [getCmdHash()](#int-getcmdhash) | Get hash of received command string |

## Example

```arduino
#include <Totem.h>
// Declare that we are willing to communicate with X4 module
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
  Totem.X4.begin(); // Initialize X4 module
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

## Description

> ### `bool` is(`command`) { data-toc-label='is(command)' }

**Description:** Check if received data is from specified `command`.  
**Result:** True if `command` and result command match.

```arduino
void onModuleData(ModuleData data) {
  if (data.is("cfg/robot/name"))
    Serial.println("Data came from 'cfg/robot/name' command");
  if (data.is("version"))
    Serial.println("Data came from 'version' command");
}
```

> ### `bool` isInt() { data-toc-label='isInt()' }

**Description:** Check if received data contains integer value.  
**Result:** True if valid data can be received with [`getInt()`](#int-getint)

```arduino
ModuleData data;
module.readWait("cfg/robot/name", data);
data.isInt(); // returns false. "cfg/robot/name" data contains string value
module.readWait("version", data);
data.isInt(); // returns true. "version" data contains integer value
```

> ### `bool` isString() { data-toc-label='isString()' }

**Description:** Check if received data contains string value.  
**Result:** True if valid data can be received with [`getString()`](#string-getstring)

```arduino
ModuleData data;
module.readWait("cfg/robot/name", data);
data.isString(); // returns true. "cfg/robot/name" data contains string value
module.readWait("version", data);
data.isString(); // returns false. "version" data contains integer value
```

> ### `int` getInt() { data-toc-label='getInt()' }

**Description:** Read integer value from received data.  
**Result:** Up to 32-bit signed value.

```arduino
ModuleData data;
// Read firmware version. Read result will be stored to "data".
if (module.readWait("version", data)) {
  Serial.print("Value of 'version': ");
  Serial.println(data.getInt()); // Prints: Value of 'version': 100
}
```

> ### `string` getString() { data-toc-label='getString()' }

**Description:** Read string value from received data.  
**Result:** Pointer to first string character with NULL terminator.

```arduino
ModuleData data;
// Read robot name. Read result will be stored to "data".
if (module.readWait("cfg/robot/name", data)) {
  Serial.print("'cfg/robot/name' is ");
  Serial.println(data.getString()); // Prints: 'cfg/robot/name' is Totem X4
}
```

> ### `bool` getData(`array`) { data-toc-label='getData(array)' }

**Description:** Get pointer to data array.  
In case of printing received data, TotemBUS protocol always adds null terminator at the end of the data, so it's safe not to check the total length.  
**Result:** Pointer to data array.

```arduino
ModuleData data;
if (module.readWait("cfg/robot/name", data)) { // Read robot name
  char *name;
  data.getData(name); // Information is pointed to name variable
  Serial.print("'cfg/robot/name' is ");
  Serial.println(name); // Prints: 'cfg/robot/name' is Totem X4
}
```

> ### `bool` getData(`array`, `length`) { data-toc-label='getData(array, len)' }

**Description:** Get pointer to data array and it's length.  
In case of printing received data, TotemBUS protocol always adds null terminator at the end of the data, so it's safe not to check the total length.  
**Result:** Pointer to data array and it's length.

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

> ### `int` getCmdHash()  { data-toc-label='getCmdHash()' }

**Description:** Get hash of result command.
Will return command identifier that can be generated with `#!arduino TotemModule::hashCmd("command")`.  
**Result:** Command hash.

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
