# TUTORIAL INSTALASI BASE ARCH LINUX
## 1. Download ISO Arch Linux
download iso arch linux menggunakan link berikut archlinux-2026.05.01-x86_64.iso  
## 2. Membuat Bootable USB

## 3. Boot ke flashdisk installer
colokkan flashdisk yang sudah berisi file bootable arch linux

restart laptop

tekan FN + F12


## 4. Sambungkan ke internet
gunakan ```iwctl```

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 14 18" src="https://github.com/user-attachments/assets/91cdfef6-e15d-43a3-9300-e8bffa84a0dc" />


```station wlan0 scan```

```station wlan0 get-networks```

```station wlan0 connect namawifi```
masukkan password wifi jika diminta

## 5. Cek struktur disk dan partisi
```lsblk```

untuk melihat nama disk, ukuran partisi, struktur storage

contoh disk ```/dev/nvme0n1```

## 6. Membuat Partisi
masuk ke cfdisk
```cfdisk /dev/nvme0n1```

## 7. Membagi Partisi
Pilih Free Space

<img width="1600" height="901" alt="WhatsApp Image 2026-05-20 at 00 54 32" src="https://github.com/user-attachments/assets/edf9a39c-371c-4745-8905-bef470d9e60b" />

Lalu bagi partisi sesuai pengkondisian penyimpanan yang sudah dibagi, kelompok kami memakai

1 GB untuk boot, type EFI system

4 GB untuk swap, type Linux swap

14 GB untuk root, type Linux filesystem

## 8. Membuat filesystem
```lsblk```

contoh hasilnya 

<img width="1600" height="900" alt="WhatsApp Image 2026-05-20 at 01 20 41" src="https://github.com/user-attachments/assets/4cedffd5-60b1-44dd-86d3-a1760ae41922" />


/dev/nvme0n1p5 untuk boot

/dev/nvme0n1p6 untuk swap

/dev/nvme0n1p7 untuk root

format root ke ext4

```mkfs.ext4 /dev/nvme0n1p7```

format partisi tambahan

```mkfs.ext4 /dev/nvme0n1p6```

aktifkan swap (swap digunakan sebagai cadangan ketika RAM penuh)

```mkswap /dev/nvme0n1p6```

```swapon /dev/nvme0n1p6```

format boot ```mkfs.fat -F 32 /dev/nvme0n1p5```

## 9. Mount Partisi

Partisi harus dipasang sebelum sistem diinstall

mount root ```mount /dev/nvme0n1p7 /mnt```

membuat folder boot ```mkdir -p /mnt/boot```

mount boot ```mount /dev/nvme0n1p5 /mnt/boot```

## 10. Install base system
install package dasar arch linux ```pacstrap -K /mnt base-devel linux linux-firmwarenvim amd-ucode (jika intel maka menjadi intel-ucode)```

## 11. Generate fstab 
fstab digunakan untuk menyimpan konfigurasi mount partisi 

```genfstab -U /mnt >> /mnt/etc/fstab```

## 12. Masuk ke  sistem arch
```arch-chroot /mnt```

## 13. Mengatur timezone
contoh mengatur sesuai timezone Indonesia

```In -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```

lalu sinkronkan dengan hardware clock ```hwclock --systoch```



