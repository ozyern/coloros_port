# port.sh 

 `port.sh`  ZIP/  
ROM 

: `port.sh`  `functions.sh` // [FUNCTIONS_SH_DOC.md](FUNCTIONS_SH_DOC.md) 

---

## 0. 

`port.sh`  2 

1) `pack_method=stock` `bin/port_config`  
   - Android  `target_files` `out/target/product/<device>/``otatools/bin/ota_from_target_files`  ** OTA ZIP** 
   - : `out/<target_folder>/ota_full-<rom_version>-<model>-<timestamp>-<region>-<spl>-<hash>.zip`

2) `pack_method!=stock`   
   - `lpmake`  `super.img` `zstd`  `super.zst` fastboot  ** ZIP**Windows/Mac/Linux  + firmware-update + META-INF
   - : `out/<OS>_<rom_version>_<hash>_<model>_<timestamp>_<pack_type>.zip`

 **ROM vbmeta ** 

---

## 1. 

:
- `sudo ./port.sh <baserom> <portrom> [portrom2] [portparts]`

:
- `<baserom>`:  ROMOTA ZIP / fastboot ZIPURL 
- `<portrom>`:  ROMColorOS / OxygenOS / realme UI URL 
- `[portrom2]`: 2 ROM `mix_port=true`
- `[portparts]`:  `portrom2` 
  - ****: `mix_port_part=($portparts)` 
  - : `("my_stock" "my_region" "my_manifest" "my_product")`

---

## 2. 

`port.sh`  `functions.sh`  `check` :
- `unzip` `aria2c` `7z` `zip` `java` `python3` `zstd` `bc` `xmlstarlet`

 **check **:
- `git``git init` `git apply`  framework.jar  smali 
- `jq`AIUnit  `unit_config_list.json` 
- `md5sum` ZIP 
- `unix2dos`Windows 
- `payload-dumper``PATH`  `bin/<OS>/<ARCH>` 
- `brotli` `sdat2img.py``.new.dat.br`  ROM 
- `extract.erofs` `gettype` `mkfs.erofs`EROFS 
- `magiskboot``functions.sh`  kernel/boot 
- `ksud`init_boot  KernelSU 

`setup.sh` git/jq 

---

## 3. bin/port_config 

`port.sh`  `bin/port_config` `grep key | cut -d= -f2`:
- `partition_to_port`  `port_partition`  
  `payload-dumper --partitions`  unzip 
- `possible_super_list`  `super_list`  
  super 
- `repack_with_ext4`  `repackext4`  `pack_type`  
  `true`  `pack_type=EXT` `pack_type=EROFS`
  - ****:  `mkfs.erofs` EXT4  `port.sh` `pack_type`  `functions.sh`  fstab 
- `super_extended`  `super_extended`  
  super debloat reserve.img 
- `pack_method`  `pack_method`  
  `stock`  `otatools/bin/ota_from_target_files`  OTA 
- `pack_with_dsu` / `ddr_type` / `reusabe_partition_list` `port.sh` 

---

## 4. 

`port.sh` :
- `build/baserom/`
  - `images/`  baserom  `.img` 
  - `firmware-update/`  `storage-fw/`  ZIP 
- `build/portrom/`
  - `images/`  portrom  `.img`  `.img` 
  - `super.zst` 
- `build/<version_name>/`
  - portrom 
- `tmp/`
  - APK/JAR smali AnyKernel 
- `out/`
  - OTA ZIP  fastboot ZIP
  - `pack_method=stock`  `out/target/product/<device>/`  target_files 

---

## 5. 

###  A:  PATH
1. 
   - `baserom=$1`, `portrom=$2`, `portrom2=$3`, `portparts=$4`
   - `work_dir=$(pwd)`
   - `tools_dir=$work_dir/bin/$(uname)/$(uname -m)`
   - `PATH`  `bin/<OS>/<ARCH>/`  `otatools/bin/` 
2. `source functions.sh`
3. `check ...` 
4. `bin/port_config`  `port_partition/super_list/pack_type/...` 

###  B:  URL 
- `baserom`/`portrom`  & `grep http`   `aria2c`  DL
- `basename | sed 's/\?t.*//'` 
- DL  `error`

###  C: ROM 
 `unzip -l`  ZIP 

#### baserom 
- `payload.bin`   `baserom_type=payload` metadata  `oplus_hex_nv_id` 
- `br$` `.new.dat.br`  `baserom_type=br`
- `\.img$`   `baserom_type=img`
- 

#### portrom 
- `payload.bin`   `portrom_type=payload`
- `\.img$`   `portrom_type=img`
- 

 `META-INF/com/android/metadata` 
- `version_name=`  `ota_version=` 

- `version_name` `ota_version`  `"V16.0.0"` 

#### portrom2
- `portrom2`  `mix_port=true`
- `portparts`  `mix_port_part=($portparts)` 4 
- `portrom2`  `payload/img` `version_name2` 

###  D: 
:
- `app/` `tmp/` `config/` `build/baserom/` `build/portrom/`
- `find . -type d -name 'ColorOS_*' | xargs rm -rf`

:
- `build/baserom/images/`  `build/portrom/images/` 
- `tmp/`  `TMPDIR` 

###  E: baserom payload / br / img
#### baserom_type=payload
- `payload-dumper --out build/baserom/images/ "$baserom"`

#### baserom_type=br
1. `unzip -q "$baserom" -d build/baserom`
2. `build/baserom/*` `name123.transfer.list` 
3. `for i in $super_list`  `<i>.new.dat.br` 
   - `brotli -d <i>.new.dat.br`
   - `python3 sdat2img.py <i>.transfer.list <i>.new.dat build/baserom/images/<i>.img`
   -  `.new.dat*`/`transfer.list`/`patch.*` 

#### baserom_type=img
- `unzip -q "$baserom" -d build/baserom/tmp/`
- `find ... -name "*.img" -exec mv ... build/baserom/images/`
- `build/baserom/tmp` 

###  F: portrom 
#### 
`build/<version_name>/` `port_partition`  `<part>.img` 

#### portrom_type=payload
- `payload-dumper --partitions "$port_partition" --out build/<version_name>/ "$portrom"`
-  `.img`  `build/portrom/images/` 

#### portrom_type=img
1. `port_partition`  `IFS=','` 
2. `<part>.img` / `<part>_a.img` / `<part>_b.img`  unzip 
3. `unzip -q "$portrom" <targets...> -d build/<version_name>/`
4. `find build/<version_name> -name "*.img"`  `build/portrom/images/` 

###  G: portrom2
 F  `mix_port_part`  `build/portrom/images/` 

###  H: baserom  system/product/system_ext/my_product/my_manifest
```
for part in system product system_ext my_product my_manifest; do
  extract_partition build/baserom/images/${part}.img build/baserom/images
done
```
- `extract_partition`  `functions.sh` `gettype`  ext4/erofs 
-  `.img`  `rm -rf ${part_img}`

###  I: baserom  vendor/odm  port 
:
- `vendor odm my_company my_preload system_dlkm vendor_dlkm my_engineering`

:
1. baserom  `<image>.img`  `build/portrom/images/<image>.img`  `mv`
2. `extract_partition build/portrom/images/<image>.img build/portrom/images/`

:
- vendor/odm feature XML SELinux config 

###  J: super_list  portrom 
- `build/portrom/images/system_dlkm`  `super_list` system_dlkm 
- `for part in $super_list`  `extract_partition` `wait` 
  -  baserom  `.img` `rm -rf build/baserom/images/${part}.img`

###  K: ROM Android/SDK///
:
- `base_android_version` / `port_android_version`
- `base_android_sdk` / `port_android_sdk`
- `base_rom_version` / `port_rom_version``ro.build.display.ota` 
- `base_device_code` / `port_device_code``ro.oplus.version.my_manifest` 
- `base_product_device` / `port_product_device``ro.product.device`
- `base_product_model` / `port_product_model``ro.product.model`
- `base_market_name` / `port_market_name``ro.vendor.oplus.market.name`
- `base_my_product_type` / `port_my_product_type``ro.oplus.image.my_product.type`
- `regionmark`portrom  `build.prop`  `ro.vendor.oplus.regionmark=` 
- `base_regionmark`baserom  `ro.oplus.image.my_region.type=` 
- `vendor_cpu_abilist32``ro.vendor.product.cpu.abilist32`
- `base_area/base_brand`  `port_area/port_brand``ro.oplus.image.system_ext.area/brand`  grep

:
- base : `baseIsColorOSCN` / `baseIsOOS` / `baseIsRealmeUI`
- port : `portIsColorOSGlobal` / `portIsOOS` / `portIsColorOS` / `portIsRealmeUI`

A/B :
- `ro.build.ab_update=true`  vendor  `is_ab_device=true`

64bit-only portrom  64bit  vendor  32bit :
- `build/portrom/images/system/system/bin/app_process32`  && `vendor_cpu_abilist32` 
  - vendor/build.prop  `abilist`  `arm64-v8a` 
  - `abilist32` 
  - vendor/default.prop  `ro.zygote`  `zygote64` 

###  L: my_manifest 
:
- `ro.build.display.id`  `target_display_id` 
- `ro.product.first_api_level`  base 
- `ro.build.display.id.show` 
- `ro.build.version.release` manifest 
- market  base 
- `ro.oplus.watermark.betaversiononly.enable` 

:
- `BASE_PROP="/home/bruce/coloros_port/build/baserom/images/my_manifest/build.prop"`
- `PORT_PROP="/home/bruce/coloros_port/build/portrom/images/my_manifest/build.prop"`
`.name/.model/.manufacturer/.device/.brand/.my_product.type`  baserom  portrom   


###  M: VNDK apex 
- vendor  `.prop`  `ro.vndk.version` 
- `system_ext/apex/com.android.vndk.v${vndk_version}.apex`  port  base 

###  N:  build.prop 
- `for prop in $(find build/portrom/images -name build.prop)` 
  - `ro.build.version.security_patch=...`  `portrom_version_security_patch` 

---

## 6. 

`port.sh` 
- 
- Android //SoC/
ZIP APK/JAR  smali XML  feature build.prop 

 **** 

### 6.1 services.jar/SharedUID 
: `build/portrom/images/system/system/framework/services.jar`

:
- `build/<app_patch_folder>/patched/services.jar` 

:
1. `APKEditor.jar`  `services.jar`  smali `tmp/services`
2. `ScanPackageUtils.smali`  `assertMinSignatureSchemeIsValid`  `patchmethod.py`  `--`  **void **
3. `getMinimumSignatureSchemeVersionForTargetSdk` `move-result`  `const/4 vX, 0x0`  0 
4. `ReconcilePackageUtils.smali`  `ALLOW_NON_PRELOADS_SYSTEM_SHAREDUIDS`  `sput-boolean`  `const/4 <reg>, 0x1`  preload  system shareduid 
5. `APKEditor.jar`  `patched/services.jar` 

### 6.2 framework.jar
: `build/portrom/images/system/system/framework/framework.jar`

:
- `build/<app_patch_folder>/patched/framework.jar` 

:
1. `tmp/framework.jar` 
2. `devices/common/0001-core-framework-Introduce-OplusPropsHookUtils-V6.patch` 
3. `APKEditor.jar`   `tmp/framework`  smali 
4. `tmp/framework`  `git init`   commit  `git apply`  patch 
5. `APKEditor.jar`  `patched/framework.jar` 

:
-  `persist.oplus.prophook.*`  patch 

### 6.3 oplus-services.jarGMS Restriction 
: `oplus-services.jar``find build/portrom/images -name "oplus-services.jar"`

:
- `build/<app_patch_folder>/patched/oplus-services.jar` 

:
1. `APKEditor.jar`   `tmp/OplusService`
2. `OplusBgSceneManager.smali`  `isGmsRestricted`  `patchmethod.py`  `-isGmsRestricted`  **false **
3. 

### 6.4 Face UnlockSoC 8250/8350 
:
- `base_device_family == OPSM8250`  `OPSM8350`

:
- `devices/common/face_unlock_fix_common.zip`  vendor overlay  unzip
- baserom  `OPFaceUnlock.apk`  face unlock :
  - `devices/<base_product_device>/face_unlock_fix.zip`  unzip
  -  OnePlus faceunlock HAL/RC/VINTF/so 

### 6.5 A13A14  base/port 
:
- `base_android_version == 13`  `port_android_version == 14`

:
- `devices/common/a13_base_fix.zip`  unzip
-  charger/wifi/felica/midas manifestjarso 

### 6.6 PORT  Android 15  RIL/charger/NFC/cryptoeng
:
- `port_android_version >= 15`

SoC:
- `OPSM8250`  `devices/common/ril_fix_sm8250.zip`  unzip + 
- `OPSM8350`  `devices/common/ril_fix_sm8350.zip`  unzip + 

charger v3v6base  14 :
- `vendor.oplus.hardware.charger-V3-service`  `devices/common/charger-v6-update.zip`  unzip
- v3  bin/rc/ndk so 

base  13 :
- `devices/common/ril_fix_a13_to_a15.zip`  unzip
- `persist.vendor.radio.virtualcomm=1`  `odm/build.prop` 
- faceunlock/charger/wifi/felica 
- NFC: `devices/common/nfc_fix_for_a13.zip`  unzip   nfc 
- cryptoeng: `devices/common/cryptoeng_fix_a13.zip`  unzip

### 6.7 SurfaceFlinger  FPS 
- `vendor/default.prop`  `ro.surface_flinger.game_default_frame_rate_override=120` 

### 6.8 AI CallHeyTapSpeechAssist.apk
:
- `HeyTapSpeechAssist.apk``targetAICallAssistant`

:
- `build/<app_patch_folder>/patched/HeyTapSpeechAssist.apk` 

:
1.   `tmp/HeyTapSpeechAssist`
2. `AiCallCommonBean.smali`  `getSupportAiCall`  true `patchmethod_v2.py ... -return true`
3.  smali  `Build.MODEL`  `const-string <reg>, "PLG110"` 
4. 

### 6.9 OTA.apkdm-verity/
 region  `OTA_CN.apk` / `OTA_IN.apk` 
- `regionmark == CN`  `devices/common/OTA_CN.apk`  `system_ext/app/OTA/OTA.apk` 
-  `devices/common/OTA_IN.apk` 

`ota_patched==false`:
1. `OTA.apk`   `tmp/OTA`
2. `patchmethod_v2.py -d tmp/OTA -k ro.boot.vbmeta.device_state locked -return false`
   - locked  true/false   false  dm-verity 
3. 

### 6.10 AIUnit.apkHigh-End AI 
:
- `AIUnit.apk`

:
-  `MODEL=PLG110`
- `regionmark != CN`  `MODEL=CPH2745` 

:
1. `Build.MODEL`  `const-string` 
2. `UnitConfig.smali`  `isAllWhiteConditionMatch/isWhiteConditionsMatch/isSupport`  true 
3. `unit_config_list.json`  `jq` 
   - `whiteModels` 
   - 
   - `minAndroidApi`  30 
4. 

### 6.11 Android 16 port + base<15  AI Eraser 
:
- `port_android_version == 16`  `base_android_version < 15`

:
- `odm/lib64/libaiboost.so`  `my_product/lib64/libaiboost.so` 

### 6.12 Gallery AI Editor / xeu_toolbox
:
- `devices/common/xeutoolbox.zip`  & `base_android_version < 15` & `portIsColorOSGlobal != true`
  - sepolicy/file_context  unzipxeu_toolbox  `toolbox_exec` 
-  `base_android_version < 15`  `portIsColorOS != true` :
  - `OppoGallery2.apk` 
  - `patchmethod_v2.py -d tmp/Gallery -k 'const-string.*"ro.product.first_api_level"' -hook 'const/16 reg, 0x22'`
    - first_api_level  0x2234 AI Editor 

### 6.13 Battery.apkBattery SOH
:
- `base_device_family`  `OPSM8250`  `OPSM8350`

:
- `Battery.apk` `getUIsohValue`  `devices/common/patch_battery_soh.txt`  smali 
  - `/sys/class/oplus_chg/battery/battery_soh` 

### 6.14 Settings.apk
:
- `regionmark != CN`  `base_product_model != "IN20*"`

:
- `DeviceChargeInfoController.smali`  `isPreferenceSupport`  true 

### 6.15 OplusLauncher.apk 
:
- `OplusLauncher.apk` 
- `base_product_first_api_level > 34`

:
- `SystemPropertiesHelper.getFirstApiLevel`  `return 0x22`  first_api_level  34 

### 6.16 SystemUI.apkPanoramic AOD / MyDevice / 
:
- `build/<app_patch_folder>/patched/SystemUI.apk` 

:
1.   `tmp/SystemUI`
2. `SmoothTransitionController.smali` 
   - `setPanoramicStatusForApplication`
   - `setPanoramicSupportAllDayForApplication`
    true  stub 
3. `AODDisplayUtil.isPanoramicProcessTypeNotSupportAllDay`  false 
4. `base_product_first_api_level > 34`  `StatusBarFeatureOption.isChargeVoocSpecialColorShow`  true 
5. `regionmark != CN`  `FeatureOption.isSupportMyDevice`  true 
6. `styles.xml`  `style/null`  `7f1403f6` 
7. 

### 6.17 Aod.apk  AOD 
:
- `Aod.apk` 
- `base_product_first_api_level <= 35`

:
- `CommonUtils.isSupportFullAod`  true 
- `SettingsUtils.getKeyAodAllDaySupportSettings`  true 

### 6.18 Debloatdel-app via 
:
- `build/portrom/images/**/del-app/*` `kept_apps` 
- `debloat_apps` 
- KB2000/LE2101  `is_ab_device`  `debloat_apps` 
- `devices/common/via`  `product/app/` 

### 6.19 build.prop   
 build.prop 

1) `prepare_base_prop`functions.sh
- portrom  build.prop baserom  build.prop  portrom   
- `my_product/etc/bruce/build.prop` import 

2) `add_prop_from_port`functions.sh
-  portrom build.prop baserom  prop  bruce/build.prop 
- `ro.build.version.oplusrom*` 

3) `find build/portrom/images -name build.prop`  prop :
- timezone  `Asia/Shanghai` 
- `port_device_code`  `base_device_code` 
- model/name/device  base 
- `ro.build.user`  `build_user` 
- region lock  false 
- Global ColorOS  `=OnePlus`  `=OPPO` 

4) bruce/build.prop :
- `persist.adb.notify=0`
- `persist.sys.usb.config=mtp,adb`
- `persist.sys.disable_rescue=true`

### 6.20 Dolby + 
:
- baserom  `my_product/build.prop`  `ro.oplus.audio.effect.type`  `dolby`

:
- Dolby permission XML  base 
- `devices/common/dolby_fix.zip`  unzipAudioEffectCenter.apk  dolby XML
- audio  XML  base Wechat/WhatsApp 

### 6.21 Feature XML 
`add_feature_v2`functions.sh XML  `<feature>` 

:
- `oplus_features=(...)`  `add_feature_v2 oplus_feature ...`
- `app_features=(...)`  `add_feature_v2 app_feature ...`
- `permission_feature` / `permission_oplus_feature` 

:
- wireless charging /
- : `xmlstarlet`  app_feature 

### 6.22 AI Memory / aisubsystem / GT Mode
- realme / region  `ai_memory*.zip` 
- `app_v2.xml`  `com.oplus.aimemory`  `<enable>` 
- `devices/common/GTMode/overlay`  overlay feature 

### 6.23  / voice isolation / alert slider
- `com.oplus.app-features-ext-bruce.xml`  `com.oplus.plc_charge.support` 
- voice isolation  permission feature 
- alert slider feature 

### 6.24  feature 
:
- palmprintvibration eSIM  `remove_feature` 

eSIM `EuiccGoogle`  + feature 

### 6.25 base 
 2 :
- `base_android_version < 33` : `OnePlusCamera.apk`  base 
- : `OplusCamera`  base `product_overlay/framework`  jar  base 

:
- SoC 8250 `sys_camera_optimize_config.xml` QR  crash 

OnePlus9/9Pro/OP4E5D/OP4E3Fport ColorOS/Global/OOS Android  camera5.0  ZIP 
- `camera5.0-fix_cos.zip` / `camera5.0-fix_cos_global.zip` / `camera5.0-fix_oos.zip`
- `camera5.0-fix_odm.zip`
-  `live_photo_adds.zip`

### 6.26 Voice triggerOnePlus8T
:
- `base_product_device == OnePlus8T`

:
- voice wakeup feature 
- `devices/common/voice_trigger_fix.zip`  unzip

### 6.27 file_contexts  ASCII 
:
- `mkfs.erofs`  non-ASCII  file_contexts 

:
- `find build/portrom/images/config -name "*file_contexts" -exec perl -i -ne 'print if /^[\x00-\x7F]+$/' {}`

### 6.28 bootanimation / quickboot / wallpaper / overlay
- base  port  OS OOS/ColorOS CN/Global bootanimation  base 
- quickboot  base 
- `wallpaper.zip`  unzip
- `devices/common/overlay/*`  `devices/<device>/overlay/*` 

### 6.29 AON service / realme gesture / brightness
- SoC  `aon_fix_sm8250.zip` / `aon_fix_sm8350.zip` 
- realme gesture: `realme_gesture.zip` `ro.camera.privileged.3rdpartyApp`  `com.aiunit.aon;com.oplus.gesture;` 
-  prop  bruce/build.prop OnePlus8Pro  prop 

### 6.30 WeChat  / atfwd policy / Torch
- `Multimedia_Daemon_List.xml`  `xmlstarlet` wechat-livephoto  attribute  `all` 
- `atfwd@2.0.policy`  `getid/gettid/setpriority: 1` 
-  Torch  camera config 

### 6.31 Android 16 port + base<15 
:
- `port_android_version == 16`  `base_android_version < 15`

:
- `system_ext/priv-app/com.qualcomm.location` 
- NFC: `nfc_fix_a16_v2.zip`  unzip NfcNci 
- Wi-Fi: CN  `wifi_fix_a16.zip`  unzip Google wifi apex 
- OOS 16.0.1  `oos_1601_fix.zip`  unzip
- Find X3 Pro  prop/

---

## 7. AnyKernel ZIP init_boot KernelSU

### 7.1 AnyKernel  boot.img 
:
1. `devices/<device>/`  `*.zip` 
2. `unzip -l`  `anykernel.sh`  ZIP 
3. ZIP :
   - `*-KSU*`  `tmp/anykernel-ksu/`
   - `*-NoKSU*`  `tmp/anykernel-noksu/`
   -   `tmp/anykernel/`
4. 
   - `Image`kernel
   - `dtb`
   - `dtbo.img`
   `functions.sh`  `patch_kernel`  `boot_ksu.img` / `boot_noksu.img` / `boot_custom.img` 
5. `dtbo.img`  `devices/<device>/dtbo_*.img`  packaging 

### 7.2 KernelSU  init_boot  ksud 
1. `build/portrom/images/**/build.prop`  `ro.build.kernel.id` 
2. `kernel_major`: 6.1/6.6/6.12 KMI :
   - 6.1  `android14-6.1`
   - 6.6  `android15-6.6`
   - 6.12  `android16-6.12`
3. KMI :
   - `build/baserom/images/init_boot.img`  `tmp/init_boot/` 
   - `ksud boot-patch -b init_boot.img --magiskboot magiskboot --kmi <kmi>`
   -  `kernelsu_*.img`  `build/baserom/images/init_boot-kernelsu.img` 

---

## 8. AVB / Data 

### 8.1 AVB 
- `disable_avb_verify build/portrom/images/`functions.sh
  - fstab  `,avb...` / `avb_keys...` 

### 8.2 Data 
:
- `bin/port_config`  `remove_data_encryption=true`

:
- `find build/portrom/images -name "fstab.*"` 
  - `fileencryption=...`  `metadata_encryption=...` 
  - `fileencryption`  `encryptable` 

---

## 9. fs_config / file_contexts   mkfs.erofs

:
-  `build/portrom/images/<part>/` 

### 9.1 super 
- `super_extended=true`  `getSuperSize.sh others`
- KB2000/LE2101 
-  `getSuperSize.sh $base_product_device`

### 9.2  fs_config / file_contexts 
 `pname in $super_list` :
1. `python3 bin/fspatch.py build/portrom/images/$pname build/portrom/images/config/${pname}_fs_config`
2. `python3 bin/contextpatch.py build/portrom/images/$pname build/portrom/images/config/${pname}_file_contexts`

### 9.3 mkfs.erofs 
 1 :
```
mkfs.erofs -zlz4hc,9 --mount-point ${pname} \
  --fs-config-file build/portrom/images/config/${pname}_fs_config \
  --file-contexts build/portrom/images/config/${pname}_file_contexts \
  -T 1648635685 \
  build/portrom/images/${pname}.img build/portrom/images/${pname}
```

:
- `pack_type=EXT`  EXT4  EROFS 

---

## 10. vbmeta 

`build/baserom/`  `vbmeta*.img` `bin/patch-vbmeta.py`  verity/verification 

---

## 11. devices/<device> 

:
- `devices/<device>/recovery.img`  `build/baserom/images/`
- `devices/<device>/vendor_boot.img`  `build/baserom/images/`
- `devices/<device>/abl.img`  `build/portrom/images/`
- `devices/<device>/odm.img`  `build/portrom/images/`
- `devices/<device>/tz.img`  `build/baserom/images/`
- `devices/<device>/keymaster.img`  `build/baserom/images/`

AB :
- `my_preload.img`/`my_company.img`  `devices/common/*_empty.img` 

A-only :
- `my_preload.img`/`my_company.img` 

---

## 12. pack_method 

### 12.1 pack_method=stockota_from_target_files 
1. `out/target/product/<device>/` 
2. `IMAGES/`  `META/`  `SYSTEM/PRODUCT/SYSTEM_EXT/VENDOR/ODM/` 
3. `build/portrom/images/*.img`  `IMAGES/` 
4. baserom  `firmware-update/` :
   - `boot.img`  `IMAGES/` 
   :
   - `build/baserom/images/*.img`  `IMAGES/` 
5. `devices/<device>/boot_ksu.img` :
   - `IMAGES/boot.img`  `IMAGES/dtbo.img` 
   -  `spoof_bootimg`  boot cmdline  unlocked 
6. `META/ab_partitions.txt` 
   - `IMAGES/*.img`  basename  `super_list`  `map_file_generator`  `.map` 
7. `META/dynamic_partitions_info.txt`  `META/misc_info.txt`  `META/update_engine_config.txt` 
   - AB/AB `virtual_ab=true` 
8. A-only :
   - `OTA/bin/updater``releasetools.py``recovery.fstab`  devices  common 
   - firmware-update  storage-fw 
9. `prop_paths`  `build.prop` 
10. `otatools/bin/ota_from_target_files`  full OTA ZIP 
11. md5 

### 12.2 pack_method!=stockfastboot ZIP 
 `<part>.img`  `lpmake`  super  ZIP 

:
1. `lpmake` A/B  A-only 
2. `build/portrom/images/super.img` 
3. `zstd`  `build/portrom/super.zst` 
4. `out/<OS>_<rom_version>/` 
   - `super.zst`
   - `firmware-update/*.img`
   - `META-INF/com/google/android/update-binary`
   - `windows_flash_script.bat` / `mac_linux_flash_script.sh`
   - Windows  platform-toolsadb/fastboot 
    `device_code`/`REGIONMARK`/boot  sed 
5. `unix2dos`  Windows 
6.  `vbmeta*.img`  `patch-vbmeta.py` 
7. boot/dtbo  KSU/NoKSU/Custom 
8. `zip -r` md5 

---

## 13. 

1. `port.sh`  `blue "..."`   
2. :
   - APK/JAR  `tmp/<name>/`  smali 
   - ZIP  `devices/common`  `devices/<device>`  ZIP 
   - feature  `build/portrom/images/my_product/etc/extension/*.xml`  `.../permissions/*.xml`  diff 
3. `build/<version_name>/patched/` 2 smali 

---

## 14. 

- `pack_type=EXT`    
  `mkfs.erofs` EXT4 
- `jq`  `check`   
  AIUnit 
- framework.jar  git   
  `git` patch 
- `BASE_PROP/PORT_PROP`   
   my_manifest  `work_dir` 
-  ASCII  file_contexts   
  SELinux mkfs 
