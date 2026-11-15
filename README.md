# CyanogenMod 7.2 UNOFFICIAL — Build Trees for Samsung Galaxy S Duos (GT-S7562 / kylessopen)

Maintainer: **proud_daycord**

This bundle contains all three repositories and system patches required to build CyanogenMod 7.2 (`Android 2.3.7`) + Linux `3.0.8` kernel for **Samsung Galaxy S Duos (`GT-S7562` / `kylessopen`)** from source.

---

## Repositories Included

1. **`android_device_samsung_kylessopen`** -> `device/samsung/kylessopen`  
   Device tree, `BoardConfig.mk`, `init.kylessopen.rc`, `init.sdcard.sh`, `vold.fstab`, `btcoex_daemon`, `libaudio`, `libcamera`, and `patches/`.
2. **`proprietary_vendor_samsung_kylessopen`** -> `vendor/samsung/kylessopen`  
   Proprietary blobs (`libril-qc-1.so`, `liboemcamera.so`, Adreno 200 EGL, `ath6kl` firmware, `qmuxd`, `netmgrd`, etc.).
3. **`android_kernel_samsung_kylessopen`** -> `kernel/samsung/kylessopen`  
   Linux `3.0.8` kernel source with `arch/arm/configs/kylessopen_defconfig`.

---

## Quick Build Guide

### 1. Initialize CyanogenMod 7.2 source tree
```bash
mkdir -p ~/cm7 && cd ~/cm7
repo init -u https://github.com/CyanogenMod/android.git -b gingerbread
repo sync -j4
```

### 2. Add `kylessopen` repositories
Copy `local_manifest.xml` to `~/cm7/.repo/local_manifests/kylessopen.xml` (updating `YOUR_GITHUB_USERNAME`) and sync:
```bash
repo sync -j4
```
*(Or manually clone the 3 folders into `device/samsung/kylessopen`, `vendor/samsung/kylessopen`, and `kernel/samsung/kylessopen`.)*

### 3. Apply AOSP/CM7 system patches
These patches apply the fixes for Bluetooth re-enable (`libbluedroid`), empty loop-device filtering in `vold`, and `ifc_del_address` in `libnetutils` (for `netmgrd`):
```bash
cd ~/cm7
./device/samsung/kylessopen/patches/apply_patches.sh
```

### 4. (Optional) Rebuild Kernel from source
```bash
cd ~/cm7/kernel/samsung/kylessopen
export ARCH=arm
export CROSS_COMPILE=~/cm7/prebuilt/linux-x86/toolchain/arm-eabi-4.4.3/bin/arm-eabi-
make kylessopen_defconfig
make -j$(nproc) zImage
cp arch/arm/boot/zImage ~/cm7/device/samsung/kylessopen/kernel
```

### 5. Build the ROM
```bash
cd ~/cm7
source build/envsetup.sh
brunch kylessopen
```
The flashable zip will be generated in `out/target/product/kylessopen/`.
