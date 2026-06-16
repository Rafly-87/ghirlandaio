# Menginstall OS Server

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```
Cek driver wifi setiap laptop
```
station wlan0 get-network
```
Melihat jaringan yang tersedia
```
station wlan0 scaan
```
Memindaai jaringan yang ada
```
station wlan0 connect "(nama wifi)"
exit
```
## Memeriksa partisi
```
lsblk
```
Membagi partisi
```
cfdiks /dev/partisi [sda/nvme0n1p1]
```
minimal partisi
```
boot = 3G [EFI system}
root = 95G [Linux filesystem]
```
```
lsblk lagi
```
## Format LUKS
```
cryptsetup luksFormat /dev/[partisi root]
```
```
masukkan kata sandi
```
```
cryptsetup luksOpen /dev/[partisi root] [nama device]
```
## Setup LVM 
```
pvcreate /dev/mapper/[nama device]
```
```
vgcreate [nama grup] /dev/mapper/[nama device]
```
Membuat Logical Volume
```
lvcreate -L 8GB [nama grup] -n root
``````
```
lvcreate -L 8GB [nama grup] -n vars
```
```
lvcreate -L 1GB [nama grup] -n vlog
```
```
lvcreate -L 1GB [nama grup] -n vtmp
```
```
lvcreate -L 1GB [nama grup] -n vaud
```
```
lvcreate -L 5GB [nama grup] -n home
```
```
lvcreate -L 5GB [nama grup] -n podman
```
## Formating
```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
```
```
mkfs.ext4 /dev/[nama grup]/root/
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mkfs.ext4 /dev[nama grup]/home
```
```
mkfs.ext4 /dev[nama grup]/podman
```
## Periksa
```
lsblk
```
## Mounting
```
mount /dev/proc/root /mnt
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nosuid,relatime /dev/nama grup]/home /mnt/home
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```
##Periksa 
```
lsblk
```
##Install Package
