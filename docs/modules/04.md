# [04] RoboBoard X4

!!! note
    This page contains information for controlling RoboBoard X4 **remotely** from other devices. If you are looking for RoboBoard X4 **programming documentation** go to [RoboBoard X4 documentation](../roboboard-x4/index.md) section.  

```arduino
#include <Totem.h>
TotemModule module(04);
```

***

## Module commands

!!! info "Command naming convention"  
    Some commands controls various channels (A, B, C, ...). There is a logic behind command naming for flexibility:  
    • `#!arduino write("commandA", 10)` - will set value `10` for individual channel A.  
    • `#!arduino write("commandAll", 100)` - will set value `100` for all existing channels.  
    • `#!arduino write("commandABCD", 10, 20, 30, 40)` - will set individual values for A, B, C, D channels.  
    • `#!arduino write("commandABC", 10, 20, 30)` - will set individual values for A, B, C channels.  
    Only commands listed in "Module commands" will be accepted. Other variations will be discarded.  
    All commands are case sensitive! Calls like `CommandA`, `COMMANDa`, `motorA/BRAKE` will be ignored.

### Configuration

Commands starting with "cfg" retains its value after module power off. Used as module configuration parameters.

#### cfg/robot/name&nbsp;( `name` )

: Configure robot name visible during connection.  
_Parameter:_  
`name` - string (text) up to 30 bytes max.  
_Default_: `Totem X4`.

```arduino
module.write("cfg/robot/name", "My robot"); // Set robot name
```

#### cfg/robot/model&nbsp;( `model` )

: Configure Totem robot model (Truck, Big Spider, ...).  
Used to identify type of the robot. Can be set to any value. Also received with [getModel()](../remote-control/arduino/TotemRobot.md#getmodel).  
Value can be generated with command: `#!arduino TotemModule::hashModel("model")`.  
_Parameter:_  
`model` - 16bit hash of robot model name [`0`:`0xFFFF`].  
_Default:_ `0`.

```arduino
// Set robot model to Truck
module.write("cfg/robot/model", TotemModule::hashModel("Truck"));
// Check if robot is Truck
ModuleData data;
if (module.readWait("cfg/robot/model", data) &&
    data.getInt() == TotemModule::hashModel("Truck")) { }
```

#### cfg/robot/color&nbsp;( `color` )

: Configure robot color.  
Used to recognize robot appearance by its color. It may be displayed inside mobile application or on-board RGB lights.  
_Parameter:_  
`color` - 24bit RGB color value (without brightness) [`0x000000`:`0xFFFFFF`].  
_Default:_ `0`.

```arduino
module.write("cfg/robot/color", 0x0000FF); // Set robot color to BLUE
// Alternative to set individual RGB colors:
module.write("cfg/robot/color", 0, 0, 255); // Set robot color to BLUE
```

#### cfg/motorABCD/invert&nbsp;(&nbsp;`onA`&nbsp;,&nbsp;`onB`&nbsp;,&nbsp;`onC`&nbsp;,&nbsp;`onD`&nbsp;)

: Configure motor output polarity.  
Individual motors can be configured to invert its spinning direction. Allows to fix reversed motor wiring without reconfiguring robot controls.  
_Parameter:_  
`onA` - invert channel A [`true`:`false`].  
`onB` - invert channel B [`true`:`false`].  
`onC` - invert channel C [`true`:`false`].  
`onD` - invert channel D [`true`:`false`].  
_Default:_ `false` (off)  

```arduino
// Invert channels A and B. C, B - not inverted.
module.write("cfg/motorABCD/invert", true, true, false, false);
```

#### cfg/motorABCD/autobrake&nbsp;(&nbsp;`powerA`&nbsp;,&nbsp;`powerB`&nbsp;,&nbsp;`powerC`&nbsp;,&nbsp;`powerD`&nbsp;)

: Configure motor autobrake feature.  
When channel power is set to `0`, an electronic motor brake will be applied.  
Can be configured with different percentage of braking power per each channel.  
_Parameter:_  
`powerA` - autobrake channel A [`0`:`100`]%.  
`powerB` - autobrake channel B [`0`:`100`]%.  
`powerC` - autobrake channel C [`0`:`100`]%.  
`powerD` - autobrake channel D [`0`:`100`]%.  
_Default:_ `0` (off)  

```arduino
// Set channels A, B to 100% brake power, C to 50%, D - no braking.
module.write("cfg/motorABCD/autobrake", 100, 100, 50, 0);
```

#### cfg/reset ( )

: Reset all module configuration to factory default.  

```arduino
module.write("cfg/reset"); // Reset all module settings to default
```

***

### DC motor control

#### motorA ( `power` )

#### motorB ( `power` )

#### motorC ( `power` )

#### motorD ( `power` )

: Set individual DC motor A, B, C, D channel output power.  
Positive - forward, negative - backward, `0` - stop.  
_Parameter:_  
`power` - channel output power [`-100`:`100`]%.  

```arduino
module.write("motorA", 100); // Write channel A to put out 100% of power
```

#### motorABCD&nbsp;(&nbsp;`powerA`&nbsp;,&nbsp;`powerB`&nbsp;,&nbsp;`powerC`&nbsp;,&nbsp;`powerD`&nbsp;)

: Set DC motor A, B, C, D channels output power with a single command call.  
Positive - forward, negative - backward, `0` - stop.  
_Parameter:_  
`powerA` - set channel A output power [`-100`:`100`]%.  
`powerB` - set channel B output power [`-100`:`100`]%.  
`powerC` - set channel C output power [`-100`:`100`]%.  
`powerD` - set channel D output power [`-100`:`100`]%.  

```arduino
module.write("motorABCD", 100, 50, 0, -100); // Write channels A, B, C, D
```

#### motorA/brake ( `brake` )

#### motorB/brake ( `brake` )

#### motorC/brake ( `brake` )

#### motorD/brake ( `brake` )

: Apply brake to individual DC motor A, B, C, D channel .  
_Will reset to `0` if power command is called._  
_Parameter:_  
`brake` - brake intensity [`0`:`100`]%.  

```arduino
module.write("motorA/brake", 100); // Set channel A brake power to 100%
```

#### motorABCD/brake&nbsp;(&nbsp;`brakeA`&nbsp;,&nbsp;`brakeB`&nbsp;,&nbsp;`brakeC`&nbsp;,&nbsp;`brakeD`&nbsp;)

: Apply brake to DC motor A, B, C, D channels with a single command call.  
_Will reset to `0` if power command is called._  
_Parameter:_  
`brakeA` - channel A brake intensity [`0`:`100`]%.  
`brakeB` - channel B brake intensity [`0`:`100`]%.  
`brakeC` - channel C brake intensity [`0`:`100`]%.  
`brakeD` - channel D brake intensity [`0`:`100`]%.  

```arduino
module.write("motorABCD/brake", 100, 50, 0, 0); // Brake channels : A - 100%; B - 50%; C, D - no brake
```

***

### Servo motor control

#### servoA ( `position` )

#### servoB ( `position` )

#### servoC ( `position` )

: Set individual servo motor A, B, C channels position (-100, 0, 100) = (0°, 90°, 180°).  
_Parameter:_  
`position` - position of servo arm [`-100`:`100`].  

```arduino
module.write("servoA", 0); // Set servo channel A to 90° angle
```

#### servoABC&nbsp;(&nbsp;`positionA`&nbsp;,&nbsp;`positionB`&nbsp;,&nbsp;`positionC`&nbsp;)

: Set servo motor A, B, C channels position with a single command call.  
Value converts to angle: (-100, 0, 100) = (0°, 90°, 180°).  
_Parameter:_  
`positionA` - channel A position of servo arm [`-100`:`100`].  
`positionB` - channel B position of servo arm [`-100`:`100`].  
`positionC` - channel C position of servo arm [`-100`:`100`].  

```arduino
module.write("servoABC", 100, 50, -100); // Write channels A, B, C
```

***

### On-board features

#### led ( `on` )

: Set on-board indication led state.  
_Parameter:_  
`on` - toggle LED [`true`:`false`].

```arduino
module.write("led", true); // Turn on-board indication led on
```

#### button ( )

: Get state of on-board button. Read it once or subscribe to receive an event when button is pressed. This command is read-only.  
_Returns:_ button is pressed [`true`:`false`].

=== "Inline"
    ```arduino
    bool buttonIsPressed = module.readWait("button"); // Read button state
    // 'readWait' will delay code execution until result is received from the module.
    ```
=== "Inline ModuleData"
    ```arduino
    ModuleData data; // Prepare variable to store read result
    module.readWait("button", data); // Perform read with wait until result is received
    bool buttonIsPressed = data.getInt(); // Get button state from result
    ```
=== "Event"
    ```arduino
    // Function that will receive all subscribed commands of RoboBoard X4
    void onModuleData(ModuleData data) {
        if (data.is("button")) { // Check if received command is "button"
            bool buttonIsPressed = data.getInt(); // Get button state
            Serial.print("Button state = ");
            Serial.println(buttonIsPressed?"PRESS":"RELEASE");
        }
    }
    void setup() {
        module.attachOnData(onModuleData); // Register onModuleData function
        module.subscribe("button"); // Subscribe to receive any state change of button
    }
    ```

#### battery ( )

: Get connected battery voltage. This command is read-only.  
Returned value is in millivolts. Divide by 1000 to get volts [`8.4V`:`12.6V`].  
_Returns:_ [`8400`:`12600`]mV. `0` - battery not connected  

=== "Inline"
    ```arduino
    // Read battery voltage
    int voltage = module.readWait("battery").getInt(); // Result: 11350 → 11.350V
    // 'readWait' will delay code execution until result is received from the module.
    ```
=== "Inline ModuleData"
    ```arduino
    ModuleData data; // Prepare variable to store read result
    module.readWait("battery", data); // Perform read with wait until result is received
    bool voltage = data.getInt(); // Get battery value from result. 11350 → 11.350V
    ```
=== "Event"
    ```arduino
    // Function that will receive all subscribed commands of module
    void onModuleData(ModuleData data) {
        if (data.is("battery")) { // Check if received command is "battery"
            bool voltage = data.getInt(); // Get battery value from result. 11350 → 11.350V
            Serial.print("Battery voltage = ");
            Serial.println(voltage);
        }
    }
    void setup() {
        module.attachOnData(onModuleData); // Register onModuleData function
        module.subscribe("battery"); // Subscribe to receive any state change of battery
    }
    ```
***

### RGB lights

#### rgbAll/totem ( )

: Set all RGB leds to Totem colors (blue, yellow, green).  

```arduino
module.write("rgbAll/totem"); // Light all RGB leds to Totem colors
```

#### rgbAll/reset ( )

: Reset all RGB leds to default color.  
Sets to color configured in `cfg/robot/color` command. If color is not configured, defaults to Totem colors.  

```arduino
module.write("rgbAll/reset"); // Light all RGB leds to default color
```

#### rgbA&nbsp;(&nbsp;`alpha`&nbsp;,&nbsp;`red`&nbsp;,&nbsp;`green`&nbsp;,&nbsp;`blue`&nbsp;)

#### rgbB&nbsp;(&nbsp;`alpha`&nbsp;,&nbsp;`red`&nbsp;,&nbsp;`green`&nbsp;,&nbsp;`blue`&nbsp;)

#### rgbC&nbsp;(&nbsp;`alpha`&nbsp;,&nbsp;`red`&nbsp;,&nbsp;`green`&nbsp;,&nbsp;`blue`&nbsp;)

#### rgbD&nbsp;(&nbsp;`alpha`&nbsp;,&nbsp;`red`&nbsp;,&nbsp;`green`&nbsp;,&nbsp;`blue`&nbsp;)

: Set individual A, B, C, D RGB light color.  
_Parameter:_  
`alpha` - brightness [`0`:`255`].  
`red` - amount of red color [`0`:`255`].  
`green` - amount of green color [`0`:`255`].  
`blue` - amount of blue color [`0`:`255`].  

=== "Decimal"
    ```arduino
    module.write("rgbA", 255, 0, 255, 0); // Set RGB A led to GREEN color
    ```
=== "Hexdecimal"
    ```arduino
    module.write("rgbA", 0xFF, 0, 0xFF, 0); // Set RGB A led to GREEN color
    ```
=== "Inline HEX"
    ```arduino
    module.write("rgbA", 0xFF00FF00); // Set RGB A led to GREEN color
    ```

#### rgbAll&nbsp;(&nbsp;`alpha`&nbsp;,&nbsp;`red`&nbsp;,&nbsp;`green`&nbsp;,&nbsp;`blue`&nbsp;)

: Set all RGB lights color.  
_Parameter:_  
`alpha` - brightness [`0`:`255`].  
`red` - amount of red color [`0`:`255`].  
`green` - amount of green color [`0`:`255`].  
`blue` - amount of blue color [`0`:`255`].  

=== "Decimal"
    ```arduino
    module.write("rgbAll", 255, 0, 255, 0); // Set all leds to GREEN color
    ```
=== "Hexdecimal"
    ```arduino
    module.write("rgbAll", 0xFF, 0, 0xFF, 0); // Set all leds to GREEN color
    ```
=== "HEX color code"
    ```arduino
    module.write("rgbAll", 0xFF00FF00); // Set all leds to GREEN color
    ```

### Auxiliary functions

#### functionA ( *`any`* )

#### functionB ( *`any`* )

#### functionC ( *`any`* )

#### functionD ( *`any`* )

: Auxiliary commands for custom implementations.  
Mainly used to execute some programmed action from a smartphone. Device calls this command with any value and RoboBoard X4 intercepts it using [`TotemApp.addEvent()`](../roboboard/api/totemapp.md#receive-events).  

***

## Commands list

These are low level TotemBUS commands accepted by module. Is not required when using objective API described above.

| Command | Parameters | Description |
| ------- | ---------- | ----------- |
| `driver/version` | _Returns:_(`int`) | Get driver firmware version |
| `cfg/robot/name` | (`string`) | Set robot name |
| `cfg/robot/model` | (`int`) | Set robot model hash |
| `cfg/robot/color` | (`int`) | Set robot appearance color  |
| `cfg/motorABCD/invert` | (`bool`, `bool`, `bool`, `bool`) | Set DC A|B|C|D channels invert |
| `cfg/motorABCD/autobrake` | (`byte`, `byte`, `byte`, `byte`) | Set DC A|B|C|D channels autobrake |
| `cfg/reset` | _None_ | Reset to default configuration |
| `motorABCD` | (`byte`, `byte`, `byte`, `byte`) | Set each DC channel spin direction and power |
| `motorA` | (`byte`) | Set DC A channel spin direction and power |
| `motorB` | (`byte`) | Set DC B channel spin direction and power |
| `motorC` | (`byte`) | Set DC C channel spin direction and power |
| `motorD` | (`byte`) | Set DC D channel spin direction and power |
| `motorABCD/brake` | (`byte`, `byte`, `byte`, `byte`) | Set each DC channel brake power |
| `motorA/brake` | (`byte`) | Set DC A channel brake power |
| `motorB/brake` | (`byte`) | Set DC B channel brake power |
| `motorC/brake` | (`byte`) | Set DC C channel brake power |
| `motorD/brake` | (`byte`) | Set DC D channel brake power |
| `servoABC` | (`byte`, `byte`, `byte`) | Set each servo channel position |
| `servoA` | (`byte`) | Set servo A channel position |
| `servoB` | (`byte`) | Set servo B channel position |
| `servoC` | (`byte`) | Set servo C channel position |
| `led` | (`bool`) | Set LED state on / off |
| `button` | _Returns:_(`bool`) | Is button pressed |
| `battery` | _Returns:_(`int`) | Get battery voltage |
| `rgbAll/totem` | _None_ | Set RGB to "Totem" color |
| `rgbAll/reset` | _None_ | Reset RGB to startup color |
| `rgbAll` | (`byte`, `byte`, `byte`, `byte`) | Set RGB color |
| `rgbA` | (`byte`, `byte`, `byte`, `byte`) | Set RGB A color |
| `rgbB` | (`byte`, `byte`, `byte`, `byte`) | Set RGB B color |
| `rgbC` | (`byte`, `byte`, `byte`, `byte`) | Set RGB C color |
| `rgbD` | (`byte`, `byte`, `byte`, `byte`) | Set RGB D color |
| `functionA` | _Any_ | Write custom user function A |
| `functionB` | _Any_ | Write custom user function B |
| `functionC` | _Any_ | Write custom user function C |
| `functionD` | _Any_ | Write custom user function D |
