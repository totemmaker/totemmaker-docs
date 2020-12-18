# TotemRobot

*[BLE]: Bluetooth Low Energy

Object representing remote Totem robot connection over BLE

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
| [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} | [getName()](#string-getname) | Get robot name |
| [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} | [getAddress()](#string-getaddress) | Get robot MAC address |
| `int` | [getModel()](#int-getmodel) | Get robot model (type) hash |
| `int` | [getColor()](#int-getcolor) | Get robot appearance color |
| `int` | [getNumber()](#int-getcolor) | Get robot module number |
| `bool` | [isConnected()](#bool-isconnected) | Check if connection is active |
| `bool` | [connect()](#bool-connect) | Connect to robot |
| _none_ | [disconnect()](#disconnect) | Disconnect from robot |
| _none_ | [attach](#attachtotemmodule)([`TotemModule`](/API/TotemModule)) | Attach module to connection |
| _none_ | [detach](#detachtotemmodule)([`TotemModule`](/API/TotemModule)) | Detach module from connection |

## Example

```arduino
void setup() {
  Serial.begin(115200);
  // Initialize Totem BLE interface
  Totem.BLE.begin();
  // Start searching for Totem robot and block further
  // code execution until any robot is connected
  TotemRobot robot = Totem.BLE.findRobot();
  // Robot (module) connected. Print name:
  Serial.print("Connected robot: ");
  Serial.println(robot.getName());
}
```

## Description

> ### [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} getName() { data-toc-label='getName()' }

**Description:** Get robot Bluetooth name.  
Same as configured in `cfg/robot/name` command.  
**Result:** [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} containing robot Bluetooth name.

```arduino
TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
Serial.print("Connected to: ");
Serial.println(robot.getName());
```

> ### [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} getAddress() { data-toc-label='getAddress()' }

**Description:** Get robot Bluetooth MAC (example: "00:11:22:33:44:55"). Each robot has unique address.  
**Result:** [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} containing robot Bluetooth address.

```arduino
TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
Serial.print("Connected robot MAC address: ");
Serial.println(robot.getAddress());
```

> ### `int` getModel()  { data-toc-label='getModel()' }

**Description:** Get robot Totem model hash.  
Each one can have assigned hashed module string to tell apart what Totem product is used with particular controller. This hash can be generated with `#!arduino TotemModule::hashModel("MiniTrooper")`.  
Can be configured with `cfg/robot/model` command.  
**Result:** Totem robot model number.

```arduino
void setup() {
  TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
  Serial.print("Connected robot model number: ");
  Serial.println(robot.getModel());
  if (robot.getModel() == TotemModule::hashModel("MiniTrooper")) {
    // Check if robot is MiniTrooper
  }
}
```

> ### `int` getColor()  { data-toc-label='getColor()' }

**Description:** Get robot appearance color. Each robot can have different color to tell them apart easier. Also this color can be used for on board RGB LEDs.  
**Result:** 16bit High color encoding.

```arduino
void setup() {
  TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
  Serial.print("Connected robot color code: 0x");
  Serial.println(robot.getColor(), HEX);
}
```

> ### `int` getNumber()  { data-toc-label='getNumber()' }

**Description:** Get robot controller board module number.  
To identify if connecting to X4 or X3.  
**Result:** module number.

```arduino
void setup() {
  TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
  Serial.print("Connected to module: ");
  Serial.println(robot.getNumber());
}
```

> ### `bool` isConnected()  { data-toc-label='isConnected()' }

**Description:** Check if robot is connected over BLE.  
**Result:** true if robot is connected, false otherwise.

```arduino
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  if (robot.isConnected())
    Serial.println("Robot is connected");
  else
    Serial.println("Robot is not connected");
}
```

> ### `bool` connect()  { data-toc-label='connect()' }

**Description:** Connect to Totem robot over BLE.  
**Result:** true if connected, false on failure.

```arduino
void onFoundRobot(TotemRobot robot) { // findRobot event listing each discovered device
  if (robot.connect()) // Establish connection to selected robot
    Serial.println("Totem robot connected");
  else
    Serial.println("Connection failed");
}
```

> ### disconnect()

**Description:** Manually disconnect from robot connected over BLE.  

```arduino
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.disconnect(); // Robot will be disconnected
}
```

> ### attach([`TotemModule`](/API/TotemModule))

**Description:** Attach selected [`TotemModule`](/API/TotemModule) to active BLE connection.  
All initialized modules are automatically attached to active connection. This is required if connecting to more than one robot at the same time and to use separate [`TotemModule`](/API/TotemModule) objects in multiple connections.

```arduino
TotemModule module(03);
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.attach(module); // Assign module X3 object to selected robot connection.
                        // "module" now will only control X3 modules that are available in provided "robot" connection.
}
```

> ### detach([`TotemModule`](/API/TotemModule))

**Description:** Detach selected [`TotemModule`](/API/TotemModule) from active BLE connection.  

```arduino
TotemModule module(03);
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.detach(module); // Detach module X3 object from selected robot connection. 
                        // "module"' will no longer respond to commands
}
```