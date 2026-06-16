## Instalasi OS

### nyalakan asciinema

### Connect Wifi

```iwctl```

```device list```

**untuk cek driver wifi setiap laptop**

```station (driver wifi) get-network```

**untuk melihat jaringan yang tersedia**

```station (driver wifi) scan```

**untuk memindai jaringan yang ada**

```station {device wifi} connect "{nama wifi}"```

```exit```

**untuk menghubungkan ke jaringan yang sudah ditentukan**

## Periksa Jaringan 

```ping 1.1.1.1```

## Check Partisi
### jika ingin melihat partisi beserta type nya
```lsblk -o name,fstype,size ```
### jika ingin melihat partisinya saja
```lsblk```

## Bagi Partisi
```cfdisk /dev/[partisi]``` 

**untuk membentuk layout yg mau di install**

## Minimal Partisi
### menyesuaikan dengan penyimpanan yang ada
```
boot = 1G [EFI system]
root = 49G [linux filesystem/]
```

**jika salah satu partisi PENTING terhapus, langsung QUIT saja jangan di WRITE**

```lsblk``` (lagi)

## Cryptsetup dan Format LUKS

```cryptsetup luksFormat /dev/partisi_root```

**lalu masukin password**

**Jika ada tulisan 'in use', hapus dulu PARTISI grupnya : ```vgsremove /dev/nama grup/nama partisi```nya. setelah dihapus, ```cryptsetup luksFormat /dev/partisi_root``` lagi**

## Buka partisi luks

```cryptsetup luksOpen /dev/(partisi_root) (nama_device)```

## Buat physical volume

```pvcreate /dev/mapper/(nama_device)```

## Buat volume grub

```vgcreate nama_grup /dev/mapper/(nama_device)```

## Buat Logical Volume 

```lvcreate -L 8G (nama_grup) -n root```

```lvcreate -L 8G (nama_grup) -n vars```

```lvcreate -L 1G (nama_grup) -n vlog```

```lvcreate -L 1G (nama_grup) -n vtmp```

```lvcreate -L 1G (nama_grup) -n vaud```

```lvcreate -L 5G (nama_grup) -n home```

```lvcreate -L 5G (nama_grup) -n podman```

## Formating

```mkfs.vfat -F32 -n BOOT /dev/partisi_boot```

```mkfs.ext4 /dev/namagrup/root```

```mkfs.ext4 /dev/namagrup/vars```

```mkfs.ext4 /dev/namagrup/vtmp```

```mkfs.ext4 /dev/namagrup/vaud```

```mkfs.ext4 /dev/namagrup/vlog```

```mkfs.ext4 /dev/namagrup/home```

```mkfs.ext4 /dev/namagrup/podman```

## Mounting

```mount /dev/nama grup/root /mnt```

```mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot```

```mount --mkdir -o rw,nodev,nosuid,relatime /dev/namagrup/vars /mnt/var```

```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/namagrup/vtmp /mnt/var/tmp```

```mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vlog /mnt/var/log```

```mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vaud /mnt/var/log/audit```

```mount --mkdir -o rw,nosuid,relatime /dev/namagrup/home /mnt/home```

```mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/podman /mnt/var/lib/containers```

## Periksa kembali

```lsblk```

## Install Package 

### intel
```pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount```

### amd
```pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount```

## Fstab

```genfstab -U /mnt > /mnt/etc/fstab```

## Salin Setingan Jaringan

```cp /etc/systemd/network/* /mnt/etc/systemd/network```

## Formating tmpfs ke tmp
```echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab```

## Periksa Kembali

```cat /mnt/etc/fstab```

## Masuk ke Sistem

```arch-chroot /mnt```

## Localtime

```ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```

```hwclock --systohc```

## Locale

```nvim /etc/locale.gen```
**di pencarian nvim menggunakan `/`**

lalu ```uncommenting kedua en_US```

### generate bahasa yg di uncommenting 
```locale-gen```

```locale > /etc/locale.conf```

### config locale
```nvim /etc/locale.conf```

### config file locale 
isi ```lang=C``` menjadi ```lang=en_US.UTF-8```
dan isi ```ALL=en_US.UTF-8```

## Buat user, contoh kenari

```useradd -m kenari```

```passwd kenari```

```echo "kenari ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/kenari```

## Atur Parameter

```mkdir /etc/cmdline.d```

```touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}```

```echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi root)=nama device root=/dev/nama grup/root" > /etc/cmdline.d/01-boot.conf```

```echo "rw" > /etc/cmdline.d/02-misc.conf```

## Atur mkinitcpio

```nvim /etc/mkinitcpio.conf```

***cari di bagian Hooks pas ada tulisan block itu di spasi sekali lalu ketik "sd-encrypt" spasi lagi sekali lalu ketik "lvm2"***

## Install bootctl

```bootctl --path=/mnt/boot install ```

**jika bukan laptop Lenovo, Lenovo bootctl nya harus di luar dari arch-chroot**

```mkinitcpio -P```

## Aktifkan sebagai berikut

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 
```

## Booting

```
exit
umount -R /mnt
```

#### hentikan asciinema

```CTRL+D```

```asciineme upload nama_file.cast```

```reboot```


