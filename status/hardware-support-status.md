# 🖥 Hardware support status

This page shows the device's feature that is working in [mainline linux](https://github.com/map220v/sm8150-mainline) kernel.



<table><thead><tr><th>Features</th><th data-type="checkbox">Linux 6.1 LTS</th><th data-type="checkbox">Latest mainline kernel</th></tr></thead><tbody><tr><td>USB networking</td><td>true</td><td>false</td></tr><tr><td>Display</td><td>true</td><td>true</td></tr><tr><td>WLAN</td><td>true</td><td>true</td></tr><tr><td>FDE</td><td>false</td><td>false</td></tr><tr><td>Battery</td><td>true</td><td>true</td></tr><tr><td>Graphics Acceleration</td><td>true</td><td>true</td></tr><tr><td>Audio</td><td>true</td><td>true</td></tr><tr><td>Bluetooth**</td><td>true</td><td>true</td></tr><tr><td>Camera</td><td>false</td><td>false</td></tr><tr><td>Storage</td><td>true</td><td>true</td></tr><tr><td>NFC</td><td>false</td><td>false</td></tr><tr><td>FOSS Bootloader*</td><td>true</td><td>true</td></tr><tr><td>Stylus</td><td>false</td><td>false</td></tr><tr><td>Keyboard</td><td>true</td><td>true</td></tr><tr><td>OTG</td><td>true</td><td>false</td></tr><tr><td>Ethernet</td><td>true</td><td>true</td></tr><tr><td>Fast charge</td><td>true</td><td>true</td></tr><tr><td>Sleep</td><td>true</td><td>false</td></tr></tbody></table>

_\*As we all know, Mi Pad 5 has an active UEFI support, for both_ [_Renegade_](https://github.com/edk2-porting/edk2-msm/tree/master/Platform/Xiaomi/sm8150)_'s UEFI and_ [_Microsoft Project MU_ ](https://github.com/woa-msmnile/msmnilePkg)_support. Now the Linux 6.1 can be booted via SimpleInit, the mainline kernel can also boot, too._

_\*\*On latest mainline(since 6.4 when I tested) kernel, Bluetooth is working, but is not able to search for or connect to devices._
