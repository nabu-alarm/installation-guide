# Installing Arch Linux ARM on the Xiaomi Pad 5

## Installation

### Prerequisites

- `Unlocked bootloader`
- `PC/Laptop with Windows 10-11/Linux/macOS`
- [`Android platform tools`](https://developer.android.com/studio/releases/platform-tools)
- [`Modified recovery image`](https://github.com/nabu-alarm/installation-guide/releases/download/files/recovery.img)
- [`UEFI installer`](https://github.com/nabu-alarm/installation-guide/releases/download/files/uefi-installer.zip)
- [`RootFS image`](https://github.com/nabu-alarm/images/releases)

### Notes
>
> [!NOTE]
> You can use any Android for dualboot - MIUI/Hyper OS or any custom ROM

> [!Warning]
> All your data will be erased! Back up now if needed.
>
> DO NOT REBOOT YOUR TABLET if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/nabulinux)

> [!NOTE]
> If any of `adb` or `fastboot` command fails with `command not found` error check if the path to the platform-tools directory is added to the PATH environment variable

> [!NOTE]
> This guide assumes that you're using cmd on Windows and bash on Linux/macOS as your shell

### Reboot into fastboot mode

- Boot your tablet holding the `volume down` button
- Alternatively, if you have enabled usb debugging, run the following command while booted in Android

```bash
adb reboot bootloader
```

### Boot modified recovery image
>
> While in fastboot mode, replace path/to/recovery.img with the actual path of the recovery image

```bash
fastboot boot path/to/recovery.img
```

### Make a backup of your existing boot image
>
> While in recovery mode, run the following command
>
> Save this image. It will be required to remove the UEFI

<details>
    <summary>Windows</summary>

```cmd
adb shell "dd if=/dev/block/platform/soc/1d84000.ufshc/by-name/boot$(getprop ro.boot.slot_suffix) of=/tmp/normal_boot.img" && adb pull /tmp/normal_boot.img
```

</details>
<details>
    <summary>Linux/macOS</summary>

```cmd
adb shell 'dd if=/dev/block/platform/soc/1d84000.ufshc/by-name/boot$(getprop ro.boot.slot_suffix) of=/tmp/normal_boot.img' && adb pull /tmp/normal_boot.img
```

</details>

### Partitioning your device

> Replace $size with the amount of storage you want Linux to have (do not add GB, just write the number)
> If it asks you to run it once again, do so

```bash
adb shell partition $size
```

### Reboot into fastboot mode

```bash
adb reboot bootloader
```

### Flash linux rootfs image to the linux partition
>
> While in fastboot mode, replace path/to/rootfs.img with the actual path of the rootfs image
> Decompress the image before flashing

```bash
fastboot flash linux path/to/rootfs.img
```

### Boot modified recovery image
>
> While in fastboot mode, replace path/to/recovery.img with the actual path of the recovery image

```bash
fastboot boot path/to/recovery.img
```

### Create a user account
>
> While in recovery mode, replace $username and $password with the actual username and password

```bash
adb shell lon-chroot /opt/nabu/postinstall setup-user $username $password
```

### Install the refind boot manager
>
> While in recovery mode, run the following command

```bash
adb shell lon-chroot /opt/nabu/postinstall extract-efi
```

### Install UEFI
>
> While in recovery mode, replace path/to/uefi-installer.zip with the actual path of the uefi installer

```bash
adb shell twrp sideload
adb wait-for-sideload
adb sideload /path/to/uefi-installer.zip
```

### Reboot your tablet

```bash
adb reboot
```

<details>
    <summary>How to use the boot picker</summary>

| Action         | Key           |
|----------------|---------------|
| Next item      | `Volume down` |
| Previous item  | `Volume up`   |
| Select an item | `Power`       |

</details>
