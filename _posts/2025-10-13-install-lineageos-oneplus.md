---
layout: post
title: Tech Diaries: Installing LineageOS 22.2 on My OnePlus Nord CE 2 Lite
date: 2025-10-13
categories: android custom-rom oneplus lineageos
---

### Introduction

After weeks of switching between PixelOS and OxygenOS 14 on my OnePlus Nord CE 2 Lite 5G (codename **oscar/holi**), I finally decided to go for something clean and stable — **LineageOS 22.2**, based on Android 15.

This isn’t a tutorial. It’s my own log of what I did, what worked, and what went wrong while flashing and setting up LineageOS on this device.  
If you’re familiar with custom ROMs, you’ll understand the feeling — that mix of curiosity, panic, and excitement.

---

## 🧰 Preparation

I had my **OnePlus Nord CE 2 Lite 5G** with the bootloader already unlocked and **OxygenOS 14** as the base firmware.  
On my Mac, I installed **ADB + Fastboot** from the Android Platform Tools.

My folder setup looked like this:

```
~/Downloads/
 ├── platform-tools/
 └── lineageos/
      ├── boot.img
      ├── vendor_boot.img
      ├── dtbo.img
      ├── vbmeta.img
      ├── lineage-22.2-20251006-nightly-oscaro-signed.zip
      ├── MindTheGapps-15.0.0-arm64.zip
      └── Magisk-v27.0.zip
```

---

## ⚙️ Fastboot and Recovery

First, I rebooted into fastboot:

```
adb reboot bootloader
```

Checked connection:
```bash
cd ~/Downloads/platform-tools
./fastboot devices
```

Then flashed the core images one by one:

```bash
./fastboot flash boot ../lineageos/boot.img
./fastboot flash vendor_boot ../lineageos/vendor_boot.img
./fastboot flash dtbo ../lineageos/dtbo.img
./fastboot flash vbmeta --disable-verity --disable-verification ../lineageos/vbmeta.img
```

After that, I rebooted straight into recovery:
```
./fastboot reboot recovery
```

---

## 🧹 Reset and Sideload

Inside Lineage Recovery, I formatted data:
```
Factory reset → Format data / factory reset → Yes
```

Then went into:
```
Apply update → Apply from ADB
```

From my Mac:
```bash
./adb sideload ../lineageos/lineage-22.2-20251006-nightly-oscaro-signed.zip
```

As usual, macOS stopped at **47%**, but the phone continued fine.  
When it showed *Install completed successfully*, I exhaled.

---

## 📦 Optional Installs

I decided to add Google Apps:
```bash
./adb sideload ../lineageos/MindTheGapps-15.0.0-arm64.zip
```

and then Magisk for root access:
```bash
./adb sideload ../lineageos/Magisk-v27.0.zip
```

After reboot, Magisk finished setup automatically.

---

## 🚀 First Boot

Finally:
```
Reboot system now
```

The first boot took about 8 minutes — that long silence before the **LineageOS** animation appeared.  
It worked. Clean, simple, smooth. Everything functional — Wi-Fi, mobile data, camera, and root.

---

## 🧩 What I Learned

- **ADB on macOS** sometimes shows 47% progress even when flashing continues fine.  
- Never flash `super_empty.img`.  
- Always disable verification when flashing `vbmeta`.  
- Android 15 + Magisk 27.0 = stable combo for now.  

If you care about Play Integrity, **TrickyStore** or **SafetyNet Fix** modules still help.  

---

## 🧠 Files I Used

| File | Purpose | Notes |
|------|----------|-------|
| `boot.img` | LineageOS boot image | Core boot partition |
| `vendor_boot.img` | Vendor partition | Needed for dynamic partitions |
| `dtbo.img` | Device tree overlay | Required |
| `vbmeta.img` | Verified Boot | Disable verification |
| `lineage-22.2-20251006-nightly-oscaro-signed.zip` | Main ROM | Android 15 build |
| `MindTheGapps-15.0.0-arm64.zip` | Google Apps | Optional |
| `Magisk-v27.0.zip` | Root access | Optional |

---

### Final Thoughts

I flashed **LineageOS 22.2** on **October 13, 2025**, and it’s been perfectly stable so far.  
Compared to the chaos of PixelOS and the bloat of OxygenOS, this one feels refreshing — light, stock, and genuinely usable.

Not a tutorial. Just what I did — and it worked.

---

**Last updated:** *October 13, 2025*  
— Krishna
