# Modules information

*[BLE]: Bluetooth Low Energy

![Totem module](/assets/images/module_14.jpg){style="display: block;margin-left: auto;margin-right: auto;padding-bottom:20px"}

This section contains the list of all available Totem modules and documentation regarding their capabilities.  

## Identifying modules

Every Totem module has number in white square printed on the board. It indicates **module number**. There is a variety of different module types (sensors, controllers, ...) and each one of them has a unique number assigned . It is used to find documentation and to control specific module on the TotemBUS network.  
There is also a unique serial identifier in case there is more than one module with the same number within TotemBUS network. A command can be sent to a specific module matching it's serial number. It isn't visible anywhere on the board and must be acquired programmatically.  
Version tag (v1.0) indicates board revision. Some boards may have hardware changes in later versions which are identified by this number.

## Connecting modules

![Totem modules daisy-chain](/assets/images/module_15_connected.jpg)

Modules are daisy-chain connectable with a 6 pin connector. It provides power and communication. Some modules (like 03 and 04) has BLE interface and can connect to TotemBUS network wirelessly instead of a physical cable. This is mostly used to control modules from a smartphone. Module is operational the second it's physically connected to TotemBUS network and powered on.

## Controlling modules

Each module accepts a set of commands to control their specific capabilities (like reading sensor data, controlling lights or motors). This list is available in documentation page for specific module. These commands can be sent using [Totem API](/API).

## General module commands

Each Totem module implements this default set of commands:

#### `indicate` ( )

**Description:** Blink on-board led. Used to indicate which module received a command.

```arduino
module.write("indicate"); // Blink on-board led
```

#### `restart` ( )

**Description:** Perform software restart of the module itself.

```arduino
module.write("restart"); // Restart module
```

#### `version` ( )

**Description:** Get firmware version of the module. This command is read-only.  
**Value:** Encoded decimal value. 100 translates to v1.00 (150 -> v1.50).

```arduino
int version = module.readWait("version").getInt(); // Read module firmware version
Serial.print("Module firmware version: v");
Serial.print(version/100); // Get first digit of value
Serial.print(".");
Serial.println(version%100); // Get last digit of value
// Will print: "Module firmware version: v1.0"
```
