---
icon: material/rotate-orbit
---

# IMU

![RoboBoard IMU sensor](../../assets/images/roboboard/roboboard-orientation.png){style="margin-top:-90px"}

Control interface for integrated RoboBoard I2C accelerometer and gyroscope sensor.

```arduino
#include <Wire.h>
void setup() {
  Wire.begin(); // Start I2C interface (for communication with sensor)
  auto result = IMU.read();
}
```

IMU sensor (based on MEMS technology) measures movement in 3 acceleration axis and 3 rotation axis. Allows to determine board movement, position, rotation angles.  
Sensor is sharing I2C `Wire` (SDA, SCL pins) with [Qwiic port](gpio-qwiic.md#qwiic-port), so it acts as additional module on the bus. Can be also used with third-party library.  

!!! tip
    In order to use sensor, you must enable I2C by calling `Wire.begin()` inside `setup()` function. Otherwise `IMU` functionality will not work.

_Only raw sensor measurements available. Advanced orientation algorithms not yet implemented._

## Code snippets

```arduino
#include <Wire.h> // Required for "Wire.begin"
void setup() {
  // Start I2C at 400kHz
  // Calling "Wire.begin()" will start in 100kHz mode
  // SDA, SCL - pins connected to IMU and Qwiic port
  Wire.begin(SDA, SCL, 400000); // Required for "IMU"
}
void loop() {
  auto result = IMU.read(); // Store measurements in "result"
  Serial.println(result.getX_G()); // Read unit from "result"
  delay(100); // Wait 100ms for next read
}
```

## Functions

### :material-download-network: Read sensor

Called `IMU.read()` function will sample measurements from IMU sensor and return in a single `IMUData` object. It contains functions to read data in different measurement units without require for additional conversions.  

<h4 class="apidec" id="begin">
<span class="object">IMU</span>.<span class="function">begin</span>()
<a class="headerlink" href="#begin" title="Permanent link">¶</a></h4>
: Initialize IMU sensor. Function `IMU.read()` calls this internally, so it's only useful if need to prepare sensor before first read operation.

<h4 class="apidec" id="read">
<code>IMUData</code> <span class="object">IMU</span>.<span class="function">read</span>()
<a class="headerlink" href="#read" title="Permanent link">¶</a></h4>
: Read measurements from IMU sensor.  
Returns structure with accelerometer, gyroscope and temperature data.  
_Note: `Wire.begin()` must be called in order for `IMU` to work!_  
**Returns:**  
`IMUData` - object with sensor data. Read [Access data](#access-data) for more info.

### :material-rotate-orbit: Access data

Available units:

- Gravitational force (G)
- Acceleration in meters per squared seconds (m/s^2^)
- Rotation speed in degrees per second (dps)
- Rotation speed in radians per second (rad/s)
- Rotation speed in rounds per minute (RPM).
- Orientation in angle degrees (°)

**Accelerometer:**

<h4 class="apidec" id="getX_G">
<code>unit</code> <span class="group">result</span>.<span class="function">getX_G</span>()
<a class="headerlink" href="#getX_G" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getY_G">
<code>unit</code> <span class="group">result</span>.<span class="function">getY_G</span>()
<a class="headerlink" href="#getY_G" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getZ_G">
<code>unit</code> <span class="group">result</span>.<span class="function">getZ_G</span>()
<a class="headerlink" href="#getZ_G" title="Permanent link">¶</a></h4>
: Get gravitational force (G) measurement of X, Y, Z axis.  
**Returns:**  
`unit` - (float) gravitational force [`-16.0`, `16.0`]G.

<h4 class="apidec" id="getX_mss">
<code>unit</code> <span class="group">result</span>.<span class="function">getX_mss</span>()
<a class="headerlink" href="#getX_mss" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getY_mss">
<code>unit</code> <span class="group">result</span>.<span class="function">getY_mss</span>()
<a class="headerlink" href="#getY_mss" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getZ_mss">
<code>unit</code> <span class="group">result</span>.<span class="function">getZ_mss</span>()
<a class="headerlink" href="#getZ_mss" title="Permanent link">¶</a></h4>
: Get acceleration in meters per second squared of X, Y, Z axis.  
**Returns:**  
`unit` - (float) [`-157.0`, `157.0`]m/s^2^.  

**Gyroscope:**

<h4 class="apidec" id="getX_dps">
<code>unit</code> <span class="group">result</span>.<span class="function">getX_dps</span>()
<a class="headerlink" href="#getX_dps" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getY_dps">
<code>unit</code> <span class="group">result</span>.<span class="function">getY_dps</span>()
<a class="headerlink" href="#getY_dps" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getZ_dps">
<code>unit</code> <span class="group">result</span>.<span class="function">getZ_dps</span>()
<a class="headerlink" href="#getZ_dps" title="Permanent link">¶</a></h4>
: Get rotation in degrees per second of X, Y, Z axis.  
**Returns:**  
`unit` - (float) [`-2000.0`, `2000.0`]dps.  

<h4 class="apidec" id="getX_rads">
<code>unit</code> <span class="group">result</span>.<span class="function">getX_rads</span>()
<a class="headerlink" href="#getX_rads" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getY_rads">
<code>unit</code> <span class="group">result</span>.<span class="function">getY_rads</span>()
<a class="headerlink" href="#getY_rads" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getZ_rads">
<code>unit</code> <span class="group">result</span>.<span class="function">getZ_rads</span>()
<a class="headerlink" href="#getZ_rads" title="Permanent link">¶</a></h4>
: Get rotation in radians per second of X, Y, Z axis.  
**Returns:**  
`unit` - (float) [`-35.0`, `35.0`]rad/s.  

<h4 class="apidec" id="getX_rpm">
<code>unit</code> <span class="group">result</span>.<span class="function">getX_rpm</span>()
<a class="headerlink" href="#getX_rpm" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getY_rpm">
<code>unit</code> <span class="group">result</span>.<span class="function">getY_rpm</span>()
<a class="headerlink" href="#getY_rpm" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getZ_rpm">
<code>unit</code> <span class="group">result</span>.<span class="function">getZ_rpm</span>()
<a class="headerlink" href="#getZ_rpm" title="Permanent link">¶</a></h4>
: Get rotation in rounds per minute of X, Y, Z axis.  
**Returns:**  
`unit` - (float) [`-333.0`, `333.0`]RPM.  

**Temperature:**

Internal sensor / board temperature (not a room temperature!).

<h4 class="apidec" id="getTempC">
<code>unit</code> <span class="group">result</span>.<span class="function">getTempC</span>()
<a class="headerlink" href="#getTempC" title="Permanent link">¶</a></h4>
: Get IMU sensor temperature in Celsius.  
**Returns:**  
`unit` - (float) temperature `25.0` °C.  

<h4 class="apidec" id="getTempF">
<code>unit</code> <span class="group">result</span>.<span class="function">getTempF</span>()
<a class="headerlink" href="#getTempF" title="Permanent link">¶</a></h4>
: Get IMU sensor temperature in Fahrenheit.  
**Returns:**  
`unit` - (float) temperature `77.0` °F.  

**Orientation:**

Accelerometer based orientation measurements (susceptible to shaking).

<h4 class="apidec" id="getOrientX">
<code>unit</code> <span class="group">result</span>.<span class="function">getOrientX</span>()
<a class="headerlink" href="#getOrientX" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="getOrientY">
<code>unit</code> <span class="group">result</span>.<span class="function">getOrientY</span>()
<a class="headerlink" href="#getOrientY" title="Permanent link">¶</a></h4>
: Board orientation in space of X and Y axis.  
**Returns:**  
`unit` - (float) orientation [`-180.0`:`180.0`] degrees.  

<h4 class="apidec" id="getRoll">
<code>unit</code> <span class="group">result</span>.<span class="function">getRoll</span>()
<a class="headerlink" href="#getRoll" title="Permanent link">¶</a></h4>
: Get roll estimation.  
**Returns:**  
`unit` - (float) roll [`-180.0`:`180.0`] degrees.  

<h4 class="apidec" id="getPitch">
<code>unit</code> <span class="group">result</span>.<span class="function">getPitch</span>()
<a class="headerlink" href="#getPitch" title="Permanent link">¶</a></h4>
: Get pitch estimation.  
**Returns:**  
`unit` - (float) pitch [`-90.0`:`90.0`] degrees.  

### :material-wrench-cog: Configure sensor

<h4 class="apidec" id="setAccelRange">
<span class="object">IMU</span>.<span class="function">setAccelRange</span>(<code>range</code>)
<a class="headerlink" href="#setAccelRange" title="Permanent link">¶</a></h4>
: Configure accelerometer measurement range (in gravitational force).  
Can be lowered if higher precision is required.  
**Parameter:**  
`range` - max (G) range [`2`, `4`, `8`, `16`]. Default: 16.

<h4 class="apidec" id="setGyroRange">
<span class="object">IMU</span>.<span class="function">setGyroRange</span>(<code>range</code>)
<a class="headerlink" href="#setGyroRange" title="Permanent link">¶</a></h4>
: Configure gyroscope measurement range (in degrees per second).  
Can be lowered if higher precision is required.  
**Parameter:**  
`range` - max (dps) range [`250`, `500`, `1000`, `2000`]. Default 2000.

<h4 class="apidec" id="getAccelRange">
<code>range</code> <span class="object">IMU</span>.<span class="function">getAccelRange</span>()
<a class="headerlink" href="#getAccelRange" title="Permanent link">¶</a></h4>
: Get configured accelerometer measurement range (in gravitational force).  
Will return exact value configured in sensor.  
**Returns:**  
`range` - max (G) range.

<h4 class="apidec" id="getGyroRange">
<code>range</code> <span class="object">IMU</span>.<span class="function">getGyroRange</span>()
<a class="headerlink" href="#getGyroRange" title="Permanent link">¶</a></h4>
: Get configured gyroscope measurement range (in degrees per second).  
Will return exact value configured in sensor.  
**Returns:**  
`range` - max (dps) range.

<h4 class="apidec" id="getName">
<code>string</code> <span class="object">IMU</span>.<span class="function">getName</span>()
<a class="headerlink" href="#getName" title="Permanent link">¶</a></h4>
: Get name of installed IMU sensor. Different variants exist in RoboBoard.  
**Returns:**  
`string` - name of IMU sensor.

<h4 class="apidec" id="getI2CAddr">
<code>number</code> <span class="object">IMU</span>.<span class="function">getI2CAddr</span>()
<a class="headerlink" href="#getI2CAddr" title="Permanent link">¶</a></h4>
: Get I2C address of installed IMU sensor. Different variants exist in RoboBoard.  
**Returns:**  
`number` - I2C address of IMU sensor.

<h4 class="apidec" id="isI2CAddr">
<code>state</code> <span class="object">IMU</span>.<span class="function">isI2CAddr</span>(<code>address</code>)
<a class="headerlink" href="#isI2CAddr" title="Permanent link">¶</a></h4>
: Check if specified `address` equal to installed IMU sensor.  
**Parameter:** `address` - I2C address.  
**Returns:** `state` - `true` if address match.  

## :fontawesome-solid-microchip: Sensor variants

Each RoboBoard contains different IMU sensor. See table below for part number and I2C address. These parameters can be also acquired dynamically using `IMU.getName()` and `IMU.getI2CAddr()` functions.

| RoboBoard X3 v3.x | RoboBoard X4 v1.0 | RoboBoard X4 v1.1 |
| :-: | :-: | :-: |
| QMI8658 | LSM6DS3 | ICM-20689 |
| 0x6B | 0x6A | 0x68 |
