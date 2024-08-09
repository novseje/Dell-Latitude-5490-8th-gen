# Dell Latitude 5490, Sonoma 14.6

Please add your own PLATFORM INFO, with https://github.com/corpnewt/GenSMBIOS, im using MacBookPro15,2 SMBIOS. But you can use any SMBIOS from MBP14,1 through MBP16,1

## System information

| Specifications      | Details                                                                                       |
|:--------------------|:----------------------------------------------------------------------------------------------|
| Computer model      | Dell Latitude 5490                                                                            |
| Processor           | Intel Core i5-8250U Processor @ 1.6 GHz                                                       |
| Memory              | 32GB (2x16GB) Samsung DDR4 2400MHZ (M471A2K43BB1-CRC)                                         |
| Hard Disk           | SSD WD Blue SN570 500GB (NVME slot),<br/>SSD Transcend MTS430S 512GB M.2 SATA III (WWAN slot) |
| Integrated Graphics | Intel(R) UHD Graphics 620                                                                     |
| Screen              | Display @ 1920 x 1080 (13.9 inch)                                                             |
| Sound Card          | Realtek ALC256                                                                                |
| Wireless Card       | Intel 8265NGW                                                                                 |
| Bluetooth Card      | Intel 8265NGW                                                                                 |
| Ethernet            | I219-LM Intel Ethernet                                                                        |


# Guides and sources:

More detailed description you can view in the source repository [juanpy0223/Dell-Latitude-5490-8th-gen](https://github.com/juanpy0223/Dell-Latitude-5490-8th-gen)


# What works on MacOS (*need to check all*):

- KeyBoard+Trackpad full (SSDT-GPI0.aml + AlpsHID.kext,VooDooI2C.kext,VooDooI2cHID.kext, VooDooPS2Controller.kext, NoTouchID.kext)

- Graphics Full Aceleration, VGA and HDMI (Plug.aml, Lilu.kext, VirtualSMC.kext, WhateverGreen.kext)

- A type USB USB Port Map kext, USB-C | DP DisplayPort - (USB Port Map kext + SSDT-EC-USBX-LAPTOP.aml + Graphic Patch)

- Brightness+Audio keys (SSDT-PNLF.aml + BrightnessKeys.kext)

- Battery Read-Outs (SSDT-AC.aml, ECEnabler.kext, SMCBatteryManager.kext, ACPIBatteryManager.kext) it actually worked without any patch or kext, but battery life was very poor, I went on with patching, now it lasts 5 hours.

- Edit: I realised that mds_stores (aka SpotLight) indexing was using too much CPU thus using too much power, making it hot and draining the battery... solution: in terminal, type: "sudo mdutil -a -i off" to disable indexing... now is running cool 43 to 45 degrees; the fan now is off, must of the time!!!! --- solution posted by Posted by u/jnr0602 on Reddit (https://www.reddit.com/r/hackintosh/comments/vz2lfq/success_macos_monterey_124_on_a_2021_razer_blade/)

- TouchSreen also works on Catalina, Big Sur and Monterrey, only remapped USB ports and boom, like a charm.

- Power Management (SSDT-EC-USBX-LAPTOP.aml, SSDT-HPET.aml, SSDT-PLUG.aml, CPUFriend+CPUFriendDataProvider kexts)

- Wifi + Bluetooth no kext needed for BCM94360NG /AirportItlwm.kext+IntelBluetooth firmware and injector kext for Intel 8265ngw

- Conexion LAN (IntelMausi.kext)

- SDCard Reader (RealtekCardREader+RealtekCardReaderFriend kexts)

- Audio: Speakers and Microphone (AppleAlc.kext alcid11 + HPET aml) ComboJack (PostInstall\ComboJack-fixforDell + VerbStub.kext) it fixes the audio distortion via wired headphones and aux audio. (I had to disable SIP in order to install, then enabled back).

# I included a file (pcidevices.aml) wich has all pci devices config needed for DeviceProperties



# This is a WORK IN PROGRESS, please let me know any problem you may have, or any suggestions.

There are other patches for correcting sleep issues, and making macOS believe that os running on a real MacBookPro, like ALSD, DMAC, EXT3-WAKESCREEN, GPIO, GPRW, MCHC, MEM2, PCMR, PTWAKKTS, and so on.

# Note: when connected to the charger it wakes up right after sleep, but if I detach charger, sleep and waking up works ok, just by closing and opening the laptop.

# Before installation you must do 2 things: 1st. Bios Config, 2nd. Config Modification via GrubShell: (Credits to franklin-gedler)


# Bios Config:

Boot mode: UEFI

Fast Boot: Minimal

SecureBoot = Disable

SATA Mode: AHCI 

Intel SGX: Disable

# Now we start the pendrive and select modGRUBShell.efi or OpenShell.efi (Be carefull with the commands):

CFG Lock = Disable
    (VarOffset/VarName): 0x52D
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x52D 0x1 0x0
____________________________________________________________________

DVMT Pre-Allocated = 64MB
    (VarOffset/VarName): 0x804
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x804 0x1 0x2
____________________________________________________________________

DVMT Total Gfx Mem = Max
    (VarOffset/VarName): 0x805
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x805 0x1 0x3
____________________________________________________________________

VT-d = Disable
    (VarOffset/VarName): 0x808
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x808 0x1 0x0 
    
After this we can reboot and then install.

