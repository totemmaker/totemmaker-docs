# Firmware update

LabBoard firmware can be updated to the latest version in order to receive new features and improvements. This guide contains instructions for 3 different ways to perform this procedure - [Update over Arduino](#update-over-arduino) (recommended), [Update over UART](#update-over-uart), [Update over SWD](#update-over-swd).

**Note:**

- Factory reset will be applied during firmware update.
- Calibration will be required after firmware update.
- 2 jumper wires are required.

## Check version

To check if new version is available:

1. Go to Menu > `0. SEtUP` > [`6. UERSI`](/labboard/features/setup/#firmware) > current installed version number is displayed.  
_Menu feature is only available from v2.00 firmware._
2. Go to [Releases section](https://github.com/totemmaker/labboard-firmware/releases){target=_blank} and check if there is more recent version available.

## Update over Arduino

Simple update procedure using TotemDuino, connected with LabBoard over flat cable.  
For smooth process - make sure to follow steps in order.  

1. Click button to download latest firmware update file.  
[Download labboard_update.ino](https://github.com/totemmaker/labboard-firmware/releases/latest/download/labboard_update.ino){ .md-button .md-button--primary }  
1. Open **labboard_update.ino** file. A message may appear requesting to create folder - click ok.
1. Connect TotemDuino to computer.
1. Select `Tools` → `Port`.
1. Upload code to TotemDuino.
1. Put LabBoard into boot mode:
    - Hold ++"SET\-"++ key and power on MiniLab. LabBoard won't turn on.
    - Firmware v2: Menu -> `0. SEtUP` -> `8. boot` -> run (hold until `boot on` displayed)
1. Connect wires (for boards v.2.1 or v.2.2):  
![Mini Lab LabBoard boot wiring](/assets/images/mini-lab/labboard-boot-wiring.png)
    - Connect D0 -> TXD
    - Connect D1 -> DIG2  
_This is not required for board version **v.2.3**!_
1. Click TotemDuino reset button to start update.  
![Mini Lab TotemDuino reset](/assets/images/mini-lab/totemduino-reset.png){width=80%}  
TX (<span style="color:red">red</span>) LED will start to blink.
1. Wait for <span style="color:mediumseagreen">green</span> LED to turn on and LabBoard to display `CALIb run`.
1. Firmware update is completed. Proceed to [calibration section](/labboard/features/setup/#calibration).  
In case green LED is flashing at 0.5s rate - update procedure failed:
    - Restart update procedure by pressing reset button again.
    - Make sure pins `D0` -> `TXD` and `D1` -> `DIG2` are connected for revision v.2.1 and v.2.2 boards. v.2.3 does not require this!
    - Make sure LabBoard is in boot mode. `boot on` should be displayed, or nothing - if ++"SET\-"++ key was used (LED ±0.5V may be lit on).
    - Disconnect everything and repeat steps in order.

## Update over UART

![LabBoard UARt connected](/assets/images/mini-lab/labboard-uart.jpg)

STM32 has integrated bootloader capable of loading firmware using UART (Serial) peripheral. This requires application on PC side and USB to Serial converter to transfer data to MCU.  
One application that is capable to do so - [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html){target=_blank}. Other applications can be used also.

### Connect over TotemDuino

1. Connect pins:
    - RST -> GND (TotemDuino)
    - D0 -> TXD
    - D1 -> DIG2
1. Hold ++"SET-"++ button and plug in TotemDuino (power on). LED ±0.5V should light up dimly.

### Connect over USB Serial

1. Unplug flat cable from LabBoard (or put TotemDuino in Reset: RST -> GND).
1. Connect pins:
    - GND -> GND
    - TX -> DIG2
    - RX -> TXD
1. Connect 3.3V -> 3.3V if there is no external power to LabBoard.
1. Hold ++"SET-"++ button and plug in Serial converter (power on). LED ±0.5V should light up dimly.

In STM32 Cube Programmer application select UART with specified settings. Baudrate value can be different. Click ++"Connect"++ to establish connection.
![STM32 Cube Programmer UART](/assets/images/stm32-cubeprog-uart.jpg)

Download firmware in .hex or .bin format and flash (download) it to LabBoard.  
For binary (.bin) file - address should be `0x08000000`.

[Download LabBoard.hex](https://github.com/totemmaker/labboard-firmware/releases/latest/download/LabBoard.hex){ .md-button .md-button--primary } [Download LabBoard.bin](https://github.com/totemmaker/labboard-firmware/releases/latest/download/LabBoard.bin){ .md-button .md-button--primary }  

## Update over SWD

![LabBoard SWD connected](/assets/images/mini-lab/labboard-swd.jpg)

SWD is a programming and debugging interface for ARM chips. A special hardware (ST-Link) and PC software is required to perform update using this method. One application that is capable to do so - [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html){target=_blank}. Other applications can be used also.  
Some ST-Link programmers has Mass storage feature where connected board opens as storage device, allowing to simply drag and drop firmware file and automatically flash it to LabBoard.

![LabBoard SWD header](/assets/images/mini-lab/labboard-swd-header.png)

1. Connect ST-Link to LabBoard:
    - SWCLK -> SWCLK
    - GND -> GND
    - SWDIO -> SWDIO
    - NRST -> RST
1. Connect 3.3V -> 3.3V if there is no external power to LabBoard.

In STM32 Cube Programmer application select ST-LINK with specified settings and click ++"Connect"++. Connection should be established if wiring is correct and LabBoard has power.  
![STM32 Cube Programmer SWD](/assets/images/stm32-cubeprog-swd.jpg)

Download firmware in .hex or .bin format and flash (download) it to LabBoard.  
For binary (.bin) file - address should be `0x08000000`.

[Download LabBoard.hex](https://github.com/totemmaker/labboard-firmware/releases/latest/download/LabBoard.hex){ .md-button .md-button--primary } [Download LabBoard.bin](https://github.com/totemmaker/labboard-firmware/releases/latest/download/LabBoard.bin){ .md-button .md-button--primary }  

## Downgrading

If you need to install older firmware version (in case of bug in latest one or etc.), browse [Releases section](https://github.com/totemmaker/labboard-firmware/releases){target=_blank} (Assets) to download required version **labboard_update.ino**, **LabBoard.hex** or **LabBoard.bin** and follow install guide with selected file.
