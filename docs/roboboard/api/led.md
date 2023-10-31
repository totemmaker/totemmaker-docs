# LED

![RoboBoard IMU sensor](../../assets/images/roboboard/roboboard-x4-led.jpg){align=left width=40%}

_RoboBoard X3 does not have dedicated LED. It is emulated trough RGB A._

<p style="clear: both;">Control interface for changing LED state and blinking.<br>
Can be used with internal RoboBoard LED or <a href="#external-gpio-led">external one connected to GPIO</a>.</p>

***

## Code snippets

```c++
// Turn LED on
LED.on();
// Turn LED off
LED.off();
// Toggle LED between on / off
LED.toggle();
// Check if LED is on
bool isOn = LED.isOn();
// Make LED blink (5 times)
LED.blink();
```

***

## Functions

### :material-toggle-switch-outline: Control

<h4 class="apidec" id="on">
<span class="object">LED</span>.<span class="function">on</span>()
<a class="headerlink" href="#on" title="Permanent link">¶</a></h4>
: Turn LED on.  

<h4 class="apidec" id="off">
<span class="object">LED</span>.<span class="function">off</span>()
<a class="headerlink" href="#off" title="Permanent link">¶</a></h4>
: Turn LED off.  

<h4 class="apidec" id="toggle">
<span class="object">LED</span>.<span class="function">toggle</span>()
<a class="headerlink" href="#toggle" title="Permanent link">¶</a></h4>
: Toggle LED between on / off states.  

<h4 class="apidec" id="isOn">
<code>state</code> <span class="object">LED</span>.<span class="function">isOn</span>()
<a class="headerlink" href="#isOn" title="Permanent link">¶</a></h4>
: Get current LED state.  
**Returns:**  
`state` - state on / off [`true`:`false`]  

<h4 class="apidec" id="setState">
<span class="object">LED</span>.<span class="function">setState</span>(<code>state</code>)
<a class="headerlink" href="#setState" title="Permanent link">¶</a></h4>
: Set LED state on / off.  
**Parameter:**  
`state` - on / off [`true`:`false`].

<h4 class="apidec" id="getState">
<code>state</code> <span class="object">LED</span>.<span class="function">getState</span>()
<a class="headerlink" href="#getState" title="Permanent link">¶</a></h4>
: Same as `isOn()`.  
**Returns:**  
`state` - on / off [`true`:`false`].

### :material-alarm-light: Blink

<h4 class="apidec" id="blink">
<span class="object">LED</span>.<span class="function">blink</span>()
<a class="headerlink" href="#blink" title="Permanent link">¶</a></h4>
: Blink LED 5 times.  

<h4 class="apidec" id="blink-count">
<span class="object">LED</span>.<span class="function">blink</span>(<code>count</code>)
<a class="headerlink" href="#blink-count" title="Permanent link">¶</a></h4>
: Blink LED number of times.  
**Parameter:**  
`count` - number of times to blink LED [`1`, `2`, `3`, ...]  

<h4 class="apidec" id="blink-count-time">
<span class="object">LED</span>.<span class="function">blink</span>(<code>count</code>,<code>onTime</code>,<code>offTime</code>)
<a class="headerlink" href="#blink-count-time" title="Permanent link">¶</a></h4>
: Blink LED number of times with custom on and off durations.  
**Parameter:**  
`count` - number of times to blink LED [`1`, `2`, `3`, ...]  
`onTime` - how long LED is ON (time ms) (default 100)  
`offTime` - how long LED is OFF (time ms) (default 200)  

<h4 class="apidec" id="wait">
<code>state</code> <span class="object">LED</span>.<span class="function">wait</span>()
<a class="headerlink" href="#wait" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="wait-time">
<code>state</code> <span class="object">LED</span>.<span class="function">wait</span>(<code>time</code>)
<a class="headerlink" href="#wait-time" title="Permanent link">¶</a></h4>
: Halts code execution until LED stops blinking.  
Contains timeout feature for maximum amount of `time` to wait.  
**Parameter:** `time` - maximum time to wait (ms) for LED blink stop. `0` - disabled.  
**Returns:** `state` - `true` if stopped, `false` if timeout.  

## External GPIO LED

=== "RoboBoard X3"
    ```c++
    IOLED gpioLed(IO33); // LED connected to pin IO33
    gpioLed.on(); // Turn LED on
    ```
=== "RoboBoard X4"
    ```c++
    IOLED gpioLed(GPIOA); // LED connected to pin GPIOA
    gpioLed.on(); // Turn LED on
    ```

`IOLED` can be used for external LED connected to any GPIO pin. It requires pin number and pin state when lit on (for algorithm to know when LED is on). All same functions are available as for `LED` object.

Note: "blink()" can be used only with single LED at a time.

**Constructor:**

<h4 class="apidec" id="IOLED">
<span class="object">IOLED</span>(<code>pin</code>)
<a class="headerlink" href="#IOLED" title="Permanent link">¶</a></h4>
<h4 class="apidec" id="IOLED">
<span class="object">IOLED</span>(<code>pin</code>,<code>onLevel</code>)
<a class="headerlink" href="#IOLED" title="Permanent link">¶</a></h4>
: Create LED control object.  
**Parameter:**  
`pin` - GPIO pin number  
`onLevel` - pin state when LED is on [LOW:HIGH]. Default - HIGH.  


```c++
IOLED led1(IO33); // LED on pin IO33. Pin state HIGH to lit on
IOLED led2(IO33, HIGH); // LED on pin IO33. Pin state HIGH to lit on
IOLED led2(IO33, LOW); // LED on pin IO33. Pin state LOW to lit on
```