# MEMS sensor control

## Description

RoboBoard X4 contains accelerometer and gyroscope to measure movement and angles. At the moment there is no high-level Totem API to easily detect all rotations and interactions. Standard Arduino libraries can be used to read raw sensor data.  

!!! info "Note"
    Board revision v1.1 and v1.0 has different MEMS chip, so follow instructions accordingly

## Setup library

=== "Board v1.1"
    ### Revision v1.1
    **I2C address:** 0x68  
    **Chip:** [ICM-20689 datasheet](https://3cfeqx1hf82y3xcoull08ihx-wpengine.netdna-ssl.com/wp-content/uploads/2021/03/DS-000143-ICM-20689-TYP-v1.1.pdf){target=_blank}  
    **Arduino library:** [ICM20689](https://github.com/finani/ICM20689){target=_blank}  
    **Arduino examples:** [library examples](https://github.com/finani/ICM20689/tree/master/examples){target=_blank}  

    1. Open Library Manager: `Tools` → `Manage Libraries...`  
    1. Install library: `ICM20689`  
    1. Load example: `File` → `Examples` → `ICM20689` → `Basic_I2C`  
=== "Board v1.0"
    ### Revision v1.0
    **I2C address:** 0x6A  
    **Chip:** [LSM6DS3 datasheet](https://www.st.com/resource/en/datasheet/lsm6ds3.pdf){target=_blank}  
    **Arduino library:** [Arduino_LSM6DS3](https://github.com/arduino-libraries/Arduino_LSM6DS3){target=_blank}  
    **Arduino examples:** [library examples](https://github.com/arduino-libraries/Arduino_LSM6DS3/tree/master/examples){target=_blank}  
    **Totem examples:** [totem examples](https://github.com/totemmaker/TotemArduinoBoards/tree/master/libraries/TotemX4/examples/MEMS){target=_blank}

    1. Open Library Manager: `Tools` → `Manage Libraries...`  
    1. Install library: `Arduino_LSM6DS3`  
    1. Load example: `File` → `Examples` → `Board` → `MEMS` → `ReadRawData`  
