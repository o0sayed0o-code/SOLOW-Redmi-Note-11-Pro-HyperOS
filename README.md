SOLOW-Redmi-Note-11-Pro-HyperOS-Flashing-Suite
Device Hardware Model Chipset OS Android Base Tool

Overview
This repository provides an automated, production-ready firmware flashing suite and technical documentation for upgrading the Redmi Note 11 Pro 5G / Pro+ 5G (Codename: pissarro / Model: 21091116C) from legacy MIUI Global 14.0.7 to official Xiaomi HyperOS 1.0 (OS1.0.17.0.TKTMIXM - Android 13.0).

The deployment pipeline integrates cryptographic firmware integrity verification (MD5 & Gzip/Tar stream analysis), automated Windows Fastboot driver injection (android_winusb.inf), and execution of a non-locking clean flash pipeline (flash_all.bat). This architecture guarantees preservation of the unlocked bootloader state, completely eliminating cross-region brick risks inherent to Chinese hardware running Global software.

Technical Specifications
Parameter	Specification
Target Device	Redmi Note 11 Pro / Redmi Note 11 Pro+ 5G
Hardware Codename	pissarro
Hardware Model Number	21091116C (Chinese Regional Motherboard)
SoC / Chipset	MediaTek Dimensity 920 5G (MT6877T, Octa-Core @ 2.5GHz)
Stock Baseline Firmware	MIUI Global 14.0.7.0 (V14.0.7.0.TKTMIXM, Android 13)
Target Firmware Version	Xiaomi HyperOS 1.0 (OS1.0.17.0.TKTMIXM, Android 13)
Firmware Type	Official Fastboot ROM (TGZ Archive / Sparse Partition Layout)
Package Filename	pissarro_global_images_OS1.0.17.0.TKTMIXM_20250401.0000.00_13.0_global_fbf41cd5ff.tgz
Cryptographic Hash (MD5)	FBF41CD5FFAA41979C76D73DAE31C7C1
Bootloader State	Unlocked (ro.boot.flash.locked: 0 / verifiedbootstate: orange)
Flashing Transport	Google Android Fastboot Interface over USB 2.0/3.0
Key Features & Safety Guardrails
Cryptographic Payload Validation: Mandates pre-flash MD5 checksum and tar-stream integrity tests to prevent flashing partially downloaded or corrupted chunks, avoiding mid-flash hard bricks.
Cross-Region Anti-Brick Guardrail: Enforces flash_all.bat (Clean All) instead of flash_all_lock.bat. Relocking the bootloader on a Chinese hardware motherboard (21091116C) running Global software (TKTMIXM) triggers permanent EDL regional brick.
Automated WinUSB Fastboot Driver Binding: Resolves Windows Device Manager Code 28 (CM_PROB_FAILED_INSTALL) by binding android_winusb.inf to Google Fastboot hardware identifiers (VID_18D1&PID_D00D).
Path Sanitization: Enforces strict directory isolation without whitespace (e.g., C:\pissarro_rom\ and C:\XiaoMi\) to prevent Mi Flash batch parser failures.
Sparse Super Partition Streaming: Manages 34-part sparse chunk streaming for the 4.5GB+ dynamic super.img containing system, vendor, product, and system_ext.
Repository Structure
Redmi-Note-11-Pro-HyperOS/
├── README.md                                       # Main repository documentation & overview
├── FLASHING_GUIDE.md                               # Technical manual & command reference
├── install_fastboot_driver.bat                     # Automated elevated Android Bootloader driver installer
├── firmware/                                       # Target HyperOS Fastboot firmware package
│   ├── images/                                     # Raw partition binaries (.img / .bin)
│   │   ├── boot.img
│   │   ├── super.img                               # Dynamic multi-partition system image
│   │   ├── userdata.img
│   │   ├── preloader_pissarro.bin
│   │   ├── vbmeta.img
│   │   ├── vbmeta_system.img
│   │   ├── vbmeta_vendor.img
│   │   └── cust.img
│   ├── flash_all.bat                               # Production clean-flash script (Leaves bootloader unlocked)
│   ├── flash_all_except_data_storage.bat           # In-place upgrade script (Not recommended for major OS jump)
│   └── misc.txt
└── tools/                                          # Host flashing utilities & SDK binaries
    ├── XiaoMiFlash.exe                             # Official Xiaomi Flash Tool GUI
    ├── adb.exe                                     # Android Debug Bridge v1.0.41
    ├── fastboot.exe                                # Android Fastboot Protocol binary
    └── driver/                                     # Signed Google WinUSB / Xiaomi USB driver suite
        ├── android_winusb.inf
        ├── xiaomiwinusba64.cat
        └── amd64/
Prerequisites & Host System Setup
Host Environment: Windows 10 / 11 64-bit with Administrator privileges.
Directory Isolation: Extract firmware and flashing tools into a root path without spaces (e.g., C:\XiaoMi\ and C:\pissarro_rom\).
Firmware Checksum Verification: Verify the downloaded .tgz archive against the official Xiaomi CDN MD5 signature prior to extraction:
Get-FileHash -Path "pissarro_global_images_OS1.0.17.0.TKTMIXM_*.tgz" -Algorithm MD5
# Expected Hash: FBF41CD5FFAA41979C76D73DAE31C7C1
Android Fastboot Driver Installation: Ensure Android Bootloader Interface is bound under Device Manager -> Android Device. If listed under Other devices with an error (Code 28):
pnputil.exe /add-driver "C:\Driver\android_winusb.inf" /install
Step-by-Step Deployment Procedure
Step 1: Device Handshake & Bootloader Verification
Enable Developer Options and toggle USB Debugging inside MIUI.

Connect the phone via USB and authorize the RSA host fingerprint.

Verify baseline properties:

adb shell "getprop ro.product.device; getprop ro.product.model; getprop ro.boot.flash.locked"
Expected Output:

pissarro
21091116C
0
(A value of 0 confirms the bootloader is officially unlocked).

Reboot into Fastboot mode:

adb reboot bootloader
(Alternatively: Power off device, hold [Volume Down + Power] until FASTBOOT appears).

Step 2: Fastboot Protocol Handshake
Verify host-to-device communication and hardware identification over fastboot:

fastboot devices
fastboot getvar product
fastboot getvar unlocked
Terminal Response:

votgmjbm65lrnjfa    fastboot
product: pissarro
unlocked: yes
Step 3: Automated Firmware Deployment (flash_all.bat)
Execute the clean flash sequence. This erases temporary caches, writes core low-level partitions, streams the sparse dynamic partitions, and writes the factory userdata table:

cd /d C:\pissarro_rom\pissarro_global_images_OS1.0.17.0.TKTMIXM_13.0
flash_all.bat
Caution

NEVER execute flash_all_lock.bat on this device!
Because hardware model 21091116C is originally China-region hardware, locking the bootloader on a Global ROM (TKTMIXM) will cause Xiaomi Anti-Rollback / Region Validation to reject the system image, resulting in a fatal hard brick. Always execute flash_all.bat.

Verification & Execution Log Analysis
1. Partition Erase & Firmware Handshake:
C:\pissarro_rom>fastboot getvar product 2>&1 | findstr "pissarro"
product: pissarro
"Now Check Finished ......"

fastboot erase boot_ab
Erasing 'boot_ab'          (bootloader) Partition boot_a erase successfully
                           (bootloader) Partition boot_b erase successfully
OKAY [  0.669s]

fastboot flash preloader_ab images\preloader_pissarro.bin
Sending 'preloader_ab' (452 KB)    OKAY [  0.080s]
Writing 'preloader_ab'             OKAY [  0.044s]
2. Core Kernel & VBMeta Flashing:
fastboot flash vbmeta_ab images\vbmeta.img
Sending 'vbmeta_ab' (4 KB)         OKAY [  0.069s]
Writing 'vbmeta_ab'                OKAY [  0.029s]

fastboot flash boot_ab images\boot.img
Sending 'boot_ab' (131072 KB)      OKAY [  3.511s]
Writing 'boot_ab'                  OKAY [  0.781s]
3. Dynamic super.img Sparse Chunk Streaming (34 Chunks):
fastboot flash super images\super.img
Sending sparse 'super' 1/34 (261134 KB)    OKAY [  7.444s]
Writing 'super'                            OKAY [  2.739s]
...
Sending sparse 'super' 34/34 (28556 KB)    OKAY [  0.864s]
Writing 'super'                            OKAY [  0.208s]
Finished. Total time: 336.777s
4. Userdata Format & Active Slot Finalization:
fastboot flash userdata images\userdata.img
Sending sparse 'userdata' 4/4 (113836 KB)  OKAY [  3.255s]
Writing 'userdata'                         OKAY [  0.368s]

fastboot set_active a
Setting current slot to 'a'                OKAY [  0.001s]

"Now Reboot ......"
fastboot reboot
Rebooting                                  OKAY [  0.001s]
Troubleshooting & Critical Resolution Matrix
Issue	Root Cause	Technical Resolution
ERROR: Data Error / Damaged tar archive	Network chunk drop during single-connection browser download.	Verify MD5 against FBF41CD5FFAA41979C76D73DAE31C7C1. Re-download using multi-threaded European mirror (bn.d.miui.com).
Windows Device Manager Code 28 (CM_PROB_FAILED_INSTALL)	Fastboot VID/PID (18D1:D00D) not mapped to generic WinUSB driver.	In Device Manager, update driver manually -> Choose Android Device -> Google, Inc. -> Android Bootloader Interface.
Mi Flash GUI crash: Could not find a part of the path ...\log	Xiaomi Flash Tool omitted creating the runtime log directory.	Create missing directory mkdir C:\XiaoMi\MiFlash20220507\log before launching GUI.
Hard Brick / Recovery Loop after flashing Global ROM	Bootloader locked (flash_all_lock.bat) on Chinese Motherboard (21091116C).	Only flash using flash_all.bat. Never re-lock bootloader on cross-region converted Xiaomi hardware.
License & Credits
Author / Maintainer: o0sayed0o-code (SOLOW)
Xiaomi Inc. for HyperOS official firmware and fastboot flashing tools.
Android Open Source Project (AOSP) for platform tools and fastboot protocol specifications.
