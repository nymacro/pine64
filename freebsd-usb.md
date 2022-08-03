FreeBSD USB Boot
================

The following instructions can be used to bootstrap FreeBSD on Pine64
to boot from USB disk. The SD card is still required in order to load
U-boot (the A64 can't directly boot from USB), but otherwise the SD
card should not be used.

Requirements:
1. Pine64 (I used an old Pine64+)
2. SD card to flash FreeBSD 13.x Pine64 image onto
3. USB disk

NOTE: You may need to play around with where the USB devices are plugged
into during this process. For me, the Pine64's upper USB doesn't correctly
initialize -- so I had to swap around USB ports many times till things
were configured as required.

# Instructions

## 1. Flash SD card
Download the `FreeBSD-13.1-RELEASE-arm64-aarch64-PINE64.img` from the
FreeBSD website.

```sh
dd if=FreeBSD-13.1-RELEASE-arm64-aarch64-PINE64.img of=/dev/YOUR_DEVICE bs=1M conv=sync
```


## 2. Run bsdinstall
Run `bsdinstall` and set up your USB disk as you like.

## 3. Edit newly installed FreeBSD environment

Copy bootloader configuration `/boot/loader.conf` across to new install.
The `loader.conf` installed to your USB disk by `bsdinstall` will likely
be empty. Without this, you will just boot to a blank screen.

```sh
cp /boot/loader.conf /mnt/boot/loader.conf
```

Ensure your `fstab` correctly refers to the appropriate device names.
If you had another USB disk uplugged in when running `bsdinstall`,
it might not be the correct value.

In nearly all instances, it should be using `da0`.

If your `fstab` isn't correctly configured, the FreeBSD loader might
prompt you to type in a new value to use as `vfs.root.mountfrom`.


## 4. Reboot and change U-boot boot priority

TODO: check exact commands for example

```
> env print boot_targets
> env set boot_targets fel usb ...
> saveenv
```

## 5. Profit

If you have configured everything correctly, you should see everything
boot correctly from your USB disk!
