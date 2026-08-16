# Docomo Galaxy S6(japanese flat model)    
# custom ROM archive  

## 海外版とのハードの違い

### １）センサーが違う
```
カーネルとセンサー.so(プロプライエタリ)を差し替える
```
### ２）GPSが違う
日本製はQualcommチップ
```
gpsdデーモンを動かないようにするため、init.rcを書き換える
```
### ３）NFCが違う
```
priv-appファイルなどモジュールが動かないようにするため、実行ファイルを削除する
```
### ４）モバイル通信
```
デーモンを動かさないようにするため、init.rcを書き換える
```
kickstart(mdm_helper)を起動させて/dev/ttyUSB1を生成する必要がある

```
## apply.bat
```
@echo off
echo "zeroflexx to zerofltedcm(sc05g)"
echo "changing to recovery mode""
adb reboot recovery
pause

adb shell "mount /system"

echo "1. editing init.rc"

adb push init.samsungexynos7420.rc /system/system/vendor/etc/init/hw/
adb push init.vendor.rilchip.rc /system/system/vendor/etc/init/
rm /system/system/vendor/etc/init/hw/init.gps.rc
rm /system/system/vendor/etc/init/hw/init.baseband.rc

echo "2. replacing sensor files"

adb push 32bit/sensors.universal7420.so /system/system/vendor/lib/hw/
adb push 64bit/sensors.universal7420.so /system/system/vendor/lib64/hw/

echo "3. stopping nfc"

rm /system/system/vendor/vendor/lib64/nfc_nci_sec.so
rm /system/system/lib64/libnfc_nci_jni.so
rm /system/system/lib64/libnfc-nci.so
rm /system/system/lib/libnfc_nci_jni.so
rm /system/system/lib/libnfc-nci.so

rm /system/system/framework/com.android.nfc_extras.jar
rm /system/system/framework/oat/arm64/com.android.nfc_extras.odex
rm /system/system/framework/oat/arm64/com.android.nfc_extras.vdex
rm /system/system/framework/oat/arm/com.android.nfc_extras.odex
rm /system/system/framework/oat/arm/com.android.nfc_extras.vdex

rm -rf /system/system/app/NfcNci
rm -rf /system/system/priv-app/Tag

echo "done"

pause

adb reboot
```

