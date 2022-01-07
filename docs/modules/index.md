# TotemBUS modules

*[BLE]: Bluetooth Low Energy

![Totem line follow module 14](/assets/images/module_14.jpg){style="display: block;margin-left: auto;margin-right: auto;padding-bottom:20px"}

Functionality extension modules for RoboBoard X4. Each one has a set of features, that become available once module is connected over TotemBUS cable. It's an easy way to enrich your project without any tinkering or looking for compatible libraries. Each module has documented objective API code to control all its capabilities.  

## Module number

Every module has number [XX] printed in a white square to identify it's type and to find documentation. Same number is used when writing the code.  

## Module serial

There is unique serial identifier in case more than one module with the same number exists within TotemBUS network. A command can be sent to a specific module matching it's serial number. It isn't visible anywhere on the board and must be acquired programmatically.  

## Module revision

Version tag (v1.0) identifies board revision. Each one may have some hardware changes or new functionality. Firmware is versioned separately.  

## Connecting modules

![Totem module 15 connected](/assets/images/module_15_connected.jpg)

Modules are daisy-chain connectable with a 6 pin connector. It provides power and communication. Some modules (boards like X3 and X4) has BLE interface and can connect to TotemBUS network wirelessly instead of a physical cable. This is mostly used to control modules from a smartphone. Module is operational the second it's physically connected to TotemBUS network and powered on.

## Controlling modules

![Totem module 14 car](/assets/images/module_14_car.jpg)

Each module accepts a set of commands to control their specific capabilities (like reading sensor data, controlling lights or motors). Controller can also subscribe to data in order for module to start broadcasting it. The list of available functions can be found in documentation page for specific module.  

## TotemBUS protocol

![TotemBUS](/assets/images/totembus.jpg)

*[CAN]: Controller Area Network
*[MQTT]: Messaging protocol for the Internet of Things (IoT)
*[CANopen]: Communication protocol and device profile specification for embedded systems used in automation
*[daisy-chain]: Wiring scheme in which multiple devices are wired together in sequence or in a ring

TotemBUS is our implemented communication protocol to control Totem modules over CAN bus network. It is designed to overcome limitations of similar systems and offer new ways of building robots. The key factors are fast response, low overhead, higher power and easy programming. Communication is peer-to-peer, so modules can respond any time, unlike I2C alternatives ([compare to Qwiic](/roboboard-x4/qwiic/#comparison-to-totembus)). This gives more flexibility in module functionality and communication types.  

This protocol utilizes advantages of CAN bus addressing. Each module has embedded number and serial that is sent over the network. Using this technique, single message can be sent to a specific module or group of them. Every device on the bus can see what is happening and intercept information.  
As CAN bus itself is robust communication protocol, it can only transfer 8 bytes of data in single frame. Here comes TotemBUS protocol to wrap all module functionality and encode it to simple CAN frames. There are similar protocols available (e.g. CANopen), but our solution is designed for maximum efficiency in this use case.

Modules accept string based messages (similar as MQTT) that are encoded to 32bit number, for communication efficiency. Each message can pass a variable length and format parameter. This saves a lot of bandwidth and increases response time. Protocol also implements reading (polling), broadcasting and subscription mechanism. This is used to remove some strain from host device and leave communication line less cluttered with only mandatory data sent.  
