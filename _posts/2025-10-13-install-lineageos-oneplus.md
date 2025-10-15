---
layout: post
title: "How I Installed LineageOS 22.2 on My OnePlus Nord CE 2 Lite (Oscar/Holi)"
date: 2025-10-13
categories: android custom-rom oneplus lineageos
---

### Introduction

After experimenting with PixelOS and OxygenOS 14 on my OnePlus Nord CE 2 Lite 5G (codename **oscar/holi**), I decided to go all-in and install **LineageOS 22.2** — a clean, Android 15-based custom ROM.

This post is both my personal installation log *and* a step-by-step tutorial in case you want to try it yourself.

---

## 🧰 Prerequisites

Before starting, make sure you have:

- A **OnePlus Nord CE 2 Lite 5G (oscar/holi)** with bootloader **unlocked**
- **OxygenOS 14** firmware already installed (Android 14 base)
- A computer with **ADB + Fastboot** installed  
  → [Download Platform-Tools (r33+)](https://developer.android.com/studio/releases/platform-tools)
- USB debugging enabled
- A good USB-A ↔ USB-C data cable
- At least **60 % battery**

Create this folder structure for clarity:

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

## ⚙️ Step 1 — Boot into Fastboot

On the phone:
```
adb reboot bootloader
```

or manually:
```
Power off → hold Volume Up + Volume Down + Power
```

Verify connection:
```bash
cd ~/Downloads/platform-tools
./fastboot devices
```

---

## ⚙️ Step 2 — Flash the Essential Images

```bash
./fastboot flash boot ../lineageos/boot.img
./fastboot flash vendor_boot ../lineageos/vendor_boot.img
./fastboot flash dtbo ../lineageos/dtbo.img
./fastboot flash vbmeta --disable-verity --disable-verification ../lineageos/vbmeta.img
```

Then boot into Lineage Recovery:
```bash
./fastboot reboot recovery
```

---

## ⚙️ Step 3 — Wipe and Prepare Data

On the phone in **Lineage Recovery**:

```
Factory reset → Format data / factory reset → Yes
```

Then:
```
Apply update → Apply from ADB
```

---

## ⚙️ Step 4 — Sideload the ROM

Back on your Mac:
```bash
./adb sideload ../lineageos/lineage-22.2-20251006-nightly-oscaro-signed.zip
```

> 💡 On macOS, ADB often stops at **47 %** — this is normal.  
> Watch your phone for the real progress bar.

Once it says *Install completed successfully*, continue.

---

## ⚙️ Step 5 — (Optionally) Install GApps

If you want Google Play and Play Services, sideload the **Android 15** GApps package:

```bash
./adb sideload ../lineageos/MindTheGapps-15.0.0-arm64.zip
```

---

## ⚙️ Step 6 — (Optionally) Install Magisk for Root

To get root access and module support:

```bash
./adb sideload ../lineageos/Magisk-v27.0.zip
```

After boot, open the Magisk app → it will finalize installation.

---

## 🚀 Step 7 — Reboot into LineageOS

On the phone:
```
Reboot system now
```

⏳ The first boot may take **5–10 minutes**.  
Eventually, you’ll see the LineageOS boot animation — success!

---

## 🎉 Conclusion

After hours of flashing, retries, and one close call with the wrong GApps version 😅, I finally booted into **LineageOS 22.2 (Android 15)** on **October 13 2025**.

Everything works smoothly — Wi-Fi, camera, cellular, and Magisk root.  
This was my cleanest custom ROM flash yet.

---

### Optional: Verify Root & SafetyNet

If you installed Magisk, open a terminal:
```bash
su
```
You should see a `#` prompt and “MagiskSU”.  
Then install **TrickyStore** or **SafetyNet Fix** if you need Play Integrity.

---

### Files Used

| File | Purpose | Notes |
|------|----------|-------|
| `boot.img` | LineageOS boot image | From Lineage build |
| `vendor_boot.img` | Required for A/B dynamic | |
| `dtbo.img` | Device tree overlay | |
| `vbmeta.img` | Verified Boot | Flashed with verity disabled |
| `lineage-22.2-20251006-nightly-oscaro-signed.zip` | Main ROM | |
| `MindTheGapps-15.0.0-arm64.zip` | Google Apps (optional) | |
| `Magisk-v27.0.zip` | Root (optional) | |

---

### Notes

- Never flash `super_empty.img` manually.  
- For microG instead of GApps, flash **LineageOS for microG** builds.  
- Keep a copy of your working fastboot/adb binaries — macOS Sequoia sometimes causes segmentation faults with r36.

---

**Last updated:** *October 13 2025*  
— Krishna
