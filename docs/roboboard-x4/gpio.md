# GPIO control

## Description

API to control 4 available GPIO pins. They can be used to custom inputs & outputs or event UART, I2C or other communications.

***

## Code examples

**Arduino projects:** [RoboBoardX4/GPIO](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/GPIO){target=_blank}

??? example "Function usage (click to expand)"
    ```arduino
    void setup() {
      pinMode(GPIOA, INPUT);
      pinMode(GPIOB, OUTPUT);
    }

    void loop() {
      // Read state of GPIOA pin
      if (digitalRead(GPIOA) == HIGH) {
        // If GPIO pin is pulled HIGH, set GPIOB to HIGH
        digitalWrite(GPIOB, HIGH);
      }
      else {
        // If GPIO pin is pulled LOW, set GPIOB to LOW
        digitalWrite(GPIOB, LOW);
      }
    }
    ```

***

## Functions

### Controlling GPIO pins

=== "RoboBoard X4 revision v.1.1"

    Pins can be controlled using standard [Arduino framework functions](https://www.arduino.cc/reference/en/){target=_blank}.  
    For pin numbers use `GPIOA`, `GPIOB`, `GPIOC`, `GPIOD` definitions.

    Set GPIO pin output state [digitalWrite()](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalwrite/){target=_blank}:  
    ```arduino
    pinMode(GPIOA, OUTPUT);
    digitalWrite(GPIOA, HIGH);
    ```
    Read GPIO pin input state [digitalRead()](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/){target=_blank}:  
    ```arduino
    pinMode(GPIOA, INPUT);
    int state = digitalRead(GPIOA);
    ```
    Read GPIO pin analog value [analogRead()](https://www.arduino.cc/reference/en/language/functions/analog-io/analogread/){target=_blank}:  
    ```arduino
    int val = analogRead(GPIOA);
    ```
    *Using `analogWrite()` is unavailable at the moment. Will be added in future.*  


=== "RoboBoard X4 revision v.1.0"

    This API is available for each GPIO pin `X4.gpioA`, `X4.gpioB`, `X4.gpioC`, `X4.gpioD`.  

    #### X4.gpioA.digitalWrite(`state`) { data-toc-label='digitalWrite()' }
    : Set GPIO pin A state. Works similar like Arduino function `digitalWrite()`.  
    Calling this function will reconfigure pin to output.  
    **Parameter:**  
    `state` - GND (0V) or VCC (3.3V) [`LOW`:`HIGH`]  

    #### (`state`) X4.gpioA.digitalRead() { data-toc-label='digitalRead()' }
    : Read GPIO pin A state. Works similar like Arduino function `digitalRead()`.  
    Calling this function will reconfigure pin to input.  
    **Returns:**  
    `state` - GND (0V) or VCC (3.3V) [`LOW`:`HIGH`]  

    #### X4.gpioA.analogWrite(`value`) { data-toc-label='analogWrite()' }
    : Set GPIO pin A analog value. Works similar like Arduino function `analogWrite()`.
    Calling this function will reconfigure pin to output.  
    **Parameter:**  
    `value` - analog value [`0`:`20`]. (0, 10, 20) = (0V, 1.65V, 3.3V).

    // Read analog input [0:1023]
    #### (`value`) X4.gpioA.analogRead() { data-toc-label='analogRead()' }
    : Read GPIO pin A analog value. Works similar like Arduino function `analogRead()`.
    Calling this function will reconfigure pin to output.  
    **Returns:**  
    `value` - analog value [0:1023]. (0, 512, 1023) = (0V, 1.65V, 3.3V).

    #### X4.gpioA.pullMode(`mode`) { data-toc-label='pullMode()' }
    : Turn on GPIO pin A pulldown or pullup resistor. Works similar like Arduino function `pinMode(pin, INPUT_PULLUP)`.  
    **Parameter:**  
    `mode` - turn on pulldown or pullup [`LOW`:`HIGH`]. (LOW, HIGH) = (GND (0V), VCC (3.3V)).  
