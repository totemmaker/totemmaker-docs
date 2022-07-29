# Firmware update

LabBoard firmware can be updated trough TotemDuino and Arduino IDE. This simple process does not require any complicated setup.  

**Note:**

- Factory reset will be applied during firmware update.
- Calibration will be required after firmware update.

## Prepare

During update procedure you will need:

- Arduino IDE
- TotemDuino and LabBoard, connected over flat cable
- 3 jumper wires and 1 male to female

## Perform update

For smooth update procedure - make sure to follow steps in order.  
Firmware changelog can be found in [Releases section](https://github.com/totemmaker/labboard-firmware/releases){target=_blank}.

1. Download latest firmware update file.  
[Download update file](https://github.com/totemmaker/labboard-firmware/releases/latest/download/labboard_update.ino){ .md-button .md-button--primary }  
1. Open **labboard_update.ino** file. A message may appear requesting to create folder - click ok.
1. Connect TotemDuino to computer.
1. Select `Tools` → `Port`.
1. Upload code to TotemDuino.
1. Put LabBoard into boot mode:
    - Hold ++"SET\-"++ key and power on MiniLab. LabBoard won't turn on.
    - Firmware v2: Menu -> Setup -> Boot -> run (hold until `boot on` displayed)
1. Open Serial Monitor, select baud **57600**, wait for info text to appear.
1. If you have LabBoard v.2.1 and v.2.2, connect these wires:  
![Mini Lab LabBoard boot wiring](/assets/images/mini-lab/labboard-boot-wiring.png)
    - Connect D0 -> TXD
    - Connect D1 -> DIG2
1. Type `start` in Serial Monitor input field. Press ++enter++ or ++"Send"++.
1. Wait for update to finish. LabBoard will turn on displaying `CALIb run`.
1. Firmware update is completed. Proceed to [calibration section](/labboard/features/setup/#calibration).

## Troubleshoot

If error `Error: failed to enter boot mode` displayed:

- Make sure pins `D0` -> `TXD` and `D1` -> `DIG2` are connected for revision v.2.1 and v.2.2 boards. v.2.3 does not require this!
- Make sure LabBoard is in boot mode. `boot on` should be displayed, or nothing - if ++"SET\-"++ key was used.
- Restart update procedure by pressing reset button on TotemDuino.
- Disconnect everything and repeat steps in order.

If problems communicating with TotemDuino

- Make sure baud 57600 is selected in Serial Monitor.
- Make sure correct port is selected in Arduino IDE (or check drivers if running Windows 11 `Device Manager` > `Ports`).
- Make sure you typed `start` in Serial Monitor input field. Pressing ++enter++ or ++"Send"++ is required to start firmware update.