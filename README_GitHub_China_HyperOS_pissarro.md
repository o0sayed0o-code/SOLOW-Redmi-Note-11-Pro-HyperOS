# SOLOW Redmi Note 11 Pro - China HyperOS Production Flashing & Banking Fix Suite

![Device](https://img.shields.io/badge/Device-Redmi%20Note%2011%20Pro%20(pissarro)-blue?style=for-the-badge&logo=xiaomi)
![Hardware Model](https://img.shields.io/badge/Hardware-21091116C%20(China%20HW)-red?style=for-the-badge)
![Chipset](https://img.shields.io/badge/SoC-MediaTek%20Dimensity%20920%20(MT6877)-orange?style=for-the-badge)
![OS Version](https://img.shields.io/badge/OS-Xiaomi%20HyperOS%201.0%20(OS1.0.24.0.TKTCNXM)-brightgreen?style=for-the-badge&logo=android)
![Bootloader](https://img.shields.io/badge/Bootloader-Locked%20(Official%20Green)-success?style=for-the-badge)
![Banking App](https://img.shields.io/badge/BenefitPay-100%25%20Functional%20(No%20Crash)-blueviolet?style=for-the-badge)
![Google Services](https://img.shields.io/badge/GMS-Native%20Google%20Play%20Store-informational?style=for-the-badge&logo=googleplay)
![Arabic Support](https://img.shields.io/badge/Localization-Full%20Arabic%20Apps%20%2B%20Gboard-teal?style=for-the-badge)

---

## 📌 1. Overview & Architecture

This repository contains the complete production-grade firmware suite, forensic security documentation, and deployment utilities for the **Redmi Note 11 Pro 5G / Pro+ 5G (Codename: `pissarro` / Model: `21091116C`)** running **Xiaomi HyperOS 1.0 (OS1.0.24.0.TKTCNXM - Android 13.0)**.

### The Problem Solved:
Users with Chinese regional hardware (`21091116C`) frequently flash Global ROMs to obtain English/Arabic menus. However:
1. On Chinese hardware running Global ROM, **the bootloader must remain permanently unlocked**. Attempting to lock the bootloader (`flash_all_lock.bat`) results in a catastrophic hard brick (`The system has been destroyed`).
2. High-security financial apps—most notably the Bahrain national payment gateway **BenefitPay (`mobi.foo.benefit`)**—employ native **DexProtector RASP (Runtime Application Self-Protection)** via `libdexprotector.so`.
3. DexProtector queries the hardware Trusted Execution Environment (TEE / Keystore Attestation). Detecting an unlocked bootloader (`verifiedbootstate: orange`), the native engine immediately invokes `android.app.TerminateException`, crashing before any UI or login loads.
4. User-space masking suites (Magisk v28.1, Zygisk, Shamiko v1.2.5, PlayIntegrityFork, Tricky Store) fail to satisfy hardware-level TEE queries on cross-region setups.

### The Definitive Architectural Solution:
Flashing the official **China HyperOS Stable ROM (`OS1.0.24.0.TKTCNXM`)** and executing **`clean all and lock` (`fastboot oem lock`)**. Because hardware region (`CN`) and software firmware (`CN`) match 100%, locking the bootloader is completely safe, native, and approved by Xiaomi factory engineering.

This restores the hardware TEE state to **Certified Green**, permanently resolving the BenefitPay crash while providing a rock-solid, ultra-fast system with native Google Play Services, Google Play Store, and full Arabic app localization.

---

## 🚀 2. Ultra-Fast High-Speed Download Mirrors

Standard Xiaomi China download links (`bigota.d.miui.com`) suffer from extreme latency and TCP window throttling for users outside China (~20-100 KB/s, taking 4+ hours). 

We have cataloged Xiaomi's official high-speed regional CDN mirror on **Alibaba Cloud Singapore**, delivering line-rate multi-threaded speeds (~10–50 MB/s) with full HTTP Range resume support:

### Fastboot ROM Archive (.tgz) - 5.58 GB:
* **Recommended Ultra-Fast Mirror (Alibaba Cloud Singapore CDN):**  
  [Download pissarro_images_OS1.0.24.0.TKTCNXM (High Speed)](https://bkt-sgp-miui-ota-update-alisgp.oss-ap-southeast-1.aliyuncs.com/OS1.0.24.0.TKTCNXM/pissarro_images_OS1.0.24.0.TKTCNXM_20260313.0000.00_13.0_cn_f92004a27b.tgz)
* **Cryptographic MD5 Checksum:** `f92004a27b49e489d74cbbc924365905`
* **Alternative Global Mirrors:**
  - `cdnorg`: `https://cdnorg.d.miui.com/OS1.0.24.0.TKTCNXM/pissarro_images_OS1.0.24.0.TKTCNXM_20260313.0000.00_13.0_cn_f92004a27b.tgz`
  - `bn`: `https://bn.d.miui.com/OS1.0.24.0.TKTCNXM/pissarro_images_OS1.0.24.0.TKTCNXM_20260313.0000.00_13.0_cn_f92004a27b.tgz`
  - `bigota`: `https://bigota.d.miui.com/OS1.0.24.0.TKTCNXM/pissarro_images_OS1.0.24.0.TKTCNXM_20260313.0000.00_13.0_cn_f92004a27b.tgz`

---

## 🔬 3. Technical Specifications

| Parameter | Specification |
| :--- | :--- |
| **Target Device** | Redmi Note 11 Pro 5G / Redmi Note 11 Pro+ 5G |
| **Device Codename** | `pissarro` |
| **Motherboard Hardware ID** | `21091116C` (China Regional Hardware) |
| **SoC / Chipset** | MediaTek Dimensity 920 5G (MT6877, Octa-Core @ 2.5 GHz) |
| **Installed Firmware** | Xiaomi HyperOS 1.0 (`OS1.0.24.0.TKTCNXM`, China Stable) |
| **Base Android Version** | Android 13.0 (Tiramisu, API 33) |
| **Anti-Rollback Index** | `anti: 2` |
| **Bootloader State** | **Locked (`ro.boot.flash.locked: 1`, Green Verified State)** |
| **Google Framework** | Native GMS Core + Official Google Play Store (`com.android.vending`) |
| **Keyboard** | Google Gboard (`com.google.android.inputmethod.latin`) |
| **App Localization** | Arabic (Bahrain / `ar-BH`) enforced for all 3rd-party apps |

---

## 🛠️ 4. Flashing & Deployment Protocol

### Step 1: Pre-Flash Hardware Validation
Verify hardware identification, anti-rollback index, and bootloader state over USB fastboot:
```powershell
fastboot devices
fastboot getvar product
fastboot getvar anti
fastboot getvar unlocked
```
*Expected Terminal Output:*
```text
votgmjbm65lrnjfa    fastboot
product: pissarro
anti: 2
unlocked: yes
```

### Step 2: Clean Flash & Re-Lock Execution
Execute the clean flash and locking sequence directly from the extracted ROM root:
```cmd
cd /d C:\pissarro_images_OS1.0.24.0.TKTCNXM_13.0
flash_all_lock.bat
```
The script flashes all 33 sparse chunks of `super.img` (8.3 GB), wipes user data, and executes:
```text
fastboot oem lock -> OKAY [1.455s]
fastboot reboot
```
The device reboots with a factory-locked bootloader and verified hardware attestation.

---

## 📲 5. Google Play & Arabic Localization Setup

### 5.1 Native Google Play Services:
1. Open **Settings** ➔ **Accounts & sync**.
2. Toggle **Basic Google services** to **ON**.

### 5.2 Official Google Play Store:
Install the official modern Google Play Store APK (included in this release package):
```cmd
adb install -r GooglePlayStore.apk
```
The Play Store icon will appear on the launcher. Launch it and sign in with your Google account.

### 5.3 Google Keyboard (Gboard):
Install Gboard (included in this release package or via Play Store):
```cmd
adb install -r Gboard.apk
```
1. Open Gboard ➔ **Enable in Settings** ➔ Select Gboard.
2. Select **Languages** ➔ **Add Keyboard** ➔ **Arabic (Bahrain)**.

### 5.4 Automated Arabic Localization for All Apps:
To ensure all installed applications (BenefitPay, WhatsApp, Chrome, Google Maps, etc.) display in full Arabic with right-to-left layout:
Double-click [`set_all_apps_arabic.bat`](set_all_apps_arabic.bat), which executes:
```cmd
adb shell "for pkg in $(pm list packages -3 | cut -d: -f2); do cmd locale set-app-locales $pkg --locales ar-BH; done"
```

---

## 📂 6. Repository & Suite Structure

```
SOLOW-Redmi-Note-11-Pro-China-HyperOS-Suite/
├── README.md                                  # Complete technical guide & manual
├── FAST_DOWNLOAD_MIRRORS.txt                  # High-speed CDN mirror links & checksums
├── set_all_apps_arabic.bat                    # One-click script to enforce Arabic on all apps
├── enable_morelocale2_arabic.bat              # One-click permission script for MoreLocale 2
├── GooglePlayStore.apk                        # Official Google Play Store APK v52.9
├── Gboard.apk                                 # Official Google Keyboard APK v18.4 (ARM64)
└── tools/                                     # Portable ADB & Fastboot binaries
    ├── adb.exe
    ├── fastboot.exe
    ├── AdbWinApi.dll
    └── AdbWinUsbApi.dll
```

---

## 🛡️ 7. Security Best Practices & Maintenance
- **Keep Bootloader Locked:** Do not unlock the bootloader. Keeping it locked maintains Green TEE Keystore status, ensuring ongoing compatibility with banking, government, and payment services.
- **App Updates:** All installed apps (BenefitPay, Gboard, WhatsApp) update seamlessly through the Google Play Store.

---
**Maintained by:** SOLOW  
*Redmi Note 11 Pro 5G / Pro+ 5G Technical Engineering Project*
