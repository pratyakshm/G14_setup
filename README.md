# Setup 2021 G14

This document covers the initial setup phase of 2021 ROG Zephyrus G14 laptop. It includes debloating OEM services to the bare minimum, and using alternative and better trusted device control apps.

# WARRANTY
THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
If you proceed anyway, know that serious problems might arise as a result of doing one or more of the software modifications as listed in this document. In some cases, your warranty might get void as well - and I won't be liable to damages.

## Windows edition
OEMs force installation of shipping edition of Windows instead of providing user the liberty to choose Windows edition on setup. Check current Windows edition using winver, I preferrably want it to be Pro, Pro for Workstations or Enterprise.

If it's a different edition, try changing edition using [this script](https://github.com/massgravel/Microsoft-Activation-Scripts/releases).

## Drivers

💡 To get access to Dolby Access, update your device from Windows Update first which will also update your audio drivers, which power the Dolby Atmos experience.

- Once Windows Update is done updating your device and all restarts are done, check Windows Update for updates again. Make sure that **no updates are left to apply**.

#### Use Group policy editor to disable driver delivery using Windows Update.
 
I disable driver delivery via Windows Update because Windows Update force deploys display drivers of Version 27 instead of 30, and that causes issues in Windows 11 operating system.

- Update display drivers from [AMD website](https://www.amd.com/en/support). Choose optional drivers.

- If AMD's Drivers tool could not install Chipset drivers, install them  from [this link](https://dlcdnets.asus.com/pub/ASUS/GamingNB/Image/Driver/Chipset/23894/AMD_Chipset_DriverOnly_ROG_AMD_Z_V1.2.0.118Sub5_23894.exe) from [ASUS' website](https://rog.asus.com/in/laptops/rog-zephyrus/2021-rog-zephyrus-g14-series/helpdesk_download). The chipset drivers are somehow, sometimes, not update-able via AMD's tool, I think this might be due to a block put in place by the OEM but I am unsure.

- Check MyASUS app for any pending driver updates, etc. This step is extremely important, do not miss it.

- Update Nvidia GeForce drivers from [Nvidia's website](https://www.nvidia.com/Download/index.aspx). Choose Studio Drivers instead of Game Drivers. Do not install GeForce Experience app.

- Install IOBit Driver Booster from winget using `winget install IOBit.DriverBooster9`, and check for any drivers that are left to be updated.

- Make sure there are no unknown devices in Device Manager. If you have one, it's most likely ASUS Wireless Radio Control - driver updates for this are available on Driver Booster 9.

- After Unknown devices are cleared from Device Manager, perform a device reboot and check if basic functionality is working as it should.



## Setup biometrics

- Register fingerprints to your device.

- Open Device manager > Biometric devices.

- Locate fingerprint devices and open its Properties.

- Go to Power management and ensure both options "Allow the computer to turn off this device to save power" and "Allow this device to wake the computer" are left unchecked.



## Perform these checks before cleanup

- Screen calibration is fine.

- Vari Bright in AMD Radeon Software is disabled.

- All hotkeys are functional.

- Dolby Access app operational as expected.

## Cleanup

- Create a System Restore point. This is highly recommended.

- Use BCUninstaller to delete every single app and / or program that is related to OEM. Tags include `asus`, `rog`, `armourycrate`, `armoury crate`, `rogliveservice`. 

- Disable all ASUS services except ``ASUS Optimization Service`` which is used to control keyboard hotkeys.

- Save the below content as a .bat file and run as admin:

```
rem Disable non-essential ASUS services
sc stop ASUSLinkNear
sc stop ASUSLinkNearExt
sc stop ASUSLinkRemote
sc stop ASUSSoftwareManager
sc stop ASUSSystemAnalysis
sc stop ASUSSystemDiagnosis

rem Delete ASUS services (yet again)
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

The above code was used from [**sammilucia/ASUS-G14-Debloating**](https://github.com/sammilucia/ASUS-G14-Debloating), with modifications.

### Kill all ASUS apps running in background

Use these commands in an elevated terminal session:

```
taskill /f /im ASUS*
taskkill /f /im Armoury*
taskkill /f /im ROG*
taskkill /f /im P50*
```

### Delete residual files and folders

Target tags: `ArmouryDevice`, `ROG Live Service`, `Update`, `ASUS`.
Find folder names that contain these tags in the following location and permanently delete them:
- C:\Program Files (x86)
- %LocalAppData%

## Scheduled Tasks

1. Open Task Scheduler.

2. Delete the ASUS folder.

3. Delete all tasks related to ASUS in other folders.

4. Disable all tasks related to AMD in other folders.

## Enable On-screen display (OSD)

These are pop-ups that appear on your screen when you toggle your Mic, or change your Power settings.

- Go to `C:\Windows\System32\DriverStore\FileRepository`.

- Type `asussci` and you will find a folder named `asussci2.inf_amd64_<UID>`. Open that folder and navigate to `ASUSOptimization` folder.

- Create a scheduled task to launch `ASUSOSD.exe` on every login, or drag the app's shortcut to `shell:common startup` folder in order to start it on boot for all users.

## Third party utilities

- Use [**cronosun/atrofac**](https://github.com/cronosun/atrofac) to control fans.

- Use [**aredden/electron-G14Control**](/aredden/electron-G14Control) to limit battery charge amongst other things.

- Use [**pratyakshm/RestorePowerOptions**](https://github.com/pratyakshm/RestorePowerOptions) to restore all advanced power options. Run this command: 
```
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://bit.ly/RestorePowerOptions'))
```

- Setup Power plan using
```
powercfg.exe -import "C:\Users\Praty\Downloads\PratyakshPlan.pow"
```
Download the power plan from this reposiory's root directory.

- Avoid usage of Fn keys using [**okkosh/FN-key-lock**](https://github.com/okkosh/FN-key-lock).

# Conclusion

This covers the initial device setup of 2021 ASUS ROG Zephyrus G14.
