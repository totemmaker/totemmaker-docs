# Board

Access RoboBoard specific features: settings storage, power, appearance, versions.

## Code snippets

```arduino title="View board information"
void setup() {
  // Print Board information
  Serial.printf("RoboBoard X%d\n", Board.getNumber());
  Serial.printf("Revision: %s\n", Board.getRevisionStr());
  if (Board.getNumber() == 4)
    Serial.printf("Driver version: %s\n", Board.getDriverVersionStr());
  Serial.printf("Software version: %s\n", Board.getSoftwareVersionStr());
  Serial.printf("Board name: %s\n", Board.getName());
  Serial.printf("Boot color: 0x%x\n", Board.getColor());
  Serial.printf("USB inserted: %s\n", Board.isUSB()  ? "Yes" : "No");
}
void loop() { }
```

```arduino title="Change appearance in Totem App"
void setup() {
  // Change board display inside Totem App connect menu
  Board.setName("My Robot"); // Display name "My Robot"
  Board.setColor(Color::Red); // Display with red color
}
void loop() { }
```

***

## Functions 

Some functions contain `async` parameter to execute action in "background" with delay of 500ms. This is used to avoid blocking currently running code. By default it is set to `false`.  
_Calling [`settingsErase()`](#settingsErase) may block code until flash operations are executed. Calling [`#!c++ settingsErase(true)`](#settingsErase-async) does not block the code and erase operation is performed in background._

### :material-power: Control

Control specific board features.

<h4 class="apidec" id="restart">
<span class="object">Board</span>.<span class="function">restart</span>()
<a class="headerlink" href="#restart" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="restart-async">
<span class="object">Board</span>.<span class="function">restart</span>(<code>async</code>)
<a class="headerlink" href="#restart-async" title="Permanent link">¶</a></h4>
: Restart ESP32 to reload running program. Same functionality as Reset button.  
**Parameter:**  
`async` - asynchronous restart. Default: `false`.

<h4 class="apidec" id="isUSB">
<code>state</code> <span class="object">Board</span>.<span class="function">isUSB</span>()
<a class="headerlink" href="#isUSB" title="Permanent link">¶</a></h4>
: Check if USB cable is connected to the board.  
**Returns:**  
`state` - is USB plugged in yes / no [`true`:`false`].  

### :frame_photo: Appearance

Functions for customizing certain robot appearance settings. Most important ones - name and color are intended to identify robot when connecting with [Totem App](../../remote-control/app/index.md). Changing color also sets default one for RGB lights during board power up.

<h4 class="apidec" id="setName">
<span class="object">Board</span>.<span class="function">setName</span>(<code>name</code>)
<a class="headerlink" href="#setName" title="Permanent link">¶</a></h4>
: Set board (or robot) name. Visible during Totem App connection.  
_Default: `RoboBoard X3/X4`_.  
**Parameter:**  
  `name` - string (text) up to 30 characters.  

<h4 class="apidec" id="setModel">
<span class="object">Board</span>.<span class="function">setModel</span>(<code>model</code>)
<a class="headerlink" href="#setModel" title="Permanent link">¶</a></h4>
: Set 16-bit appearance identifier (robot model, MiniTrooper, RoboCar, ..).  
_Note: not used in Totem App._  
**Parameter:**  
`model` - 16-bit appearance identifier [`0x0000`:`0xFFFF`]  

<h4 class="apidec" id="setColor-rgb">
<span class="object">Board</span>.<span class="function">setColor</span>(<code>red</code>, <code>green</code>, <code>blue</code>)
<a class="headerlink" href="#setColor-rgb" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="setColor-hex">
<span class="object">Board</span>.<span class="function">setColor</span>(<code>hex</code>)
<a class="headerlink" href="#setColor-hex" title="Permanent link">¶</a></h4>
: Set board color. Displayed upon board start (RGB lights) and Totem App connection.  
_Default: `0`. If 0, [`RGB.colorTotem()`](rgb.md#colorTotem) is set instead._  
**Parameter:**  
:red_circle: `red` - amount of red color [`0`:`255`]  
:green_circle: `green` - amount of green color [`0`:`255`]  
:blue_circle: `blue` - amount of blue color [`0`:`255`]  
:fontawesome-solid-hashtag: `hex` - 24-bit HEX color code [`0x000000`:`0xFFFFFF`].  

<h4 class="apidec" id="getName">
<code>string</code> <span class="object">Board</span>.<span class="function">getName</span>()
<a class="headerlink" href="#getName" title="Permanent link">¶</a></h4>
: Get board name. Visible during Totem App connection.  
**Returns:**  
`string` - configured board name.  

<h4 class="apidec" id="getModel">
<code>number</code> <span class="object">Board</span>.<span class="function">getModel</span>()
<a class="headerlink" href="#getModel" title="Permanent link">¶</a></h4>
: Get 16-bit appearance identifier (robot model, MiniTrooper, RoboCar, ..).  
**Returns:**  
`number` - 16-bit appearance identifier [`0x0000`:`0xFFFF`]  

<h4 class="apidec" id="getColor">
<code>hex</code> <span class="object">Board</span>.<span class="function">getColor</span>()
<a class="headerlink" href="#getColor" title="Permanent link">¶</a></h4>
: Get board color. Displayed upon board start (RGB light) and Totem App connection.  
**Returns:**  
:fontawesome-solid-hashtag: `hex` - 24-bit HEX color code [`0x000000`:`0xFFFFFF`].  

### :material-numeric: Versions

Functions to read hardware and software versions.

<h4 class="apidec" id="getNumber">
<code>number</code> <span class="object">Board</span>.<span class="function">getNumber</span>()
<a class="headerlink" href="#getNumber" title="Permanent link">¶</a></h4>
: Get RoboBoard number (board type X3 or X4).  
**Returns:**  
`number` - board number [`3`:`4`].  

<h4 class="apidec" id="getRevision">
<code>number</code> <span class="object">Board</span>.<span class="function">getRevision</span>()
<a class="headerlink" href="#getRevision" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getRevisionStr">
<code>string</code> <span class="object">Board</span>.<span class="function">getRevisionStr</span>()
<a class="headerlink" href="#getRevisionStr" title="Permanent link">¶</a></h4>
: Get RoboBoard hardware revision (version). Printed on the board in format: _vX.X_.  
**Returns:**  
`number` - revision [`10`,`11`,`30`]. Format: `11` -> `v1.1`.  
`string` - revision (text) [`1.0`,`1.1`,`3.0`].  

<h4 class="apidec" id="getSoftwareVersion">
<code>number</code> <span class="object">Board</span>.<span class="function">getSoftwareVersion</span>()
<a class="headerlink" href="#getSoftwareVersion" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getSoftwareVersionStr">
<code>string</code> <span class="object">Board</span>.<span class="function">getSoftwareVersionStr</span>()
<a class="headerlink" href="#getSoftwareVersionStr" title="Permanent link">¶</a></h4>
: Get RoboBoard software version (Totem Boards core).  
Format: `<arduino>-totem.<build>`  
• _arduino_ - ESP32 Arduino core version.  
• _build_ - Totem API build number.  
**Returns:**  
`number` - 32-bit version value (major | minor | patch | build). Example: `0x02000E01`.  
`string` - version (text). Example: `2.0.14-totem.1`.  

<h4 class="apidec" id="getDriverVersion">
<code>number</code> <span class="object">Board</span>.<span class="function">getDriverVersion</span>() <code style="background:lightBlue">X4 only</code>
<a class="headerlink" href="#getDriverVersion" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getDriverVersionStr">
<code>string</code> <span class="object">Board</span>.<span class="function">getDriverVersionStr</span>() <code style="background:lightBlue">X4 only</code>
<a class="headerlink" href="#getDriverVersionStr" title="Permanent link">¶</a></h4>
: Get RoboBoard X4 co-processor (driver) firmware version.  
**Returns:**  
`number` - version. Format: `150` -> `1.50`.  
`string` - version (text) `1.50`.  

## :material-wrench-cog: Board settings

RoboBoard has some extra features that can be enabled in a few different ways. By default it's disabled to not intervene if using as development board (programming with Arduino).  
Recommended to enable when using robotic kits (like MiniTrooper, RoboCar).  
Some of the settings can be changed in Arduino IDE > Tools menu.  

**Features:**

- On/Off RoboBoard X3 3V3 power
- Force RoboBoard X3 into charging mode when USB-C cable is plugged in
- Display if board restarted due low battery (blink red LED)
- Display battery state of charge during startup (RGB animation)
- Play sound on board power on (motor tone)
- Play sound on Totem App connect / disconnect (motor tone)
- Animate RGB if Totem App is disconnected

**System startup order:**

1. System power on
1. Initial (default) setting values loaded
1. Arduino framework initialized
1. Saved settings ([`settingsSave()`](#settingsSave)) loaded from flash memory
1. Function `initRoboBoard()` is called. Loads Arduino IDE menu settings
1. (X3 only) If [`#!c++ setChargingMode(true)`](#setChargingMode), start charging mode and halt
1. RoboBoard system started
1. [`TotemApp.begin()`](totemapp.md#begin) gets called (if selected in Arduino IDE menu)
1. Function `setup()` is called
1. Start repeat function `loop()`

### Arduino IDE menu

After settings are loaded from flash in step :four:, system overwrites them with ones configured in Arduino IDE > Tools menu (step :five:). Only settings selected as "Enabled" gets turned on. If selection is "Default" - Arduino menu item is ignored and value loaded in step :four: is used.  
Enabled settings will be loaded each time board is powered on and won't be disabled if [`settingsSave()`](#settingsSave) is called. To turn setting off - select "Default" and upload the code again.

### Force override

Step :five: allows to specify setting values that should be used during board initialization in step :seven:.  
To override settings before system initialization - add function `#!c++ void initRoboBoard()`.  
_Note: if function `#!c++ void initRoboBoard()` is added - Arduino IDE menu settings are ignored._

```c++
// Function called after loading configuration from memory
// and before system is initialized
void initRoboBoard() {
  // Configuration is loaded from memory
  Board.setChargingMode(false); // Disable X3 charging mode
  // System initialization is started and setup() is called
}
void setup() {
  // Board.getChargingMode() -> always returns false
}
void loop() {

}
```

### :floppy_disk: Save to memory

Calling [`settingsSave()`](#settingsSave) will remember configured values as default ones and gets loaded each time in step :four:. Used when changing "Board settings" inside Totem App.  

<h4 class="apidec" id="settingsSave">
<span class="object">Board</span>.<span class="function">settingsSave</span>()
<a class="headerlink" href="#save" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="settingsSave-async">
<span class="object">Board</span>.<span class="function">settingsSave</span>(<code>async</code>)
<a class="headerlink" href="#settingsSave-async" title="Permanent link">¶</a></h4>
: Store current configuration to flash.  
_Also called by Totem App board settings dialog._  
**Parameter:**  
`async` - asynchronous save. Default: `false`.

<h4 class="apidec" id="settingsErase">
<span class="object">Board</span>.<span class="function">settingsErase</span>()
<a class="headerlink" href="#settingsErase" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="settingsErase-async">
<span class="object">Board</span>.<span class="function">settingsErase</span>(<code>async</code>)
<a class="headerlink" href="#settingsErase-async" title="Permanent link">¶</a></h4>
: Erase whole configuration stored in flash.  
_Does not reset runtime settings. Only removes ones stored in flash memory.  
After restart - factory default settings will be loaded._  
**Parameter:**  
`async` - asynchronous erase. Default: `false`.

### Function list

<h4 class="apidec" id="setEnable3V3">
<span class="object">Board</span>.<span class="function">setEnable3V3</span>(<code>state</code>) <code style="background:lightBlue">X3 only</code>
<a class="headerlink" href="#setEnable3V3" title="Permanent link">¶</a></h4>
: Set state of RoboBoard X3 [peripheral LDO regulator](../../roboboard-x3/index.md#power-circuit).  
• Allows to disable external components for low power applications.  
_Provides power for: `RGB lights`, `GPIO 3V3 pin`, `Qwiic port`._  
**Parameter:**  
`state` - LDO regulator on / off [`true`:`false`]. Default: `true`.  

<h4 class="apidec" id="setChargingMode">
<span class="object">Board</span>.<span class="function">setChargingMode</span>(<code>state</code>) <code style="background:lightBlue">X3 only</code>
<a class="headerlink" href="#setChargingMode" title="Permanent link">¶</a></h4>
: Enable RoboBoard X3 charging mode.  
• Powers off board when USB-C cable is plugged in.  
• Indicates charging process with RGB lights.  
Not recommended to use when programming with Arduino. It prevents code from executing because USB-C cable will be plugged in.  
**Parameter:**  
`state` - turn on / off [`true`:`false`]. Default: `false`.  

<h4 class="apidec" id="setStatusRGB">
<span class="object">Board</span>.<span class="function">setStatusRGB</span>(<code>state</code>)
<a class="headerlink" href="#setStatusRGB" title="Permanent link">¶</a></h4>
: Enable board status RGB display.  
• Show battery SOC with RGB animation during power on.  
• Blink red LED if board shut down due low battery.  
• Running RGB animation - Totem App not connected.  
• Steady RGB color - Totem App connected.  
**Parameter:**  
`state` - turn on / off [`true`:`false`]. Default: `false`.  

<h4 class="apidec" id="setStatusSound">
<span class="object">Board</span>.<span class="function">setStatusSound</span>(<code>state</code>)
<a class="headerlink" href="#setStatusSound" title="Permanent link">¶</a></h4>
: Enable board status sounds (motor tone).  
• Play pitch up sound when board is powered on.  
• Play pitch up sound - Totem App connected.  
• Play pitch down sound - Totem App disconnected.  
_Note: DC motors has to be connected to emit any sound._  
**Parameter:**  
`state` - Sound is on / off [`true`:`false`]. Default: `false`.  

<h4 class="apidec" id="getEnable3V3">
<code>state</code> <span class="object">Board</span>.<span class="function">getEnable3V3</span>() <code style="background:lightBlue">X3 only</code>
<a class="headerlink" href="#getEnable3V3" title="Permanent link">¶</a></h4>
: Check if RoboBoard X3 3V3 LDO regulator is enabled.  
**Returns:**  
`state` - is enabled [`true`:`false`]. Default: `true`.  

<h4 class="apidec" id="getChargingMode">
<code>state</code> <span class="object">Board</span>.<span class="function">getChargingMode</span>() <code style="background:lightBlue">X3 only</code>
<a class="headerlink" href="#getChargingMode" title="Permanent link">¶</a></h4>
: Check if RoboBoard X3 charging mode is enabled.  
**Returns:**  
`state` - is enabled [`true`:`false`]. Default: `false`.  

<h4 class="apidec" id="getStatusRGB">
<code>state</code> <span class="object">Board</span>.<span class="function">getStatusRGB</span>()
<a class="headerlink" href="#getStatusRGB" title="Permanent link">¶</a></h4>
: Check if board status RGB display is enabled.  
**Returns:**  
`state` - is enabled [`true`:`false`]. Default: `false`.  

<h4 class="apidec" id="getStatusSound">
<code>state</code> <span class="object">Board</span>.<span class="function">getStatusSound</span>()
<a class="headerlink" href="#getStatusSound" title="Permanent link">¶</a></h4>
: Check if board status sounds are enabled.  
**Returns:**  
`state` - is enabled [`true`:`false`]. Default: `false`.  
