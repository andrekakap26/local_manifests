# local_manifests for itel RS4 (S666LN) — LineageOS 23.2

Manifest lokal untuk membangun LineageOS 23.2 untuk itel RS4 (S666LN, MediaTek Helio G99 / MT6789).

## Persyaratan

- Linux (Ubuntu 22.04+ direkomendasikan; Android tidak dibangun di Windows)
- `repo` tool
- Disk kosong >= 250 GB, RAM >= 32 GB
- `git-lfs` untuk beberapa `project` Enterprise/LFS LineageOS (`apt install git-lfs`)

## Setup

```bash
mkdir -p ~/los-s666ln && cd ~/los-s666ln

repo init -u https://github.com/LineageOS/android.git -b lineage-23.2 --git-lfs
git clone https://github.com/andrekakap26/local_manifests.git .repo/local_manifests

repo sync -c -j$(nproc) --force-sync --no-clone-bundle
```

## Build

```bash
source build/envsetup.sh
lunch lineage_S666LN-userdebug
mka bacon
```

Hasil: `out/target/product/S666LN/lineage-*.zip`

## Instalasi

Flash dengan recovery TWRP/OrangeFox untuk S666LN, atau fasilitas sistem:

```
adb reboot bootloader   # atau masuk ke recovery
# sideload: adb sideload lineage_*.zip
```

Catatan: perangkat A/B virtual dengan dynamic partitions, aman `fastboot flashall` bila memakai keys konsisten dengan ROM stok — untuk build `userdebug` lokal cukup sideload via recovery.

## Repo yang dipakai

| Path | Sumber | Revision |
| ---- | ------ | -------- |
| `device/itel/S666LN` | andrekakap26/device_itel_S666LN | lineage-23.2 |
| `device/itel/S666LN-kernel` | andrekakap26/device_itel_S666LN-kernel | sixteen |
| `device/millennium/common-kernel` | MillenniumOSS/android_device_millennium_common-kernel | sixteen-qpr2 |
| `vendor/itel/S666LN` | andrekakap26/vendor_itel_S666LN | lineage-23.2 |
| `hardware/mediatek` | MillenniumOSS/android_hardware_mediatek | sixteen |
| `hardware/millennium` | MillenniumOSS/android_hardware_millennium | sixteen |
| `device/mediatek/sepolicy_vndr` | MillenniumOSS/android_device_mediatek_sepolicy_vndr | sixteen-qpr2-rebase |
| `vendor/mediatek/ims` | MillenniumOSS/android_vendor_mediatek_ims | sixteen-oem |
| `vendor/JamesDSP` | MillenniumOSS/android_vendor_JamesDSP | sixteen |
| `packages/apps/Aperture` | MillenniumOSS/android_packages_apps_Aperture | lineage-23.2 |

## Catatan tanda tangan (opsional)

Build `userdebug` lokal tidak memerlukan kunci. Untuk build `user` bertanda tangan resmi, ganti/munculkan `vendor/lineage-priv/keys` di manifest (repo ini tidak menyertakan kunci; session sebelumnya memakai remote yang sudah dihapus).