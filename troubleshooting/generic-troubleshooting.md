---
description: If your tablet run into problems...
---

# ❕ Generic troubleshooting

###

{% hint style="info" %}
I am bad at writing troubleshooting guide, so feel free to change this page at anytime.
{% endhint %}

### Device rebooted to fastboot after several reboots

Try re-flashing `boot.img` image to solve this problem.

```bash
fastboot flash boot /path/to/boot.img
```

### Device booted with graphical glitches

Sometimes, your tablet can run into graphical glitch. This is because the GPU driver didn't load properly, a force reboot can solve this problem. This situation can also take place when tablet is woke up from suspend mode when the "Close lid to suspend" feature was added in kernel 6.1 . The solution is simple: Press power button (or close the cover on your case, )to suspend, then press power button again (or open the cover ) to unlock your tablet.

### Kernel panic: unable to mount rootfs at unknown x,y(x and y are some random numbers)&#x20;

In vary rare times, this error will happen when your Ubuntu partition is erased by accident. Or you may have a faulty kernel build.

This error will also take place when you flashed at least two `boot` images in different partitions.

### Black screen after kernel logs

This means you didn't flash the rootfs image properly. Follow the [installation guide](../installation-guide/install-ubuntu-on-your-tablet.md) to flash it. If you don't have it, you can download it now.

### No console output when using UEFI to boot kernel

This can be happened with either GRUB issues, faulty kernel, Kconfig file missing, or an incorrect device tree is used.

If you successfully installed GRUB, and got the proper Kconfig (kernel configuration ) file and kernel, you probably used a incorrect device tree. Ensure you can use UEFI boot at anytime, you should get the device tree file from [original kernel repository](https://github.com/inofficial-linux-on-nabu/linux-kernel-6.1.10).
