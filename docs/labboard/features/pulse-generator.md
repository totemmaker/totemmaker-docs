# 3. Pulse generator

![Mini Lab LabBoard photo](/assets/images/mini-lab/labboard-pulse-generator-mode.png)

## About

Mode used to generate rectangular waveform with configurable frequency and duty cycle. Can work in infinite mode or output burst of preset number of pulses.

![Mini Lab LabBoard photo](/assets/images/digital-signal.jpg)

## Details

- Output configured signal at **TXD** pin.
- Inactive **TXD** pin state - LOW.
- Period (frequency) up to 65535 µs (between 15 and 1000000 Hz).
- Width (duty cycle) up to 65535 µs (between 15 and 1000000 Hz).
- Output continuos signal.
- Output burst of up to 999999 pulses.

## Controls

Consists of 3 screens to set signal generator parameters and 1 main screen to activate signal output.  

Numerical value can be entered using button combination:

- ++"Left SELECT"++, ++"Middle SELECT"++ - select digit to edit (indicated by a blinking dot).
- ++"SET\+"++, ++"SET\-"++ - change selected digit.
- ++"Right SELECT"++ - jump to next parameter.  

![Mini Lab LabBoard photo](/assets/images/mini-lab/labboard-pulse-generator-mode-entry0.png)

### Pulse period configuration

![Mini Lab LabBoard period configuration](/assets/images/mini-lab/labboard-pulse-generator-mode-entry2.png)

Pulse period (frequency) of the signal. Value can be set between 1 and 65535 microseconds (µs). This translates to 1000000 and ~15 Hertz frequency.  
Microseconds can be converted to frequency using [online converter](https://www.unitjuggler.com/convert-frequency-from-µs(p)-to-Hz.html?val=10){target=_blank} or equation:  
`frequency = 1000000 / microseconds`

### Pulse width configuration

![Mini Lab LabBoard pulse width configuration](/assets/images/mini-lab/labboard-pulse-generator-mode-entry3.png)

Pulse width (duty cycle) of the signal. Value can be set between 1 and 65535 microseconds (µs). This translates to 1000000 and ~15 Hertz frequency.  
**Note: pulse width can't be higher than pulse period!.**  
Microseconds can be converted to frequency using [online converter](https://www.unitjuggler.com/convert-frequency-from-µs(p)-to-Hz.html?val=10){target=_blank} or equation:  
`frequency = 1000000 / microseconds`  
`microseconds = 1000000 / frequency`  
`microseconds = pulse_period_us * duty_cycle / 100`  

Duty cycle is a percentage 0..100% of pulse period. It specifies how long signal is in HIGH state after pulse start.  
For example: If we set 2500µs as pulse period and want 35% time HIGH and 65% time LOW, we need to set pulse width to: `875 = 2500 * 35 / 100` 875µs. It means signal will stay HIGH (⎽/⎺) for 875µs and then LOW (⎺\⎽) for 1625µs.

### Pulse count configuration

![Mini Lab LabBoard pulse count configuration](/assets/images/mini-lab/labboard-pulse-generator-mode-entry4.png)

This mode allows to output burst of pulses (\_|⎺|\_) when button is pressed. Number of pulses can be set between 1 and 999999. Leave 0 if not required.

Example waveform of 3 pulse output: \_\_\_|⎺|\_\_|⎺|\_\_|⎺|\_\_\_

### Control screen

![Mini Lab LabBoard generator control screen](/assets/images/mini-lab/labboard-pulse-generator-mode-entry5.png)

- ++"Left SELECT"++ - start/stop infinite series of pulse generation with current settings. Once active, this is indicated by series of square symbols.
- ++"Middle SELECT"++ - start/stop finite generation of pulses, making number of pulses entered in pulse count (`CCC`). Once finished, indicate value goes back to a single underscore symbol/ When active a single square symbol is shown.
- ++"Right SELECT"++ - (`SEL`) go back to configuration.

Enter mode:

- Select Menu > `PULSE`.
- In [main screen](/labboard/main-screen/) hold ++"Left SELECT"++ for 3 seconds.

Exit mode:

- Open menu and select other mode.

## Example

![Mini Lab LabBoard generator example LED](/assets/images/mini-lab/labboard-pulse-generator-mode-example.png)

1. Enter pulse generation mode, select continuous pulse mode with following parameters:  
Period = 30,000 µs  
Pulse width = 15,000 µs  
2. Enable infinite pulse mode by pressing corresponding mode button.  
3. Connect **TXD** pin to an LED.  
4. Observe that LED blinks, experiment by changing pulse width and see that LED dims or brightens. Now you’ve got a working PWM module.  
