# EBC7700-SBC-Ubuntu
Ubuntu Releases for EBC7700 Series Single Board Computer.

## Description

Ubuntu Image releases for EBC7700 Series Single Board Computer.
- Based on Ubuntu 24.04.2 LTS.
- Prebuilt Ubuntu image in compressed format named `sbc-ubuntu-24.04-preinstalled-server-riscv64.img.zst`.
- Please ensure that the validated combination of the bootchain image and the Ubuntu image are flashed to the board. The release notes provide the version and validation details.

## Burning
Copy the bootloader and Ubuntu image to an ext4/fat32 formatted USB disk, connect it to the usb2.0 port of the sbc board, and enter the uboot command line mode through the serial port.

### Bootloader burning

If your device already has a compatible bootloader installed, you can skip this step. You can use fatload instead of ext4load if using fat32 USB disk.

    #ext4load usb 0 0x90000000 bootloader.bin
    #es_burn write 0x90000000 flash

### Ubuntu image burning

    #es_fs write usb 0 sbc-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
    #reset

## Login to the board Using Serial Console

Default username and password for ubuntu image is `ubuntu`.
On first boot, user will be prompted to change the default password.
