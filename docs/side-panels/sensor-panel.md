# Sensor panel #2

<blockquote style="text-align:right;margin-top:-20px;margin-right:14px;border-left:0;line-height:0;font-size:9pt;">Click image to jump to specific module</blockquote>

![Sensor side panel photo](/assets/images/mini-lab/sensor-side-panel-sketch.svg){ align=right style="height:750px" usemap="#panel_modules" }

<map name="panel_modules">
  <area shape="rect" coords="0,0,250,40" alt="Power" href="#power">
  <area shape="rect" coords="0,40,250,158" alt="Microphone" href="#microphone">
  <area shape="rect" coords="0,158,250,317" alt="128×64 OLED display" href="#display">
  <area shape="rect" coords="0,317,177,445" alt="DHT11 humidity sensor" href="#humidity-sensor">
  <area shape="rect" coords="177,317,250,445" alt="VCC Select" href="#power">
  <area shape="rect" coords="0,445,250,540" alt="NTC thermistor" href="#ntc-thermistor">
  <area shape="rect" coords="0,540,90,656" alt="Piezo electric buzzer" href="#buzzer">
  <area shape="poly" coords="90,540,250,540,250,750,0,750,0,656,90,656" alt="DC motor driver" href="#dc-motor-driver">
</map>

**Contains:**  

* [Microphone](#microphone)
* [128×64 OLED display](#display)
* [DHT11 humidity sensor](#humidity-sensor)
* [NTC thermistor](#ntc-thermistor)
* [Piezo electric buzzer](#buzzer)
* [DC motor driver](#dc-motor-driver)

**Arduino examples:** [Github](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos){target=_blank}

This side panel is a combination of sensors with display and motor driver as a bonus. Can be used to detect sound, room temperature, visualize data or alert with buzzer. Motor driver allows to control it manually or digitally using TotemDuino.

## Power

On the top of the board there is (POWER IN) pin header to supply power to side panel components. Some of them requires certain voltage to operate, but otherwise they’re fully isolated from one another, and can be used independently.

**Power up side panel:**  
Side panel requires power (3.3V, 5V) for certain modules to work. It does not have its own voltage regulator so typically, regulated voltage has to be sourced from TotemDuino or LabBoard. Recommended to plug in all 3 wires as some modules use different voltages.  
[![Sensor Side panel power](/assets/images/mini-lab/io-panel-power_bb.png)](/assets/images/mini-lab/io-panel-power_bb.png)

![Sensor panel Voltage selector visual](/assets/images/mini-lab/io-panel-voltage-selector_bb.png){ align=right }

**Select logic voltage level:**  
Side panel contains logic level voltage (VCC) select for digital signals to work either at 3.3 or 5 Volt. Marked OBS! (Observe!).  
By moving **JP7** jumper up or down - you can select between 3.3V and 5V. This is useful if you have some components that are 3.3 Volt only (could be damaged if used with 5V). In that case place jumper on 3.3V position and use side panel pins safely. It’s important to have the same logic level as the controller board (e.g., TotemDuino), for best results. 

![Sensor Side panel voltage selector schematic](/assets/images/mini-lab/io-panel-voltage-selector_sh.png){ align=right width=170 }

Places where selected voltage (VCC) is used:  

- Display power and logic
- Humidity sensor power and logic
- NTC reference voltage
- DC motor control buttons

Microphone and buzzer modules are always using **+5v** to power their circuit.

## Microphone

![Sensor side panel microphone visual](/assets/images/mini-lab/sensor-panel-mic_vi.png){ align=right }

Microphone module with integrated amplifier. Outputs analog audio signal in **0**..**5V** range.

**Pin header:**  

- **pin 1, 2** - microphone signal output

**Info:**

- Arduino example: [Sound alarm](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/soundalarm){target=_blank}.

**Power:**  

- Uses **+5v** POWER IN for amplification circuit.

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel microphone schematic](/assets/images/mini-lab/sensor-panel-mic_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-mic_sh.png) | [![TotemDuino sound alarm example](/assets/images/mini-lab/sensor-panel-mic_bb.png){width=370}](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/soundalarm) <br> _Read microphone using TotemDuino._ |


## Display

![Sensor side panel display visual](/assets/images/mini-lab/sensor-panel-display_vi.png){ align=right }

Monochrome I2C display to show text and simple graphics. Uses **SSD1306** driver with **0x3C** I2C address.

**Pin header:**  

- **pin 1** - data signal **SDA**
- **pin 2** - clock signal **SCL**

**Info:**

- Arduino library: [Adafruit SSD1306](https://github.com/adafruit/Adafruit_SSD1306){target=_blank}.
- Arduino example: [Display test](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/displaytest){target=_blank}.
- Integrated pull-up resistors for I2C communication.

**Power:**

- Uses **VCC** to power display. Works with both 3.3 or 5 V supply voltages.

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel display schematic](/assets/images/mini-lab/sensor-panel-display_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-display_sh.png) | [![TotemDuino display example](/assets/images/mini-lab/sensor-panel-display_bb.png){width=370}](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/displaytest) <br> _Control display using TotemDuino._ |


## Humidity sensor

![Sensor side panel humidity visual](/assets/images/mini-lab/sensor-panel-humidity_vi.png){ align=right }

Digital temperature and humidity sensor **DHT11**. Uses **1-wire** protocol to communicate with a microcontroller.

**Pin header:**  

- **pin 1, 2** - 1-wire data signal

**Info:**

- Arduino library: [DHT sensor library](https://github.com/adafruit/DHT-sensor-library){target=_blank}.
- Arduino example: [Temperature and humidity read](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/humidity){target=_blank}.
- Measures temperature in Celsius and humidity in percentage.

**Power:**

- Uses **VCC** to power sensor. Works with both 3.3 or 5 V supply voltages.

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel DHT11 schematic](/assets/images/mini-lab/sensor-panel-dht11_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-dht11_sh.png) | [![LabBoard DHT11 sensor monitor](/assets/images/mini-lab/labboard-dht11-mode-example.png){width=370}](/labboard/features/dht11-monitor/) <br> _Read measurements using LabBoard._ |

## NTC thermistor

![Sensor side panel NTC visual](/assets/images/mini-lab/sensor-panel-ntc_vi.png){ align=right }

Analog temperature sensor.

**Pin header:**  

- **pin 1, 2** - thermistor signal output

**Info:**

- Arduino example: [NTC temperature read](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/ntc){target=_blank}.
- Negative temperature coefficient (NTC) thermistor working in voltage divider configuration. **OUT** voltages goes down if temperature increases.
- At room temperature (25 °C) has 10k resistance.

**Power:**

- Uses **VCC** for reference voltage.

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel NTC schematic](/assets/images/mini-lab/sensor-panel-ntc_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-ntc_sh.png) | [![Sensor side panel NTC experiment](https://raw.githubusercontent.com/totemmaker/arduino-examples/master/mini-lab/sidepanel2_demos/ntc/ntc-wiring.png){width=370 style="height:180px; object-fit: cover; object-position: 0% 50%;"}](/assets/images/mini-lab/audio-panel-generator-experiment_vi.png) |

## Buzzer

![Sensor side panel buzzer visual](/assets/images/mini-lab/sensor-panel-buzzer_vi.png){ align=right }

Piezo speaker to produce beeping sounds.

**Pin header:**  

- **pin 1, 2** - buzzer enable signal (HIGH - on, LOW - off)

**Info:**

- Buzzer type: **active**.
- Can't be used with `#! tone()` function to generate different frequencies. For that case - **passive** buzzer is required or use [Audio side panel](/side-panels/audio-panel/).

**Power:**

- Uses **+5v** POWER IN to drive buzzer.

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel buzzer schematic](/assets/images/mini-lab/sensor-panel-buzzer_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-buzzer_sh.png) | [![Sensor side panel buzzer experiment](/assets/images/mini-lab/sensor-panel-buzzer_bb.png){width=300}](/assets/images/mini-lab/sensor-panel-buzzer_bb.png) <br> _Turn on buzzer by connecting VCC to H7._ |

## DC motor driver

![Sensor side panel H-bridge visual](/assets/images/mini-lab/sensor-panel-motor_vi.png){ align=right width=290 }

DC motor driver with **BD6220F** chip.  
Supports two speed control types: manual with potentiometer and buttons or modulated signal input.

**Control:**  

- **button FW** - manual spin motor forward.
- **button RW** - manual spin motor reverse.
- **potentiometer** - manual motor speed control.
- **JP2: POT CONTROL** - use potentiometer control.
- **JP2: PWM CONTROL** - use PWM pin control.

**Pin header:**

- **PWM IN FW** - PWM motor speed control (forward direction)
- **PWM IN RW** - PWM motor speed control (reverse direction)
- **POWER IN 1, 2** - supply power for motor (18V max)
- **K1 power in** - supply power for motor (screw terminal)
- **K2 motor output** - motor output pins (screw terminal)

**Info:**

- Arduino example: [Speed and direction control](https://github.com/totemmaker/arduino-examples/tree/master/mini-lab/sidepanel2_demos/motor){target=_blank}.

**Manual control mode:**

Place **JP2** to **POT CONTROL**. In this mode motor speed and direction can be controlled using buttons and potentiometer.  
Wire motor to screw terminals. Supply power for motor trough **POWER IN** (or screw terminal).  
Click **FW** button to spin motor forward direction.  
Click **RW** button to spin motor reverse direction.  
Turn potentiometer to control motor speed.  

**Signal control mode:**

Place **JP2** to **PWM CONTROL**. In this mode motor speed and direction is controlled using **PWM IN** input pins. Potentiometer won't work.  
Wire motor to screw terminals. Supply power for motor trough **POWER IN** (or screw terminal).  
Supply PWM signal to **PWM IN FW** pin to spin motor forward. Control speed with duty cycle.  
Supply PWM signal to **PWM IN RW** pin to spin motor reverse. Control speed with duty cycle.  
May use LabBoard [Pulse generator](/labboard/features/pulse-generator/) mode to supply control signal. Wire **TXD** (typically set 100Hz and adjust duty cycle to change speed).

Motor state depending on input signal:  

| Input FW | Input RW | Motor state |
| --- | --- | --- |
| LOW | LOW | Idle |
| HIGH (PWM) | LOW | Rotation FW |
| LOW | HIGH (PWM) | Rotation RW |
| HIGH | HIGH | Brake |

**Power:**

- All required power has to be supplied trough **POWER IN 1, 2** pins or **K1 power in**. It does not use main power input (top of side panel).
- **POWER IN** pin is used to power motor from LabBoard (e.g. connecting to **VIN** pin). **K1 power in** screw terminal is used to power motor from external battery or power supply by connecting **-** and **+**.  

| Schematic | Experiment |
| --- | --- |
| [![Sensor side panel DC-Motor schematic](/assets/images/mini-lab/sensor-panel-motor_sh.png){width=250}](/assets/images/mini-lab/sensor-panel-motor_sh.png) | [![Sensor side panel DC-Motor experiment](/assets/images/mini-lab/sensor-panel-motor_bb.png){width=370}](/assets/images/mini-lab/sensor-panel-motor_bb.png) <br> _Press **FW** of **RW** button to spin motor. <br>Turn potentiometer to control speed._ |

**Legacy documentation of #2 Sensor side panel:**

This page contains latest documentation of audio side panel. Link below is outdated and only specified for reference.

[Version 1.1 datasheet](https://totemmaker.net/wp-content/uploads/2018/04/sensor-side-panel-1.1.pdf) (legacy)  
