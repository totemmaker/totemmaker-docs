# Modules information

*[BLE]: Bluetooth Low Energy

![Totem module](/assets/images/module_14.jpg){style="display: block;margin-left: auto;margin-right: auto;padding-bottom:20px"}

This section contains the list of all available Totem modules and documentation regarding their capabilities.  

## Identifying modules

Every Totem module has number in white square printed on the board. It indicates **module number**. There is a variety of different module types (sensors, controllers, ...) and each one of them has a unique number assigned . It is used to find documentation and to control specific module on the TotemBUS network.  
There is also a unique serial identifier in case there is more than one module with the same number within TotemBUS network. A command can be sent to a specific module matching it's serial number. It isn't visible anywhere on the board and must be acquired programmatically.  
Version tag (v1.0) indicates board revision. Some boards may have hardware changes in later versions which are identified by this number.

## Connecting modules

![Totem module 15 connected](/assets/images/module_15_connected.jpg)

Modules are daisy-chain connectable with a 6 pin connector. It provides power and communication. Some modules (like 03 and 04) has BLE interface and can connect to TotemBUS network wirelessly instead of a physical cable. This is mostly used to control modules from a smartphone. Module is operational the second it's physically connected to TotemBUS network and powered on.

## Controlling modules

![Totem module 14 car](/assets/images/module_14_car.jpg)

Each module accepts a set of commands to control their specific capabilities (like reading sensor data, controlling lights or motors). Controller can also subscribe to data in order for module to start broadcasting it. The list of available commands can be found in documentation page for specific module. Modules can be interfaced using [Totem API](/remote-control).

## General module commands

Each Totem module implements default set of commands:

#### `indicate` ( )

: Blink on-board led. Used to indicate which module received a command.

```arduino
module.write("indicate"); // Blink on-board led
```

#### `restart` ( )

: Perform module software restart.

```arduino
module.write("restart"); // Restart module
```

#### `version` ( )

: Get module firmware version. This command is read-only.  
Version format is [Semantic Versioning](https://semver.org) split for each byte.  

```arduino
int version = module.readWait("version").getInt();
Serial.printf("Firmware version: v%d.%d.%d\n", 
    (version >> 16) & 0xFF, (version >> 8) & 0xFF, (version >> 0) & 0xFF);
// Will print: "Firmware version: v1.0.0"
```

#### `revision` ( )

: Get module hardware revision. This command is read-only.  
Version format is [Semantic Versioning](https://semver.org) split for each byte.  

```arduino
int revision = module.readWait("revision").getInt();
Serial.printf("Hardware revision: v%d.%d\n", 
    (revision >> 16) & 0xFF, (revision >> 8) & 0xFF);
// Will print: "Hardware revision: v1.0"
```
