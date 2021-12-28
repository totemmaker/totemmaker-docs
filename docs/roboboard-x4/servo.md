# Servo motor control

## Description

API to control 3 servo motor output channels.  

Multiple interfaces available:  
- [`X4.servo`](#all-channels) - Specific functions affecting all (A, B, C) channels.  
- [`X4.servoA`](#single-channel), [`X4.servoB`](#single-channel), [`X4.servoC`](#single-channel) - Functions affecting individual channels.  

***

## All channels

#### X4.servo.pos(`A`, `B`, `C`) { data-toc-label='pos()' }
: Set individual servo position for all channels.  
**Parameter:** `A`, `B`, `C` - servo position [`-100`:`100`]%  

#### X4.servo.angle(`A`, `B`, `C`) { data-toc-label='angle()' }
: Set individual servo angle for all channels.  
**Parameter:** `A`, `B`, `C` - servo angle [`0`:`180`]°  

#### X4.servo.pulse(`A`, `B`, `C`) { data-toc-label='pulse()' }
: Set exact pulse time for each channel. Out of range values can be used if using custom servo motors, supporting different ranges.  
**Parameter:** `A`, `B`, `C` - pulse time [`500`:`2500`]µs (microseconds)  

#### X4.servo.enable() { data-toc-label='enable()' }
#### X4.servo.disable() { data-toc-label='disable()' }
: Enable or disable all servo output channels. If disabled will stop signal generation.  

#### X4.servo.setPeriod(`period`) { data-toc-label='setPeriod()' }
: Set custom servo signal period (default 20000µs (20ms))  
For better explanation, check [ControlPosition.ino](https://github.com/totemmaker/TotemArduinoBoards/blob/master/libraries/TotemX4/examples/SERVO/ControlPosition/ControlPosition.ino){target=_blank} example.  
**Parameter:** `period` - signal window time [`0`:`65535`]µs (microseconds)  

***

## Single channel

This API is available for each servo motor channel `X4.servoA`, `X4.servoB`, `X4.servoC`.

#### X4.servoA.pos(`position`) { data-toc-label='pos()' }
: Set servo position.  
`pos(position, duration)` - slow down position change.  
**Parameter:**  
`position` - servo position [`-100`:`100`]%  
`duration` - time duration for position to change [`0`:`65535`]ms  

#### X4.servoA.angle(`angle`) { data-toc-label='angle()' }
: Set servo angle.  
`angle(angle, duration)` - slow down position change.  
**Parameter:**  
`angle` - servo angle [`0`:`180`]°  
`duration` - time duration for position to change [`0`:`65535`]ms  

#### X4.servoA.pulse(`pulse`) { data-toc-label='pulse()' }
: Set exact pulse time for servo. Out of range values can be used if using custom servo motor, supporting different ranges.  
`pulse(pulse, duration)` - slow down position change.  
**Parameter:**  
`pulse` - pulse time [`500`:`2500`]µs (microseconds)  
`duration` - time duration for position to change [`0`:`65535`]ms  

#### (`position`) X4.servoA.getPos() { data-toc-label='getPos()' }
: Get current servo position. Will return value in real time if `setSpeed` is set or `duration` parameter was used.  
**Returns:**  
`position` - servo position [`-100`:`100`]%  

#### (`angle`) X4.servoA.getAngle() { data-toc-label='getAngle()' }
: Get current servo angle. Will return value in real time if `setSpeed` is set or `duration` parameter was used.  
**Returns:**  
`angle` - servo angle [`0`:`180`]°  

#### (`pulse`) X4.servoA.getPulse() { data-toc-label='getPulse()' }
: Get current servo pulse time. Will return value in real time if `setSpeed` is set or `duration` parameter was used.  
**Returns:**  
`pulse` - pulse time [`500`:`2500`]µs (microseconds)  

***

## Channel config

Some parameters can be configured to change behavior of motor positioning.

#### X4.servoA.enable() { data-toc-label='enable()' }
: Enable servo output channel.  

#### X4.servoA.disable() { data-toc-label='disable()' }
: Disable servo output channel. Will stop signal generation.  

#### X4.servoA.setInvert(`invert`) { data-toc-label='setInvert()' }
: Invert servo spin direction.  
**Parameter:**  
`invert` - invert spin direction [`false`:`true`]  

#### X4.servoA.setSpeed(`rpm`) { data-toc-label='setSpeed()' }
: Set constant servo speed RPM (Rounds-Per-Minute).  
Servo motors typically has maximum speed of 60 RPM, so maximum value is limited by motor capabilities. Setting value `0` will use maximum motor speed.  
**Parameter:**  
`rpm` - motor speed [`0`:`65535`] RPM.  

#### X4.servoA.setSpeedRPH(`rph`) { data-toc-label='setSpeedRPH()' }
: Set constant Servo motor speed RPH (Rounds-Per-Hour). Use when less than 1 RPM speed is required. Servo motors typically has maximum speed of 60 RPM (3600 RPH), so maximum value is limited by motor capabilities. Setting value `0` will use maximum motor speed.  
**Parameter:**  
`rph` - motor speed [`0`:`65535`] PRH  

#### X4.servoA.setPulseMinMax(`min`, `max`) { data-toc-label='setPulseMinMax()' }
: Set channel low & high pulse (microseconds) limits (default `500`, `2500` µs). These values are used when calculating motor position or angle. Can be changed if motor supports different range.  
**Parameter:**  
`min`, `max` - pulse time [`0`:`20000`]µs (microseconds)  

## Example

Arduino examples: [RoboBoardX4/SERVO](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/SERVO){target=_blank}

```arduino
/* All channels */
// Rotate A, B, C servo to left, center, right
X4.servo.pos(-100, 0, 100);
X4.servo.angle(0, 90, 180);
X4.servo.pulse(500, 1500, 2500);
// Enable servo peripheral
X4.servo.enable();
// Disable servo peripheral
X4.servo.disable();
// Modify servo signal PWM period
X4.servo.setPeriod(20000); // Set to default 20000µs
```
```arduino
/* Single channel */
// Rotate servo A
X4.servoA.pos(-65);
X4.servoA.angle(65);
X4.servoA.pulse(650);
// Get current servo position "-65"
X4.servoA.getPos();
// Get current servo angle "65"
X4.servoA.getAngle();
// Get current servo position pulse "650"
X4.servoA.getPulse();
```
```arduino
/* Channel config */
// Enable servo A channel
X4.servoA.enable();
// Disable servo A channel
X4.servoA.disable();
// Invert servo A channel spin direction
X4.servoA.setInvert(true);
// Set servo B channel constant rotation speed
X4.servoB.setSpeed(30); // 30 RPM
X4.servoB.setSpeedRPH(1800); // 1800 RPH -> 30 RPM
// Configure min and max servo pulse to adjust position for pos, angle functions
X4.servoA.setPulseMinMax(500, 2500); // Set to default
```