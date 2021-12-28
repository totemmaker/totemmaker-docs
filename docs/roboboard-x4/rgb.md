# RGB LED control

## Description

API to control 4 separate RGB LED located at the bottom of RoboBoard X4. By default it shines "Totem" colors. It can be changed from smartphone app to customize robot appearance or controlled using API to show different colors or animations.

Multiple interfaces available:  
- [`X4.rgb`](#all-led-control) - Specific functions affecting all (A, B, C, D) LEDs.  
- [`X4.rgbA`](#single-led-control), [`X4.rgbB`](#single-led-control), [`X4.rgbC`](#single-led-control), [`X4.rgbD`](#single-led-control) - Functions affecting individual LEDs.  

***

## All LED control

Functions using `X4.rgb` interface will affect all (A, B, C, D) LEDs at once.  

#### X4.rgb.colorTotem() { data-toc-label='colorTotem()' }
: Set to "Totem" color.  

#### X4.rgb.color(`alpha`, `red`, `green`, `blue`) { data-toc-label='color()' }
: Set LEDs color. It consists of brightness and amount of red, green, blue colors.  
**Alternatives:**  
`color(red, green, blue)` - alpha default to `255` (max)  
`color(alpha, hex)` - use 24-bit HEX color code.  
`color(hex)` - use 32-bit HEX color code  
HEX (hexadecimal) color code `0xFFFFFF` is similar to HTML color code `#FFFFFF`.  
24-bit: `0xAABBCC` -> `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
32-bit: `0xFFAABBCC` -> `0xFF` (255 alpha), `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
**Parameter:**  
`alpha` - brightness [`0`:`255`]  
`red` - amount of red color [`0`:`255`]  
`green` - amount of green color [`0`:`255`]  
`blue` - amount of blue color [`0`:`255`]  
`hex` - hexadecimal color code [`0x00000000`:`0xFFFFFFFF`]  

#### X4.rgb.on() { data-toc-label='on()' }
: Turn LEDs on to last used color.  

#### X4.rgb.off() { data-toc-label='off()' }
: Turn LEDs off.  

#### X4.rgb.set(`state`) { data-toc-label='set()' }
: Set LEDs to specific state (on / off).  
**Parameter:**  
`state` - state on / off [`HIGH`:`LOW`] or [`true`:`false`]  

#### X4.rgb.toggle() { data-toc-label='toggle()' }
: Toggle LEDs between on / off states.  

#### X4.rgb.reset() { data-toc-label='reset()' }
: Reset to default color ("Totem" or custom startup color configured with `X4.config.setRobotColor()`).  

#### X4.rgb.enable() { data-toc-label='enable()' }
: Enable RGB peripheral. Turn on power.  

#### X4.rgb.disable() { data-toc-label='disable()' }
: Disable RGB peripheral. Turn off power.  

## All LED fade animations

#### X4.rgb.fadeColorTotem() { data-toc-label='fadeColorTotem()' }
: Prepare LEDs fade with "Totem" color.  

#### X4.rgb.fadeColor(`alpha`, `red`, `green`, `blue`) { data-toc-label='fadeColor()' }
: Prepare LEDs fade color. It consists of brightness and amount of red, green, blue colors.  
Will gradually transit to this color when `fadeStart()` is called.  
**Alternatives:**  
`fadeColor(red, green, blue)` - alpha default to `255` (max)  
`fadeColor(alpha, hex)` - use 24-bit HEX color code.  
`fadeColor(hex)` - use 32-bit HEX color code  
HEX (hexadecimal) color code `0xFFFFFF` is similar to HTML color code `#FFFFFF`.  
24-bit: `0xAABBCC` -> `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
32-bit: `0xFFAABBCC` -> `0xFF` (255 alpha), `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
**Parameter:**  
`alpha` - brightness [`0`:`255`]  
`red` - amount of red color [`0`:`255`]  
`green` - amount of green color [`0`:`255`]  
`blue` - amount of blue color [`0`:`255`]  
`hex` - hexadecimal color code [`0x00000000`:`0xFFFFFFFF`]  

#### X4.rgb.fadeStart(`duration`) { data-toc-label='fadeStart()' }
: Start fading animation for all LEDs at once. Will gradually transit to color set with `fadeColor()` function.  
Will also play animations if color was individually set with `X4.rgbA.fadeColor()`.  
**Parameter:**  
`duration` - animation (transition) duration time [`1`:`65535`]ms  

## Single LED control

This API is available for each individual LED `X4.rgbA`, `X4.rgbB`, `X4.rgbC`, `X4.rgbD`.  

#### X4.rgbA.color(`alpha`, `red`, `green`, `blue`) { data-toc-label='color()' }
: Set LED A color. It consists of brightness and amount of red, green, blue colors.  
**Alternatives:**  
`color(red, green, blue)` - alpha default to `255` (max)  
`color(alpha, hex)` - use 24-bit HEX color code.  
`color(hex)` - use 32-bit HEX color code  
HEX (hexadecimal) color code is similar to HTML color code.  
24-bit: `0xAABBCC` -> `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
32-bit: `0xFFAABBCC` -> `0xFF` (255 alpha), `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
**Parameter:**  
`alpha` - brightness [`0`:`255`]  
`red` - amount of red color [`0`:`255`]  
`green` - amount of green color [`0`:`255`]  
`blue` - amount of blue color [`0`:`255`]  
`hex` - hexadecimal color code [`0x00000000`:`0xFFFFFFFF`]  

#### X4.rgbA.on() { data-toc-label='on()' }
: Turn LED A on to last used color.  

#### X4.rgbA.off() { data-toc-label='off()' }
: Turn LED A off.  

#### X4.rgbA.set(`state`) { data-toc-label='set()' }
: Set LED A to specific state (on / off).  
**Parameter:**  
`state` - state on / off [`HIGH`:`LOW`] or [`true`:`false`]  

#### X4.rgbA.toggle() { data-toc-label='toggle()' }
: Toggle LED A between on / off states.  

## Single LED fade animations

#### X4.rgbA.fadeColor(`alpha`, `red`, `green`, `blue`) { data-toc-label='fadeColor()' }
: Prepare LED A fade color. It consists of brightness and amount of red, green, blue colors.  
Will gradually transit to this color when `fadeStart()` is called.  
**Alternatives:**  
`fadeColor(red, green, blue)` - alpha default to `255` (max)  
`fadeColor(alpha, hex)` - use 24-bit HEX color code.  
`fadeColor(hex)` - use 32-bit HEX color code  
HEX (hexadecimal) color code `0xFFFFFF` is similar to HTML color code `#FFFFFF`.  
24-bit: `0xAABBCC` -> `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
32-bit: `0xFFAABBCC` -> `0xFF` (255 alpha), `0xAA` (170 red), `0xBB` (187 green), `0xCC` (204 blue).  
**Parameter:**  
`alpha` - brightness [`0`:`255`]  
`red` - amount of red color [`0`:`255`]  
`green` - amount of green color [`0`:`255`]  
`blue` - amount of blue color [`0`:`255`]  
`hex` - hexadecimal color code [`0x00000000`:`0xFFFFFFFF`]  

#### X4.rgbA.fadeStart(`duration`) { data-toc-label='fadeStart()' }
: Start individual LED A fading animation. Will gradually transit to color set with `fadeColor()` function. Each LED can fade have separate fade animation concurrently.  
**Parameter:**  
`duration` - animation duration time [`1`:`65535`]ms  

## Example

Arduino examples: [RoboBoardX4/RGB](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/RGB){target=_blank}

```arduino
/* All LED control */
// Set all LED to "Totem" colors
X4.rgb.colorTotem();
// Set all LED to blue color
X4.rgb.color(128, 0, 0, 255); // 50% brightness
X4.rgb.color(0, 0, 255);      // 100% brightness
X4.rgb.color(128, 0x0000FF);  // 50% brightness
X4.rgb.color(0x800000FF);     // 50% brightness
// Turn LED on to last used color
X4.rgb.on();
// Turn LED off
X4.rgb.off();
// Set LED to on
X4.rgb.set(HIGH);
X4.rgb.set(true);
// Toggle LED between on / off
X4.rgb.toggle();
// Reset LED to default (startup) color
X4.rgb.reset();
// Enable LED peripheral
X4.rgb.enable();
// Disable LED peripheral
X4.rgb.disable();
```
```arduino
/* All LED fade animations */
// Set fade color to "Totem"
X4.rgb.fadeColorTotem();
// Set fade color to red
X4.rgb.fadeColor(128, 255, 0, 0); // 50% brightness
X4.rgb.fadeColor(255, 0, 0);      // 100% brightness
X4.rgb.fadeColor(128, 0xFF0000);  // 50% brightness
X4.rgb.fadeColor(0x80FF0000);     // 50% brightness
// Start fade animation with duration of 2s
X4.rgb.fadeStart(2000);
```
```arduino
/* Single LED control */
// Set single LED color
X4.rgbA.color(255, 0, 0); // Set LED A to "red"
X4.rgbB.color(0, 255, 0); // Set LED B to "green"
X4.rgbC.color(0, 0, 255); // Set LED C to "blue"
X4.rgbD.color(255, 255, 255); // Set LED D to "white"
// Turn LED B to last used color
X4.rgbB.on();
// Turn LED A off
X4.rgbA.off();
// Set LED C to on
X4.rgbC.set(HIGH);
// Toggle LED D between on / off
X4.rgbD.toggle();
```
```arduino
/* Single LED fade animations */
// Set individual LED fade color
X4.rgbA.fadeColor(0, 0, 255); // Set LED A fade to blue
X4.rgbB.fadeColor(255, 0, 0); // Set LED B fade to red
// Start individual LED fade animation
X4.rgbA.fadeStart(1000); // Fade LED A in 1 second
X4.rgbB.fadeStart(2000); // Fade LED B in 2 seconds
```