# Driver install

TotemDuino contains PL2303 USB to Serial chip that is no more supported in **Windows 11 OS**. This results in problems when trying to flash code sketch using Arduino. This tutorial will instruct on resolving this issue. ^^Other operating systems should work out of the box.^^

![Windows 11 driver issue](/assets/images/windows-11-driver-issue.jpg)

_Driver status can be view in Windows → Device Manager application._

## Install driver

Windows 11 uses latest Prolific driver that does not work with this chip. Installing older version resolves the issue.

1. Make sure TotemDuino is disconnected from the computer.
1. Download driver.  
[Download Prolific Driver](https://web.archive.org/web/20211221091151/http://www.prolific.com.tw/UserFiles/files/PL2303_Prolific_DriverInstaller_v1_12_0.zip){.md-button .md-button--primary target=_blank}
1. Extract `PL2303_Prolific_DriverInstaller_v1_12_0.zip` archive file.
1. Run `PL2303_Prolific_DriverInstaller_v1.12.0.exe` installer file.
1. If `User Account Control` window appeared - click ++"Yes"++.
1. Driver install window opens.  
![Windows 11 driver install](/assets/images/windows-11-driver-install.jpg)
1. Click ++"Next >"++
1. Click ++"Finish"++
1. Now TotemDuino should be fully working.  
![Windows 11 driver issue](/assets/images/windows-11-driver-fixed.jpg)

If message "DO NOT SUPPORT ..." is still displayed in Device Manager - run same installer again:

1. Select `Repair` and click ++"Next >"++
1. Select `No` to restart and click ++"Finish"++

## Lock driver

Windows will try to update driver on each computer restart. This step will prevent to overwrite installed version.

1. Download `Show or hide updates troubleshooter`.  
_Microsoft Edge may show this file as "harmfull". Select `Keep` option._  
[Download application](https://download.microsoft.com/download/f/2/2/f22d5fdb-59cd-4275-8c95-1be17bf70b21/wushowhide.diagcab){.md-button .md-button--primary}
1. Run `wushowhide.diagcab` file.
1. Click ++"Next"++. Wait for detection process to finish.
1. Click ++"→ Hide updates"++.  
![Select option](/assets/images/show-or-hide-menu.jpg)
1. Select `Prolific - ports - 3.9.1.0`  
![Select option](/assets/images/show-or-hide-select.jpg)
1. Click ++"Next"++, wait for "Resolving problems" screen to finish.  
![Select option](/assets/images/show-or-hide-finish.jpg)
1. Click ++"Close"++

Now TotemDuino should be fully working, even after computer restart or Windows Update.

## Restore driver

Follow these instructions if you want to restore driver to original state. This will undo changes done in previous tutorial.

1. Download `Show or hide updates troubleshooter`.  
[Download application](https://download.microsoft.com/download/f/2/2/f22d5fdb-59cd-4275-8c95-1be17bf70b21/wushowhide.diagcab){.md-button .md-button--primary}
1. Run `wushowhide.diagcab` file.
1. Click ++"Next"++. Wait for detection process to finish.
1. Click ++"→ Show hidden updates"++.  
![Select option](/assets/images/show-or-hide-menu.jpg)
1. Select `Prolific - ports - 3.9.1.0`  
![Select option](/assets/images/show-or-hide-select2.jpg)
1. Click ++"Next"++, wait for "Resolving problems" screen to finish.  
![Select option](/assets/images/show-or-hide-finish.jpg)
1. Click ++"Close"++

Restart computer and Prolific driver should roll back to the latest version.
