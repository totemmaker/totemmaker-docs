# Audio panel

![I/O side panel photo](/assets/images/mini-lab/audio-side-panel-sketch.svg){ align=right style="height:750px" }

**Contains:**  

* Audio amplifier with speaker.
* LED VU-meter.
* Digital function generator.
* Filter bench.

## Modules

In the board only the supply power is shared between modules. Otherwise they’re fully isolated from one another, and can be used independently. Logic level for digital signals have a jumper which selects the boards to work either at 3.3 or 5 V logic level. It’s important for the side panel to have the same logic level as the controller board (e.g., TotemDuino), for best results.

### Audio amplifier with speaker

This is an AB class discrete audio amplifier module, capable of up to 1 W output RMS power. Module is designed to work from externally applied 12 V line with connector J1 or contact terminal H6. Input volume is adjusted with potentiometer P1.

Inbuilt speaker is enabled by connecting a jumper to JP2 connector, amplified signal output can be connected with a H4 female connector.

You can use the function generator as a signal source (described below) by just connecting a jumper cable between amplifier input and generator output. Another possible input source is using Totem side panel 1 microphone module.

While the amplifier will work in a wide input voltage range, the upper voltage limit is 12 V. You can use the same 12 V used to power Mini Lab by connecting into VIN line on the Mini Lab board.

### VU Meter

An Audio Volumetric Unit (VU) meter module is relying on a 10-bar LED display as an output. Each bar represents a 3dB change in the input signal level. The module shares the same 12 V voltage supply line as the audio amplifier module described above.

Input signal level sensitivity can be adjusted by potentiometer U$3. We suggest that before using the module a calibration sequence be performed — apply the maximum allowed input signal to the module, and turn the potentiometer until all LED are lit, except top 3 which are red. During use lit red LEDs will then indicate overload of the input level.

### Function generator

Side panels has different function generator circuitry, depending on revision version.

**Panel ver 3.5:**

This module can generate either sine or square wave signals in up to 1MHz frequency. Module shares the same 12 V power supply line as the audio amplifier, described above. Frequency range can be changed by sliding switches S1 and further adjusted either in coarse or fine detail with two potentiometer. Sine wave amplitude can be adjusted with a separate potentiometer.

Output sine wave can be selected to be output either with a DC offset, when output is from 0 to VCC with a bias of VCC/ 2, or shifted to AC when the jumper is not connected on the JP4.

[version 3.5 datasheet](https://totemmaker.net/wp-content/uploads/2018/08/side-panel-3-Optimized.pdf)

**Panel ver 3.7:**

This module consists of a digitally controlled function generator AD9833 chip. It is capable to generate sine, triangle and square wave output signal in up to 12.5 MHz frequency. Generated signal is buffered with an operation amplifier, giving user the ability to control output signal amplitude.

Arduino [AD9833 library](https://github.com/Billwilliams1952/AD9833-Library-Arduino) can be used to control this chip from TotemDuino.  
LabBoard [AD9833 mode](/labboard/features/ad9833-control/) can be used to wire and control this chip directly.

Output of the function generator can be wired to the input of audio amplifier, allowing you to hear the output of if with no extra parts. Additionally, filter bench module can be placed between those modules to also experiment with pi-filters, and its effect to the signal.

### Filter Bench

This is a simple framework for experimenting with pi-type filters. It allows you to easily connect different THT components, such as resistors, capacitors and inductors. Filter bench module is meant to be used as middle module between signal generator and audio amplifier modules, allowing user to quickly experience the effect that various components has on the signal.

## Datasheet

Side panel instructions, assembly, mounting and electrical wiring.

<a href="https://totemmaker.net/wp-content/uploads/2018/08/audio-side-panel-1.1.pdf" class="image fit">audio-side-panel-1.1.pdf</a>
<object style="width:100%; height:600px;" data="https://totemmaker.net/wp-content/uploads/2018/08/audio-side-panel-1.1.pdf" type="application/pdf">
    <embed src="https://totemmaker.net/wp-content/uploads/2018/08/audio-side-panel-1.1.pdf" type="application/pdf" />
</object>
