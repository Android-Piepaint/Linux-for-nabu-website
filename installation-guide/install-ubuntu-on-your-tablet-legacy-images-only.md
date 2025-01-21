---
icon: ubuntu
---

# Install Ubuntu on your tablet (Legacy images only)

This document is a guide for installing Ubuntu Linux on your Xiaomi Mi Pad 5 with the latest mainline kernel.

[_**We don’t take any responsibility for any damage done to your device. By following this guide, you agree to take full responsibility of your actions.**_](#user-content-fn-1)[^1]

{% hint style="info" %}
**PLEASE READ SLOWLY AND CAREFULLY!! BE SURE TO UNDERSTAND THE ENTIRE GUIDE BEFORE STARTING!!**&#x20;

**Don’t run all commands at once** and **don’t rerun the commands if you interrupt the process**. You  **need** to be familiar with command line interfaces beforehand and you **must not** commit any typo with any commands. _You may permanently break your device!_
{% endhint %}

### Download images

You can get a copy of Ubuntu image in the "[Download](../download/resource-download.md)" section for free.

Download them and continue reading...



### Partition the UFS



{% hint style="success" %}
This procedure will not erase your Android data files, since Android will automatically fixing the `userdata` partition.
{% endhint %}

To modify the partitions on the UFS, we'll need to download a 3rd-party recovery environment called "Orangefox Recovery" Link is here below:

{% embed url="https://drive.google.com/file/d/1gCNtoDMNCAmMR61xegvCC3mxv28gMJbi/view?usp=drive_link" %}

Once you've downloaded, open a terminal and type the following commands:

```bash
adb reboot bootloader
fastboot boot /path/to/recovery.img 
```

This will start booting recovery image. Once the screen is on, use the terminal to continue processing. We well use `adb shell` command to finish the rest of this guide. Enter this command in your terminal, it well help you to check the userdata partition's location:

```bash
ls -l /dev/block/bootdevice/by-name/ | grep userdata
lrwxrwxrwx 1 root root 16 1971-06-22 05:04 userdata -> /dev/block/sda31    #Example output will like this.
```

In this example, the `userdata` partition is located in the 31st partition of the whole disk. It has the biggest size, compared with other partitions. So this is the key for requiring new space for our Ubuntu installation.&#x20;

To resize the `userdata` partition, we'll need to use the [parted](https://renegade-project.tech/tools/parted.7z) command tools to do this. Now let's open `adb shell` again and start typing `parted` in the terminal.

```bash
parted /dev/block/sda
GNU Parted 3.3
Using /dev/block/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
```

Enter `print` command to list all partitions for `/dev/block/sda` :



{% hint style="info" %}
If you are using newer recovery like [TWRP](https://sourceforge.net/projects/xiaomi-pad-5/files/TWRP/Android%2014/) or PBRP Recovery, you need to download `parted` and use `adb` command to copy it to internal storage. Run these commands to finish this:

```bash
adb push parted /tmp/
adb shell chmod 755 /tmp/parted
adb shell
```
{% endhint %}

Then you will see your current partition table with **`userdata` being the last partition**\
Below is an example of output:

```bash
(parted) print
print
Model: SAMSUNG KLUDG4UHDC-B0E1 (scsi)
Disk /dev/block/sda: 126GB
Sector size (logical/physical): 4096B/4096B
Partition Table: gpt
Disk Flags:
Number  Start   End     Size    File system  Name             Flags
 1      24.6kB  32.8kB  8192B                switch
 2      32.8kB  65.5kB  32.8kB               ssd
 3      65.5kB  98.3kB  32.8kB               dbg
 4      98.3kB  131kB   32.8kB               bk01
 5      131kB   262kB   131kB                bk02
 6      262kB   524kB   262kB                bk03
 7      524kB   1049kB  524kB                bk04
 8      1049kB  1573kB  524kB                keystore
 9      1573kB  2097kB  524kB                frp
10      2097kB  4194kB  2097kB               countrycode
11      4194kB  8389kB  4194kB               misc
12      8389kB  12.6MB  4194kB               vm-data
13      12.6MB  16.8MB  4194kB               bk06
14      16.8MB  25.2MB  8389kB               logfs
15      25.2MB  33.6MB  8389kB               ffu
16      33.6MB  50.3MB  16.8MB               oops
17      50.3MB  67.1MB  16.8MB               devinfo
18      67.1MB  83.9MB  16.8MB               oem_misc1
19      83.9MB  101MB   16.8MB  ext4         metadata
20      101MB   134MB   32.9MB               bk08
21      134MB   168MB   34.2MB               splash
22      168MB   201MB   33.6MB               bk09
23      201MB   9328MB  9127MB               super
24      9328MB  9328MB  131kB                vbmeta_system_a
25      9328MB  9328MB  131kB                vbmeta_system_b
26      9328MB  9396MB  67.1MB               logdump
27      9396MB  9530MB  134MB                minidump
28      9530MB  9664MB  134MB                rawdump
29      9664MB  10.7GB  1074MB  ext4         cust
30      10.7GB  10.9GB  134MB   ext4         rescue
31      10.9GB  126GB   115GB                userdata
```

Now let’s continue partitioning:\
Here the size of `userdata` can be decided by yourself.

Delete partition 31 and again make sure it is not deleted incorrectly.

```bash
(parted) rm 31
rm 31
```



{% hint style="info" %}
126GB is the `End` value for the new `userdata` partition.\
Since the starting point for `userdata` is 10.9GB, the new size would be `126G - 10.9G = 115G`.
{% endhint %}

Check the results:

```bash
(parted) print
print
Model: SAMSUNG KLUDG4UHDC-B0E1 (scsi)
Disk /dev/block/sda: 126GB
Sector size (logical/physical): 4096B/4096B
Partition Table: gpt
Disk Flags:
Number  Start   End     Size    File system  Name             Flags
 1      24.6kB  32.8kB  8192B                switch
 2      32.8kB  65.5kB  32.8kB               ssd
 3      65.5kB  98.3kB  32.8kB               dbg
 4      98.3kB  131kB   32.8kB               bk01
 5      131kB   262kB   131kB                bk02
 6      262kB   524kB   262kB                bk03
 7      524kB   1049kB  524kB                bk04
 8      1049kB  1573kB  524kB                keystore
 9      1573kB  2097kB  524kB                frp
10      2097kB  4194kB  2097kB               countrycode
11      4194kB  8389kB  4194kB               misc
12      8389kB  12.6MB  4194kB               vm-data
13      12.6MB  16.8MB  4194kB               bk06
14      16.8MB  25.2MB  8389kB               logfs
15      25.2MB  33.6MB  8389kB               ffu
16      33.6MB  50.3MB  16.8MB               oops
17      50.3MB  67.1MB  16.8MB               devinfo
18      67.1MB  83.9MB  16.8MB               oem_misc1
19      83.9MB  101MB   16.8MB  ext4         metadata
20      101MB   134MB   32.9MB               bk08
21      134MB   168MB   34.2MB               splash
22      168MB   201MB   33.6MB               bk09
23      201MB   9328MB  9127MB               super
24      9328MB  9328MB  131kB                vbmeta_system_a
25      9328MB  9328MB  131kB                vbmeta_system_b
26      9328MB  9396MB  67.1MB               logdump
27      9396MB  9530MB  134MB                minidump
28      9530MB  9664MB  134MB                rawdump
29      9664MB  10.7GB  1074MB  ext4         cust
30      10.7GB  10.9GB  134MB   ext4         rescue
```

Note the end of the last partition in the above list, 10.9GB, this number will be used as the start of the new `userdata` partition, followed by the end of the partition. Let say that we want to make an approx 40GB `userdata` partititon using the following command:&#x20;

```bash
(parted) mkpart userdata   10.9GB 50GB
mkpart userdata   10.9GB 50GB    #If you added "ext4" behind the "userdata" text, your data will be erased when you reboot. 
```

Between `userdata` and 10.9GB are 3 spaces, one of them replace the partition type flag, it is important to use 3 spaces at this step.

Run print again to see the results.

```bash
(parted) print
print
Model: SAMSUNG KLUDG4UHDC-B0E1 (scsi)
Disk /dev/block/sda: 126GB
Sector size (logical/physical): 4096B/4096B
Partition Table: gpt
Disk Flags:
Number  Start   End     Size    File system  Name             Flags
 1      24.6kB  32.8kB  8192B                switch
 2      32.8kB  65.5kB  32.8kB               ssd
 3      65.5kB  98.3kB  32.8kB               dbg
 4      98.3kB  131kB   32.8kB               bk01
 5      131kB   262kB   131kB                bk02
 6      262kB   524kB   262kB                bk03
 7      524kB   1049kB  524kB                bk04
 8      1049kB  1573kB  524kB                keystore
 9      1573kB  2097kB  524kB                frp
10      2097kB  4194kB  2097kB               countrycode
11      4194kB  8389kB  4194kB               misc
12      8389kB  12.6MB  4194kB               vm-data
13      12.6MB  16.8MB  4194kB               bk06
14      16.8MB  25.2MB  8389kB               logfs
15      25.2MB  33.6MB  8389kB               ffu
16      33.6MB  50.3MB  16.8MB               oops
17      50.3MB  67.1MB  16.8MB               devinfo
18      67.1MB  83.9MB  16.8MB               oem_misc1
19      83.9MB  101MB   16.8MB  ext4         metadata
20      101MB   134MB   32.9MB               bk08
21      134MB   168MB   34.2MB               splash
22      168MB   201MB   33.6MB               bk09
23      201MB   9328MB  9127MB               super
24      9328MB  9328MB  131kB                vbmeta_system_a
25      9328MB  9328MB  131kB                vbmeta_system_b
26      9328MB  9396MB  67.1MB               logdump
27      9396MB  9530MB  134MB                minidump
28      9530MB  9664MB  134MB                rawdump
29      9664MB  10.7GB  1074MB  ext4         cust
30      10.7GB  10.9GB  134MB   ext4         rescue
31      10.9GB  50.0GB  39.1GB               userdata
```

Exit the parted tool finally.

Now `userdata` resizing is done. Restart your tablet to apply changes.





{% hint style="info" %}
Here comes a fun thing:

Android stores your data inside the `userdata` partition. When you reset your Android device to its factory default settings, your data will be erased. This is because factory reset is equal to format the `userdata` partition using ext4 filesystem. However, if you delete the `userdata` partition and recreate it without formating, your data will be saved after a reboot. Probably because Android can fix this problem automatically.
{% endhint %}



{% hint style="danger" %}
```
If you added "ext4" behind the "userdata" text, your data will be erased when you reboot. 
```
{% endhint %}



### Install new system

Enter fastboot mode and repeat the [previous steps](install-ubuntu-on-your-tablet-legacy-images-only.md#partition-the-ufs).

We'll use the free space for our Ubuntu installtion:

```bash
(parted) mkpart pmos ext4 50.0GB 126GB
mkpart pmos ext4 50.0GB 126GB
```

The output will look like this:

```bash
(parted) print free
print free
Model: SAMSUNG KLUDG4UHDC-B0E1 (scsi)
Disk /dev/block/sda: 126GB
Sector size (logical/physical): 4096B/4096B
Partition Table: gpt
Disk Flags:
Number  Start   End     Size    File system  Name             Flags
        12.3kB  24.6kB  12.3kB  Free Space
 1      24.6kB  32.8kB  8192B                switch
 2      32.8kB  65.5kB  32.8kB               ssd
 3      65.5kB  98.3kB  32.8kB               dbg
 4      98.3kB  131kB   32.8kB               bk01
 5      131kB   262kB   131kB                bk02
 6      262kB   524kB   262kB                bk03
 7      524kB   1049kB  524kB                bk04
 8      1049kB  1573kB  524kB                keystore
 9      1573kB  2097kB  524kB                frp
10      2097kB  4194kB  2097kB               countrycode
11      4194kB  8389kB  4194kB               misc
12      8389kB  12.6MB  4194kB               vm-data
13      12.6MB  16.8MB  4194kB               bk06
14      16.8MB  25.2MB  8389kB               logfs
15      25.2MB  33.6MB  8389kB               ffu
16      33.6MB  50.3MB  16.8MB               oops
17      50.3MB  67.1MB  16.8MB               devinfo
18      67.1MB  83.9MB  16.8MB               oem_misc1
19      83.9MB  101MB   16.8MB  ext4         metadata
20      101MB   134MB   32.9MB               bk08
21      134MB   168MB   34.2MB               splash
22      168MB   201MB   33.6MB               bk09
23      201MB   9328MB  9127MB               super
24      9328MB  9328MB  131kB                vbmeta_system_a
25      9328MB  9328MB  131kB                vbmeta_system_b
26      9328MB  9396MB  67.1MB               logdump
27      9396MB  9530MB  134MB                minidump
28      9530MB  9664MB  134MB                rawdump
29      9664MB  10.7GB  1074MB  ext4         cust
30      10.7GB  10.9GB  134MB   ext4         rescue
        10.9GB  10.9GB  786kB   Free Space
31      10.9GB  50.0GB  39.1GB               userdata
32      50.0GB  126.0GB  76.0GB  ext4         pmos
        
```

Exit the parted tool and reboot once again.

Now we need to check which slot Android is installed.



{% hint style="info" %}
**NOTE:** The concept of "slot"  probably unfamiliar for you, since it was included in Android 10 as a feature, which called "**Dynamic Partitions**". Dynamic partitions are a userspace partitioning system for Android. About its further infomations, [located here](https://source.android.com/docs/core/ota/dynamic_partitions/implement).
{% endhint %}

You can check this infomation via `fastboot` . Command is listed below:

```bash
fastboot getvar current-slot
current-slot: b
Finished. Total time: 0.004s
```

Force select Slot A as active slot:

```bash
fastboot set_active a
Setting current slot to 'a'                        OKAY [  0.049s]
Finished. Total time: 0.064s
```

Now, we are ready to flash the system image. But before we start, we need to disable Android Verified Boot (AVB) feature, otherwise it will pervent booting Ubuntu system.



{% hint style="info" %}
AVB is implementation of verified boot process, current version (since Android 8 Oreo) is called AVB 2.0. Verified boot is a process of assuring the end user of the integrity of the software running on a device. It typically starts with a read-only portion of the device firmware which loads code and executes it only after cryptographically verifying that the code is authentic. It also helps in implementing rollback protection.

&#x20;More infomations and technical details, [located here.](https://wiki.postmarketos.org/wiki/Android_Verified_Boot_\(AVB\))
{% endhint %}

Flash the `vbmeta` with `vbmeta_disabled.img` to disable this feature:

```bash
fastboot flash vbmeta_a vbmeta_disabled.img
Sending 'vbmeta_a' (4 KB)                          OKAY [  0.007s]
Writing 'vbmeta_a'                                 OKAY [  0.003s]
Finished. Total time: 0.048s
```

Erase Android `DTBO` partition, we do not need it, but if present will be loaded and will prevent our boot.



{% hint style="info" %}
If you want to boot your system via UEFI, skip this.
{% endhint %}

```bash
fastboot erase dtbo_a
Erasing 'dtbo_a'                                   OKAY [  0.007s]
Finished. Total time: 0.022s
```

Flash `boot.img :`

```bash
flash boot_a boot.img
Sending 'boot_a' (13052 KB)                        OKAY [  0.327s]
Writing 'boot_a'                                   OKAY [  0.052s]
Finished. Total time: 0.663s
```

Flash `rootfs` image, please use the partition name you created, this process will takew a while, grab another beer.

```bash
fastboot flash partition_name /path/to/rootfs.img
```

Finally, reboot and you are ready to rock!

[^1]: 
