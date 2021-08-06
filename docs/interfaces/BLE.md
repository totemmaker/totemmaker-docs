# BLE

*[BLE]: Bluetooth Low Energy

Provides wireless communication with Totem modules. This interface can be used whit any BLE capable ESP32 development board to control Totem modules, even from multiple connections. Interface provides all required functionality to discover Totem robots and connect them.
Modules are able to represent themselves as "robot", providing name, color, model (type of robot). This information is available before BLE connection is established.

```arduino
void setup() {
  Totem.BLE.begin(); // Initialize Bluetooth Low Energy interface
}
```

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
| _none_ | [begin()](#begin) | Initialize interface |
| [`TotemRobot`](/API/TotemRobot) | [findRobot()](#findrobot) | Blocking find and connect |
| _none_ | [findRobotNoBlock()](#findrobotnoblock) | Non-blocking find and connect |
| `bool` | [isFinding()](#isfinding) | Check if find process is active |
| _none_ | [stopFind()](#stopfind) | Stop find process |
| _none_ | [attachOnConnect(`RobotReceiver`)](#attachonconnectrobotreceiver) | Register on robot connected function |
| `int` | [getConnectedCount()](#getconnectedcount) | Get active connections count |
| [`TotemRobot`](/API/TotemRobot) | [getConnectedLast()](#getconnectedlast) | Get last connected robot |
| [`TotemRobot[]`](/API/TotemRobot) | [getConnectedList()](#getconnectedlist) | Get active connections list |

## Example

```arduino
// Include Totem library
#include <Totem.h>
// Use MiniTrooper as example robot
TotemMiniTrooper mini;
// User defined function that receives all discovered Totem robots
void onFoundRobot(TotemRobot robot) {
  // Check each found robot and connect to only whose name is "MyRobot"
  // For String reference check: https://www.arduino.cc/reference/en/language/variables/data-types/stringobject/
  if (robot.getName().compareTo("MyRobot") == 0) {
    // Start BLE connection to selected robot.
    if (robot.connect())
      Serial.println("Robot connected");
    else
      Serial.println("Robot connection failed");
    // After successfully connected, findRobot(onFoundRobot) will unblock and application proceeds
  }
}

void setup() {
  // put your setup code here, to run once:
  Totem.BLE.begin(); // Initialize Bluetooth Low Energy interface
  // Totem.BLE.findRobot(); // Optional: automatically connect to first discovered Totem robot
  Totem.BLE.findRobot(onFoundRobot); // Start searching over BLE. This function will block until connected to any 
                                     // Totem robot. After connection is successful, loop() will be executed.
                                     // onFoundRobot will list every discovered robot.
}

void loop() {
  // put your main code here, to run repeatedly:
  // Example to open and close flipper on MiniTrooper
  mini.flipperOpen();
  delay(1000);
  mini.flipperClose();
  delay(1000);
}
```

## API

### Functions

#### begin()

: Initialize Bluetooth Low Energy interface. Must be executed inside [setup()](https://www.arduino.cc/reference/en/language/structure/sketch/setup/){target=_blank} function before using library.  

```arduino
void setup() {
  Totem.BLE.begin(); // Initialize Bluetooth Low Energy interface
}
```

#### findRobot() { data-toc-label='findRobot()' }

#### findRobot(`RobotReceiver`) { data-toc-label='findRobot(result)' }

: Start searching for available Totem robots over Bluetooth Low Energy. This function will block until connection to any robot is established.  
On successful connection find process will be stopped. Additionally it can be stopped with call to [stopFind()](#stopfind).  
`RobotReceiver` is a user defined function to get all discovered robots.  
Format: `#!arduino void onFoundRobot(TotemRobot robot)`. Robot information can be accessed trough [`TotemRobot`](/API/TotemRobot) parameter.
By calling `robot .`[`connect()`](/API/TotemRobot#connect) user can decide which robot to select.  
_Parameter:_  
`()` - automatically connect to first discovered Totem robot.  
`RobotReceiver` - discovered robot connect function `onFoundRobot`.  
_Returns:_  [`TotemRobot`](/API/TotemRobot).

```arduino
void onFoundRobot(TotemRobot robot) {
  // Totem robot discovered
  robot.connect();
}
void function() {
  Totem.BLE.findRobot(); // Automatically connect to first discovered Totem robot
  Totem.BLE.findRobot(onFoundRobot); // List result in onFoundRobot before connecting to Totem robot
}
```

#### findRobotNoBlock()

#### findRobotNoBlock(`RobotReceiver`)  { data-toc-label='findRobotNoBlock(result)' }

: Start searching for available Totem robots over Bluetooth Low Energy. This function does not block main program execution. All connection process will be done in background.  
After connection is established, this process will be stopped. Additionally it can be stopped with call to [stopFind()](#stopfind).  
`RobotReceiver` is a user defined function to get all discovered robots.  
Format: `#!arduino void onFoundRobot(TotemRobot robot)`. Robot information can be accessed trough [`TotemRobot`](/API/TotemRobot) parameter.
By calling `robot .`[`connect()`](/API/TotemRobot#connect) user can decide which robot to select.  
_Parameter:_  
`()` - automatically connect to first discovered Totem robot.  
`RobotReceiver` - discovered robot connect function `onFoundRobot`.  

```arduino
void onFoundRobot(TotemRobot robot) {
  // Totem robot discovered
  robot.connect();
}
void function() {
  Totem.BLE.findRobotNoBlock(); // Automatically connect to first discovered Totem robot
  Totem.BLE.findRobotNoBlock(onFoundRobot); // List result in onFoundRobot before connecting to Totem robot
}
```

#### isFinding() { data-toc-label='isFinding()' }

: Check if [findRobot()](#findrobot) or [findRobotNoBlock()](#findrobotnoblock) was called and process is active.  
_Returns:_ discovery process is active [`true`:`false`].

```arduino
void function() {
  if (Totem.BLE.isFinding())
    Serial.println("Robot find is active");
  else
    Serial.println("Robot find is inactive);
}
```

#### stopFind()

: Stop searching for robot and unblock [findRobot()](#findrobot). Only used when required to stop this process manually. After connection, the search process is stopped automatically and calling this function is not required.  

```arduino
void function() {
  Totem.BLE.stopFind(); // Stop searching for robot over Bluetooth Low Energy
}
```

#### attachOnConnect(`RobotReceiver`) { data-toc-label='attachOnConnect(result)' }

: Register function that will be called when [findRobot()](#findrobot) process connects to a robot. This is handy when using non-blocking find process and we don't know when connection is established.  
`RobotReceiver` is a user defined function to connected robot event.  
Format: `#!arduino void onConnectedRobot(TotemRobot robot)`. Robot information can be accessed trough [`TotemRobot`](/API/TotemRobot) parameter.  
_Parameter:_
`RobotReceiver` - connected robot event function `onConnectedRobot`.  

```arduino
void onConnectedRobot(TotemRobot robot) {
  // findRobotNoBlock connected to robot in background and ended find procedure
  Serial.print("Connected robot: ");
  Serial.println(robot.getName());
}
void function() {
  Totem.BLE.findRobotNoBlock(); // Automatically connect to first discovered Totem robot
                                // Code execution continues with active find procedure
}
```

#### getConnectedCount() { data-toc-label='getConnectedCount()' }

: Count of currently connected Totem robots.  
_Returns:_ connected robots count [`0`:`20`]

```arduino
void function() {
  Serial.print("Currently connected robots count: ");
  Serial.println(Totem.BLE.getConnectedCount());
}
```

#### getConnectedLast() { data-toc-label='getConnectedLast()' }

: Get last connected robot.  
If no connection was made before, this function returns a dummy object that isn't connected.  
_Returns:_  [`TotemRobot`](/API/TotemRobot).

```arduino
void function() {
  TotemRobot robot = Totem.BLE.getConnectedLast();
  Serial.print("Is last connected robot still connected: ");
  Serial.println(robot.isConnected() ? "Yes" : "No");
}
```

#### getConnectedList() { data-toc-label='getConnectedList()' }

: Get list of connected robots.  
_Returns:_ [`TotemRobot[]`](/API/TotemRobot).

```arduino
void function() {
  for (auto robot : getConnectedList()) {
    Serial.print("Connected robot: ");
    Serial.println(robot.getName());
  }
}
```
