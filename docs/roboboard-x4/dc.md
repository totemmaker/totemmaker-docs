# DC motor control

## Description

API to control 4 DC motor output channels.

Multiple interfaces available:  
- [`X4.dc`](#all-dc-channels-control) - Specific functions affecting all (A, B, C, D) DC channels.  
- [`X4.dcAB`](#control-dc-channel-group), [`X4.dcCD`](#control-dc-channel-group) - Specific functions affecting only AB or CD channel groups.  
- [`X4.dcA`](#control-single-dc-channel), [`X4.dcB`](#control-single-dc-channel), [`X4.dcC`](#control-single-dc-channel), [`X4.dcD`](#control-single-dc-channel) - Functions affecting individual DC channels.  

***

## Code examples

**Arduino projects:** [RoboBoardX4/DC](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/DC){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    /* All DC channels control */
    // Set all DC outputs to 50% power
    X4.dc.power(50, 50, 50, 50);
    // Brake all DC outputs at 100% power
    X4.dc.brake(100, 100, 100, 100);
    // Invert all DC outputs spin direction
    X4.dc.setInvert(true, true, true, true);
    // Enable automatic braking for all DC outputs
    X4.dc.setAutobrake(50, 50, 100, 100); // 50% for A,B. 100% for C,D.
    // Enable DC peripheral
    X4.dc.enable();
    // Disable DC peripheral
    X4.dc.disable();
    ```
    ```arduino
    /* Control DC channel group */
    // Play 123 Hz tone on A and B DC channels
    X4.dcAB.tone(123); // Play tone
    X4.dcAB.tone(123, 1000); // Play tone for 1 second and stop
    X4.dcAB.tone(0); // Stop tone playing
    tone(0, 123); // Same as "X4.dcAB.tone(123)"
    tone(0, 123, 1000); // Same as "X4.dcAB.tone(123, 1000)"
    // Play 300 Hz tone on C and D DC channels
    X4.dcCD.tone(300); // Play tone
    X4.dcCD.tone(300, 500); // Play tone for 500 milliseconds and stop
    // Set A and B PWM frequency to 200 Hz
    X4.dcAB.setFreq(200);
    // Set A and B power range (resolution) to 300
    X4.dcAB.setPowerRange(300);
    X4.dcA.power(300); // Now power can be set more precisely in range [-300:300]
    // Set A and B channels to individually controllable
    X4.dcAB.setModeIndividual();
    // Set A and B channels to combined controllable
    X4.dcAB.setModeCombined();
    ```
    ```arduino
    /* Control single DC channel */
    // Enable DC A output
    X4.dcA.enable();
    // Disable DC D output
    X4.dcD.disable();
    // Spin DC C output backwards and 50% power
    X4.dcC.power(-50);
    // Brake DC A output at 100% power
    X4.dcA.brake(100);
    // Invert DC C spin direction
    X4.dcC.setInvert(true);
    // Enable automatic brake for DC B channel
    X4.dcB.setAutobrake(true); // Enable to 100% power
    X4.dcB.setAutobrake(70); // To 70% power
    X4.dcB.setAutobrake(0); // Disable
    ```

***

## Functions

### All DC channels control

Functions using `X4.dc` interface will affect all (A, B, C, D) DC channels at once.  

#### X4.dc.power(`powerA`, `powerB`, `powerC`, `powerD`) { #dc.power data-toc-label='power()' }
: Set individual output power for all DC channels at once.  
Positive - spin forward, negative - spin backward, `0` - no power.  
**Parameter:**  
`powerA` - channel A output power [`-100`:`100`]%.  
`powerB` - channel B output power [`-100`:`100`]%.  
`powerC` - channel C output power [`-100`:`100`]%.  
`powerD` - channel D output power [`-100`:`100`]%.  

#### X4.dc.brake(`powerA`, `powerB`, `powerC`, `powerD`) { #dc.brake data-toc-label='brake()' }
: Set individual brake for all DC channels at once.  
If `power()` function was called, brake is automatically removed (set to `0`).  
**Parameter:**  
`powerA` - channel A brake power [`0`:`100`]%.  
`powerB` - channel B brake power [`0`:`100`]%.  
`powerC` - channel C brake power [`0`:`100`]%.  
`powerD` - channel D brake power [`0`:`100`]%.  

#### X4.dc.setInvert(`invertA`, `invertB`, `invertC`, `invertD`) { #dc.setInvert data-toc-label='setInvert()' }
: Invert individual DC motor spin direction for all channels at once.  
**Parameter:**  
`invertA` - channel A inverted spin direction [`true`:`false`]  
`invertB` - channel B inverted spin direction [`true`:`false`]  
`invertC` - channel C inverted spin direction [`true`:`false`]  
`invertD` - channel D inverted spin direction [`true`:`false`]  

#### X4.dc.setAutobrake(`powerA`, `powerB`, `powerC`, `powerD`) { #dc.setAutobrake data-toc-label='setAutobrake()' }
: Set automatic individual brake for all DC channels at once.  
Will automatically apply specified braking power when `power()` is set to `0`.  
Used to prevent motor from free spinning.  
**Parameter:**  
`powerA` - channel A automatic brake power [`0`:`100`]%.  
`powerB` - channel B automatic brake power [`0`:`100`]%.  
`powerC` - channel C automatic brake power [`0`:`100`]%.  
`powerD` - channel D automatic brake power [`0`:`100`]%.  

#### X4.dc.enable() { #dc.enable data-toc-label='enable()' }
: Enable DC peripheral. Turn on power.  

#### X4.dc.disable() { #dc.disable data-toc-label='disable()' }
: Disable DC peripheral. Turn off power.  


### Control DC channel group

This API is available for DC channel groups `X4.dcAB` and `X4.dcCD` only. It will affect both combined (A+B or C+D) channels and can't be separated.  

#### X4.dcAB.tone(`frequency`, `duration`) { #dcxx.tone data-toc-label='tone()' }
: Play tone (sound) by vibrating DC motor. Same as Arduino [`tone()`](https://www.arduino.cc/reference/en/language/functions/advanced-io/tone/){target=_blank} function, but with motors. By setting tone, both motor channels (A and B) will start to output specified frequency. If you want only single channel, disable other one `X4.dcB.disable()`. Function `X4.dcCD.tone()` for channels C and D can be used independently to channels A and B.  
**Alternatives:**  
`X4.dcAB.tone(frequency)` - duration default to `0` (indefinitely)  
`tone(pin, frequency, duration)` - alias to `X4.dcAB.tone(frequency, duration)`.  
`tone(pin, frequency)` - alias to `X4.dcAB.tone(frequency)`.  
Arduino `tone()` is forward to `X4.dcAB.tone()` for easy use with Buzzer example code.  
**Parameter:**  
`frequency` - sound frequency [`0`:`20000`]Hz. `0` - stop.  
`duration` - tone output duration [`0`:`65535`]ms. `0` - indefinitely.  
`pin` - ignored. Any value.  

#### X4.dcAB.setFreq(`frequency`) { #dcxx.setFreq data-toc-label='setFreq()' }
: Set motor group (A and B channels) PWM frequency. By default it's 50Hz. Can be changed if certain applications or motors requires so.  
**Parameter:**  
`frequency` - PWM frequency [`1`:`65535`]Hz.  

#### X4.dcAB.setPowerRange(`power`) { #dcxx.setPowerRange data-toc-label='setPowerRange()' }
: Set motor group maximum power range. By default `power()` function accepts range between `0`% and `100`%. This range can be increased if more precise control if required for a certain application. Values larger than `100` will be accepted by `power()` function.  
**Parameter:**  
`power` - max percentage of power [`0`:`32767`]%.

#### X4.dcAB.setModeIndividual() { #dcxx.setModeIndividual data-toc-label='setModeIndividual()' }
: Switch A and B channel control to individual (default).  

#### X4.dcAB.setModeCombined() { #dcxx.setModeCombined data-toc-label='setModeCombined()' }
: Switch A and B channel control to combined, for output power up to 2 Amps. Single motor should be wired to both ports.  
**Warning: Experimental feature. Proper implementation and documentation is still missing. Do not use with rev 1.0 board as it will short DC channel.**  

### Control single DC channel

This API is available for each DC motor channel `X4.dcA`, `X4.dcB`, `X4.dcC`, `X4.dcD`.  

#### X4.dcA.enable() { #dcx.enable data-toc-label='enable()' }
: Enable DC A channel.  

#### X4.dcA.disable() { #dcx.disable data-toc-label='disable()' }
: Disable DC A channel. Won't be affected by any functions.  

#### X4.dcA.power(`power`) { #dcx.power data-toc-label='power()' }
: Set output power for DC channel A.  
Positive - spin forward, negative - spin backward, `0` - no power.
**Parameter:**  
`power` - channel A output power [`-100`:`100`]%.

#### X4.dcA.brake(`power`) { #dcx.brake data-toc-label='brake()' }
: Set brake for DC channel A.  
If `power()` function was called, brake is automatically removed (set to `0`).  
**Parameter:**  
`power` - channel A brake power [`0`:`100`]%.

#### X4.dcA.setInvert(`invert`) { #dcx.setInvert data-toc-label='setInvert()' }
: Invert DC motor spin direction for channel A.  
**Parameter:**  
`invert` - channel A inverted spin direction [`true`:`false`]  

#### X4.dcA.setAutobrake(`power`) { #dcx.setAutobrake data-toc-label='setAutobrake()' }
: Set automatic brake for DC channel A.  
Will automatically apply specified braking power when `power()` is set to `0`.  
Used to prevent motor from free spinning.  
**Parameter:**  
`power` - channel A automatic brake power [`0`:`100`]% or [`false`:`true`].
