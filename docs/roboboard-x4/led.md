# Status LED control

## Description

API to control status LED. By default it indicates state of X4 boot (is on if success, error if blinking).  
When X4 is started this LED can be used for any custom scenario.  

***

## Code examples

**Arduino projects:** [RoboBoardX4/LED](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/LED){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    // Turn LED on
    X4.led.on();
    // Turn LED off
    X4.led.off();
    // Toggle LED between on / off
    X4.led.toggle();
    // Set LED to on
    X4.led.set(HIGH);
    X4.led.set(true);
    // Check if LED is on
    bool isOn = X4.led.isOn();
    // Make LED blink
    X4.led.blink();
    // Make LED blink 10 times
    X4.led.blinkTimes(10);
    // Make LED blink 10 times with 500ms delay between blinks
    X4.led.blinkTimes(10, 500);
    // Make LED blink for 1 second
    X4.led.blinkFor(1000);
    // Make LED blink for 1 second with 300ms delay between blinks
    X4.led.blinkFor(1000, 300);
    ```

***

## Functions

### LED control

#### X4.led.on() { #led.on data-toc-label='on()' }
: Turn LED on

#### X4.led.off() { #led.off data-toc-label='off()' }
: Turn LED off

#### X4.led.toggle() { #led.toggle data-toc-label='toggle()' }
: Toggle LED on / off

#### X4.led.set(`state`) { #led.set data-toc-label='set()' }
: Set LED state.  
**Parameter:**  
`state` - state on / off [`HIGH`:`LOW`] or [`true`:`false`]  

#### (`state`) X4.led.isOn() { #led.isOn data-toc-label='isOn()' }
: Get current LED state.  
**Returns:**  
`state` - state on / off [`HIGH`:`LOW`] or [`true`:`false`]  

#### X4.led.blink() { #led.blink data-toc-label='blink()' }
: Blink LED few times.  

#### X4.led.blinkTimes(`count`) { #led.blinkTimes data-toc-label='blinkTimes()' }
: Blink LED number of times.  
`blinkTimes(count, blinkDuration)` - blink with custom delay between blinks.  
**Parameter:**  
`count` - number of times to blink LED [`1`, `2`, `3`, ...]  
`blinkDuration` - delay time between blinks [`0`, `1000`]ms. Default `200`ms  

#### X4.led.blinkFor(`duration`) { #led.blinkFor data-toc-label='blinkFor()' }
: Blink LED for specified amount of time.  
`blinkFor(count, blinkDuration)` - blink with custom delay between blinks.  
**Parameter:**  
`duration` - amount of time to blink LED [`0`, `65535`]ms  
`blinkDuration` - delay time between blinks [`0`, `1000`]ms. Default `100`ms  
