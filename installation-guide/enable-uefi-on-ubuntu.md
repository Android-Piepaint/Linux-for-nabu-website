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
git clone https://github.com/map220v/sm8150-mainline.git
```

Once it finished, go to the repository's directory. We need to find the proper device tree file that ended with file extension name (\*.dts). It is stored under `arch/arm64/boot/dts` . `cd` into this directory, find the DTS file named "`sm8150-xiaomi-nabu.dts`".

Now, let's install some basic tool for compiling the Linux kernel.

On Ubuntu and other Debian-based distributions:

```bash
sudo apt build-dep linux linux-image-unsigned-$(uname -r)
```

Unfortunately, the above does not install all of the necessary dependencies. The current Disco Dingo release requires the following additional packages.&#x20;

* ```bash
  sudo apt install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf llvm gcc-aarch64-linux-gnu
  ```

If you are going to be using git, install it via:&#x20;

* ```bash
  sudo apt install git
  ```

{% hint style="info" %}
This is an Ubuntu-specific hint. For Debian-based distributions, skip readng this.

The above command requires your system to have the correct `deb-src` lines in `/etc/apt/sources.list`. For example, on Disco Dingo you should have:&#x20;

* ```purebasic
  deb-src http://archive.ubuntu.com/ubuntu disco main
  deb-src http://archive.ubuntu.com/ubuntu disco-updates main
  ```
{% endhint %}

On Arch Linux(and distributions based on it):

Install the [base-devel](https://archlinux.org/packages/?name=base-devel) [meta package](https://wiki.archlinux.org/title/Meta\_package), which pulls in necessary packages such as [make](https://archlinux.org/packages/?name=make) and [gcc](https://archlinux.org/packages/?name=gcc). It is also recommended to install the following packages, as listed in the default Arch kernel [PKGBUILD](https://gitlab.archlinux.org/archlinux/packaging/packages/linux/-/blob/main/PKGBUILD): [xmlto](https://archlinux.org/packages/?name=xmlto), [kmod](https://archlinux.org/packages/?name=kmod), [inetutils](https://archlinux.org/packages/?name=inetutils), [bc](https://archlinux.org/packages/?name=bc), [libelf](https://archlinux.org/packages/?name=libelf), [git](https://archlinux.org/packages/?name=git), [cpio](https://archlinux.org/packages/?name=cpio), [perl](https://archlinux.org/packages/?name=perl), [tar](https://archlinux.org/packages/?name=tar), [xz](https://archlinux.org/packages/?name=xz).

```bash
sudo pacman -S base-devel make gcc git gcc-aarch64-linux-gnu
```

Now compile the DTS file into DTB files:

```bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- dtbs
```

You should have the Device tree files now.

### Compile the Linux kernel

Simply enter these commands on terminal:

{% code overflow="wrap" fullWidth="true" %}
```bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image.gz dtbs
make ARCH=arm64 install INSTALL_PATH=../install/boot
make ARCH=arm64 dtbs_install INSTALL_DTBS_PATH=../install/boot/dtbs

make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- modules
rm -rf ../install/lib/modules/
make ARCH=arm64 modules_install INSTALL_MOD_PATH=../install
```
{% endcode %}

Notice the Image.gz is the Android kernel image we won't use. After compiling kernel, you will find the two files we need: a sysmap file and the final kernel image(name begin with "vmlinuz"). Copy them to your tablet's EFI directory.

