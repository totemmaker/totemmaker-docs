# Board configuration

## Description

API to change board settings. Any changes saved retain their values after power off.  

***

## Code examples

**Arduino projects:** [RoboBoardX4/CONFIG](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/CONFIG){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    // Set robot name to "My Robot"
    X4.config.setRobotName("My Robot");
    // Read robot name "My Robot"
    char *name = X4.config.getRobotName();
    // Set robot model
    X4.config.setRobotModel("MiniTrooper"); // Set string
    X4.config.setRobotModel(0xe6f1); // Set hash (int)
    // Read model hash "0xe6f1"
    int model = X4.config.getRobotModel();
    // Set robot color "red"
    X4.config.setRobotColor(0xff0000);
    X4.config.setRobotColor(255, 0, 0);
    // Read robot color "0xff0000"
    int color = X4.config.getRobotColor();
    // Set DC motor output invert
    X4.config.setDCInvert(true, true, false, false);
    // Read DC motor output invert "0x01010000"
    int dcInvert = X4.config.getDCInvert();
    // Set DC motor autobrake
    X4.config.setDCAutobrake(50, 0, 100, 100);
    // Read DC motor autobrake "0x32006464"
    int dcAutobrake = X4.config.getDCAutobrake();
    // Reset stored configuration to default
    X4.config.reset();
    ```

***

## Functions

### Settings

#### X4.config.setRobotName(`name`) { #config.setRobotName data-toc-label='setRobotName()' }
: Set Robot name. Will be visible during Totem App connection.  
**Parameter:**  
`name` - string (text) up to 30 characters.  

#### (`name`) X4.config.getRobotName() { #config.getRobotName data-toc-label='getRobotName()' }
: Get Robot name, configured with `setRobotName()` or inside Totem App.  
**Returns:**  
`name` - string (text) up to 30 characters.  

#### X4.config.setRobotModel(`description`) { #config.setRobotModel data-toc-label='setRobotModel()' }
: Set Robot model description. Used to describe robot type `MiniTrooper`, etc.  
This description is internally converted to 16-bit hash value.  
`description` - string (text) of any length.  

#### X4.config.setRobotModel(`hash`) { #config.setRobotModel-hash data-toc-label='setRobotModel(hash)' }
: Set Robot model description hash. Can be acquired with `TotemModule::hashModel("description")`.  
`hash` - 16-bit value hash of model description string (text) [`0x0000`:`0xFFFF`]  

#### (`hash`) X4.config.getRobotModel() { #config.getRobotModel data-toc-label='getRobotModel()' }
: Get Robot model description hash, configured with `setRobotModel()` function.  
**Returns:**  
`hash` - 16-bit value hash of model description string (text) [`0x0000`:`0xFFFF`]  

#### X4.config.setRobotColor(`red`, `green`, `blue`) { #config.setRobotColor data-toc-label='setRobotColor()' }
: Set the Robot appearance color. Used for easier robot identification. Will be displayed during RoboBoard X4 power-on and Totem App connection. No alpha (brightness) is used here. Value `0` sets to default (disabled).  
**Alternatives:**  
`setRobotColor(hex)` - use 24-bit HEX color code  
HEX (hexadecimal) color code `0xFFFFFF` is similar to HTML color code `#FFFFFF`.  
24-bit: `0xAABBCC` → `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
**Parameter:**  
`red` - amount of red color [`0`:`255`]  
`green` - amount of green color [`0`:`255`]  
`blue` - amount of blue color [`0`:`255`]  
`hex` - 24-bit hexadecimal color code [`0x000000`:`0xFFFFFF`]  

#### (`color`) X4.config.getRobotColor() { #config.getRobotColor data-toc-label='getRobotColor()' }
: Get the Robot appearance color, configured with `setRobotColor()` function or inside Totem App.  
`color` - 24-bit hexadecimal color code [`0x000000`:`0xFFFFFF`].  

#### X4.config.setDCInvert(`invertA`, `invertB`, `invertC`, `invertD`) { #config.setDCInvert data-toc-label='setDCInvert()' }
: Set invert of individual DC motor spin direction. Different from `X4.dc.setInvert()` as this function preserves setting after power-off.  
**Parameter:**  
`invertA` - channel A inverted spin direction [`true`:`false`]  
`invertB` - channel B inverted spin direction [`true`:`false`]  
`invertC` - channel C inverted spin direction [`true`:`false`]  
`invertD` - channel D inverted spin direction [`true`:`false`]  

#### (`invert`) X4.config.getDCInvert() { #config.getDCInvert data-toc-label='getDCInvert()' }
: Get invert setting of individual DC channels, configured with `setDCInvert()`.  
`invert` - encoded 32-bit value [A | B | C | D] (8-bit per channel)  

#### X4.config.setDCAutobrake(`powerA`, `powerB`, `powerC`, `powerD`) { #config.setDCAutobrake data-toc-label='setDCAutobrake()' }
: Set automatic brake of individual DC channel.  
Will automatically apply specified braking power when `power()` is set to `0`.  
Used to prevent motor from free spinning.  
Different from `X4.dc.setAutobrake()` as this function preserves setting after power-off.  
**Parameter:**  
`powerA` - channel A automatic brake power [`0`:`100`]%.  
`powerB` - channel B automatic brake power [`0`:`100`]%.  
`powerC` - channel C automatic brake power [`0`:`100`]%.  
`powerD` - channel D automatic brake power [`0`:`100`]%.  

#### (`autobrake`) X4.config.getDCAutobrake() { #config.getDCAutobrake data-toc-label='getDCAutobrake()' }
: Get autobrake setting of individual DC channels, configured with `setDCAutobrake()`.  
`autobrake` - encoded 32-bit value [A | B | C | D] (8-bit per channel)  

#### X4.config.reset() { #config.reset data-toc-label='reset()' }
: Reset all settings to default.  
