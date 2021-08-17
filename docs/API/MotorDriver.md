# MotorDriver

*[dead-zone]: A range of power too small to spin a motor

This class allows to define robot wheel configuration and make required calculations in order to move or turn to a certain direction. It makes robot driving more predictable and hides inconveniences of controlling raw motor channels.  
Each robot has a different wheel configuration and motors specifications. Driver should be configured with these parameters before use.  
Supports up to 4 DC motors and 3 servo motors.

## Functions list

| Result | Function | Description |
| :----- | :------- | :---------- |
|  | [MotorDriver()](#motordriver_1) | Create object |
|  | [MotorDriver(`singleCmd`)](#motordriversinglecommand) | Create object with settings |
| _none_ | [setTurnIntensity(`intensity`)](#setturnintensityintensity) | Set turn intensity |
| _none_ | [addFrontLeft(`command`, `min`, `max`, `inv`)](#addfrontleftcommand-minpower-maxpower-inverted) | Configure front left wheel |
| _none_ | [addFrontRight(`command`, `min`, `max`, `inv`)](#addfrontrightcommand-minpower-maxpower-inverted) | Configure front right wheel |
| _none_ | [addRearLeft(`command`, `min`, `max`, `inv`)](#addrearleftcommand-minpower-maxpower-inverted) | Configure rear left wheel |
| _none_ | [addRearRight(`command`, `min`, `max`, `inv`)](#addrearrightcommand-minpower-maxpower-inverted) | Configure rear right wheel |
| _none_ | [addServo(`channel`, `cmd`, `min`, `center`, `max`, `inv`)](#addservochannel-command-minpos-centerpos-maxpos-inverted) | Configure servo motor |
| _none_ | [moveServo(`channel`, `position`)](#moveservochannel-position) | Move servo arm to position |
| _none_ | [move(`drive`, `turn`)](#movedrive-turn) | Set drive and turn power to wheels |
| _none_ | [brake(`fl`, `fr`, `rl`, `rr`)](#brakefrontleft-frontright-rearleft-rearright) | Set braking power to certain wheels |
| _none_ | [brakeAll(`power`)](#brakeallpower) | Set braking power to all wheels |
| _none_ | [brakeRear(`power`)](#brakerearpower) | Set braking power to rear wheels |
| _none_ | [brakeFront(`power`)](#brakefrontpower) | Set braking power to front wheels |
| _none_ | [setModule(`number`, `serial`)](#setmodulenumber-serial) | Set module number and serial |
| [`TotemModule`](/API/TotemModule) | [getModule()](#totemmodule-getmodule) | Get TotemModule object |

## Example

```arduino
#include <Totem.h>
// Define use of class
MotorDriver driver;
// Arduino setup
void setup() {
  Totem.X4.begin();
  // Configure 4 wheel robot
  // Configure Front Left wheel to motorA with starting power of 10%
  driver.addFrontLeft("motorA", 10, 100, true);
  // Configure Front Right wheel to motorB with starting power of 10%
  driver.addFrontRight("motorB", 10, 100);
  // Configure Rear Left wheel to motorC with starting power of 10%
  driver.addRearLeft("motorC", 10, 100, true);
  // Configure Rear Right wheel to motorD with starting power of 10%
  driver.addRearRight("motorD", 10, 100);
  // Configure servo motor channel 0 to "servoA" command with limits [-90:90]
  driver.addServo(0, "servoA", -90, 0, 90);
  // Set turning intensity to 80%
  driver.setTurnIntensity(80);
}
// Arduino loop
void loop() {
  // Drive forward at 60% power while turning right at 20% power
  driver.move(20, 60);
  driver.moveServo(0, -60); // Set servo to -60 position
  delay(2000); // Wait 2 seconds
  // Drive forward at 20% power while turning left at 70% power
  driver.move(-70, 20);
  delay(2000); // Wait 2 seconds
  // Drive backwards at 50% power withour turning
  driver.move(0, -50);
  driver.moveServo(0, 60); // Set servo to 60 position
  delay(2000); // Wait 2 seconds
}
```

## API

### Constructor

#### MotorDriver()

#### MotorDriver(`singleCommand`) { data-toc-label='MotorDriver(singleCmd)' }

: Create driver object.  
MotorDriver internally writes `motorABCD` command. If individual motor updates are required (`motorA`), pass parameter _false_ `#!arduino MotorDriver(false)` to disable single command mode.  
Disabling single command mode would be required if using one of DC motor channel for other purposes rather than robot wheels.  
_Parameter:_  
`singleCommand` - write command mode [`true`:`false`].  

```arduino
MotorDriver driver; // Create driver object
MotorDriver driver(false); // Create driver with individual command mode
```

***

### Functions

#### setTurnIntensity(`intensity`) { data-toc-label='setTurnIntensity()' }

: Limit turn sensitivity if it's too high. This can be useful when turning (steering) affects driving too much.  
It modifies internal computation and does not change parameters of `move()` function.  
It only affects turning with 2 or 4 static wheels. **Does not affect servo motors**.  
_Parameter:_  
`turn` - turn sensitivity [`0`:`100`]%. _Default:_ 100.  

```arduino
driver.turnIntensity(50); // Set to 50%
```

#### addFrontLeft(`command`, `minPower`, `maxPower`, `inverted`) { data-toc-label='addFrontLeft()' }

#### addFrontRight(`command`, `minPower`, `maxPower`, `inverted`) { data-toc-label='addFrontRight()' }

#### addRearLeft(`command`, `minPower`, `maxPower`, `inverted`) { data-toc-label='addRearLeft()' }

#### addRearRight(`command`, `minPower`, `maxPower`, `inverted`) { data-toc-label='addRearRight()' }

: Configure robot wheel motors.  
By providing settings for each motor, MotorDriver will adjust output power to make predictable movements. Functions can be called multiple times to reconfigure parameters during runtime (e.g. limiting max power).  
`minPower` indicates minimum amount of power required to spin a wheel. This is used to compensate motor dead-zone.  
`maxPower` by default should be set to _100_. It may be lowered if there is a need to limit power of the robot or using lower voltage motors.  
`inverted` used to change motor spin direction by providing [`true`:`false`].  
_Parameter:_  
`command` - command of the motor channel (e.g. `motorA`).  
`minPower` - minimum power required to spin a motor [`0`:`100`]%.  
`maxPower` - maximum allowed power for a motor [`0`:`100`]%.  
`inverted` - invert motor pins [`true`:`false`]. _Default:_ `false`.  

```arduino
// Set front left motor to use with channel motorA and start with power from 20%
driver.addFrontLeft("motorA", 20, 100);
// Set front left right to use with channel motorB and start with power from 20% + invert direction
driver.addFrontRight("motorB", 20, 100, true);
```

#### addServo(`channel`, `command`, `minPos`, `centerPos`, `maxPos`, `inverted`) { data-toc-label='addServo()' }

: Configure servo motor.  
Define parameters for servo motor, to use with this driver. Parameters are saved to specified `channel`. Same number must be used when calling `moveServo()`. It can be any digit in range [`0`:`2`].  
Function can be called multiple times to reconfigure parameters during runtime (e.g. changing motor limits).  
Position [`-100`:`100`]% set by `moveServo()` will be mapped to `minPos`, `maxPos` values.  
_Parameter:_  
`channel` - number of servo channel [`0`:`2`].  
`command` - command of the motor channel (e.g. `servoA`).  
`minPos` - minimum allowed position for servo motor to spin (limit) [`-100`:`100`]%.  
`centerPos` - center position offset for servo motor on value `0` [`-100`:`100`]%.  
`maxPower` - maximum allowed position for servo motor to spin (limit) [`-100`:`100`]%.  
`inverted` - invert servo motor spin direction [`true`:`false`]. _Default:_ `false`.  

```arduino
// Configure servo motor to channel 0
// With command "servoA"
// Min pos: 10, center pos: 55, max pos: 94
driver.addServo(0, "servoA", 10, 55, 94);
// Configure servo motor to channel 1
// With command "servoB"
// Min pos: -70, center pos: 50, max pos: 90
// Invert spin direction
driver.addServo(1, "servoB", -70, 50, 90, true);
// Spin motor 0 to position -100. Motor internally is set to value 10
driver.moveServo(0, -100);
// Spin motor 1 to position -100. Motor internally is set to value 90
driver.moveServo(1, -100);
```

#### moveServo(`channel`, `position`) { data-toc-label='moveServo()' }

: Move servo motor to desired position.  
_Parameter:_  
`channel` - number of configured servo channel [`0`:`2`].  
`position` - motor position [`-100`:`100`]%. `0` - center.

```arduino
driver.moveServo(0, 50); // Spin motor 0 to position 50
driver.moveServo(1, -6); // Spin motor 1 to position -6
```

#### move(`drive`, `turn`) { data-toc-label='move()' }

: Set robot move parameters that will be converted to motor output.  
This function call will write configured motor channels.  
`turn` parameter will spin wheels in opposite direction.  
_Parameter:_  
`drive` - power of forward/backward direction [`-100`:`100`]%.  
`turn` - power of turning left/right [`-100`:`100`]%. _Default:_ `0`.  

```arduino
driver.move(50); // Drive forward at 50% power
driver.move(-20); // Drive backward at 20% power
driver.move(0, 50); // Turn right at 50% power
driver.move(0, -30); // Turn left at 30% power
driver.move(100, -30); // Drive forward at 100% power while turning left at 30% power
driver.move(-80, 90); // Drive backward at 80% power while turning right at 90% power
```

#### brake(`frontLeft`, `frontRight`, `rearLeft`, `rearRight`) { data-toc-label='brake()' }

: This will apply electronic brake for modules supporting this feature. Braking power can be set per each wheel.  
If `move()` and `brake()` functions parameters are non-zero, driver will cut power to motors only if braking power is higher than motor spin power. Otherwise - motor spin power is lowered as if real brake pads are pressing spinning wheel. Setting full braking power will always stop motor completely.  
_Parameter:_  
`power` - braking power [`0`:`100`]%.  

```arduino
driver.brake(100, 0, 0, 0); // Brake front left wheel at 100%
driver.brake(0, 0, 80, 80); // Brake rear wheels at 80%
driver.brake(0, 0, 0, 0); // None of wheels are braking
// Can be used with motor spinning power
driver.move(100); // Set wheels spin at 100% power
driver.brake(60, 60, 60, 60); // Will slow down wheels spin by applying 60% of brake power
```

#### brakeAll(`power`) { data-toc-label='brakeAll()' }

: Apply electronic brake `power` to all wheels.  
_Parameter:_  
`power` - braking power [`0`:`100`]%.  

```arduino
driver.brakeAll(100); // Brake all wheels at 100%
```

#### brakeRear(`power`) { data-toc-label='brakeRear()' }

: Apply electronic brake `power` to rear wheels.  
_Parameter:_  
`power` - braking power [`0`:`100`]%.  

```arduino
driver.brakeRear(100); // Brake only rear wheels at 100% (handbrake)
```

#### brakeFront(`power`) { data-toc-label='brakeFront()' }

: Apply electronic brake `power` to front wheels.  
_Parameter:_  
`power` - braking power [`0`:`100`]%.  

```arduino
driver.brakeFront(100); // Brake only front wheels at 100%
```

#### setModule(`number`, `serial`) { data-toc-label='setModule()' }

: MotorDriver contains [TotemModule](/API/TotemModule) to write motor output values. This function allows to configure exact module to communicate with. By default it's set to 00 (all modules). Same as [setNumber(`number`)](/API/TotemModule/#setnumbernumber) and [setSerial(`serial`)](/API/TotemModule/#setserialserial).  
_Parameter:_  
`number` - module number [`0`:`255`]. `0` - all modules.  
`serial` - module serial [`0`:`32767`]. `0` - ignore serial.  

```arduino
driver.setModule(04); // Send move() updates to 04 modules only
driver.setModule(04, 21015); // Send move() updates to 04 module with serial 21015 only
```

#### [`TotemModule`](/API/TotemModule) getModule() { data-toc-label='getModule()' }

: MotorDriver contains [TotemModule](/API/TotemModule) to write motor output values. This function returns it to use for other purposes. Allows to reuse already existing TotemModule instance without creating a new one.  
_Returns:_ [TotemModule](/API/TotemModule).  

```arduino
int voltage = driver.getModule().readWait("battery").getInt(); // Read battery using returned TotemModule
```
