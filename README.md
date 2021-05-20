# HP Pavilion CE-2072NL - MacBookPro15,2

This repository contains the necessary files and information to successfully boot macOS on this laptop. 
- Bootloader version: **OpenCore 0.6.8**
- Kexts version: everything up-to-date with the latest version (check the links below)
- macOS version: [Big Sur 11.3.1](https://www.apple.com/macos/big-sur) - Release channel

![image](https://github.com/1alessandro1/HP-Pavilion-CE2072NL-macOS/blob/main/Misc/Screenshots/terminal.png)

## Specs

| Component      | Brand                                     |
|----------------|-------------------------------------------|
| **CPU**        | `Intel Core i5-8265U @ 1.8 GHz`           |
| **iGPU**       | `Intel UHD Graphics 620 - Whiskey Lake`   |
| **Storage**    | `Samsung 840 EVO 256GB + Intel 660p 512GB`|
| **Audio**      | `Realtek ALC295 - layout 13`              |
| **Ethernet**   | `Realtek RTL8111/8168`                    |
| **WiFi Card**  | `BCM94360NG - device-id 43A0`             |
| **OS**         | `macOS Big Sur 11.3 (20E62)`              |
| **BIOS**       | `F23`                                     |

## Important notes

- In the config.plist, section `PlatformInfo > Generic`, the following fields are currently edited with "CHANGEME" in order to force the user to generate his own serials. Refer to this guide to [know how](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#platforminfo). 
  - `MLB`
  - `ROM`
  - `SystemSerialNumber` 
  - `SystemUUID`

- For those who have set up a multiboot environment with multiple drives as I did please note that BigSur dropped support for the RAID only controller present in this laptop, vendor-id and device-id `8086:282A`, hence `CtlnaAHCIPort.kext` is required to boot Big Sur from the internal SATA drive. 

- If you are planning to use the built-in NVMe, the stock Intel 660p is perfectly fine with macOS.

- The HDMI port is fully working, in the `DeviceProperties` section you can find all the correct BusIDs. I've even tested a connection with a 4K monitor, the [LG27UD59](https://www.amazon.it/LG-Monitor-Desktop-FreeSync-Multitasking/dp/B0764DMTPN), and it works perfectly (maximum external resolution with this iGPU).

## How to get this laptop to boot macOS flawlessly

I highly suggest to read the [OpenCore guide](https://dortania.github.io/OpenCore-Install-Guide/)

For the ACPI configuration, I have to thank [dreamwhite](https://github.com/dreamwhite) for his help in the conversion from DSDT patches to pure modular SSDTs hotpatching, and [Gengik84](https://github.com/Gengik84) for the initial patched DSDT, which now is **not in use anymore**.

### Drivers

Must have to boot Big Sur:

* OpenRuntime.efi (bundled in OpenCore package)
* HfsPlus.efi (if you created the USB with [this method](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install-recovery.html#legacy-macos-online-method) or with `createinstallmedia`) and can be found either in the `OC/Drivers` folder of this repository or in [acidanthera/OcBinaryData](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi)

### Kexts

* [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases/latest)
* [AppleALC](https://github.com/acidanthera/AppleALC/releases/latest)
* [CPUFriend](https://github.com/acidanthera/CPUFriend/releases/latest)
* [CtlnaAHCIPort.kext](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/CtlnaAHCIPort.kext.zip) - for those of you who use the SATA Cable or M2 SATA SSD
* [Lilu](https://github.com/acidanthera/Lilu/releases/latest)
* [NVMeFix](https://github.com/acidanthera/NVMeFix/releases/latest)
* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases/latest)
* [SMCBatteryManager](https://github.com/acidanthera/VirtualSMC/releases/latest) - shipped inside **VirtualSMC**
* [SMCProcessor](https://github.com/acidanthera/VirtualSMC/releases/latest) - shipped inside **VirtualSMC**
* [SMCSuperIO](https://github.com/acidanthera/SuperIO/releases/latest) - shipped inside **VirtualSMC**
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases/latest) 
* [VoodooPS2Controller](https://github.com/acidanthera/VoodooPS2/releases/latest) - bundled with **VoodooInput**, **VoodooPS2Keyboard.kext** and **VoodooPS2Trackpad.kext** 
* [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases/latest)

**Note**: The kexts in this are not loaded in this order. Refer to the [config.plist](https://github.com/1alessandro1/1alessandro1-HP-Pavilion-CE2072NL-macOS/blob/main/EFI/OC/config.plist) to see the correct order under `Kernel > Add` section.

## Entering the advanced BIOS

Steps to enter the advanced BIOS (Insyde BIOS)

Method A.
  1. Power on the machine pressing the power button
  2. Press `Esc` multiple times, a message `Esc: Pause Startup` should appear
  3. Hold down the `Ctrl` button, then press `F10`: the `Advanced` tab should appear in the submenus.

Method B.
  1. Power on the machine pressing the power button
  2. Press `F9` multiple times to enter the BIOS boot menu
  3. Hold down the `Ctrl` button, then press `F10`: the `Advanced` tab should appear in the submenus.
 
## BIOS Settings

Right now, you should dig around to find the correct settings. All of these are present, be patient and explore every possible submenu. This list is accurately organized with subsections (they are on par with the BIOS sections)

* **Security**: 
  - Intel SGX = Disabled
  - TPM State = Disabled (set this first)
  - TPM Device = Hidden (then this one)
* **System Configuration**
   * Language = English
   * Virtualization Technology = Enabled 
   * Fan Always On = Disabled (for a quieter system overall)
   * Action Keys Mode = Enabled (so you don't have to press always Fn to use brightness keys)
   * Boot Options: 
     - USB Boot = Enabled
     - Network Boot = Disabled
     - Legacy Support = Disabled
     - Secure Boot (is not selectable, ensure it is Disabled by clicking on "Clear All Secure Boot Keys")
     - UEFI Boot order = Set it up as you wish 
* **Advanced**:
    * Boot Configuration:
      - Numlock = On
    * Peripheral Configuration:
      - Serial Port A = Disabled
      - Infrared Port = Disabled
    * For SATA Configuration, the configurable section is under *PCH-IO Configuration*. Continue on reading.
    * USB Configuration:
      - USB BIOS Support = Enabled
      - USB Legacy SMI bit Clean = Disabled
    * Chipset Configuration:
      - ME Unconfig on RTC Clear State = Disabled
      - Platform Trust Technology = Disabled
    * Debug Settings: 
      - Leave everything default here
    * Type C Support = Platform-POR
    * ACPI Table/Features Control:
       - Leave everything default here
    * CPU Configuration: 
      - Leave everything default here
    * Connectivity Configuration: 
      - Leave everything default here
    * Power and Performance:
         - CPU Power Management Control
              - CPU Lock Configuration (scroll all the way down leave other settings as default):
                - **CFG Lock** = Disabled
         - GT Power Management Control 
              - RC6 (Render Standby) = Disabled
              - Maximum GT frequency = Default Max Frequency 
              - Disable Turbo GT Frequency = Disabled
    * Memory Configuration: 
      - Leave everything default here
    * System Agent (SA) Configuration:
      - **VT-d** = Disabled
      - **Above 4G MMIO BIOS assignment** = Disabled
      - Graphics Configuration:
           - Primary Display = IGFX
           - Select PCIE Card = PEG Eval
           - Internal Graphics = Enabled
           - GTT Size = 8MB
           - Aperture Size = 256MB
           - PSMI Support = Disabled
           - **DVMT Pre-Allocated** = 64MB 
           - **DVMT Total Gfx Mem** = MAX
           - Leave other settings as default
     * PCH-IO Configuration
       - SATA And RST Configuration:
        - SATA Controller(s) = Enabled (do this only if you bought the SATA Cable separately)
        - **NOTE**: Read the [beginning of this page](https://github.com/1alessandro1/HP-Pavilion-CE2072NL-macOS/blob/main/README.md#important-notes) for the SATA info.

This way, if you applied these settings correctly: 
- You won't need `framebuffer-fbmem` and `framebuffer-stolenmem` properties under `DeviceProperties` for the graphics patch
- You won't need `AppleXCPMCfgLock` or similar kernel quirks

## Brightness keys 

There is a simple `SSDT-PS2.aml` which adds the correct `Notify` parameters to handle screen brightness while pressing `F2` or `F3`. Please note that `SSDT-PNLFCFL.aml` is reqired too.

## MAT Support is 0 

Hence, the only `Booter > Quirks` required to boot are `AvoidRuntimeDefrag`, `EnableWriteUnprotector` and `SetupVirtualMap`, maybe one day I'll whitelist the regions, but I think that right now the less patches in use, the better for stability.

## Gestures

Thanks to HP for installing a PS-2 trackpad, `VoodooPS2Trackpad.kext` and `VoodooInput.kext` successfully handle all the native gestures. Everything is working, even the 4-fingers gestures.

## USB Mapping

With the help of @dreamwhite we found a clever way to map usb ports present in the DSDT without having to drop tables or having to load SSDT-RHUB mentioned above.
Basically by hot patching the `_UPC` method to `XUPC` we managed to define that method in an external `SSDT-USB.aml`.

## Benchmarks

The following benchmarks were made using Geekbench V5:

- [CPU Single-core and Multi-core](https://browser.geekbench.com/v5/cpu/3406822)

## Credits

* [Apple](https://apple.com) for macOS
* [Acidanthera](https://github.com/Acidanthera) for OpenCore and Lilu-based kexts 
* [dreamwhite](https://github.com/dreamwhite) for helping me to transition to SSDT-only modifications.
* [Gengik84](https://www.macos86.it/profile/1-gengik84/) for the `GENG` method and for the original custom DSDT
* [dortania](https://github.com/dortania) team for its detailed guides
* [Corpnewt](https://github.com/CorpNewt) for SSDTTime and [fewtarius](https://github.com/fewtarius) for CPUFriend fork
* [Mieze](https://github.com/Mieze) for RealtekRTL8111 LAN driver
* [RehabMan](https://github.com/RehabMan) for his DSDT patching guide
