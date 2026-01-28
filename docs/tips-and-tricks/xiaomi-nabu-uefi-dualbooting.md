# Xiaomi Pad 5 UEFI Dualbooting guide

> [!CAUTION]
> This guide is tested on DerpFest ROM. I don't tested it on HyperOS/MIUI stock roms.
> ***Please read this guide with caution! We are not responsible for any carelessness or damaged devices; that is your responsibility. You have been warned.***
>
> Also, it's recommended to restore partitions and do clean flash your rom/stock, as described below.
>
> *Your data will be lost.*

> [!IMPORTANT]
> This guide is WIP.

## Preparation

First, you need a PC running linux or windows with adb and fastboot installed. For windows, recommended to install 7-zip.

Also, you should to prepare some files before starting:
 - Your custom or stock rom
 - Pocketblue install files
 - [Modded TWRP by ArKT-7](https://github.com/ArKT-7/nabu-files/raw/refs/heads/main/recovery/mod-linux-twrp.img)
 - [jhuang6451's fedora esp-files](https://github.com/jhuang6451/nabu_fedora/releases/download/release-20260119-0437/efi-files-43.0.zip)
 - [Aloha installer](https://github.com/rodriguezst/nabu-dualboot-img/releases/download/20250422/installer_bootmanager_NOSB.zip)

You should unpack pocketblue's `fedora_esp.raw` with 7-zip on Windows. Or, just mount it somewhere on Linux. Like
```
sudo mount pocketblue/images/fedora_esp.raw /mnt
```

---

Then, boot into TWRP:
```
fastboot boot twrp.img
```

Press onto **Penguin** button, and press on "Restore part..." and type "yes". It will restore your partition table for further repartitioning.

## Installing Android and Pocketblue

Flash your prefered custom, or stock rom and boot into TWRP again:
```
fastboot boot twrp.img
```

Go to the **Penguin** menu again, and press on "Partitioning". It will ask for linux partition size. I'll recomment a half of android size.

Then, boot into bootloader, and flash some pocketblue files (notice, we don't flash esp from it)
```
adb reboot bootloader
fastboot flash cust pocketblue/images/fedora_boot.raw
fastboot flash linux pocketblue/images/fedora_rootfs.raw
fastboot reboot bootloader # It's normal if the reboot freezes. The tablet needs to write images from RAM to the ufs.
```

## Installing UEFI and efi files.

Boot into twrp again:
```
fastboot boot twrp.img
```

Activate sideload and flash the aloha installer zip:
```
adb sideload installer_bootmanager_NOSB.zip
```

Copy `ESP` from efi-files, and delete old `/esp/EFI/fedora`:
```
adb push efi-files/EFI /esp
adb shell rm -r /esp/EFI/fedora
```

Copy pocketblue's `fedora` to esp:
```
adb push /mnt/EFI/fedora /mnt/EFI
```

## Fin

And... here you go! After you reboot, you should see rEFInd menu! There's some controls:
 - Volume buttons for choosing
 - Power button for selecting

Enjoy your uefi dualboot!
