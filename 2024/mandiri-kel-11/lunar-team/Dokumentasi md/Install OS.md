# Dokumentasi Install OS
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
### Bagi Partisi
```
lsblk
```
```
pvcreate /dev/[nama partisi]
```
```
vgcreate [nama grup] /dev/[nama partisi]
```
```
lvcreate -L [size G|M] [nama grup] -n root
```
```
lvcreate -L [size G|M] [nama grup] -n vars
```
```
lvcreate -L [size G|M] [nama grup] -n vlog
```
```
lvcreate -L [size G|M] [nama grup] -n vaud
```
```
lvcreate -L [size G|M] [nama grup] -n vtmp
```
```
lvcreate -L [size G|M] [nama grup] -n home
```
```
lvcreate -l 50%FREE [nama grup] -n podman
```
```
lvcreate -l 50%FREE [nama grup] -n [nama home untuk internal]
```
### Format Partisi
```
mkfs.ext4 /dev/[nama grup]/root
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mkfs.ext4 /dev/[nama grup]/home
```
```
mkfs.ext4 /dev/[nama grup]/podman
```
```
mkfs.vfat -F32 /dev/[nama partisi boot]
```
### Mounting
```
mount /dev/[nama grup]/root /mnt
```
```
mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```
### Setup LUKS untuk home internal
```
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
```
### Install Package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware \ mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep \ podman podman-compose asciinema
```
### Membuat fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Menambah ftmps
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```
arch-chroot /mnt
```
```
