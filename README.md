# EBC7700-SBC-Ubuntu
Ubuntu Releases for EBC7700 Series Single Board Computer.

## Description

Ubuntu Image releases for EBC7700 Series Single Board Computer.
- Based on Ubuntu 24.04.2 LTS.
- Prebuilt Ubuntu image in compressed format named `sbc-ubuntu-24.04-preinstalled-server-riscv64.img.zst`.
- Please ensure that the validated combination of the bootchain image and the Ubuntu image are flashed to the board.
- The latest images release is available [here](https://github.com/eswincomputing/ebc7700-sbc-ubuntu/releases/tag/2025.0730).

## Hardware preparation
- One Type-C power cable (power the board)
- One Micro USB cable (for serial port monitor and uboot command)
- One USB flash driver that at least 16G (image source)
- One SD card that at least 16G (boot device)

## Flashing images
Copy the bootloader and uncompressed Ubuntu image to an **ext4** formatted the USB flash driver

For example
```
USB    /- bootloader_SBC-A1.bin
        |- sbc-ubuntu-24.04-preinstalled-server-riscv64.img
```
connect the USB flash driver to the **usb2.0** port of the sbc board and connect the power cable and wait for the uboot shell through the serial port.

### Bootloader flashing

If your device already has a compatible bootloader installed, you can skip this step.
```
=> usb reset
=> ls usb 0
=> ext4load usb 0 0x90000000 bootloader_SBC-A1.bin
=> es_burn write 0x90000000 flash
```
Demo output
```
=> usb reset
resetting USB...
Bus usb1@50490000: Register 2000140 NbrPorts 2
Starting the controller
USB XHCI 1.10
scanning bus usb1@50490000 for devices... 3 USB Device(s) found
       scanning usb for storage devices... 1 Storage Device(s) found
=> ls usb 0
<DIR>       4096 .
<DIR>       4096 ..
<DIR>      16384 lost+found
      7633633280 sbc-ubuntu-24.04-preinstalled-server-riscv64.img
         4392824 bootloader_SBC-A1.bin
=> ext4load usb 0 0x90000000 bootloader_SBC-A1.bin
4392824 bytes read in 545 ms (7.7 MiB/s)
=> es_burn write 0x90000000 flash
SF: 224 bytes @ 0x0 Read: OK
FIRMWARE writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 2004 bytes @ 0x140000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x7d4 bytes @ 0x140000 Written: OK
Bootspi flash write protection enabled
DDR writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 273064 bytes @ 0x40000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x42aa8 bytes @ 0x40000 Written: OK
Bootspi flash write protection enabled
BOOTLOADER writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 4116952 bytes @ 0x1c0000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x3ed1d8 bytes @ 0x1c0000 Written: OK
Bootspi flash write protection enabled
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 224 bytes @ 0x0 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0xe0 bytes @ 0x0 Written: OK
Bootspi flash write protection enabled
bootloader write OK
```

⚠️ **Warning**: 
If you are using a bootloader version prior to `2025.06.30` and want to  upgrade to bootloader version `2025.06.30` or later, you cannot directly use the `es_burn` command to flash the bootloader; instead, you must flash it following the `RECOVER BOOTLOADER` method. Refer to Section 6.1.1 of the document `EBC77_Series_Single_Board_Computer_UG_EN.pdf` in documents directory.

### Ubuntu image burning
Insert SD Card into EBC7700 then enter the following command (it might takes 20 minutes depends on SD card performance)
```
=> es_fs write usb 0 sbc-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
=> reset
```
Demo output
```
=> es_fs write usb 0 sbc-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
Write progress:  87%:+++++++++++++++++++++++++++++++++++++++++++
```

### Essdk deb packeges install
If you find that installing essdk deb packages using `apt install` is too slow, you can download the essdk and ffmpeg deb packages from essdk_ffmpeg_0630.zip [here](https://github.com/eswincomputing/ebc7700-sbc-ubuntu/releases/tag/2025.0730) and install them by `dpkg -i XXXX.deb`.

## Download from network disk
If you are unable to download images from GitHub and you are in China, you can try downloading them [here](https://pan.baidu.com/s/1SL-peBYUcQypXETvam-qCw?pwd=sg94).

## Login to the board Using Serial Console

Default username and password for ubuntu image is `ubuntu`.
On first boot, user will be prompted to change the default password.

## FAQ

**Q**: How to burn or update software?

**A**: Please refer to EBC77 Series Single Board Computer User Guide Chapter 6 Burn program, the section provides a detailed introduction to the installation and update of Bootloader and Ubuntu system images.

---
**Q**: How to switch from HDMI to MIPI DSI output?

**A**: Follow the steps below:
Firstly, when the power is disconnected, you need to connect the raspberry 7-inch screen with the MIPI DSI interface. After startup, log in to the ubuntu system.
Refer to the following command to modify the dtb files that the ubuntu system depends on. After completing the command and restarting, you can use the MIPI DSI interface to output.
```
    sudo su
    cp /lib/firmware/6.6.92-2025-eic7700/device-tree/eswin/eic7700-sbc-a1-mipi.dtb /boot/dtbs/6.6.92-2025-eic7700/eswin/ 	
    ln -sf /boot/dtbs/6.6.92-2025-eic7700/eswin/eic7700-sbc-a1-mipi.dtb /boot/dtb-6.6.92-2025-eic7700
    reboot
```
In addition, if you want to switch back to HDMI, the user needs to operate the following command and then restart.
```
    sudo su
    ln -sf /boot/dtbs/6.6.92-2025-eic7700/eswin/eic7700-sbc-a1.dtb /boot/dtb-6.6.92-2025-eic7700
    reboot
```

---	
**Q**: Which models of sd cards are recommended for loading system images?

**A**: The following models of sd cards have been verified to be compatible with the EBC77 Series Single Board Computer.
- SanDisk Ultra 32GB A1
- SanDisk Ultra 64GB A1
- SanDisk Ultra 256GB A1
- Kingston SDCS2/64GB A1
- Kingston SDCS2/256GB A1
---
**Q**: How to use 2lanes camera?

**A**: At present, need to change the dts and compile kernel. Follow the below steps to config 2lanes imx219 dts:
- find the eic7700-sbc-a1-camera-imx219.dtsi
- change property phy_mode=<5> under vi_subsys node
- change property num_lanes = <2> under mipi_csi2_x node 
- change properties lanes = <1 2> and lanes-dp-dn = <0 0> under csi2_dphy_hw node
- change properties data-lanes = <1 2> and link-frequencies = <456000000> under imx219_x node

You can adapt other sensors based on this ​​imx219 dts​​.

