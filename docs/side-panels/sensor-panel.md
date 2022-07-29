# Sensor panel

![I/O side panel photo](/assets/images/mini-lab/sensor-side-panel-sketch.svg){ align=right style="height:750px" }

**Contains:**  

* Microphone
* 128×64 OLED display
* DHT11 humidity sensor
* NTC thermistor
* Piezo electric buzzer
* DC motor driver module

## Modules

In the board only the supply power is shared between modules. Otherwise they’re fully isolated from one another, and can be used independently. Logic level for digital signals have a jumper which selects the boards to work either at 3.3 or 5 V logic level. It’s important for the side panel to have the same logic level as the controller board (e.g., TotemDuino), for best results.

### Microphone

This module takes care of all things needed to drive the microphone, and outputs an analog signal in the 0..Vcc range. Suggested uses for this module are reading the value with TotemDuino, or using it as an input to operational amplifiers.

### Display

Screen uses a SSD1306 chip for control. I2C interface with integrated pull-up resistors is used to interface with the driver. Works with both 3.3 or 5 V supply voltages, selected by jumper pin. Refer to the SSD1306 datasheet for interface specifics, or to Totem examples sketch for sample usage of the module. I2C address: 0x3C.

### Humidity sensor

Digital, 16bit humidity and temperature sensor using one wire protocol. Accepted temperature range of -40 to 80 °C with 0.5% accuracy and humidity from 0 to 100% at 2-5% accuracy.

Can be used with 3.3 V or 5 V supply voltages, selected by jumper pin.

### NTC thermistor

Negative temperature coefficient (NTC) thermistor working in voltage divider configuration. At room temperature (25 °C) has 10k resistance. Can be used from -40 to 125 °C temperature.

### Buzzer

An active piezo electric buzzer with transistor driver. Powered from 5V supply voltage. Active logic level in its input activates the buzzer.

### DC motor driver

An h-bridge motor driver powered by BD6220F chip. Can be controlled from integrated buttons and potentiometer or accepts logic level inputs. Can be used in pulse width modulation (PWM) mode for controlling motor speed.

Depending on the input state, motor can receive four different working modes:  

Input A | Input B | Motor state
--- | --- | ---
LOW | LOW | Idle
LOW | HIGH (PWM) | Rotation A
HIGH (PWM) | LOW | Rotation B
HIGH | HIGH | Brake

Driver contains protections against over-current, short-circuit conditions. Jumper **JP2** on the module controls the input reference (maximum voltage applied to the motor) source - when in **PWM control** mode the reference voltage equals supply voltage, when in **Pot Meter Cntrl** mode the reference voltage is controlled by potentiometer.

## Datasheet

Side panel instructions, assembly, mounting and electrical wiring.

<a href="https://totemmaker.net/wp-content/uploads/2018/04/sensor-side-panel-1.1.pdf" class="image fit">sensor-side-panel-1.1.pdf</a>
<object style="width:100%; height:600px;" data="https://totemmaker.net/wp-content/uploads/2018/04/sensor-side-panel-1.1.pdf" type="application/pdf">
    <embed src="https://totemmaker.net/wp-content/uploads/2018/04/sensor-side-panel-1.1.pdf" type="application/pdf" />
</object>
