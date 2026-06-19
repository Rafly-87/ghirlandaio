# Dokumentasi penginstallan penugasan kelompok 9
## Nama: Nuraini As Syifa
## NIM: 12402051030088
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.
### Konfigurasi Koneksi Jaringan:
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect nama_wifi
```
### Pengujian koneksi jaringan
```
ping 1.1.1.1
```
```
ping 8.8.8.8
```
### Melakukan rekaman dengan asciinema
```
asciinema rec [nama_file].cast
```
### Melihat Partisi
```
lsblk
```
### Membagi partisi dengan EFI System dan Linux Filesystem
```
cfdisk /dev/nvme0n1
```
### Mengenkripsi Partisi dengan LUKS
```
cryptsetup luksFormat /dev/nvme0n1p8
```
### Membuka Partisi Enkripsi
```
cryptsetup luksOpen /dev/nvme0n1p8 aw
```
### Membuat LVM
```
pvcreate /dev/mapper/aw
```
### Membuat volume group
```
vgcreate system /dev/mapper/aw
```
### Membuat LV
```
lvcreate -L 8GB system -n root
```
```
lvcreate -L 8GB system -n vars
```
```
lvcreate -L 1GB system -n vlog
```
```
lvcreate -L 1GB system -n vtmp
```
```
lvcreate -L 1GB system -n vaud
```
```
lvcreate -L 15GB system -n home
```
```
lvcreate -l100%FREE system -n podman
```
### Format Filesystem
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p6
```
```
mkfs.ext4 /dev/system/root
```
```
mkfs.ext4 /dev/system/vars
```
```
mkfs.ext4 /dev/system/vlog
```
```
mkfs.ext4 /dev/system/vtmp
```
```
mkfs.ext4 /dev/system/vaud
```
```
mkfs.ext4 /dev/system/home
```
```
mkfs.ext4 /dev/system/podman
```
### Mount
```
mount /dev/system/root /mnt
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/podman /mnt/var/lib/containers
```
### Pacstrap
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose
```
### Membuat fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Copy Konfigurasi Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
### Menambah tmpfs
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
