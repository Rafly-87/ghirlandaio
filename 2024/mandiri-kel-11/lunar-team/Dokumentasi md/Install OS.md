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
### Copy Konfigurasi Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
### Menambah tmpfs
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
### Melihat file fstab
```
cat /mnt/etc/fstab
```
### Masuk ke sistem baru
```
arch-chroot /mnt
```
### Mengatur Timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
### Sinkronisasi Jam
```
hwclock --systohc
```
### Mengatur locale
```
nvim /etc/locale.gen
```
```
en_US.UTF-8 UTF-8
```
```
en_US ISO-8859-1
```
```
locale.gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
LANG=en_US.UTF-8
```
```
LC_ALL=en_US.UTF-8
```
### Membuat User
```
useradd -m Lunar
```
```
passwd Lunar
```
### Memberikan Hak Sudo
```
echo "Lunar ALL=(ALL:ALL) ALL" > /etc/sudoers.d/Lunar
```
### Konfigurasi Kernel Command Line
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p8)=aw root=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
### Konfigurasi mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
```
tambahkan kalimat sesudah "block" "sd-encrypt lvm2"
##   NOTE: If you have /usr on a separate partition, you MUST include the
#    usr and fsck hooks.
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block [sd-encrypt lvm2] filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
ALL_config="/etc/mkinitcpio.conf" 
ALL_kver="/boot/vmlinuz-linux-lts"   
ALL_kerneldest="/boot/vmlinuz-linux-lt

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/boot/efi/Linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"
```
### Menginstall Bootctl
```
bootctl --path=/boot install
```
### Generate Initramfs
```
mkinitcpio -P
```
### Install Network Manager
```
pacman -S networkmanager
```
### Mengaktifkan Network
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```
```
systemctl enable firewalld
```
### Booting
```
exit
```
```
umount -R /mnt
```
### Mematikan asciinema
```
ctrl+d
```
```
asciinema rec [nama_file].cast
```
### Reboot
```
Reboot
```
