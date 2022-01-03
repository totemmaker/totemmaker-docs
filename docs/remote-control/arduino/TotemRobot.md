# TotemRobot

*[BLE]: Bluetooth Low Energy

Object representing remote Totem robot connection over BLE. Received from [BLE interface](/interfaces/BLE/).

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
| [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} | [getName()](#getname) | Get robot name |
| [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} | [getAddress()](#getaddress) | Get robot MAC address |
| `int` | [getModel()](#getmodel) | Get robot model (type) hash |
| `int` | [getColor()](#getcolor) | Get robot appearance color |
| `int` | [getNumber()](#getNumber) | Get robot module number |
| `bool` | [isConnected()](#isconnected) | Check if connection is active |
| `bool` | [connect()](#connect) | Connect to robot |
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

## API

### Functions

#### getName() { data-toc-label='getName()' }

: Get robot Bluetooth name.  Same as configured in `cfg/robot/name` command.  
_Returns:_ [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} containing robot Bluetooth name.

```arduino
TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
Serial.print("Connected to: ");
Serial.println(robot.getName());
```

#### getAddress() { data-toc-label='getAddress()' }

: Get robot Bluetooth MAC (e.g. "00:11:22:33:44:55"). Each robot has unique address.  
_Returns:_ [`String`](https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/){target=_blank} containing robot Bluetooth address.

```arduino
TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
Serial.print("Connected robot MAC address: ");
Serial.println(robot.getAddress());
```

#### getModel()  { data-toc-label='getModel()' }

: Get robot Totem model hash.  
Each one can have assigned hashed string to tell apart what Totem product is used with particular controller. This hash can be generated with `#!arduino TotemModule::hashModel("MiniTrooper")`.  
Can be configured with `cfg/robot/model` command.  
_Returns:_ Totem robot model hash [`0`:`0xFFFF`].

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

#### getColor()  { data-toc-label='getColor()' }

: Get robot appearance color. Each robot can have different color to tell them apart easier. Also this color can be used for on board RGB LEDs.  
_Returns:_ 24bit color code [`0x000000`:`0xFFFFFF`].

```arduino
void setup() {
  TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
  Serial.print("Connected robot color code: 0x");
  Serial.println(robot.getColor(), HEX);
}
```

#### getNumber()  { data-toc-label='getNumber()' }

: Get robot controller board module number.  
To identify if connecting to X4 or X3.  
_Returns:_ module number [`0`:`255`].

```arduino
void setup() {
  TotemRobot robot = Totem.BLE.findRobot(); // Find and connect a robot
  Serial.print("Connected to module: ");
  Serial.println(robot.getNumber());
}
```

#### isConnected()  { data-toc-label='isConnected()' }

: Check if robot is connected over BLE.  
_Returns:_ is robot connected [`true`:`false`].

```arduino
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  if (robot.isConnected())
    Serial.println("Robot is connected");
  else
    Serial.println("Robot is not connected");
}
```

#### connect()  { data-toc-label='connect()' }

: Connect to Totem robot over BLE.  
_Returns:_ connection is successful [`true`:`false`].

```arduino
void onFoundRobot(TotemRobot robot) { // findRobot event listing each discovered device
  if (robot.connect()) // Establish connection to selected robot
    Serial.println("Totem robot connected");
  else
    Serial.println("Connection failed");
}
```

#### disconnect()

: Manually disconnect from robot connected over BLE.  

```arduino
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.disconnect(); // Robot will be disconnected
}
```

#### attach([`TotemModule`](/API/TotemModule))

: Attach selected [`TotemModule`](/API/TotemModule) to active BLE connection.  
All initialized modules are automatically attached to active connection. This is required if connecting to more than one robot at the same time and to use separate [`TotemModule`](/API/TotemModule) objects in multiple connections.

```arduino
TotemModule module(03);
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.attach(module); // Assign module X3 object to selected robot connection.
                        // "module" now will only control Mini Control Board X3 that are available in provided "robot" connection.
}
```

#### detach([`TotemModule`](/API/TotemModule))

: Detach selected [`TotemModule`](/API/TotemModule) from active BLE connection.  

```arduino
TotemModule module(03);
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast(); // Get last connected robot
  robot.detach(module); // Detach module X3 object from selected robot connection. 
                        // "module"' will no longer respond to commands
}
```
