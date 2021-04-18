# Testing methodology

All these tests are conducted in the same way, these are the steps:

- Turn on the machine (in the EFI it is supposed to be True a different value of `ssiownd=0xnn` each time different)
- Open *Intel Power Gadget*, *Macs Fan Control* and *iStatMenus*
- Open a Terminal and start the `ffmpeg -c:v libx265` encoding benchmark
- The CPU goes from low RPM to high RPM since `ffmpeg` brings the CPU to 70-75 C without the power adapter and to 90 C with the power adapter connected, since this unlocks the TDP to go higher than 15W.
- Open macsfancontrol, check all the 5 values reported, as long as the values keep boucing around when the FAN is OFF (from what you hear) then your offset is wrong.
- Values from MacsFancontrol are going to jump around and are incorrect, but the only value which stays correct is when fan stops, going to zero. You know you found the correct offset when macsfancontrol reports `0 rpm` immediately once you hear that your FAN has stopped, (you don't hear it spinning anymore). 


### Used boot arguments:
- `-v` 
- `ssioec=debug`
- `ssiowndsz=5`
- `ssiownd=0xnn`

### Legenda:
- **ssiownd**: offset value, to test from `0x00` to `0xFF` in steps of `+0x05` for every reboot.

- **FAN steady**: refers to the condition where the RPM values in *Macs Fan Control* or *iStatMenus*. Only the first two values which can be found in *Macs Fan Control* are reported in *iStat Menus*). I chose to use YES/NO values in order to distinguish them more quickly when searching with `CMD+F`.

  - `YES`: Fan RPMs are detected by the applications
  - `NO`: Fan RPMs are *NOT* detected by the applications

    - **Note**: in this specific instance, I can hear from the fan increasing noise whether a change in higher load reflects to a higher RPM, hence a higher value should be reported. Variable values are hereby reported even though their changes do not reflect exactly what I hear (and see from the values) when the fan speeds up (or slows down).

- **Temperatures**: tells if *MacsFanControl* (as well as *iStatMenus*) can still see temperatures of the CPU
  - `True`: temperatures are detected by the applications
  - `False`: temperatures are *NOT* detected by the applications

- **Voltages**: tells if *MacsFanControl* (or *iStatMenus*) can see see any kind of voltage value.
  - `True`: voltages are detected by the applications
  - `False`: voltages are *NOT* detected by the applications

- **Watts**: tells if *MacsFanControl* (or *iStatMenus*) can see see any kind of power (watts) value.
  - `True`: watts are detected by the applications
  - `False`: watts are *NOT* detected by the applications

### Results
| ssiownd | Is FAN steady | Temperatures | Voltages | Watts | 
|:----:|:-------------:|:-------:|:------:|:------:|
| 0x00 |YES | True | False | True |
| 0x05 |YES | True | False | True |
| 0x0A |YES | True | False | True |
| 0x0F |YES | True | False | True |
| 0x14 |YES | True | False | True |
| 0x19 |YES | True | False | True |
| 0x1E |YES | True | False | True |
| 0x23 |YES | True | False | True |
| 0x28 |YES | True | False | True |
| 0x2D |YES | True | False | True |
| 0x32 |YES | True | False | True |
| 0x37 |YES | True | False | True |
| 0x3C |YES | True | False | True |
| 0x41 |YES | True | False | True |
| 0x46 |YES | True | False | True |
| 0x4B |YES | True | False | True |
| 0x50 |YES | True | False | True |
| 0x55 |YES | True | False | True |
| 0x5A |YES | True | False | True |
| 0x5F |YES | True | False | True |
| 0x64 |YES | True | False | True |
| 0x69 |YES | True | False | True |
| 0x6E |YES | True | False | True |
| 0x73 |YES | True | False | True |
| 0x78 |YES | True | False | True |
| 0x7D |YES | True | False | True |
| 0x82 |NO | True | False | True |
| 0x87 |NO | True | False | True |
| 0x8C |YES | True | False | True |
| 0x91 |YES | True | False | True |
| 0x96 |YES | True | False | True |
| 0x9B |YES | True | False | True |
| 0xA0 |YES | True | False | True |
| 0xA5 |YES | True | False | True |
| 0xAA |YES | True | False | True |
| 0xAF |YES | True | False | True |
| 0xB4 |YES | True | False | True |
| 0xB9 |YES | True | False | True |
| 0xBE |YES | True | False | True |
| 0xC3 |NO | True | False | True |
| 0xC8 |YES | True | False | True |
| 0xCD |YES | True | False | True |
| 0xD2 |YES | True | False | True |
| 0xD7 |YES | True | False | True |
| 0xDC |YES | True | False | True |
| 0xE1 |YES | True | False | True |
| 0xE6 |YES | True | False | True |
| 0xEB |YES | True | False | True |
| 0xF0 |YES | True | False | True |
| 0xF5 |YES | True | False | True |
| 0xFA |YES | True | False | True |
| 0xFF |YES | True | False | True |
