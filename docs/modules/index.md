# Totem modules

A list of Totem products controllable with [Totem Arduino](https://github.com/totemmaker/TotemArduino){target=_blank} library.

```arduino title="Totem Library headers"
// Control over Bluetooth
#include <TotemBLE.h>              // Discover Totem boards (scan)
#include <TotemMiniControlBoard.h> // Connect Mini Control Board
#include <TotemRoboBoardX3.h>      // Connect RoboBoard X3
#include <TotemRoboBoardX4.h>      // Connect RoboBoard X4
// Control over Serial
#include <TotemLabBoard.h>         // Interface Mini Lab
```

<div class="grid cards" markdown>

-   :material-bluetooth:{ .lg .middle } **Bluetooth boards**

    ---

    Boards featuring remote control option over Bluetooth.

    [:octicons-arrow-right-24: BLE Modules](ble/index.md)

-   :fontawesome-solid-flask:{ .lg .middle } **Mini Lab**

    ---

    Monitor and control Mini Lab from TotemDuino over Serial.

    [:octicons-arrow-right-24: LabBoard](labboard.md)

-   :material-ev-plug-type2:{ .lg .middle } **X4 TBUS modules**

    ---

    Control RoboBoard X4 extension modules over TBUS.

    [:octicons-arrow-right-24: X4 modules](legacy-x4/index.md)

</div>

## Install library

This functionality requires [Totem Arduino](https://github.com/totemmaker/TotemArduino){target=_blank} library. Install it before using in a project.  

1. Select `Sketch` → `Include Library` → `Manage libraries..`  
1. In search field type in `totem`.  
1. Click ++"Install"++ and wait for it to complete.  
1. Close Library Manager window.  
![Arduino IDE Library install](../assets/images/arduino_ide_totem_install.gif)
