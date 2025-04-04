# MAGCH-thingy
Documenting reverse-engineering of MAGCH M610-EEA Android tablet

## Goal
 - Android 7+ ROM with support for at least:
   - Wifi
   - Touchscreen
   - GPU
   - USB OTG

## Device info
> Information from CPU-Z
 - Model: M610-EEA
 - Brand: MAGCH
 - Manufacturer: incar
 - Board: rk30sdk
 - Hardware: rk30board
 - Screen Resolution: 600x1024
 - Screen Density: 170dpi
 - CPU: 4x ARM Cortex-A35 @ 1.51GHz (Rockchip RK3066)
 - GPU Renderer: Mali-G31

## Preinstalled software
> Information from CPU-Z
 - Android Version 11 (Go)
   - Build ID: MAGCH_863T_RK1_CG_SQ_BOE_1M_YQ3_M610-EEA-V1.1_20231025
 - Kernel 4.19.193 (hxl20220311)
 - Google Play Services: 21.45.16 (170302-414021728)

## IC's on PCB
PCB has identification silkscreen text of `R863T-RK3326S-V1.0-M`. CPU-Z shows RK3066.
 - GSL1680 - Capacative touch screen controller ([docs](https://dl.linux-sunxi.org/touchscreen/GSL1680.pdf))
 - RK817-6 - Power management IC ([docs](https://rockchip.fr/RK817%20datasheet%20V1.01.pdf))
 - RTL8723CS - WiFi/BT ([some kernel driver development](https://github.com/Icenowy/rtl8723cs), [docs](https://datasheet4u.com/pdf-down/R/T/L/RTL8723CS-Realtek.pdf))
 - ARTMEM AT70B32G4 - eMMC ([B16* docs](https://bbs.aw-ol.com/assets/uploads/files/1659577035353-4f411bbf-e07a-4bfe-9996-ed2a12deeabe-at70b16g4t06f_datasheet_v0.1.pdf))
 - RAM (?) SEC 646 K3QF0F0 0AMFGCF - Probably RAM ([some random sell offer online with same "SEC 646"](https://www.cpuprocessorchip.com/sale-11104366-emcp-32gb-emmc-flash-drive-storage-kmqx10013m-b419-32-16-emcp-d3-lpddr3-1866mhz.html), [archive](https://web.archive.org/web/20240813145758/https://www.cpuprocessorchip.com/sale-11104366-emcp-32gb-emmc-flash-drive-storage-kmqx10013m-b419-32-16-emcp-d3-lpddr3-1866mhz.html))
 - Rockchip RK3326 S - CPU ([docs](https://rockchip.fr/RK3326%20datasheet%20V1.2.pdf))
 - LTK5135M - Audio amplifier ([docs](https://www.chipsourcetek.com/DataSheet/LTK5135.pdf))

## ADB Device shows as...
```
$ ./adb devices
List of devices attached
P1127BSD004594  device
```

## ADB Shell...

### RAM usage
Weird... Should be 8GB?
```
free -m
                total        used        free      shared     buffers
Mem:             2963        1969         993          10           3
-/+ buffers/cache:           1965         997
Swap:            2222           0        2222

dumpsys meminfo
Applications Memory Usage (in Kilobytes):
Uptime: 849550 Realtime: 849550


Total RSS by process:
    269,260K: system (pid 423)
    237,220K: com.google.android.gms (pid 1080)
    211,300K: com.cpuid.cpu_z (pid 3982 / activities)
    204,100K: com.android.systemui (pid 549)
    189,572K: com.android.settings (pid 750 / activities)
    182,108K: com.google.android.gms.persistent (pid 965)
    160,428K: com.android.vending (pid 2929)
    160,196K: zygote (pid 298)
    157,744K: com.google.android.inputmethod.latin (pid 1280)
    151,752K: com.android.launcher3 (pid 847 / activities)
    149,628K: com.google.android.gms.unstable (pid 3216)
    145,976K: com.google.android.gm (pid 2414)
    138,272K: com.google.android.webview:sandboxed_process0:org.chromium.content.app.SandboxedProcessService0:0 (pid 4055)
    136,740K: com.google.android.apps.photos (pid 3771)
    136,632K: com.termux (pid 3913 / activities)
    132,236K: com.google.android.gms.ui (pid 3647)
    129,212K: com.android.vending:background (pid 3139)
    126,876K: com.android.phone (pid 726)
    117,956K: com.google.android.calendar (pid 3266)
    116,580K: com.google.android.apps.nbu.files (pid 3404)
    114,296K: com.android.providers.media.module (pid 993)
    113,480K: android.process.media (pid 1442)
     99,488K: com.google.process.gapps (pid 1363)
     99,124K: com.google.android.permissioncontroller (pid 2355)
     99,000K: com.google.process.gservices (pid 1104)
     98,800K: com.google.android.ext.services (pid 771)
     98,280K: com.incar.update (pid 1013)
     96,896K: com.google.android.webview:webview_service (pid 4031)
     92,656K: com.google.android.partnersetup (pid 2588)
     90,748K: com.android.traceur (pid 3670)
     88,844K: com.google.android.configupdater (pid 3594)
     84,428K: com.android.se (pid 682)
     61,040K: webview_zygote (pid 667)
     31,532K: surfaceflinger (pid 186)
     19,352K: mediaserver (pid 362)
     18,148K: media.extractor (pid 360)
     17,416K: cameraserver (pid 352)
     16,272K: audioserver (pid 315)
     14,404K: media.swcodec (pid 373)
     13,940K: media.codec (pid 371)
      9,744K: android.hardware.graphics.composer@2.1-service (pid 218)
      7,560K: android.hardware.audio.service (pid 303)
      7,496K: update_engine (pid 377)
      7,296K: init (pid 1)
      7,224K: netd (pid 297)
      6,816K: android.hardware.wifi@1.0-service-lazy (pid 536)
      6,584K: vold (pid 166)
      6,540K: android.hardware.graphics.allocator@4.0-service (pid 213)
      6,436K: keystore (pid 359)
      5,924K: wificond (pid 370)
      5,884K: credstore (pid 316)
      5,668K: ueventd (pid 128)
      5,520K: gatekeeperd (pid 376)
      5,500K: android.hardware.keymaster@4.0-service.optee (pid 185)
      5,384K: gpuservice (pid 318)
      5,356K: init (pid 126)
      5,300K: media.metrics (pid 361)
      5,028K: rockchip.hardware.rockit.hw@1.0-service (pid 314)
      5,016K: installd (pid 358)
      4,992K: adbd (pid 3862)
      4,824K: android.hardware.weaver@1.0-service (pid 309)
      4,808K: hwservicemanager (pid 164)
      4,740K: android.hardware.gatekeeper@1.0-service.optee (pid 305)
      4,572K: storaged (pid 369)
      4,564K: drmserver (pid 353)
      4,388K: android.hardware.boot@1.1-service (pid 184)
      4,316K: android.hardware.health@2.1-service (pid 306)
      4,308K: statsd (pid 296)
      4,208K: android.hardware.sensors@1.0-service (pid 308)
      4,164K: android.hardware.bluetooth@1.0-service (pid 304)
      4,160K: incidentd (pid 357)
      4,140K: servicemanager (pid 163)
      3,952K: android.system.suspend@1.0-service (pid 183)
      3,924K: android.hardware.power-service.rockchip (pid 313)
      3,864K: vndservicemanager (pid 165)
      3,792K: android.hardware.lights-service.rockchip (pid 310)
      3,760K: logd (pid 161)
      3,700K: bash (pid 3966)
      3,660K: tee-supplicant (pid 182)
      3,324K: android.hidl.allocator@1.0-service (pid 302)
      3,052K: dumpsys (pid 4320)
      3,004K: traced (pid 351)
      2,916K: traced_probes (pid 350)
      2,640K: iptables-restore (pid 307)
      2,628K: lmkd (pid 162)
      2,616K: ip6tables-restore (pid 311)
      2,504K: sh (pid 4318)
      2,392K: tombstoned (pid 291)

Total RSS by OOM adjustment:
    582,488K: Native
        160,196K: zygote (pid 298)
         61,040K: webview_zygote (pid 667)
         31,532K: surfaceflinger (pid 186)
         19,352K: mediaserver (pid 362)
         18,148K: media.extractor (pid 360)
         17,416K: cameraserver (pid 352)
         16,272K: audioserver (pid 315)
         14,404K: media.swcodec (pid 373)
         13,940K: media.codec (pid 371)
          9,744K: android.hardware.graphics.composer@2.1-service (pid 218)
          7,560K: android.hardware.audio.service (pid 303)
          7,496K: update_engine (pid 377)
          7,296K: init (pid 1)
          7,224K: netd (pid 297)
          6,816K: android.hardware.wifi@1.0-service-lazy (pid 536)
          6,584K: vold (pid 166)
          6,540K: android.hardware.graphics.allocator@4.0-service (pid 213)
          6,436K: keystore (pid 359)
          5,924K: wificond (pid 370)
          5,884K: credstore (pid 316)
          5,668K: ueventd (pid 128)
          5,520K: gatekeeperd (pid 376)
          5,500K: android.hardware.keymaster@4.0-service.optee (pid 185)
          5,384K: gpuservice (pid 318)
          5,356K: init (pid 126)
          5,300K: media.metrics (pid 361)
          5,028K: rockchip.hardware.rockit.hw@1.0-service (pid 314)
          5,016K: installd (pid 358)
          4,992K: adbd (pid 3862)
          4,824K: android.hardware.weaver@1.0-service (pid 309)
          4,808K: hwservicemanager (pid 164)
          4,740K: android.hardware.gatekeeper@1.0-service.optee (pid 305)
          4,572K: storaged (pid 369)
          4,564K: drmserver (pid 353)
          4,388K: android.hardware.boot@1.1-service (pid 184)
          4,316K: android.hardware.health@2.1-service (pid 306)
          4,308K: statsd (pid 296)
          4,208K: android.hardware.sensors@1.0-service (pid 308)
          4,164K: android.hardware.bluetooth@1.0-service (pid 304)
          4,160K: incidentd (pid 357)
          4,140K: servicemanager (pid 163)
          3,952K: android.system.suspend@1.0-service (pid 183)
          3,924K: android.hardware.power-service.rockchip (pid 313)
          3,864K: vndservicemanager (pid 165)
          3,792K: android.hardware.lights-service.rockchip (pid 310)
          3,760K: logd (pid 161)
          3,700K: bash (pid 3966)
          3,660K: tee-supplicant (pid 182)
          3,324K: android.hidl.allocator@1.0-service (pid 302)
          3,052K: dumpsys (pid 4320)
          3,004K: traced (pid 351)
          2,916K: traced_probes (pid 350)
          2,640K: iptables-restore (pid 307)
          2,628K: lmkd (pid 162)
          2,616K: ip6tables-restore (pid 311)
          2,504K: sh (pid 4318)
          2,392K: tombstoned (pid 291)
    269,260K: System
        269,260K: system (pid 423)
    513,684K: Persistent
        204,100K: com.android.systemui (pid 549)
        126,876K: com.android.phone (pid 726)
         98,280K: com.incar.update (pid 1013)
         84,428K: com.android.se (pid 682)
    114,296K: Persistent Service
        114,296K: com.android.providers.media.module (pid 993)
    349,572K: Foreground
        211,300K: com.cpuid.cpu_z (pid 3982 / activities)
        138,272K: com.google.android.webview:sandboxed_process0:org.chromium.content.app.SandboxedProcessService0:0 (pid 4055)
    593,088K: Visible
        182,108K: com.google.android.gms.persistent (pid 965)
        160,428K: com.android.vending (pid 2929)
        151,752K: com.android.launcher3 (pid 847 / activities)
         98,800K: com.google.android.ext.services (pid 771)
    294,376K: Perceptible
        157,744K: com.google.android.inputmethod.latin (pid 1280)
        136,632K: com.termux (pid 3913 / activities)
  2,135,356K: Cached
        237,220K: com.google.android.gms (pid 1080)
        189,572K: com.android.settings (pid 750 / activities)
        149,628K: com.google.android.gms.unstable (pid 3216)
        145,976K: com.google.android.gm (pid 2414)
        136,740K: com.google.android.apps.photos (pid 3771)
        132,236K: com.google.android.gms.ui (pid 3647)
        129,212K: com.android.vending:background (pid 3139)
        117,956K: com.google.android.calendar (pid 3266)
        116,580K: com.google.android.apps.nbu.files (pid 3404)
        113,480K: android.process.media (pid 1442)
         99,488K: com.google.process.gapps (pid 1363)
         99,124K: com.google.android.permissioncontroller (pid 2355)
         99,000K: com.google.process.gservices (pid 1104)
         96,896K: com.google.android.webview:webview_service (pid 4031)
         92,656K: com.google.android.partnersetup (pid 2588)
         90,748K: com.android.traceur (pid 3670)
         88,844K: com.google.android.configupdater (pid 3594)

Total RSS by category:
  1,280,520K: .apk mmap
  1,116,852K: .so mmap
    715,456K: .jar mmap
    486,748K: .art mmap
    342,624K: .oat mmap
    245,020K: Dalvik
    243,196K: Native
    148,444K: .dex mmap
     96,704K: Other mmap
     69,096K: Dalvik Other
     47,552K: Unknown
     24,180K: Stack
     19,888K: Other dev
     14,300K: .ttf mmap
      1,540K: Ashmem
          0K: Cursor
          0K: Gfx dev
          0K: EGL mtrack
          0K: GL mtrack
          0K: Other mtrack

Total PSS by process:
    106,080K: system (pid 423)
     64,856K: com.android.settings (pid 750 / activities)
     61,838K: com.google.android.gms (pid 1080)
     61,829K: com.cpuid.cpu_z (pid 3982 / activities)
     53,627K: com.android.systemui (pid 549)
     49,145K: com.google.android.gm (pid 2414)
     45,893K: com.google.android.gms.persistent (pid 965)
     45,617K: com.android.vending (pid 2929)
     41,050K: com.google.android.apps.photos (pid 3771)
     39,623K: com.google.android.inputmethod.latin (pid 1280)
     36,471K: com.android.launcher3 (pid 847 / activities)
     34,248K: zygote (pid 298)
     30,677K: com.google.android.webview:sandboxed_process0:org.chromium.content.app.SandboxedProcessService0:0 (pid 4055)
     29,441K: com.termux (pid 3913 / activities)
     27,101K: com.google.android.calendar (pid 3266)
     22,866K: com.android.phone (pid 726)
     21,792K: com.google.android.apps.nbu.files (pid 3404)
     21,465K: android.process.media (pid 1442)
     21,215K: com.android.vending:background (pid 3139)
     19,907K: com.google.android.gms.unstable (pid 3216)
     15,897K: com.google.android.gms.ui (pid 3647)
     14,650K: surfaceflinger (pid 186)
     14,362K: com.android.providers.media.module (pid 993)
     11,889K: com.google.android.permissioncontroller (pid 2355)
     11,304K: com.incar.update (pid 1013)
      9,693K: media.swcodec (pid 373)
      7,430K: com.google.android.configupdater (pid 3594)
      7,129K: com.android.traceur (pid 3670)
      7,018K: com.google.android.ext.services (pid 771)
      7,003K: com.google.process.gapps (pid 1363)
      6,880K: com.google.process.gservices (pid 1104)
      6,878K: media.codec (pid 371)
      6,407K: com.google.android.webview:webview_service (pid 4031)
      6,267K: audioserver (pid 315)
      6,232K: cameraserver (pid 352)
      6,165K: com.google.android.partnersetup (pid 2588)
      5,382K: mediaserver (pid 362)
      4,771K: com.android.se (pid 682)
      4,760K: media.extractor (pid 360)
      4,231K: webview_zygote (pid 667)
      3,226K: android.hardware.wifi@1.0-service-lazy (pid 536)
      3,203K: init (pid 1)
      3,182K: android.hardware.graphics.composer@2.1-service (pid 218)
      3,047K: android.hardware.audio.service (pid 303)
      2,904K: netd (pid 297)
      2,899K: update_engine (pid 377)
      2,065K: ueventd (pid 128)
      2,055K: vold (pid 166)
      1,797K: init (pid 126)
      1,784K: bash (pid 3966)
      1,696K: keystore (pid 359)
      1,648K: adbd (pid 3862)
      1,634K: android.hardware.keymaster@4.0-service.optee (pid 185)
      1,490K: statsd (pid 296)
      1,464K: logd (pid 161)
      1,444K: android.hardware.graphics.allocator@4.0-service (pid 213)
      1,438K: rockchip.hardware.rockit.hw@1.0-service (pid 314)
      1,405K: credstore (pid 316)
      1,398K: installd (pid 358)
      1,378K: wificond (pid 370)
      1,362K: media.metrics (pid 361)
      1,250K: tee-supplicant (pid 182)
      1,222K: android.hardware.weaver@1.0-service (pid 309)
      1,182K: gpuservice (pid 318)
      1,157K: android.hardware.boot@1.1-service (pid 184)
      1,116K: android.hardware.health@2.1-service (pid 306)
      1,112K: android.hardware.gatekeeper@1.0-service.optee (pid 305)
      1,072K: gatekeeperd (pid 376)
      1,057K: storaged (pid 369)
      1,011K: android.hardware.sensors@1.0-service (pid 308)
        976K: android.hardware.bluetooth@1.0-service (pid 304)
        975K: incidentd (pid 357)
        962K: hwservicemanager (pid 164)
        932K: drmserver (pid 353)
        922K: vndservicemanager (pid 165)
        913K: servicemanager (pid 163)
        860K: android.hardware.power-service.rockchip (pid 313)
        840K: traced (pid 351)
        837K: android.hardware.lights-service.rockchip (pid 310)
        777K: android.system.suspend@1.0-service (pid 183)
        752K: traced_probes (pid 350)
        695K: sh (pid 4318)
        616K: iptables-restore (pid 307)
        612K: android.hidl.allocator@1.0-service (pid 302)
        612K: dumpsys (pid 4320)
        610K: ip6tables-restore (pid 311)
        564K: lmkd (pid 162)
        525K: tombstoned (pid 291)

Total PSS by OOM adjustment:
    159,019K: Native
         34,248K: zygote (pid 298)
         14,650K: surfaceflinger (pid 186)
          9,693K: media.swcodec (pid 373)
          6,878K: media.codec (pid 371)
          6,267K: audioserver (pid 315)
          6,232K: cameraserver (pid 352)
          5,382K: mediaserver (pid 362)
          4,760K: media.extractor (pid 360)
          4,231K: webview_zygote (pid 667)
          3,226K: android.hardware.wifi@1.0-service-lazy (pid 536)
          3,203K: init (pid 1)
          3,182K: android.hardware.graphics.composer@2.1-service (pid 218)
          3,047K: android.hardware.audio.service (pid 303)
          2,904K: netd (pid 297)
          2,899K: update_engine (pid 377)
          2,065K: ueventd (pid 128)
          2,055K: vold (pid 166)
          1,797K: init (pid 126)
          1,784K: bash (pid 3966)
          1,696K: keystore (pid 359)
          1,648K: adbd (pid 3862)
          1,634K: android.hardware.keymaster@4.0-service.optee (pid 185)
          1,490K: statsd (pid 296)
          1,464K: logd (pid 161)
          1,444K: android.hardware.graphics.allocator@4.0-service (pid 213)
          1,438K: rockchip.hardware.rockit.hw@1.0-service (pid 314)
          1,405K: credstore (pid 316)
          1,398K: installd (pid 358)
          1,378K: wificond (pid 370)
          1,362K: media.metrics (pid 361)
          1,250K: tee-supplicant (pid 182)
          1,222K: android.hardware.weaver@1.0-service (pid 309)
          1,182K: gpuservice (pid 318)
          1,157K: android.hardware.boot@1.1-service (pid 184)
          1,116K: android.hardware.health@2.1-service (pid 306)
          1,112K: android.hardware.gatekeeper@1.0-service.optee (pid 305)
          1,072K: gatekeeperd (pid 376)
          1,057K: storaged (pid 369)
          1,011K: android.hardware.sensors@1.0-service (pid 308)
            976K: android.hardware.bluetooth@1.0-service (pid 304)
            975K: incidentd (pid 357)
            962K: hwservicemanager (pid 164)
            932K: drmserver (pid 353)
            922K: vndservicemanager (pid 165)
            913K: servicemanager (pid 163)
            860K: android.hardware.power-service.rockchip (pid 313)
            840K: traced (pid 351)
            837K: android.hardware.lights-service.rockchip (pid 310)
            777K: android.system.suspend@1.0-service (pid 183)
            752K: traced_probes (pid 350)
            695K: sh (pid 4318)
            616K: iptables-restore (pid 307)
            612K: android.hidl.allocator@1.0-service (pid 302)
            612K: dumpsys (pid 4320)
            610K: ip6tables-restore (pid 311)
            564K: lmkd (pid 162)
            525K: tombstoned (pid 291)
    106,080K: System
        106,080K: system (pid 423)
     92,568K: Persistent
         53,627K: com.android.systemui (pid 549)
         22,866K: com.android.phone (pid 726)
         11,304K: com.incar.update (pid 1013)
          4,771K: com.android.se (pid 682)
     14,362K: Persistent Service
         14,362K: com.android.providers.media.module (pid 993)
     92,506K: Foreground
         61,829K: com.cpuid.cpu_z (pid 3982 / activities)
         30,677K: com.google.android.webview:sandboxed_process0:org.chromium.content.app.SandboxedProcessService0:0 (pid 4055)
    134,999K: Visible
         45,893K: com.google.android.gms.persistent (pid 965)
         45,617K: com.android.vending (pid 2929)
         36,471K: com.android.launcher3 (pid 847 / activities)
          7,018K: com.google.android.ext.services (pid 771)
     69,064K: Perceptible
         39,623K: com.google.android.inputmethod.latin (pid 1280)
         29,441K: com.termux (pid 3913 / activities)
    397,169K: Cached
         64,856K: com.android.settings (pid 750 / activities)
         61,838K: com.google.android.gms (pid 1080)
         49,145K: com.google.android.gm (pid 2414)
         41,050K: com.google.android.apps.photos (pid 3771)
         27,101K: com.google.android.calendar (pid 3266)
         21,792K: com.google.android.apps.nbu.files (pid 3404)
         21,465K: android.process.media (pid 1442)
         21,215K: com.android.vending:background (pid 3139)
         19,907K: com.google.android.gms.unstable (pid 3216)
         15,897K: com.google.android.gms.ui (pid 3647)
         11,889K: com.google.android.permissioncontroller (pid 2355)
          7,430K: com.google.android.configupdater (pid 3594)
          7,129K: com.android.traceur (pid 3670)
          7,003K: com.google.process.gapps (pid 1363)
          6,880K: com.google.process.gservices (pid 1104)
          6,407K: com.google.android.webview:webview_service (pid 4031)
          6,165K: com.google.android.partnersetup (pid 2588)

Total PSS by category:
    280,864K: .apk mmap
    170,916K: Native
    127,629K: .dex mmap
    121,020K: .so mmap
    107,854K: Dalvik
     55,810K: .jar mmap
     47,920K: .art mmap
     41,155K: Dalvik Other
     34,545K: Unknown
     26,900K: Other mmap
     23,864K: Stack
     13,265K: .oat mmap
     10,917K: .ttf mmap
      2,332K: Other dev
        776K: Ashmem
          0K: Cursor
          0K: Gfx dev
          0K: EGL mtrack
          0K: GL mtrack
          0K: Other mtrack

Total RAM: 8,092,276K (status normal)
 Free RAM: 2,063,497K (  397,169K cached pss +   649,736K cached kernel + 1,016,592K free)
      ION:    50,628K (   28,880K mapped +         0K unmapped +    21,748K pools)
 Used RAM:   854,710K (  668,598K used pss +   186,112K kernel)
 Lost RAM: 5,174,065K
     ZRAM:         4K physical used for         0K in swap (2,275,948K total swap)
   Tuning: 128 (large 256), oom   184,320K, restore limit    61,440K (low-ram)
```

### `/proc/cpuinfo`
```
cat /proc/cpuinfo
processor       : 0
model name      : ARMv8 Processor rev 2 (v8l)
BogoMIPS        : 48.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt lpae evtstrm aes pmull sha1 sha2 crc32
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd04
CPU revision    : 2

processor       : 1
model name      : ARMv8 Processor rev 2 (v8l)
BogoMIPS        : 48.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt lpae evtstrm aes pmull sha1 sha2 crc32
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd04
CPU revision    : 2

processor       : 2
model name      : ARMv8 Processor rev 2 (v8l)
BogoMIPS        : 48.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt lpae evtstrm aes pmull sha1 sha2 crc32
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd04
CPU revision    : 2

processor       : 3
model name      : ARMv8 Processor rev 2 (v8l)
BogoMIPS        : 48.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt lpae evtstrm aes pmull sha1 sha2 crc32
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd04
CPU revision    : 2

Hardware        : Rockchip rk3326 863 rkisp1 board
Serial          : 7c5bff253bb9e52f
```

### Partitions
```
df -h
Filesystem            Size  Used Avail Use% Mounted on
tmpfs                 1.4G  816K  1.4G   1% /dev
tmpfs                 1.4G     0  1.4G   0% /mnt
/dev/block/mmcblk2p15  11M  160K   11M   2% /metadata
/dev/block/dm-5       810M  807M     0 100% /
/dev/block/dm-6       129M  128M     0 100% /vendor
/dev/block/dm-7       244M  244M     0 100% /odm
/dev/block/dm-8       1.0G  1.0G     0 100% /product
/dev/block/dm-9       125M  125M     0 100% /system_ext
tmpfs                 1.4G     0  1.4G   0% /apex
/dev/block/mmcblk2p14 356M  216K  344M   1% /cache
/dev/block/dm-10      115G   20G   95G  18% /data
/dev/fuse             115G   20G   95G  18% /storage/emulated
```

### Processes
```
ps -eo pid,user,%cpu,%mem,cmd --sort=-%cpu
   PID USER         %CPU  %MEM CMD
   423 system        7.9   8.7 Binder:423_3
   750 system        5.6   6.3 ndroid.settings
   218 system        5.5   0.3 composer@2.1-se
   186 system        4.3   1.1 surfaceflinger
  3982 u0_a164       5.6   6.7 com.cpuid.cpu_z
   965 u0_a122       3.8   5.8 .gms.persistent
  1080 u0_a122       2.2   7.6 gle.android.gms
   549 u0_a153       2.0   6.9 ndroid.systemui
  2929 u0_a123       1.8   5.0 android.vending
   847 u0_a150       0.6   5.6 droid.launcher3
   298 root          0.6   5.2 main
  1104 u0_a122       0.6   3.2 ocess.gservices
  1280 u0_a126       0.5   5.2 putmethod.latin
  4243 root          0.4   0.0 kworker/2:0-events
   993 u0_a154       0.3   3.7 rs.media.module
     1 root          0.3   0.2 init
  3913 u0_a165       0.3   4.4 com.termux
   111 root          0.2   0.0 kworker/u8:4-adb
  3216 u0_a122       0.2   4.8 id.gms.unstable
    32 root          0.2   0.0 kworker/u8:1-adb
   726 radio         0.2   4.1 m.android.phone
   308 system        0.2   0.1 sensors@1.0-ser
  2414 u0_a145       0.2   4.6 ogle.android.gm
   358 root          0.2   0.1 Binder:358_2
    45 root          0.1   0.0 kworker/u9:0-blk_crypto_wq
    49 root          0.1   0.0 cfinteractive
    84 root          0.1   0.0 kworker/u8:3-adb
   161 logd          0.1   0.1 logd
  3404 u0_a116       0.1   3.7 .apps.nbu.files
   296 statsd        0.1   0.1 Binder:296_2
   128 root          0.1   0.1 ueventd
  3139 u0_a123       0.1   4.1 ding:background
   163 system        0.1   0.1 servicemanager
    92 root          0.1   0.0 kworker/u9:1-blk_crypto_wq
  3266 u0_a136       0.1   3.7 ndroid.calendar
  3771 u0_a159       0.1   4.3 oid.apps.photos
   389 root          0.1   0.0 kworker/0:3H-mmc_complete
  2355 u0_a155       0.1   3.1 ssioncontroller
   373 mediacodec    0.0   0.4 mediaswcodec
    10 root          0.0   0.0 rcu_preempt
   162 lmkd          0.0   0.0 lmkd
  4055 u0_i9000      0.1   4.3 ocessService0:0
   315 audioserver   0.0   0.5 audioserver
  3647 u0_a122       0.0   4.2 .android.gms.ui
   185 system        0.0   0.1 keymaster@4.0-s
   362 media         0.0   0.6 mediaserver
   360 mediaex       0.0   0.5 mediaextractor
   117 root          0.0   0.0 kworker/2:2H-kblockd
  1363 u0_a122       0.0   3.2 e.process.gapps
   536 wifi          0.0   0.2 wifi@1.0-servic
    87 root          0.0   0.0 kworker/0:1H-mmc_complete
  1442 u0_a71        0.0   3.6 d.process.media
  4289 root          0.1   0.0 kworker/3:2-events_power_efficient
   166 root          0.0   0.2 Binder:166_2
    98 root          0.0   0.0 kworker/1:1H-kblockd
   352 cameraserver  0.0   0.5 cameraserver
  1013 u0_a110       0.0   3.1 om.incar.update
   306 system        0.0   0.1 health@2.1-serv
   130 root          0.0   0.0 kworker/0:2-events_freezable
   771 u0_a156       0.0   3.1 id.ext.services
  2588 u0_a117       0.0   2.9 id.partnersetup
    35 root          0.0   0.0 kworker/3:1-events_power_efficient
   213 system        0.0   0.2 allocator@4.0-s
   371 mediacodec    0.0   0.4 omx@1.0-service
    96 root          0.0   0.0 kworker/3:1H-kblockd
   164 system        0.0   0.1 hwservicemanage
   313 root          0.0   0.1 android.hardwar
  4328 root          0.1   0.0 kworker/3:0-events_freezable
   297 root          0.0   0.2 Binder:297_3
   183 system        0.0   0.1 suspend@1.0-ser
   126 root          0.0   0.1 init
  4031 u0_a138       0.0   3.1 webview_service
    80 root          0.0   0.0 ion_system_heap
  3862 shell         0.0   0.1 adbd
   182 root          0.0   0.1 tee-supplicant
    91 root          0.0   0.0 mali-simple-pow
  3594 u0_a113       0.0   2.8 d.configupdater
    11 root          0.0   0.0 rcu_sched
    37 root          0.0   0.0 kworker/1:1-mm_percpu_wq
  3670 u0_a108       0.0   2.9 android.traceur
   667 webview_zygote 0.0  2.0 webview_zygote
   369 root          0.0   0.1 storaged
   303 audioserver   0.0   0.2 audio.service
   377 root          0.0   0.2 update_engine
   361 media         0.0   0.1 mediametrics
   682 secure_element 0.0  2.6 com.android.se
   359 keystore      0.0   0.2 keystore
   270 root          0.0   0.0 f2fs_discard-25
   309 system        0.0   0.1 weaver@1.0-serv
    13 root          0.0   0.0 migration/0
  3966 u0_a165       0.0   0.1 bash
  4345 shell        43.7   0.1 ps
   307 root          0.0   0.0 iptables-restor
   273 root          0.0   0.0 f2fs_gc-253:10
   311 root          0.0   0.0 ip6tables-resto
   318 gpu_service   0.0   0.1 Binder:318_2
   316 credstore     0.0   0.1 credstore
   314 media         0.0   0.1 hw@1.0-service
   370 wifi          0.0   0.1 wificond
   165 system        0.0   0.1 vndservicemanag
     9 root          0.0   0.0 ksoftirqd/0
   184 root          0.0   0.1 boot@1.1-servic
    17 root          0.0   0.0 migration/1
   376 system        0.0   0.1 gatekeeperd
   305 system        0.0   0.1 gatekeeper@1.0-
   351 nobody        0.0   0.0 traced
   357 incidentd     0.0   0.1 Binder:357_2
    22 root          0.0   0.0 migration/2
   353 drm           0.0   0.1 drmserver
   310 system        0.0   0.1 android.hardwar
   350 nobody        0.0   0.0 traced_probes
   304 bluetooth     0.0   0.1 bluetooth@1.0-s
     2 root          0.0   0.0 kthreadd
   302 system        0.0   0.1 allocator@1.0-s
    28 root          0.0   0.0 ksoftirqd/3
    18 root          0.0   0.0 ksoftirqd/1
    36 root          0.0   0.0 kauditd
    31 root          0.0   0.0 kdevtmpfs
  4341 shell         0.0   0.0 sh
    27 root          0.0   0.0 migration/3
   291 tombstoned    0.0   0.0 tombstoned
    23 root          0.0   0.0 ksoftirqd/2
   167 root          0.0   0.0 psimon
     3 root          0.0   0.0 rcu_gp
     4 root          0.0   0.0 rcu_par_gp
     8 root          0.0   0.0 mm_percpu_wq
    12 root          0.0   0.0 rcu_bh
    14 root          0.0   0.0 kworker/0:1-cgroup_pidlist_destroy
    15 root          0.0   0.0 cpuhp/0
    16 root          0.0   0.0 cpuhp/1
    21 root          0.0   0.0 cpuhp/2
    26 root          0.0   0.0 cpuhp/3
    33 root          0.0   0.0 netns
    34 root          0.0   0.0 rcu_tasks_kthre
    39 root          0.0   0.0 khungtaskd
    40 root          0.0   0.0 oom_reaper
    41 root          0.0   0.0 writeback
    42 root          0.0   0.0 kcompactd0
    43 root          0.0   0.0 crypto
    44 root          0.0   0.0 kblockd
    46 root          0.0   0.0 blk_crypto_wq
    47 root          0.0   0.0 devfreq_wq
    48 root          0.0   0.0 watchdogd
    50 root          0.0   0.0 cfg80211
    51 root          0.0   0.0 kswapd0
    52 root          0.0   0.0 irq/46-rockchip
    53 root          0.0   0.0 irq/47-rockchip
    54 root          0.0   0.0 irq/48-rockchip
    55 root          0.0   0.0 irq/49-rockchip
    56 root          0.0   0.0 hevc
    57 root          0.0   0.0 irq/31-ff440000
    58 root          0.0   0.0 vdpu
    59 root          0.0   0.0 irq/28-ff442400
    60 root          0.0   0.0 vepu
    61 root          0.0   0.0 irq/30-ff442000
    62 root          0.0   0.0 hwrng
    64 root          0.0   0.0 nvme-wq
    65 root          0.0   0.0 nvme-reset-wq
    66 root          0.0   0.0 nvme-delete-wq
    67 root          0.0   0.0 uas
    68 root          0.0   0.0 goodix_wq
    69 root          0.0   0.0 irq/50-rk817
    70 root          0.0   0.0 rk817-bat-monit
    71 root          0.0   0.0 rk817-dc-wq
    72 root          0.0   0.0 rk817-usb-wq
    73 root          0.0   0.0 kworkqueue_ts
    74 root          0.0   0.0 gsl_monitor_wor
    76 root          0.0   0.0 irq/18-rockchip
    77 root          0.0   0.0 dm_bufio_cache
    78 root          0.0   0.0 kworker/1:2-cgroup_pidlist_destroy
    81 root          0.0   0.0 ipv6_addrconf
    82 root          0.0   0.0 krfcommd
    85 root          0.0   0.0 mmc_complete
    88 root          0.0   0.0 irq/36-rga
    90 root          0.0   0.0 gpu_power_off_w
    93 root          0.0   0.0 irq/77-headset_
    99 root          0.0   0.0 jbd2/mmcblk2p15
   100 root          0.0   0.0 ext4-rsv-conver
   101 root          0.0   0.0 kdmflush
   102 root          0.0   0.0 kdmflush
   103 root          0.0   0.0 kdmflush
   104 root          0.0   0.0 kdmflush
   105 root          0.0   0.0 kdmflush
   106 root          0.0   0.0 kdmflush
   107 root          0.0   0.0 kverityd
   109 root          0.0   0.0 ext4-rsv-conver
   112 root          0.0   0.0 kdmflush
   113 root          0.0   0.0 kverityd
   114 root          0.0   0.0 ext4-rsv-conver
   115 root          0.0   0.0 kdmflush
   116 root          0.0   0.0 kverityd
   118 root          0.0   0.0 ext4-rsv-conver
   119 root          0.0   0.0 kdmflush
   120 root          0.0   0.0 kverityd
   121 root          0.0   0.0 ext4-rsv-conver
   122 root          0.0   0.0 kdmflush
   123 root          0.0   0.0 kverityd
   124 root          0.0   0.0 ext4-rsv-conver
   176 root          0.0   0.0 jbd2/mmcblk2p14
   177 root          0.0   0.0 ext4-rsv-conver
   197 root          0.0   0.0 kbase_event
   265 root          0.0   0.0 kdmflush
   269 root          0.0   0.0 f2fs_flush-253:
   632 root          0.0   0.0 rtw_workqueue
   639 root          0.0   0.0 ksdioirqd/mmc1
   809 root          0.0   0.0 kbase_event
   897 root          0.0   0.0 kbase_event
   947 root          0.0   0.0 loop0
  1192 root          0.0   0.0 kbase_event
  3567 root          0.0   0.0 kbase_event
  3733 root          0.0   0.0 kbase_event
  3878 root          0.0   0.0 kbase_event
  3953 root          0.0   0.0 kbase_event
  4094 root          0.0   0.0 kbase_event
  4219 root          0.0   0.0 kbase_event
  4246 root          0.0   0.0 kworker/1:0H
  4262 root          0.0   0.0 kworker/3:0H
  4284 root          0.0   0.0 kworker/2:0H
  4288 root          0.0   0.0 kworker/2:1-mm_percpu_wq
  4293 root          0.0   0.0 kworker/0:0H-mmc_complete
  4321 root          0.0   0.0 kworker/1:2H
  4329 root          0.0   0.0 kworker/0:2H
  4333 root          0.0   0.0 kworker/2:1H
  4343 root          0.0   0.0 kworker/u9:2
  4344 root          0.0   0.0 kworker/u9:3
```

### Installed packages*
*After I installed VLC, Opera and Termux
```
pm list packages
package:com.android.cts.priv.ctsshim
package:com.google.android.youtube
package:com.android.internal.display.cutout.emulation.corner
package:com.google.android.ext.services
package:com.android.internal.display.cutout.emulation.double
package:com.android.providers.telephony
package:com.android.dynsystem
package:com.android.theme.color.amethyst
package:com.android.theme.icon.pebble
package:com.android.providers.calendar
package:com.android.providers.media
package:com.google.android.onetimeinitializer
package:com.google.android.ext.shared
package:com.android.internal.systemui.navbar.gestural_wide_back
package:com.incar.update
package:com.android.theme.color.sand
package:com.android.wallpapercropper
package:com.android.theme.icon.vessel
package:com.android.theme.color.cinnamon
package:com.android.theme.icon_pack.victor.settings
package:com.android.theme.icon_pack.rounded.systemui
package:com.android.theme.icon.taperedrect
package:com.android.documentsui
package:com.android.externalstorage
package:com.android.htmlviewer
package:com.android.companiondevicemanager
package:com.android.mms.service
package:com.android.providers.downloads
package:com.android.networkstack.inprocess
package:com.android.theme.icon_pack.rounded.android
package:com.flyz.pcbtest
package:com.android.theme.icon_pack.victor.systemui
package:com.android.theme.icon_pack.circular.themepicker
package:com.google.android.configupdater
package:com.google.android.overlay.modules.permissioncontroller
package:com.android.soundrecorder
package:com.android.theme.color.tangerine
package:com.android.providers.downloads.ui
package:com.android.vending
package:com.android.pacprocessor
package:com.android.simappdialog
package:com.android.theme.color.aquamarine
package:com.android.internal.display.cutout.emulation.hole
package:com.android.internal.display.cutout.emulation.tall
package:com.android.certinstaller
package:com.android.theme.color.black
package:com.google.android.marvin.talkback
package:com.android.theme.color.green
package:com.android.theme.color.ocean
package:com.android.theme.color.space
package:com.android.internal.systemui.navbar.threebutton
package:android
package:com.android.camera2
package:com.android.theme.icon_pack.rounded.launcher
package:com.android.theme.icon_pack.kai.settings
package:com.android.egg
package:com.android.mtp
package:com.android.nfc
package:com.android.launcher3
package:com.android.backupconfirm
package:com.google.android.deskclock
package:com.android.provision
package:com.android.statementservice
package:com.google.android.gm
package:com.google.android.apps.tachyon
package:com.google.android.overlay.gmsconfig.common
package:com.android.theme.icon_pack.sam.settings
package:com.android.settings.intelligence
package:com.google.android.apps.searchlite
package:com.android.internal.systemui.navbar.gestural_extra_wide_back
package:com.google.android.permissioncontroller
package:com.android.theme.icon_pack.kai.themepicker
package:com.android.providers.settings
package:com.android.sharedstoragebackup
package:com.android.theme.icon_pack.victor.launcher
package:com.android.printspooler
package:com.android.theme.icon_pack.filled.settings
package:com.android.dreams.basic
package:com.google.android.overlay.modules.ext.services
package:com.android.theme.icon_pack.kai.systemui
package:com.sencatech.iwawa.iwawahomeoverlay
package:com.android.se
package:com.android.inputdevices
package:com.google.android.apps.wellbeing
package:com.android.bips
package:com.google.android.apps.nbu.files
package:com.android.theme.icon_pack.circular.settings
package:com.google.android.overlay.gmsconfig.comms
package:com.android.musicfx
package:com.google.android.apps.docs
package:com.google.android.apps.maps
package:com.android.theme.icon_pack.sam.systemui
package:com.google.android.modulemetadata
package:com.sencatech.androidsystembridge
package:com.android.cellbroadcastreceiver
package:com.google.android.webview
package:com.android.theme.icon.teardrop
package:com.opera.browser
package:uni.UNI62A01B3
package:com.google.android.contacts
package:com.android.server.telecom
package:com.google.android.syncadapters.contacts
package:com.android.cellbroadcastservice
package:com.android.theme.icon_pack.rounded.themepicker
package:com.android.keychain
package:com.android.chrome
package:com.android.theme.icon_pack.filled.systemui
package:com.google.android.packageinstaller
package:com.google.android.gms
package:com.google.android.gsf
package:com.google.android.ims
package:com.google.android.tag
package:com.google.android.tts
package:com.android.wifi.resources
package:com.google.android.gmsintegration
package:com.google.android.partnersetup
package:com.android.localtransport
package:com.google.android.videos
package:com.sencatech.iwawa.iwawahome
package:com.android.theme.icon_pack.sam.android
package:com.android.theme.font.notoserifsource
package:com.android.theme.icon_pack.filled.android
package:com.android.proxyhandler
package:com.android.internal.display.cutout.emulation.waterfall
package:com.android.theme.icon_pack.circular.systemui
package:org.videolan.vlc
package:com.google.android.overlay.modules.permissioncontroller.forframework
package:com.google.android.feedback
package:com.google.android.printservice.recommendation
package:com.google.android.apps.photos
package:com.google.android.calendar
package:com.android.theme.icon_pack.kai.android
package:com.android.managedprovisioning
package:com.android.soundpicker
package:com.android.theme.icon_pack.kai.launcher
package:com.sencatech.iwawa.iwawahomemanual
package:com.google.android.apps.speechservices
package:com.android.providers.partnerbookmarks
package:com.google.android.apps.youtube.kids
package:com.android.theme.icon_pack.sam.launcher
package:com.google.android.gms.policy_sidecar_aps
package:com.android.theme.icon.squircle
package:com.google.android.overlay.gmsconfig.assistantgo
package:com.android.theme.icon_pack.victor.android
package:com.android.storagemanager
package:com.android.theme.color.palette
package:com.android.bookmarkprovider
package:com.android.settings
package:com.android.theme.icon_pack.filled.launcher
package:com.android.networkstack.tethering.inprocess
package:com.android.networkstack.permissionconfig
package:com.termux
package:com.android.calculator2
package:com.android.cts.ctsshim
package:com.android.theme.color.carbon
package:com.google.android.overlay.modules.modulemetadata.forframework
package:com.android.theme.icon_pack.circular.launcher
package:com.google.android.apps.assistant
package:com.android.vpndialogs
package:com.android.phone
package:com.android.shell
package:com.android.theme.icon_pack.filled.themepicker
package:com.android.wallpaperbackup
package:com.android.providers.blockednumber
package:com.android.providers.userdictionary
package:com.android.providers.media.module
package:com.android.hotspot2.osulogin
package:com.google.android.gms.location.history
package:com.android.internal.systemui.navbar.gestural
package:com.android.location.fused
package:com.android.theme.icon_pack.victor.themepicker
package:com.android.theme.color.orchid
package:com.android.systemui
package:com.google.android.apps.youtube.music
package:com.android.theme.color.purple
package:com.android.bluetoothmidiservice
package:com.android.traceur
package:com.cpuid.cpu_z
package:android.auto_generated_rro_product__
package:com.android.theme.icon_pack.sam.themepicker
package:com.android.bluetooth
package:com.android.wallpaperpicker
package:com.android.providers.contacts
package:com.android.captiveportallogin
package:com.android.theme.icon.roundedrect
package:com.android.internal.systemui.navbar.gestural_narrow_back
package:com.android.theme.icon_pack.rounded.settings
package:com.google.android.overlay.gmsconfig.go
package:com.google.android.inputmethod.latin
package:android.auto_generated_rro_vendor__
package:com.android.theme.icon_pack.circular.android
package:com.google.android.apps.restore
```
