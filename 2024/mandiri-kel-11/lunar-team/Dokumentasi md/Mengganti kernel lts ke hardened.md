# Mengganti kernel linux-lts ke linux-hardened
### Konfigurasi Koneksi Jaringan
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
```
exit
```
### Memeriksa koneksi jaringan
```
ping 8.8.8.8
```
### Menginstall Kernel Linux Hardened
```
pacman -S linux-hardened linux-hardened-headers
```
### Mengedit file konfigurasi
```
nvim /etc/fstab
nvim /etc/mkinitcpio.d/linux
nvim /etc/mkinitcpio.d/linux-lts.preset
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

 hapus semua # lalu tambahkan:

```
- ALL_config="/etc/mkinitcpio.conf"
- ALL_kver="/boot/vmlinuz-linux-hardened"
- ALL_kerneldest="/boot/kernel/vmlimuz-linux-hardened"

- #default_image="/boot/initramfs-linux-hardened.img" UBAH MENJADI - default_uki="/boot/efi/Linux/arch-linux-hardened.efi"
```
### Generate initramfs
  ```
mkinitcpio -P
```
### Verifikasi Hasil Kompilasi Kernel
```
ls /boot/efi/Linux/arch-linux-hardened.efi
ls /boot/efi/Linux
```
### Keluar dari akses Superuser
```
exit
```
