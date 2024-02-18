---
description: A simple user guide helps you to enable UEFI boot on your tablet.
---

# Enable UEFI on Ubuntu



### Get the Device Tree config file

In [computing](https://en.wikipedia.org/wiki/Computing), a devicetree (also written device tree) is a [data structure](https://en.wikipedia.org/wiki/Data\_structure) describing the hardware components of a particular computer so that the [operating system](https://en.wikipedia.org/wiki/Operating\_system)'s [kernel](https://en.wikipedia.org/wiki/Kernel\_\(operating\_system\)) can use and manage those components, including the [CPU](https://en.wikipedia.org/wiki/Central\_processing\_unit) or CPUs, the [memory](https://en.wikipedia.org/wiki/Computer\_memory), the [buses](https://en.wikipedia.org/wiki/Bus\_\(computing\)) and the [integrated peripherals](https://en.wikipedia.org/wiki/Integrated\_peripheral).



{% hint style="info" %}
Device tree is widely used on ARM64 platform when it uses Linux kernel, instead of ACPI. The latter can be found on most x86\_64 based devices.

Systems which use device trees usually pass a static device tree (perhaps stored in [EEPROM](https://en.wikipedia.org/wiki/EEPROM), or stored in NAND device like [eUFS](https://en.wikipedia.org/wiki/EUFS)) to the operating system, but can also generate a device tree in the early stages of [booting](https://en.wikipedia.org/wiki/Booting).&#x20;
{% endhint %}

On Renegade Project's UEFI firmware, the device tree is used when your device is booting Linux kernel via EFI stub. To get the device tree file, we need to look up it in kernel repository. So open your terminal, and type this command, it will simply download the whole repository and store it on your disk:

```bash
git clone https://github.com/inofficial-linux-on-nabu/linux-kernel-6.1.10.git
```

Once it finished, go to the repository's directory. We can look at its structure:

