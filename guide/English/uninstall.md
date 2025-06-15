# Installing Arch Linux ARM on the Xiaomi Pad 5

## Uninstallation

### Prerequisites

- `Unlocked bootloader`
- `PC/Laptop with Windows 10-11/Linux/macOS`
- [`Android platform tools`](https://developer.android.com/studio/releases/platform-tools)
- [`Modified recovery image`](https://github.com/nabu-alarm/installation-guide/releases/download/files/recovery.img)

### Notes
>
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

### Flash backed up boot image
>
> While in fastboot mode, replace path/to/normal-boot.img with the actual path of the boot image

```bash
fastboot flash boot path/to/normal-boot.img
```

### Boot modified recovery image
>
> While in fastboot mode, replace path/to/recovery.img with the actual path of the recovery image

```bash
fastboot boot path/to/recovery.img
```

### Restore stock partitons
>
> While in recovery mode, run the following command

```bash
adb shell restore
```

### Reboot your tablet

```bash
adb reboot
```
