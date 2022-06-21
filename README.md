# Setup 2021 G14

In this document, I have documented how I set up and manage my 2021 ASUS ROG G14 (GA401QH) device.


# WARRANTY
THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
If you proceed anyway, know that serious problems might arise as a result of doing one or more of the software modifications as listed in this document. In some cases, your warranty might get void as well - and I won't be liable to damages.


## Re-install Windows

This is extremely important. It's extremely important to re-install Windows.

To learn how to back up your apps, documents, files, etc., re-install Windows, and restore your data, read [this document](https://github.com/pratyakshm/WinRice/wiki/Fresh-installation-of-Windows).


## Windows edition

Use an edition of Windows that's based on the Professional SKU. Windows 11 Enterprise, Pro or Pro for Workstations will suffice.

OEMs put a block while re-installing Windows defaults to installing the shipping edition of Windows (usually Home) and does not enable users to select an edition of their choice.

If you have already installed an edition of Windows that's not based on the Professional SKU, you may change your Windows edition without having to re-install the OS using [this script](https://github.com/massgravel/Microsoft-Activation-Scripts).


## Device Drivers

### Things to know:

- Update all your drivers from Windows Update, and ensure that all drivers are updated from MyASUS app.

- Do not get driver updates from AMD's official website. If you do, compatibility issues may arise with AMD Radeon Software.

- If Windows Update drivers do not suffice, another place to look at is [this webpage in ASUS's website](https://rog.asus.com/in/laptops/rog-zephyrus/2021-rog-zephyrus-g14-series/helpdesk_download).


### Notes:

- If Windows installs a version 27 graphics driver, disable driver delivery from Windows Update using Group policy editor and re-install a newer driver from ASUS's website

- If Dolby Access asks for a subscription, install your Audio drivers and Dolby Atmos for PC driver from the MyASUS app or ASUS' website, though the former is recommended.

- Make sure there are no unknown devices in Device manager. If you see any, its most likely the ASUS Wireless Radio Control that enables Airplane mode hotkey, and you can get the driver from Driver Booster by IOBit.


## Setup Windows Hello and Biometric Hardware

- Register fingerprints to your device.

- Open Device manager > Biometric devices.

- Locate fingerprint devices and open its Properties.

- Go to Power management and ensure both options "Allow the computer to turn off this device to save power" and "Allow this device to wake the computer" are left unchecked.


## De-ASUSify

### Pre-requisites:

- Vari Bright is disabled in AMD Radeon Software.

- Color calibration is ok.

- All hotkeys functional.

- Dolby Access functional.

- On screen display (OSD) functional. 


### Kill ASUS apps to avoid conflicts during uninstallation

Copy paste this code into elevated CMD/PS:
```
taskill /f /im ASUS*
taskkill /f /im Armoury*
taskkill /f /im ROG*
taskkill /f /im P50*
```


### Cleanup

- **Crucial**: Create a System Restore point.

- Install BCUninstaller using `winget install bcuninstaller`.

- Use BCUninstaller to delete programs related to ASUS. Tags include `asus`, `rog`, `armourycrate`, `armoury crate` and `rogliveservice`. 

- Stop and delete redundant ASUS-related services by copy pasting this code into an elevated CMD/PS window:

<details><summary>Click here to expand/collapse</summary>

```
rem Stop & Delete ASUS services
sc stop ASUSLinkNear
sc stop ASUSLinkNearExt
sc stop ASUSLinkRemote
sc stop ASUSSoftwareManager
sc stop ASUSSystemAnalysis
sc stop ASUSSystemDiagnosis

sc delete ASUSLinkNear
sc delete ASUSLinkNearExt
sc delete ASUSLinkRemote
sc delete ASUSSoftwareManager
sc delete ASUSSystemAnalysis
sc delete ASUSSystemDiagnosis

rem Disable AMD Crash Defender
sc stop "AMD Crash Defender Service"
sc config "AMD Crash Defender Service" start=disabled
```
</details>

**P.S.:** Do not disable ASUS Optimization as it is used to power On screen display (OSD) and keyboard hotkeys.

- Delete everything associated with `ArmouryDevice`, `ROG Live Service`, `Update`, `ASUS` in these folders:
    
    - C:\Program Files (x86)

    - %LocalAppData%

    - C:\ProgramData

    - C:\Program Files

- Additionally, install Everything (``winget install voidtools.Everything``) and search for files and folders that contain the names mentioned above. If you find anything, research about them and take actions accordingly.

- In Task Scheduler, delete everything associated with ASUS and disable everything associated with AMD.


## Enable On-screen display (OSD)

These are pop-ups that appear on your screen when you toggle your Mic, change your keyboard brightness or modify your Power settings.

- Go to the following directory:

```
C:\Windows\System32\DriverStore\FileRepository
```

- Type `asussci` and you will find a folder named `asussci2.inf_amd64_<UID>`. Open that folder and navigate to `ASUSOptimization` folder.

- Create a shortcut for ASUSOSD.exe and place it in ``shell:common startup`` directory.

- Done!


## Energy Management

- Run [**pratyakshm/RestorePowerOptions**](https://github.com/pratyakshm/RestorePowerOptions) to expose power settings: 
```
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://bit.ly/RestorePowerOptions'))
```

- Use [**aredden/electron-G14Control**](https://github.com/aredden/electron-G14Control). This is the utility that I use to manage my device. Import [downloaded config](https://github.com/pratyakshm/G14_setup/blob/main/config/G14ControlConfig.datas).

- Import [downloaded Power plan](https://github.com/pratyakshm/G14_setup/blob/main/config/PowerPlan.pow):
```
powercfg.exe -import "path-to-downloaded-plan"
```

~~- Avoid usage of Fn keys using [**okkosh/FN-key-lock**](https://github.com/okkosh/FN-key-lock).~~


# Conclusion

That is all, hopefully!