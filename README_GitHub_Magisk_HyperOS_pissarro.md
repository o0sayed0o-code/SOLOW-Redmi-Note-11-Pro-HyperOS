# SOLOW Redmi Note 11 Pro - Magisk Root & Security Research Suite

![Device](https://img.shields.io/badge/Device-Redmi%20Note%2011%20Pro%20(pissarro)-blue?style=for-the-badge&logo=xiaomi)
![Hardware Model](https://img.shields.io/badge/Hardware-21091116C%20(China%20HW)-red?style=for-the-badge)
![SoC](https://img.shields.io/badge/SoC-MediaTek%20Dimensity%20920%20(MT6877)-orange?style=for-the-badge)
![OS Version](https://img.shields.io/badge/OS-Xiaomi%20HyperOS%201.0%20(OS1.0.18.0.TKTMIXM)-brightgreen?style=for-the-badge&logo=android)
![Root Framework](https://img.shields.io/badge/Root-Magisk%20v28.1%20(Zygisk)-purple?style=for-the-badge)
![Injections](https://img.shields.io/badge/Modules-Shamiko%20|%20PlayIntegrity%20|%20TrickyStore-black?style=for-the-badge)
![Research](https://img.shields.io/badge/Research-DexProtector%20RASP%20Analysis-informational?style=for-the-badge)

---

## 📌 Overview

This repository provides a complete, production-grade root deployment package and technical security research dossier for the **Redmi Note 11 Pro 5G / Pro+ 5G (Codename: `pissarro` / Model: `21091116C`)** running **Xiaomi HyperOS 1.0 (OS1.0.18.0.TKTMIXM - Android 13.0)**.

In addition to ready-to-flash patched boot binaries and automated Windows scripts, this release documents deep-dive reverse engineering findings on **DexProtector RASP (Runtime Application Self-Protection)** and **Hardware-backed TEE / Keystore Attestation** in cross-region Xiaomi devices.

---

## 🔬 Technical Specifications

| Parameter | Specification |
| :--- | :--- |
| **Target Device** | Redmi Note 11 Pro / Redmi Note 11 Pro+ 5G |
| **Codename** | `pissarro` |
| **Hardware Motherboard** | `21091116C` (China Regional Hardware) |
| **Processor (SoC)** | MediaTek Dimensity 920 5G (MT6877T, Octa-Core @ 2.5 GHz) |
| **Base Firmware** | Xiaomi HyperOS 1.0 (`OS1.0.18.0.TKTMIXM`, Global Stable, Android 13) |
| **Kernel / Boot Size** | 134,217,728 bytes (128 MB Raw Partition) |
| **Root Solution** | Magisk v28.1 (Topjohnwu) with native Zygisk |
| **Hiding Architecture** | Shamiko v1.2.5 + PlayIntegrityFork v18 + Tricky Store v1.4.1 |
| **Bootloader State** | Unlocked (`ro.boot.flash.locked: 0`, `verifiedbootstate: orange`) |

---

## ⚡ Architecture & Components

```
SOLOW-Redmi-Note-11-Pro-Magisk-Suite/
├── README.md                                  # Complete Technical Guide & Analysis
├── flash_magisk_boot.bat                      # One-click Fastboot Magisk boot flasher
├── restore_stock_boot.bat                     # One-click Fastboot stock boot restore script
├── binaries/
│   ├── magisk_patched_boot.img                # Magisk v28.1 patched boot (128 MB)
│   └── boot_18.img                            # Untouched HyperOS 1.0.18 Stock boot (128 MB)
├── modules/
│   ├── Shamiko-v1.2.5.zip                     # Zygisk-based DenyList enforcer & root unmounting
│   ├── PlayIntegrityFork-v18.zip              # Device fingerprint spoofing (MEETS_DEVICE_INTEGRITY)
│   └── TrickyStore-v1.4.1.zip                 # Hardware Keystore / Leaf Certificate spoofer
├── research/
│   ├── libdexprotector.so                     # Extracted native RASP security engine binary
│   ├── inspect_native_pass.py                 # ELF inspection script for DexProtector exports
│   ├── dump_manifest.py                       # AndroidManifest AXML binary parser
│   └── query_magisk_db.py                     # SQLite interface for Magisk DenyList validation
└── tools/
    ├── adb.exe                                # Android Debug Bridge CLI
    └── fastboot.exe                           # Android Fastboot protocol CLI
```

---

## 🛠️ Boot Image Modification Workflow

### 1. Payload Demuxing
The clean stock boot image `boot_18.img` was extracted directly from the official Fastboot archive `pissarro_global_images_OS1.0.17.0.TKTMIXM_20250401.0000.00_13.0_global_fbf41cd5ff.tgz` and updated to match the HyperOS 1.0.18.0 build.

### 2. Ramdisk Patching via Magisk v28.1
The boot partition was patched via the official Magisk v28.1 engine:
- Injects `magiskinit` replacing the standard stock second-stage init.
- Patches the ramdisk overlay while preserving MediaTek vendor-specific early boot arguments.
- Maintains AVB (Android Verified Boot) compatibility using permissive vbmeta flags.

---

## 💉 Module Injections & Integrity Spoofing

The root suite implements a 3-tier stealth architecture designed for strict financial and enterprise environments:

### Tier 1: Zygote Isolation (Shamiko v1.2.5)
- Runs inside the Zygote 64-bit daemon.
- Unmounts Magisk loop mounts (`/sbin`, `/debug_ramdisk`, `mirror`) from the app mount namespace before `forkAndSpecialize`.
- Strips traces from `/proc/self/mountinfo`, `/proc/self/maps`, and `/proc/self/status`.

### Tier 2: Device Profile Integrity (PlayIntegrityFork v18)
- Spoofs valid certified hardware fingerprints to Google Play Services (`com.google.android.gms.unstable`).
- Bypasses basic integrity checks and satisfies `MEETS_DEVICE_INTEGRITY`.

### Tier 3: Keystore Attestation Interception (Tricky Store v1.4.1)
- Intercepts Android Keymaster / StrongBox TEE attestation calls.
- Injects a valid pre-generated cryptographic keybox to spoof the hardware certificate chain when the bootloader is marked `orange` (unlocked).

---

## 🔍 In-Depth Research: DexProtector RASP & Banking Crashes

During testing with the Bahrain national payment gateway **BenefitPay (`mobi.foo.benefit`)**, the application terminated immediately upon launch with the following stack trace:

```text
AndroidRuntime: FATAL EXCEPTION: main
Process: mobi.foo.benefit, PID: 11632
android.app.TerminateException$7JoCAQJPd3EOV9R0PN3o__$U0YMeX7W2rbVxUrkFZrKr3lo...
```

### Forensic Analysis Findings:

1. **Native Engine (`libdexprotector.so`)**:
   BenefitPay uses **Licel DexProtector Enterprise RASP**. The native library executes in the `.init_array` section before the Dalvik/ART VM executes `Application.onCreate()`.
2. **Dual-Vector Integrity Verification**:
   - **Vector A (User-space)**: Detects SU binaries, debuggers (ptrace), developer options, Frida hooks, and Xposed/LSPosed bridges. (Successfully concealed by Shamiko).
   - **Vector B (Hardware-backed TEE)**: Queries the hardware Trusted Execution Environment (`/dev/tee0` / MTK TrustZone) for hardware root of trust (`verifiedbootstate`).
3. **The Cross-Region Hardware Mismatch**:
   - The device motherboard is Chinese (`21091116C`), but was running Global firmware (`TKTMIXM`).
   - On cross-region firmware, locking the bootloader is impossible without hard-bricking the device (`The system has been destroyed`).
   - Consequently, the bootloader remained unlocked (`verifiedbootstate: orange`).
   - **DexProtector's RASP engine queries hardware attestation directly**. Because the hardware TEE reports an unlocked bootloader and a regional signing mismatch, it executes `abort()` / throws `TerminateException` at runtime.

### Conclusion:
Software root hiding (Magisk/Zygisk/Shamiko) cannot satisfy hardware TEE attestation when the bootloader is unlocked on mismatched regional hardware. The definitive resolution requires flashing the **Official China HyperOS ROM** and **safely locking the bootloader (`clean all and lock`)**, restoring 100% green hardware attestation without brick risk.

---

## 🚀 Quick Deployment Guide

### Prerequisites
- Windows 10/11 with Fastboot drivers installed.
- Unlocked bootloader on Redmi Note 11 Pro (`pissarro`).

### To Flash Magisk Root:
1. Put the device into Fastboot mode:
   ```cmd
   adb reboot bootloader
   ```
2. Double-click `flash_magisk_boot.bat`.
3. The script verifies device connectivity, flashes `magisk_patched_boot.img`, and reboots into system with Magisk v28.1 installed.

### To Revert to 100% Stock:
1. Put the device into Fastboot mode.
2. Double-click `restore_stock_boot.bat`.
3. The script flashes `boot_18.img`, removing Magisk and returning the boot partition to factory state.

---

## 📜 Credits & References
- **Magisk**: [topjohnwu/Magisk](https://github.com/topjohnwu/Magisk)
- **Shamiko**: [LSPosed/LSPosed.github.io](https://github.com/LSPosed)
- **PlayIntegrityFork**: [osm0sis/PlayIntegrityFork](https://github.com/osm0sis/PlayIntegrityFork)
- **TrickyStore**: [5ec1cff/TrickyStore](https://github.com/5ec1cff/TrickyStore)
- **Suite Author**: SOLOW
