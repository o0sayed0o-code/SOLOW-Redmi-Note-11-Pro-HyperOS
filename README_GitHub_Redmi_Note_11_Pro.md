# SOLOW Redmi Note 11 Pro (pissarro) - Unified HyperOS Engineering & Security Suite

[![Device](https://img.shields.io/badge/Device-Redmi%20Note%2011%20Pro%20%2F%20Pro%2B%20(pissarro)-blue?style=for-the-badge&logo=xiaomi)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Hardware Model](https://img.shields.io/badge/Hardware-21091116C%20(China%20Motherboard)-red?style=for-the-badge)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Chipset](https://img.shields.io/badge/SoC-MediaTek%20Dimensity%20920%20(MT6877)-orange?style=for-the-badge)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![OS Versions](https://img.shields.io/badge/HyperOS-Global%201.0.17%20%26%20China%201.0.24-brightgreen?style=for-the-badge&logo=android)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Banking App](https://img.shields.io/badge/BenefitPay-100%25%20Verified%20(No%20Crash)-blueviolet?style=for-the-badge)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Root Framework](https://img.shields.io/badge/Root-Magisk%20v28.1%20%2B%20Zygisk-purple?style=for-the-badge)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Localization](https://img.shields.io/badge/Localization-Full%20Arabic%20Apps%20%2B%20Gboard-teal?style=for-the-badge)](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
[![Author](https://img.shields.io/badge/Author-SOLOW%20(o0sayed0o--code)-darkred?style=for-the-badge&logo=github)](https://github.com/o0sayed0o-code)

---

## 📌 1. Repository Overview & Purpose

This repository is the definitive, production-grade firmware deployment suite, security research archive, and automation toolkit for the **Redmi Note 11 Pro 5G / Redmi Note 11 Pro+ 5G (Device Codename: `pissarro` / Motherboard ID: `21091116C`)**.

It bridges the architectural gap between Chinese regional hardware and modern Android security requirements, resolving complex cross-region brick risks, high-security banking app crashes (**Licel DexProtector RASP** on **BenefitPay**), and hardware-backed **TEE / Keystore Attestation** challenges.

The project is structured into **three modular, ready-to-deploy release suites**:
1. 🌟 **Suite 1: China HyperOS & Banking Fix Suite** (`SOLOW-Redmi-Note-11-Pro-China-HyperOS-Suite.zip` - 95.1 MB)
2. 🔬 **Suite 2: Magisk Root & Security Research Suite** (`SOLOW-Redmi-Note-11-Pro-Magisk-Suite.zip` - 70.7 MB)
3. 🛠️ **Suite 3: Base Flashing Tools & Driver Suite** (`SOLOW-Redmi-Note-11-Pro-Flashing-Tools.zip` - 92.3 MB)

---

## 📦 2. Master Release Packages Index

| Package / Suite | Size | Target Use Case | Key Highlights |
| :--- | :---: | :--- | :--- |
| 🌟 [**China HyperOS & Banking Suite**](#-suite-1-china-hyperos--banking-fix-suite) | **95.1 MB** | **Banking / Daily Driver** | Factory Bootloader Lock (`Green TEE`), BenefitPay working without crash, Google Play Store integration, full Arabic app localization, Gboard keyboard. |
| 🔬 [**Magisk Root & Security Suite**](#-suite-2-magisk-root--security-research-suite) | **70.7 MB** | **Development / Power Users** | Magisk v28.1 pre-patched boot (`magisk_patched_boot.img`), stock boot (`boot_18.img`), 1-click batch scripts, Shamiko v1.2.5, Tricky Store v1.4.1, DexProtector reverse engineering binaries. |
| 🛠️ [**Flashing Tools & Driver Suite**](#-suite-3-base-flashing-tools--driver-suite) | **92.3 MB** | **Host Setup & Global Flash** | Automated elevated Google Fastboot WinUSB installer (fixes Code 28), patched Mi Flash Tool 20220507, portable ADB/Fastboot, cross-region anti-brick guardrails. |

---

## 🌟 Suite 1: China HyperOS & Banking Fix Suite

**Archive:** `SOLOW-Redmi-Note-11-Pro-China-HyperOS-Suite.zip` (95.1 MB)  
**Primary Target:** Users requiring **100% functional banking applications (BenefitPay)**, official factory security, and seamless daily usability.

### Key Capabilities:
- **Zero-Crash Banking Environment:** Eliminates the fatal `android.app.TerminateException` by locking the bootloader cleanly with matching regional firmware (`CN` Hardware + `CN` Software).
- **Official Google Integration:** Leverages HyperOS's native GMS core, bundled with the modern Google Play Store (`com.android.vending` v52.9).
- **Comprehensive Arabic Localization:**
  - Includes official Google Keyboard (**Gboard** v18.4) with Arabic (Bahrain) layout.
  - Automated 1-click script (`set_all_apps_arabic.bat`) enforcing Arabic (`ar-BH`) on all 3rd-party apps (BenefitPay, WhatsApp, Chrome, Maps).
  - Permission grant script (`enable_morelocale2_arabic.bat`) for MoreLocale 2.
- **Ultra-Fast Regional CDN Links:** Bundles direct access to Xiaomi's Alibaba Cloud Singapore CDN mirror delivering 10–50 MB/s line speed.

### Suite 1 File Manifest:
```
SOLOW-Redmi-Note-11-Pro-China-HyperOS-Suite/
├── README.md                                  # Complete deployment documentation
├── FAST_DOWNLOAD_MIRRORS.txt                  # High-speed Singapore & Global CDN links
├── set_all_apps_arabic.bat                    # One-click Arabic app locale enforcer (ar-BH)
├── enable_morelocale2_arabic.bat              # One-click CHANGE_CONFIGURATION granter
├── GooglePlayStore.apk                        # Official Google Play Store APK
├── Gboard.apk                                 # Official Google Keyboard APK (ARM64)
└── tools/                                     # Portable Fastboot & ADB interface
    ├── adb.exe
    ├── fastboot.exe
    ├── AdbWinApi.dll
    └── AdbWinUsbApi.dll
```

---

## 🔬 Suite 2: Magisk Root & Security Research Suite

**Archive:** `SOLOW-Redmi-Note-11-Pro-Magisk-Suite.zip` (70.7 MB)  
**Primary Target:** Developers, reverse engineers, and power users who need kernel-level root access, Zygisk module injection, or forensic analysis tools.

### Key Capabilities:
- **Pre-Patched Magisk v28.1 Boot Image:** Ready-to-flash 128 MB kernel image (`magisk_patched_boot.img`) with MediaTek early boot parameters preserved.
- **Factory Clean Kernel Image:** Untouched 128 MB official boot image (`boot_18.img`) extracted from official HyperOS 1.0.18 payload.
- **One-Click Fastboot Batch Scripts:**
  - `flash_magisk_boot.bat`: Instantly flashes patched boot and reboots into rooted system.
  - `restore_stock_boot.bat`: Instantly flashes factory boot, removing Magisk cleanly.
- **Advanced Stealth Injections:**
  - **Shamiko v1.2.5:** Zygote process-level DenyList enforcement and `/proc/mounts` unmounting.
  - **PlayIntegrityFork v18:** Fingerprint spoofing satisfying `MEETS_DEVICE_INTEGRITY`.
  - **Tricky Store v1.4.1:** Android Keymaster / StrongBox TEE leaf certificate interception.
- **DexProtector RASP Security Dossier:** Decompiled binaries and Python inspection scripts for `libdexprotector.so`, demonstrating why software root masking fails against hardware-backed TEE attestation.

### Suite 2 File Manifest:
```
SOLOW-Redmi-Note-11-Pro-Magisk-Suite/
├── README.md                                  # In-depth technical & security dossier
├── flash_magisk_boot.bat                      # One-click Fastboot Magisk boot flasher
├── restore_stock_boot.bat                     # One-click Fastboot stock boot restore
├── binaries/
│   ├── magisk_patched_boot.img                # Magisk v28.1 patched boot (128 MB)
│   └── boot_18.img                            # Untouched HyperOS stock boot (128 MB)
├── modules/
│   ├── Shamiko-v1.2.5.zip
│   ├── PlayIntegrityFork-v18.zip
│   └── TrickyStore-v1.4.1.zip
├── research/
│   ├── libdexprotector.so                     # Extracted native RASP engine binary
│   ├── inspect_native_pass.py                 # ELF inspection script for DexProtector exports
│   ├── dump_manifest.py                       # AndroidManifest AXML binary parser
│   └── query_magisk_db.py                     # SQLite interface for Magisk DenyList
└── tools/
    ├── adb.exe
    ├── fastboot.exe
    ├── AdbWinApi.dll
    └── AdbWinUsbApi.dll
```

---

## 🛠️ Suite 3: Base Flashing Tools & Driver Suite

**Archive:** `SOLOW-Redmi-Note-11-Pro-Flashing-Tools.zip` (92.3 MB)  
**Primary Target:** Initial host machine setup, automated driver installation, and safe flashing of Global HyperOS without cross-region bricks.

### Key Capabilities:
- **Automated WinUSB Fastboot Driver Binding:** `install_fastboot_driver.bat` automatically binds `android_winusb.inf` to Google Fastboot identifiers (`VID_18D1&PID_D00D`), permanently fixing Windows Device Manager Code 28 (`CM_PROB_FAILED_INSTALL`).
- **Patched Xiaomi Flash Tool GUI:** `MiFlash20220507` with pre-created `log/` directory to prevent unhandled runtime exceptions.
- **Cross-Region Anti-Brick Guardrail:** Strictly enforces `flash_all.bat` (Clean All, leaving bootloader unlocked) for Global ROM on Chinese hardware.

### Suite 3 File Manifest:
```
SOLOW-Redmi-Note-11-Pro-Flashing-Tools/
├── install_fastboot_driver.bat                # Automated elevated WinUSB driver installer
├── FIRMWARE_DOWNLOAD_LINKS.txt                # Multi-region ROM mirrors and checksums
└── MiFlash20220507/                           # Official Xiaomi Flash Tool with log patch
    ├── XiaoMiFlash.exe
    ├── Driver/                                # Signed Qualcomm & Xiaomi drivers
    └── Source/ThirdParty/Google/Android/      # Fastboot & ADB toolchain
```

---

## ⚖️ 3. Comparative Architecture Matrix

| Parameter | Track A: Global ROM (Unlocked) | Track B: China ROM (Locked - Recommended) | Track C: Magisk Rooted (Zygisk) |
| :--- | :---: | :---: | :---: |
| **Installed Firmware** | HyperOS Global `OS1.0.17.0.TKTMIXM` | HyperOS China `OS1.0.24.0.TKTCNXM` | HyperOS Global/China with Patched Boot |
| **Bootloader State** | **Unlocked (`Orange State`)** | **Locked (`Green State`)** ✅ | Unlocked (`Orange State`) |
| **Cross-Region Brick Risk** | High if locked (Must stay unlocked) | **Zero Risk (Matching Hardware & Software)** ✅ | High if locked |
| **TEE Keystore Attestation** | Failed / Untrusted | **Certified Green (Hardware Verified)** ✅ | Failed (Hardware Mismatch) |
| **BenefitPay (Banking Apps)** | ❌ Crashes (`TerminateException`) | ✅ **100% Operational (No Crash)** | ❌ Crashes (`libdexprotector.so`) |
| **Google Play Services** | Pre-installed | Native Toggle (`Basic Google services`) | Working with MicroG / GMS |
| **Google Play Store** | Pre-installed | Installed via Suite APK | Installed via Suite APK |
| **Settings UI Language** | Multilingual (Inc. Arabic) | English / Chinese | Inherits base ROM |
| **App Language (WhatsApp, etc.)** | Arabic | **100% Arabic (`set_all_apps_arabic.bat`)** ✅ | 100% Arabic |
| **Typing Keyboard** | Standard | **Google Gboard (Arabic Bahrain Layout)** ✅ | Google Gboard |

---

## 🔬 4. Forensic Security Research: DexProtector RASP & Hardware TEE

During empirical testing with **BenefitPay (`mobi.foo.benefit`)**, the app terminated instantly on startup:

```text
AndroidRuntime: FATAL EXCEPTION: main
Process: mobi.foo.benefit, PID: 11632
android.app.TerminateException$7JoCAQJPd3EOV9R0PN3o__$U0YMeX7W2rbVxUrkFZrKr3lo...
```

### Forensic Analysis Breakdown:
1. **The Native Hook (`libdexprotector.so`):**
   BenefitPay employs **Licel DexProtector Enterprise RASP**. The native engine executes in the ELF `.init_array` section prior to the ART VM initiating `Application.onCreate()`.
2. **Dual-Tier Integrity Verification:**
   - **Tier 1 (User-Space Heuristics):** Scans for `su` binaries, Magisk sockets, SELinux state, test-keys, ptrace, and Zygote hooks. (Successfully neutralized by Shamiko).
   - **Tier 2 (Hardware-Backed TEE Attestation):** Directly queries the MediaTek TrustZone / Keymaster hardware via `/dev/tee0`.
3. **The Architectural Catch:**
   On Chinese hardware (`21091116C`), running Global software mandates an unlocked bootloader (`verifiedbootstate: orange`).
   DexProtector's Tier 2 checks communicate directly with hardware-level keystore registers. Even when Magisk conceals all user-space traces, **the hardware attestation reports an unlocked bootloader**, triggering an uncatchable `abort()` via JNI.
4. **The Resolution:**
   Switching to official **China HyperOS (`OS1.0.24.0.TKTCNXM`)** enables factory bootloader locking (`fastboot oem lock`), which restores hardware-backed TEE attestation to **Certified Green**, satisfying DexProtector completely.

---

## 🚀 5. High-Speed Fastboot ROM Mirrors & Checksums

Standard mirrors (`bigota.d.miui.com`) throttle international connections to ~20–100 KB/s. Use the validated **Alibaba Cloud Singapore CDN** mirror for full line-rate multi-threaded downloads:

| ROM Package | File Size | MD5 Checksum | High-Speed CDN Download Link |
| :--- | :---: | :---: | :--- |
| **China HyperOS Fastboot (`OS1.0.24.0.TKTCNXM`)** | 5.58 GB | `f92004a27b49e489d74cbbc924365905` | [**Download via Singapore CDN**](https://bkt-sgp-miui-ota-update-alisgp.oss-ap-southeast-1.aliyuncs.com/OS1.0.24.0.TKTCNXM/pissarro_images_OS1.0.24.0.TKTCNXM_20260313.0000.00_13.0_cn_f92004a27b.tgz) |
| **China HyperOS Recovery (`OS1.0.24.0.TKTCNXM`)** | 4.10 GB | `77f85b2180f45ef51d321a018c4bbeec` | [**Download via Singapore CDN**](https://bkt-sgp-miui-ota-update-alisgp.oss-ap-southeast-1.aliyuncs.com/OS1.0.24.0.TKTCNXM/miui_PISSARRO_OS1.0.24.0.TKTCNXM_77f85b2180_13.0.zip) |
| **Global HyperOS Fastboot (`OS1.0.17.0.TKTMIXM`)** | 5.60 GB | `fbf41cd5ffaa41979c76d73dae31c7c1` | [Download via Official Global Mirror](https://bn.d.miui.com/OS1.0.17.0.TKTMIXM/pissarro_global_images_OS1.0.17.0.TKTMIXM_20250401.0000.00_13.0_global_fbf41cd5ff.tgz) |

---

## 📖 6. Step-by-Step Deployment Guide

### Track 1: Clean Flash China ROM with Bootloader Lock (Banking Fix)
1. Download `pissarro_images_OS1.0.24.0.TKTCNXM_...tgz` and extract to `C:\pissarro_images_OS1.0.24.0.TKTCNXM_13.0\` (no spaces in path).
2. Reboot phone to Fastboot mode:
   ```powershell
   adb reboot bootloader
   ```
3. Run the locking clean-flash script:
   ```cmd
   flash_all_lock.bat
   ```
4. Phone flashes all partitions, executes `fastboot oem lock`, and reboots into locked, banking-certified HyperOS.
5. In Phone Settings: Enable **Basic Google services** (`Settings -> Accounts & sync`).
6. Install **Google Play Store** and **Gboard** from Suite 1.
7. Run [`set_all_apps_arabic.bat`](set_all_apps_arabic.bat) to enforce Arabic (`ar-BH`) on all apps.

---

### Track 2: Safe Flash Global HyperOS (Unlocked - Anti-Brick)
> [!CAUTION]
> NEVER execute `flash_all_lock.bat` when flashing Global ROM on hardware model `21091116C`! Doing so triggers an EDL hard brick. Always execute `flash_all.bat`.

1. Extract Global ROM to `C:\pissarro_rom\`.
2. Run `flash_all.bat` (Clean All, leaving bootloader unlocked).
3. System boots into multilingual Global HyperOS. Note: Strict banking apps (BenefitPay) will not operate in this track.

---

### Track 3: Flashing / Reverting Magisk Root
1. Put device into Fastboot mode (`adb reboot bootloader`).
2. Double-click `flash_magisk_boot.bat` to flash `magisk_patched_boot.img`.
3. To return to factory unrooted state at any time: double-click `restore_stock_boot.bat` to flash `boot_18.img`.

---

## 🛠️ 7. Troubleshooting & FAQ Matrix

| Issue | Root Cause | Technical Resolution |
| :--- | :--- | :--- |
| **BenefitPay crashes with `TerminateException`** | Bootloader unlocked (`Orange`) detected by hardware TEE | Flash China ROM via Suite 1 with `flash_all_lock.bat`. |
| **Windows Device Manager Code 28** | Fastboot VID:PID (`18D1:D00D`) unmapped | Run `install_fastboot_driver.bat` from Suite 3. |
| **Mi Flash GUI fails with log path error** | Omitted runtime `log/` folder | Create folder `mkdir C:\XiaoMi\MiFlash20220507\log`. |
| **Download from Xiaomi CDN is extremely slow** | High latency on Chinese `bigota` server | Use the Singapore CDN link from `FAST_DOWNLOAD_MIRRORS.txt`. |
| **Apps showing in English instead of Arabic** | China ROM defaults locale to English | Double-click `set_all_apps_arabic.bat` or use MoreLocale 2. |

---

## 📜 Credits & License
- **Author & Maintainer:** [o0sayed0o-code](https://github.com/o0sayed0o-code) (SOLOW)
- **Repository:** [SOLOW-Redmi-Note-11-Pro-HyperOS](https://github.com/o0sayed0o-code/SOLOW-Redmi-Note-11-Pro-HyperOS)
- **Upstream Vendors:** Xiaomi Inc. (HyperOS Firmware & Mi Flash Tool), Google LLC (Android Open Source Project & Platform Tools), Topjohnwu (Magisk Framework).
