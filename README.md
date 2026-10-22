# LineageOS 22.1 (Android 15) — Build Images & OTA Zip for Motorola Edge 40 (`lyriq`)

Official-style flashable OTA `.zip` and individual partition `.img` files for **Motorola Edge 40 (`lyriq` / `XT2303-2`)** running **LineageOS 22.1 (`lineage_lyriq-ap4a-userdebug`, Build Date: `2026-10-22`)**.

> [!IMPORTANT]
> **Base Firmware Requirement:** Ensure your device is on Stock **Android 15 (`V1TLS35.73-60-3-10`)** firmware on both slots (`a` and `b`) before flashing LineageOS 22.1.

---

## 1. Downloads (Release `v22.1-20261022`)

All files (including large >100 MB logical partition images and the `1.4 GB` OTA `.zip`) are hosted in the **[GitHub Releases (`v22.1-20261022`)](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/tag/v22.1-20261022)** page of this repository:

| File | Size | Type / Target Partition | Direct Download Link |
| :--- | :--- | :--- | :--- |
| **`lineage-22.1-20261022-UNOFFICIAL-lyriq.zip`** | `1.4 GB` | Full Flashable OTA Zip (Recovery Sideload) | **[Download ZIP](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/lineage-22.1-20261022-UNOFFICIAL-lyriq.zip)** |
| **`lineage-22.1-20261022-UNOFFICIAL-lyriq.zip.sha256sum`** | `109 B` | SHA256 Checksum | **[Download SHA256](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/lineage-22.1-20261022-UNOFFICIAL-lyriq.zip.sha256sum)** |
| **`vbmeta.img`** | `8.0 KB` | Physical Partition (`fastboot`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/vbmeta.img)** |
| **`vbmeta_system.img`** | `4.0 KB` | Physical Partition (`fastboot`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/vbmeta_system.img)** |
| **`boot.img`** | `64 MB` | Physical Partition (`fastboot`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/boot.img)** |
| **`init_boot.img`** | `8.0 MB` | Physical Partition (`fastboot`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/init_boot.img)** |
| **`vendor_boot.img`** | `64 MB` | Physical Partition + Lineage Recovery (`fastboot`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/vendor_boot.img)** |
| **`dtb.img`** | `242 KB` | Device Tree Image (`mt6893.dtb`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/dtb.img)** |
| **`super_empty.img`** | `5.1 KB` | Super Partition Metadata Layout (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/super_empty.img)** |
| **`system.img`** | `607 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/system.img)** |
| **`system_ext.img`** | `202 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/system_ext.img)** |
| **`product.img`** | `324 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/product.img)** |
| **`vendor.img`** | `685 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/vendor.img)** |
| **`system_dlkm.img`** | `12 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/system_dlkm.img)** |
| **`vendor_dlkm.img`** | `42 MB` | Logical Partition inside `super` (`fastbootd`) | **[Download](https://github.com/ayushraj7520/lineage_motorola_lyriq_releases/releases/download/v22.1-20261022/vendor_dlkm.img)** |

*(Note: Physical boot/recovery images under 100 MB are also committed directly into the repository root for quick access).*

---

## Method 1: Flashing the OTA `.zip` via LineageOS Recovery (Recommended & Safest)

Flashing the OTA `.zip` via Recovery automatically creates/resizes all dynamic logical partitions inside `super` (`system`, `system_ext`, `product`, `vendor`, `system_dlkm`, `vendor_dlkm`) and flashes all boot partitions cleanly.

### Step 1: Boot into Bootloader (`fastboot`) Mode
Power off the phone, then hold **Volume Down + Power** to enter Bootloader (`fastboot`) mode.

### Step 2: Flash VBMeta & Boot/Recovery Images
*(Note: `vbmeta.img` is already compiled with `--set_hashtree_disabled_flag` (`Flags: 1`) via `BoardConfig.mk` and signed against this build's images, so `--disable-verity --disable-verification` is not required).*
```bash
fastboot flash vbmeta vbmeta.img
fastboot flash vbmeta_system vbmeta_system.img
fastboot flash boot boot.img
fastboot flash init_boot init_boot.img
fastboot flash vendor_boot vendor_boot.img
```

### Step 3: Reboot into LineageOS Recovery
Use Volume keys on the bootloader screen to select **Recovery Mode** and press **Power** (or run):
```bash
fastboot reboot recovery
```

### Step 4: Format Data (Required on First Clean Install)
In LineageOS Recovery:
1. Tap **Factory reset** $\rightarrow$ **Format data / factory reset** $\rightarrow$ Confirm **Format data**.
2. Go back to the main menu.

### Step 5: Sideload the ROM `.zip`
1. In LineageOS Recovery, tap **Apply update** $\rightarrow$ **Apply from ADB**.
2. On your PC, run:
```bash
adb sideload lineage-22.1-20261022-UNOFFICIAL-lyriq.zip
```
3. *(Note: Output stopping at `47%` with `Total xfer: 1.00x` is completely normal for A/B payload sideloading).*
4. Tap the back arrow and select **Reboot system now**.

---

## Method 2: Flashing Individual `.img` Files in Series (`fastboot` + `fastbootd`)

> [!CAUTION]
> **Critical Distinction — Bootloader (`fastboot`) vs Userspace (`fastbootd`):**
> - **Physical partitions** (`vbmeta`, `vbmeta_system`, `boot`, `init_boot`, `vendor_boot`) MUST be flashed in **Bootloader `fastboot` mode**.
> - **Dynamic/Logical partitions** (`system`, `system_ext`, `product`, `vendor`, `system_dlkm`, `vendor_dlkm`) live inside `super` and **MUST be flashed in Userspace `fastbootd` mode** (`fastboot reboot fastboot`).
> - If you are coming directly from Motorola Stock ROM, the stock `super` partition does not have a `system_ext` logical partition mapped. You **must** run `fastboot wipe-super super_empty.img` (or create the logical partitions) inside `fastbootd` first, otherwise `init` will fail with `DM_DEV_STATUS failed for system_ext`!

### Phase 1: Flash Physical Partitions (in Bootloader `fastboot` Mode)
```bash
# 1. Flash VBMeta images (already built with hashtree_disabled flag = 1)
fastboot flash vbmeta vbmeta.img
fastboot flash vbmeta_system vbmeta_system.img

# 2. Flash Kernel, Ramdisk, and Vendor Boot (Recovery/fastbootd)
fastboot flash boot boot.img
fastboot flash init_boot init_boot.img
fastboot flash vendor_boot vendor_boot.img
```

### Phase 2: Enter Userspace `fastbootd` Mode
```bash
fastboot reboot fastboot
```
*(Wait until the phone screen shows the **LineageOS Recovery `fastbootd`** screen).*

### Phase 3: Initialize `super` (First-Time from Stock ROM) & Flash Logical Partitions in Order
```bash
# 1. Reset super partition layout using super_empty.img (Recommended when coming from Stock ROM)
fastboot wipe-super super_empty.img

# 2. Flash Dynamic Partitions in exact order
fastboot flash system system.img
fastboot flash system_ext system_ext.img
fastboot flash product product.img
fastboot flash vendor vendor.img
fastboot flash system_dlkm system_dlkm.img
fastboot flash vendor_dlkm vendor_dlkm.img
```

*(Alternative if `wipe-super` is not supported by your host `fastboot` binary: manually ensure logical partitions exist for your active slot, e.g. `a` or `b`):*
```bash
# Example if active slot is 'a' (replace _a with _b if on slot b):
fastboot delete-logical-partition product_a
fastboot delete-logical-partition system_ext_a
fastboot create-logical-partition product_a 400000000
fastboot create-logical-partition system_ext_a 300000000
```

### Phase 4: Format Userdata & Reboot
```bash
fastboot -w
fastboot reboot
```

---

## Post-Install Notes
- **Fingerprint / UDFPS:** `mcDriverDaemon` is patched (`test-xxxx`) to load factory trustlets directly from `/data/vendor/mcRegistry`, and `init.lyriq.trustlet_restore.sh` also backs up trustlets to `/mnt/vendor/tzapp` on first boot. If fingerprint enrollment is ever needed after a fresh trustlet sync, simply reboot once after first boot.
- **USB Debugging:** Enabled by default on `userdebug` builds (`ro.adb.secure=0`, `init.lyriq.adb.rc`) for easy log capture.
