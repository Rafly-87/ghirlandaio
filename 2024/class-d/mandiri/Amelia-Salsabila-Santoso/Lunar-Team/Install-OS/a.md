# Dokumentasi Installasi OS Admin
## Nama: Amelia Salsabila Santoso
## NIM: 12402051030089
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.


## 1. Cek Layout Disk Awal

Sebelum memulai, cek kondisi disk dan partisi yang sudah ada (termasuk LUKS container yang sudah dibuka sebelumnya).

```bash
lsblk
```

**Output:**
```
NAME      MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
loop0       7:0    0 979.8M  1 loop  /run/archiso/airootfs
sda         8:0    0 931.5G  0 disk
├─sda1      8:1    0   100M  0 part
├─sda2      8:2    0    16M  0 part
├─sda3      8:3    0 225.7G  0 part
├─sda4      8:4    0 392.7G  0 part
├─sda5      8:5    0   530M  0 part
sda6 dan sda7 belum ada di sini — akan dibuat saat partisi baru ditambahkan
```

> **Catatan:** Pada titik ini LUKS container (`admin`) dan Volume Group `team` sudah ada dan sudah dibuka dari sesi sebelumnya. Rekaman ini melanjutkan dari sesi tersebut, langsung ke pembuatan LV baru.

---

## 2. Membuat Logical Volume Tambahan

VG `team` sudah ada. Di sini dibuat sisa LV yang belum ada: `vars`, `vtmp`, `vlog`, `vaud`, `home`.

```bash
lvcreate -L 15G team -n vars
lvcreate -L 5G team -n vtmp
lvcreate -L 5G team -n vlog
lvcreate -L 5G team -n vaud
lvcreate -L 60G team -n home
```

**Output masing-masing:**
```
Logical volume "vars" created.
Logical volume "vtmp" created.
Logical volume "vlog" created.
Logical volume "vaud" created.
Logical volume "home" created.
```

Verifikasi Volume Group setelah pembuatan:

```bash
vgs
```

```
VG   #PV #LV #SN Attr    VSize   VFree
team   1   7   0 wz--n-- 307.48g 167.48g
```

> Total 7 LV di VG `team`: `root` (25G), `vars` (15G), `vtmp` (5G), `vlog` (5G), `vaud` (5G), `home` (60G), `podman` (25G).

---

## 3. Format Filesystem pada Setiap LV

Semua LV diformat dengan ext4, block size 4096 byte.

### Format `/dev/team/root` (25G)

```bash
mkfs.ext4 -b 4096 /dev/team/root
```

```
Creating filesystem with 6553600 4k blocks and 1638400 inodes
Filesystem UUID: 9bf2048a-35cb-4a4d-a78b-de7b503e92bf
...
Writing superblocks and filesystem accounting information: done
```

### Format `/dev/team/vars` (15G)

```bash
mkfs.ext4 -b 4096 /dev/team/vars
```

```
Creating filesystem with 3932160 4k blocks and 983040 inodes
Filesystem UUID: 13458038-2182-4bb3-aa8e-7ecc999dc492
...
Writing superblocks and filesystem accounting information: done
```

### Format `/dev/team/vtmp` (5G)

```bash
mkfs.ext4 -b 4096 /dev/team/vtmp
```

```
Filesystem UUID: ff4f3389-cad1-42c3-80c6-06e13eef8a49
```

### Format `/dev/team/vlog` (5G)

```bash
mkfs.ext4 -b 4096 /dev/team/vlog
```

```
Filesystem UUID: a6a15af6-1484-49be-b582-6a8fb74613c4
```

### Format `/dev/team/vaud` (5G)

```bash
mkfs.ext4 -b 4096 /dev/team/vaud
```

```
Filesystem UUID: babaf93b-ae98-4b94-9b34-65315f7f4602
```

### Format `/dev/team/home` (60G)

```bash
mkfs.ext4 -b 4096 /dev/team/home
```

```
Creating filesystem with 15728640 4k blocks and 3932160 inodes
Filesystem UUID: 9c41734f-696c-4975-af08-8f45eacd97b8
```

### Format `/dev/team/podman` (25G)

```bash
mkfs.ext4 -b 4096 /dev/team/podman
```

```
Creating filesystem with 6553600 4k blocks and 1638400 inodes
Filesystem UUID: ab87843a-7c28-445a-918b-5a03d2ec16af
```

---

## 4. Format Partisi Boot (FAT32)

Partisi `sda6` (5G) diformat sebagai FAT32 untuk digunakan sebagai ESP (EFI System Partition).

> **Catatan:** Ada typo `mksf.vfat` — zsh secara otomatis mengoreksinya ke `mkfs.vfat`.

```bash
mkfs.vfat -F32 -n BOOT /dev/sda6
```

```
mkfs.fat 4.2 (2021-01-31)
```

---

## 5. Mount Partisi

Mount semua partisi ke `/mnt` dengan opsi keamanan yang sesuai.

### Mount root terlebih dahulu

```bash
mount /dev/team/root /mnt
```

### Mount partisi lainnya dengan opsi mount yang aman

```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/team/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,relatime /dev/team/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/team/podman /mnt/var/lib/containers
```

| Mount Point | LV | Opsi Keamanan |
|---|---|---|
| `/mnt` | `team/root` | `rw,nodev,nosuid,relatime` |
| `/mnt/var` | `team/vars` | `rw,nodev,nosuid,relatime` |
| `/mnt/var/tmp` | `team/vtmp` | `rw,nodev,nosuid,noexec,relatime` |
| `/mnt/var/log` | `team/vlog` | `rw,nodev,nosuid,noexec,relatime` |
| `/mnt/var/log/audit` | `team/vaud` | `rw,nodev,nosuid,noexec,relatime` |
| `/mnt/home` | `team/home` | `rw,nodev,nosuid,relatime` |
| `/mnt/var/lib/containers` | `team/podman` | `rw,nodev,nosuid,noexec,relatime` |

> **Penjelasan opsi:**
> - `nodev` — mencegah device file di partisi ini diinterpretasikan sebagai device
> - `nosuid` — mencegah bit SUID/SGID pada executable
> - `noexec` — mencegah eksekusi binary langsung (cocok untuk `/var/tmp`, `/var/log`)
> - `relatime` — update atime hanya jika lebih lama dari mtime (lebih efisien dari `atime`)

---

## 6. pacstrap — Instalasi Paket Dasar (Percobaan Pertama, Gagal)

Percobaan pertama pacstrap **gagal** karena laptop belum terhubung ke internet — semua mirror tidak bisa diakses.

```bash
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose
```

```
==> Creating install root at /mnt
==> Installing packages to /mnt
:: Synchronizing package databases...
 core.db failed to download
 extra.db failed to download
error: failed retrieving file 'extra.db' from fastly.mirror.pkgbuild.com : Could not resolve host: fastly.mirror.pkgbuild.com
...
==> ERROR: Failed to install packages to new root
```

> **Penyebab:** WiFi belum aktif. Semua mirror gagal dengan `Could not resolve host` karena tidak ada koneksi jaringan.

---

## 7. Koneksi WiFi via iwd

### Cek perangkat WiFi

```bash
iwctl
```

```
[iwd]# device list
  Name    Address               Powered   Adapter   Mode
  wlan0   b0:fc:36:58:b5:a5     off       phy0      station
```

WiFi dalam kondisi `off`. Keluar dari iwd dan aktifkan dengan `rfkill`:

```bash
rfkill unblock all
```

### Scan dan koneksi ke jaringan

```bash
iwctl
```

```
[iwd]# station wlan0 get-networks
[iwd]# station wlan0 connect "Anak gembala."
Type the network passphrase for Anak gembala. psk.
Passphrase: ***************
[iwd]# exit
```

> **Catatan:** Ada dua typo (`connetc` dan `connet`) sebelum akhirnya berhasil mengetik `connect` dengan benar. iwd memunculkan error `Invalid command` untuk keduanya.

### Verifikasi koneksi

```bash
ping 8.8.8.8
```

```
64 bytes from 8.8.8.8: icmp_seq=1 ttl=111 time=157 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=111 time=74.7 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=111 time=57.9 ms
```

Koneksi berhasil.

---

## 8. pacstrap — Instalasi Ulang (Berhasil)

### Percobaan kedua — sebagian berhasil, sebagian timeout

```bash
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose
```

Download 770 MiB berhasil sebagian besar, namun gagal di akhir karena beberapa paket timeout dari mirror yang lambat:

```
error: failed retrieving file 'linux-firmware-mediatek-...' from fastly.mirror.pkgbuild.com : Operation too slow.
==> ERROR: Failed to install packages to new root
```

### Percobaan ketiga — berhasil penuh

Jalankan pacstrap yang sama lagi. Paket yang sudah ter-cache tidak perlu didownload ulang, hanya sisa paket yang belum lengkap:

```bash
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose
```

```
Total Download Size:     81.84 MiB
Total Installed Size:  1602.43 MiB

:: Proceed with installation? [Y/n]
```

Instalasi 213 paket berhasil, termasuk:

| Paket | Versi | Keterangan |
|---|---|---|
| `linux-lts` | 6.18.35-1 | Kernel LTS |
| `linux-lts-headers` | 6.18.35-1 | Header kernel |
| `linux-firmware` | 20260519-1 | Firmware hardware |
| `amd-ucode` | 20260519-1 | Microcode AMD |
| `lvm2` | 2.03.41-1 | LVM tools |
| `cryptsetup` | 2.8.6-1 | Untuk LUKS |
| `mkinitcpio` | 41-4 | Generator initramfs |
| `neovim` | 0.12.3-1 | Text editor |
| `iwd` | 3.12-1 | WiFi daemon |
| `firewalld` | 2.4.2-1 | Firewall |
| `podman` | 5.8.3-1 | Container runtime |
| `podman-compose` | 1.6.0-1 | Docker Compose kompatibel |
| `sudo` | 1.9.17.p2-6 | Privilege escalation |

---

## 9. Konfigurasi di dalam chroot

Masuk ke sistem yang baru diinstall:

```bash
arch-chroot /mnt
```

### Edit locale

Di dalam chroot, buka `/etc/locale.gen` dengan nvim dan uncomment baris `en_US.UTF-8 UTF-8`. Kemudian set locale environment:

```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

### Edit mkinitcpio preset untuk UKI

Edit `/etc/mkinitcpio.d/linux-lts.preset` untuk menggunakan Unified Kernel Image (UKI) sebagai output:

**Isi preset setelah diedit:**
```ini
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"

PRESETS=('default')

default_uki="/boot/efi/linux/arch-linux-lts.efi"
```

> **Penjelasan:**
> - `default_uki` — output berupa file `.efi` (Unified Kernel Image), langsung bisa diload oleh UEFI firmware tanpa bootloader terpisah seperti GRUB.
> - `ALL_kver` — path kernel yang akan dipaket ke dalam UKI.

### Buat file vconsole

```bash
touch /etc/vconsole.conf
```

### Coba install bootloader (gagal karena /boot belum di-mount)

```bash
bootctl --path=/boot install
```

```
Running in a chroot, enabling --graceful.
File system "/boot" is not a FAT EFI System Partition (ESP) file system.
```

> **Penyebab:** Partisi `sda6` (FAT32/ESP) belum di-mount ke `/boot`. Perlu keluar dari chroot dulu.

```bash
exit
```

---

## 10. Mount Partisi Boot dan Konfigurasi Ulang mkinitcpio Preset

Setelah keluar dari chroot, verifikasi mount point dan mount partisi ESP.

```bash
lsblk
```

Konfirmasi semua LV sudah ter-mount di `/mnt/*`. Kemudian mount ESP:

```bash
mount --mkdir /dev/sda6 /mnt/boot
```

Verifikasi:

```bash
lsblk
```

```
sda6    8:6    0     5G  0 part  /mnt/boot
```

### Masuk chroot kembali dan perbaiki preset

```bash
arch-chroot /mnt
```

Edit kembali `/etc/mkinitcpio.d/linux-lts.preset` — path UKI disesuaikan agar sesuai dengan struktur boot yang benar:

```ini
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"

PRESETS=('default')

default_uki="/boot/efi/linux/arch-linux-lts.efi"
```

### Coba generate mkinitcpio (masih gagal)

```bash
mkinitcpio -P
```

```
==> ERROR: Invalid option -k -- '/boot/kernel/vmlinuz-linux-lts' must be readable
```

> **Penyebab:** Kernel belum terinstall dengan benar ke path tersebut. Perlu reinstall paket kernel dari dalam chroot.

---

## 11. Inisialisasi Pacman Keyring di dalam chroot

Percobaan reinstall kernel gagal karena keyring belum diinisialisasi di sistem baru:

```bash
pacman -S linux-lts linux-lts-headers
```

```
error: keyring is not writable
error: required key missing from keyring
```

### Inisialisasi keyring

```bash
mount -o remount,rw /
pacman-key --init
```

```
==> Generating pacman master key. This may take some time.
gpg: Generating pacman keyring master key...
gpg: Done
==> Updating trust database...
```

### Populate keyring dengan kunci Arch Linux

```bash
pacman-key --populate archlinux
```

```
==> Appending keys from archlinux.gpg...
==> Locally signed 5 keys.
==> Importing owner trust values...
==> Disabling revoked keys in keyring...
  -> Disabled 38 keys.
==> Updating trust database...
gpg: next trustdb check due at 2026-10-21
```

### Update archlinux-keyring

```bash
pacman -Sy archlinux-keyring
```

```
(1/1) reinstalling archlinux-keyring
==> Appending keys from archlinux.gpg...
==> Updating trust database...
```

---

## 12. Reinstall Kernel dan Generate Initramfs + UKI

Setelah keyring berhasil, reinstall kernel:

```bash
pacman -S linux-lts linux-lts-headers
```

```
:: Proceed with installation? [Y/n] y
(1/2) reinstalling linux-lts
(2/2) reinstalling linux-lts-headers
:: Running post-transaction hooks...
(1/3) Arming ConditionNeedsUpdate...
(2/3) Updating module dependencies...
(3/3) Updating linux initcpios...
```

Pacman otomatis menjalankan mkinitcpio sebagai post-transaction hook:

```
==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'default'
==> Using configuration file: '/etc/mkinitcpio.conf'
  -> -k /boot/kernel/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -U /boot/efi/linux/arch-linux-lts.efi
==> Starting build: '6.18.35-1-lts'
  -> Running build hook: [base]
  -> Running build hook: [systemd]
  -> Running build hook: [autodetect]
  -> Running build hook: [microcode]
  -> Running build hook: [modconf]
  -> Running build hook: [kms]
  -> Running build hook: [keyboard]
  -> Running build hook: [sd-vconsole]
==> WARNING: sd-vconsole: "/etc/vconsole.conf" not found, will use default values
  -> Running build hook: [sd-encrypt]
==> WARNING: Possibly missing firmware for module: 'qat_6xxx'
  -> Running build hook: [lvm2]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [fsck]
==> Generating module dependencies
==> Creating zstd-compressed initcpio image
  -> Early uncompressed CPIO image generation successful
==> Initcpio image generation successful
==> Creating unified kernel image: '/boot/efi/linux/arch-linux-lts.efi'
==> Unified kernel image generation successful
```

### Verifikasi manual mkinitcpio

```bash
mkinitcpio -P
```

```
==> Unified kernel image generation successful
```

> **Penjelasan hooks:**
> - `systemd` — init system berbasis systemd
> - `sd-encrypt` — decrypt LUKS container saat boot
> - `lvm2` — aktivasi LVM VG setelah LUKS dibuka
> - `microcode` — load AMD microcode
> - `autodetect` — detect modul yang dibutuhkan saja (ukuran initramfs lebih kecil)

---

## 13. Instalasi Desktop Environment (XFCE4, SDDM, PipeWire, NetworkManager)

```bash
pacman -S sddm xfce4 pipewire pipewire-pulse pipewire-jack pipewire-alsa network-manager-applet networkmanager firefox
```

Saat prompt pilih anggota grup `xfce4` (14 paket) → tekan Enter untuk memilih semua.

Saat prompt pilih `ttf-font` → tekan Enter untuk memilih default (`gnu-free-fonts`).

Total download: **326.55 MiB**, total install: **1400.46 MiB** — mencakup 297 paket termasuk:

| Paket | Keterangan |
|---|---|
| `sddm` | Display manager (login screen) |
| `xfce4-*` | Semua komponen XFCE4 (session, panel, terminal, dll) |
| `pipewire` + `pipewire-pulse` + `pipewire-jack` + `pipewire-alsa` | Audio stack modern |
| `networkmanager` + `network-manager-applet` | Manajemen jaringan + systray |
| `firefox` | Browser web |
| `xorg-server`, `libxfce4ui`, `xfwm4` | Xorg + window manager XFCE |
| `mesa`, `libglvnd` | OpenGL/GPU support |
| `wireplumber` | Session manager untuk PipeWire |

---
