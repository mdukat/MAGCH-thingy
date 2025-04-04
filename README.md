# MAGCH-thingy
Documenting reverse-engineering of MAGCH M610-EEA Android tablet

## Goal
 - Android 7+ ROM with support for at least:
   - Wifi
   - Touchscreen
   - GPU
   - USB OTG

## Preinstalled software
> TODO add information on android build, kernel, etc.

## IC's on PCB
PCB has identification silkscreen text of `R863T-RK3326S-V1.0-M`. CPU-Z shows RK3066.
 - GSL1680 - Capacative touch screen controller ([docs](https://dl.linux-sunxi.org/touchscreen/GSL1680.pdf))
 - RK817-6 - Power management IC ([docs](https://rockchip.fr/RK817%20datasheet%20V1.01.pdf))
 - RTL8723CS - WiFi/BT ([some kernel driver development](https://github.com/Icenowy/rtl8723cs), [docs](https://datasheet4u.com/pdf-down/R/T/L/RTL8723CS-Realtek.pdf))
 - ARTMEM AT70B32G4 - eMMC ([B16* docs](https://bbs.aw-ol.com/assets/uploads/files/1659577035353-4f411bbf-e07a-4bfe-9996-ed2a12deeabe-at70b16g4t06f_datasheet_v0.1.pdf))
 - RAM (?) SEC 646 K3QF0F0 0AMFGCF - Probably RAM ([some random sell offer online with same "SEC 646"](https://www.cpuprocessorchip.com/sale-11104366-emcp-32gb-emmc-flash-drive-storage-kmqx10013m-b419-32-16-emcp-d3-lpddr3-1866mhz.html), [archive](https://web.archive.org/web/20240813145758/https://www.cpuprocessorchip.com/sale-11104366-emcp-32gb-emmc-flash-drive-storage-kmqx10013m-b419-32-16-emcp-d3-lpddr3-1866mhz.html))
 - Rockchip RK3326 S - CPU ([docs](https://rockchip.fr/RK3326%20datasheet%20V1.2.pdf))
 - LTK5135M - Audio amplifier ([docs](https://www.chipsourcetek.com/DataSheet/LTK5135.pdf))
