# I/O panel

![I/O side panel photo](/assets/images/mini-lab/io-side-panel-sketch.svg){ align=right style="height:750px" }

**Contains:**  

* 3 push-push switches with LED indicators
* 3 Potentiometers (10kOhms)
* 1 Rotary encoder switch
* 3 push button switches
* 1 RGB-LED (can show virtually any color)
* 1 3.5mm Jack connector
* 1 Relay, 10A/250v, with LED indicator

## Overview 

This is a getting started guide for the first expansion board for the Totem Mini Lab system. In this document we’ll go over all of the available modules in the board, together with explanations on their interface with the Mini Lab as well as usage examples.

Side panels are meant to bring easy access to commonly used components that are difficult to wire on breadboards due to their dimensions, or require additional equipment to operate them. These panels give a plug and play interface that lets users to concentrate on experiments or learning, rather than spending time solving issues on how to interface with the part.

This document is divided into sections where each separate module is described. In the side panel 1 these modules are available for use:  

* **Latched switches with LED indication** - can be wired to use normally-on or normally-off position. Includes an LED for push indication.
* **Potentiometers** - group of potentiometers that can also be used as a simple voltage divider.
* **Encoder with push switch** - a two line encoder that gives out pulses when rotated. Also includes a pushable button.
* **Push switches** - simple buttons for signal output.
* **RGB LED** - full color LED that can be used to learn about PWM.
* **Relay module** - includes an integrated driver, can be used with currents of up to 10 A.
* **3.5 mm jack adapter** - provides an easy access to a headphone jack connectors.

## Modules

In the board only the supply power is shared between modules. Otherwise they’re fully isolated from one another, and can be used independently. Logic level for digital signals have a jumper which selects the boards to work either at 3.3 or 5 V logic level. It’s important for the side panel to have the same logic level as the controller board (e.g., TotemDuino), for best results.

### Switches

Push-button switches have two channels - one is used for indicating the current state
with an LED, and the other can be employed for own use:  

Pins 1 and 2 (together with 5 and 6) are normally connected when the switch is in the “off” position. When button is pushed in, pins 2 and 3 (together with 5 and 4) become connected.  
On the side panel 1 there are 3 identical switches.

### Potentiometers

Using jumpers JP3 and JP4 you can select the range of the potentiometer to be between supply voltage. In any other case jumpers may be removed, and side pins for connector H5 should be used.  
Similarly as with the switches, there are 3 identical potentiometers in side panel 1 as well.

### Encoder

This device captures control knob rotation and outputs two phase-locked signals on it’s output at connector H7, pins 2 and 3. As these signals are always delayed by 90 degrees from each other, its possible to extract direction information from these signals, together with rotation speed from the frequency of it.  
Push-button is also included in the encoder, and connected to the H7 connector pin 1.

### Buttons

These are simple non-locked buttons that connect the signals from connector H8 to ground. Because of their size buttons are sometimes difficult to mount on breadboard, so side panel 1 gives a convenient access to them, where only a single wire can be used to wire a button. You get 3 buttons on a side panel.

### RGB LED

This is a three color LED in one package. Each LED can be controlled independently from others from pin H9. This is a great module to experiment with PWM signals, and to create different colours by varying intensities of separate LEDs.

### Relay module

This module has a single relay, driven by a transistor. Using H11 connector as an input, you can control the relay with logic level signals. Relay activation state is show with LED4, and its output is connected to B1 terminals.  
Used relay can control loads with up to 10 A.

### 3.5mm adapter

Purpose of this module is to give an easy access to a difficult to mount on a breadboard part. All signals from the jack are connected to H10 connector:

## Datasheet

Side panel instructions, assembly, mounting and electrical wiring.

<a href="https://totemmaker.net/wp-content/uploads/2018/04/io-side-panel-1.1.pdf" class="image fit">io-side-panel-1.1.pdf</a>
<object style="width:100%; height:600px;" data="https://totemmaker.net/wp-content/uploads/2018/04/io-side-panel-1.1.pdf" type="application/pdf">
    <embed src="https://totemmaker.net/wp-content/uploads/2018/04/io-side-panel-1.1.pdf" type="application/pdf" />
</object>
