# INSTALASI BASE ARCH LINUX
Tugas ini disusun untuk memenuhi tugas mata kuliah Perpustakaan dan Arsip Digital


## Dosen Pengampu :
Al Muhdil Karim, S.IP, M.Hum.

## Disusun oleh Kelompok 5 :
1.	Nur Padilah (12402051030034)
2.	Vanesa Putri Zahra (12402051050108)
3.	Muhammad Suyuthi Alkautsar (12402051050115)

## Pendahuluan
**Latar Belakang**

Arch Linux merupakan distribusi Linux yang menekankan pada prinsip kesederhanaan, modernitas, pragmatisme, terpusat pada pengguna, serta keserbagunaan dengan tujuan memberikan kebebasan kepada pengguna untuk mengatur sistem sesuai kebutuhan. Arch Linux mengutamakan sistem yang sederhana tanpa tambahan atau modifikasi yang tidak diperlukan sehingga pengguna dapat mengatur dan menyesuaikan sistem sesuai kebutuhan. Selain itu, Arch Linux menggunakan sistem rilis bergulir (rolling release) agar dalam sekali pemasangan dan terus diperbarui.

Arch Linux dirancang untuk pengguna yang ingin memahami dan mengelola sistem operasi secara mandiri. Arch Linux menyediakan berkas konfigurasi berdasarkan pembuat atau pengurus asli perangkat lunak dengan tambahan perubahan yang spesifik pada sistem Arch Linux, seperti pengaturan path agar sistem dapat berjalan dengan baik dan tidak mengalami error. Konfigurasi tersebut juga tidak menambahkan fitur otomatis yang tidak diperlukan, seperti menyalakan layanan secara otomatis setelah paket terpasang, sehingga pengguna perlu mengatur dan menyalakan layanan sistem secara mandiri sesuai kebutuhan.
Dalam proses instalasi tersebut terdapat beberapa bagian penting, seperti partisi boot, swap, dan root yang memiliki fungsi berbeda dalam mendukung jalannya sistem operasi. Pemahaman mengenai fungsi partisi dan konfigurasi sistem dasar sangat diperlukan agar sistem dapat berjalan dengan baik dan stabil.

Namun, proses instalasi Arch Linux memerlukan pemahaman mengenai perintah terminal dan konfigurasi sistem Linux. Oleh karena itu, pembahasan mengenai instalasi Arch Linux penting untuk dipelajari agar pengguna dapat memahami langkah-langkah instalasi serta fungsi partisi boot, swap, dan root pada sistem Linux.


## Pembahasan

### 1. Persiapan sebelum instalasi
**Mengunduh File ISO** 

Langkah pertama yaitu mengunduh file ISO (International Organization for Standardization) Arch Linux. File ISO digunakan sebagai media instalasi yang nantinya dimasukkan ke flashdisk agar komputer dapat melakukan boot dan menjalankan instalasi Arch Linux.

File ISO merupakan file image yang berisi salinan sistem operasi dalam satu file, termasuk kernel Linux, installer, sistem boot, drive, dan berbagai paket yang diperlukan untuk menjalankan instalasi.

### 2. Instalasi
**2.1 Membuat Bootable Flashdisk**

Memindahkan file ISO ke flashdisk dilakukan menggunakan Terminal (Linux & macOS) atau aplikasi Rufus atau Balena Etcher untuk membuat flashdisk menjadi bootable sehingga dapat digunakan sebagai media instalasi sistem operasi.

Terdapat 3 opsi yang dapat digunakan untuk memindahkan file ISO ke flashdisk agar menjadi bootable, yaitu menggunakan terminal pada Linux dan macOS, menggunakan Rufus pada Windows, dan menggunakan Balena Etcher untuk semua sistem operasi.

**2.2 Langkah Booting**

Booting merupakan proses saat komputer mulai menjalankan sistem operasi. Booting dari flashdisk dilakukan agar komputer dapat membuka installer Arch Linux dan memulai proses instalasi sistem operasi. Langkah yang dilakukan yaitu menyambungkan USB bootable ke komputer yang akan diinstal, kemudian masuk ke menu UEFI atau Boot Menu dengan menekan tombol seperti F2, F12, Del, atau ESC saat komputer mulai menyala, lalu pilih flashdisk sebagai perangkat boot utama.

Setelah berhasil boot ke live environment Arch Linux, langkah selanjutnya yaitu memeriksa mode boot yang digunakan sistem, apakah UEFI atau BIOS, dengan menjalankan perintah 

```cat /sys/firmware/efi/fw_platform_size```

hasil perintah tersebut menunjukkan tiga kemungkinan:
- Jika mengembalikan nilai 64, berarti sistem berjalan dalam mode UEFI x64 64-bit.
- Jika mengembalikan nilai 32, berarti sistem berjalan dalam mode UEFI IA32 32-bit dan akan membatasi pilihan boot loader.
- Jika mengembalikan pesan No such file or directory, berarti sistem berjalan dalam mode BIOS atau CSM.

Informasi ini penting karena langkah instalasi bootloader pada tahap akhir akan berbeda tergantung mode yang digunakan.

**Perbedaan UEFI atau BIOS**


**2.3 Perintah Awal di Live Environment**

Setelah berhasil masuk ke menu UEFI atau Boot Menu, langkah selanjutnya adalah memilih flashdisk bootable yang berisi file instalasi Arch Linux sebagai perangkat boot utama. Setelah dipilih, komputer akan memulai booting dari flashdisk dan menampilkan halaman awal Arch Linux. Pada tahap ini pengguna memilih opsi “Arch Linux install medium” untuk menjalankan live environment Arch Linux. Jika proses berhasil, sistem akan masuk ke tampilan terminal dengan prompt seperti ```root@archiso``` yang menandakan bahwa live environment Arch Linux sudah aktif dan siap digunakan untuk proses instalasi, seperti mengecek koneksi internet, membuat partisi disk, memformat partisi, dan menginstal sistem operasi ke penyimpanan komputer.

**2.4 Menghubungkan Internet**

Koneksi internet wajib tersedia sebelum memulai instalasi karena Arch Linux mengunduh semua paket sistem langsung dari mirror online saat instalasi. Tanpa internet, proses instalasi tidak dapat dilakukan.

Untuk terhubung ke internet, pastikan interface jaringan sudah aktif dengan perintah ```ip link```. Jika menggunakan Ethernet, cukup colokkan kabelnya. Jika menggunakan Wi-Fi, gunakan iwctl untuk terhubung ke jaringan nirkabel. Setelah terhubung, verifikasi koneksi dengan perintah ```ping ping.archlinux.org```

iwcl merupakan

**2.5 Singkronisasi Waktu**

Waktu sistem yang akurat penting dalam proses instalasi untuk mencegah kegagalan verifikasi paket dan kesalahan sertifikat TLS (Transport Layer Security). Layanan systemd-timesyncd sudah aktif secara default dan akan otomatis menyinkronkan waktu begitu internet terhubung. Untuk memastikan waktu sudah tersinkronisasi, jalankan perintah timedatectl.

Layanan systemd-timesyncd merupakan

**2.6 Partisi Diks**

Saat sistem live berjalan, disk yang terpasang di komputer akan otomatis terdeteksi dan diberi nama perangkat blok seperti ```/dev/sda```, ```/dev/nvme0n1```, atau ```/dev/mmcblk0```. Untuk melihat daftar disk yang tersedia dapat menggunakan perintah ```lsblk``` atau ```fdisk -l```.

Terdapat dua partisi yang wajib dibuat yaitu partisi untuk direktori root ```/``` dan partisi EFI untuk pengguna UEFI. Untuk membuat partisi gunakan fdisk:

```fdisk /dev/the_disk_to_be_partitioned```

contoh :

```fdisk /dev/nvme0n1```


**Struktur Partisi**
**UEFI**


**BIOS Legacy**



**Format Partisi**

setelah partisi selesai dibuat, setiap partisi harus diformat dengan sistem file yang sesuia agar dapat dogunakan. untuk partisi root, format menggunakan ext4:

```mkfs.ext4 /dev/nvme0n1```

Untuk partisi swap (digunakan sebagai memori candangan ketika RAM penuh), menggunakan perintah:

```mkswap/ dev/nvme0n1p2```

Untuk mengaktifkan swap, gunakan perintah:

```swapon /dev/swap_partition```

Untuk partisi EFI (harus menggunakan FAT32), format ke FAT32 menggunakan perintah:

```mkfs.fat -f 32 /dev/efi_system_partition```

Perlu diperhatikan, format partisi EFI hanya dilakukan jika partisi tersebut baru saja dibuat. Jika sebelumnya sudah ada partisi EFI di disk dari sistem operasi lain, jangan diformat ulang karena dapat merusak bootloader sistem operasi tersebut.

**Mount Filesystem**
setelah partisi diformat, langkah selanjutnya memasang (mount) partisi ke dalam sistem agar installer dapat mengaksesnya. pertama untuk partisi root ke ```/mnt```:

```mount /dev/root_partition /mnt```

Untuk sistem UEFI, pasang partisi ke ```/mnt/boot```:

```mount --mkdir /dev/efi_system_partition /mnt/boot```


### 3. Konfigurasi sistem
**3.1 Instalasi Sistem Dasar**

**3.2 Membuat Fstab**

**3.3 Masuk ke Sistem Baru (Chroot)**

**3.4 Mengatur Timezone**

**3.5 Localization**

**3.6 Hostname**

**3.7 Generate Initramfs**

**3.8 Password Root**

**3.9 Install Bootloader**



### 4. Reboot

## Praktik

1. Persiapan Sebelum Instalasi
a. Dapaktkan file ISO Arch Linux berikut link "archlinux.org/download/" pastikan sudah ada signature
b. partisi disk menggunakan mini partition wizard
c. siapkan flashdisk minimal 8gb

2. Masukan ISO ke flashdisk
3. boot ke live environment
4. pastikan masuk ke boot installer UEFI bukan BIOS
5. Konek ke Internet
6. Sinkronisasi waktu
7. Partisi Disk
8. Mount filesystem
9. Instalasi sistem dasar
10. Membuat Fstab
11. masuk ke sistem boot
12. mengatur timezone
13. localization
14. generate Initframs
15. password
16. install bootloader GRUB2
17. Reboot


## Penutup

## Daftar Pustaka

## Dokumentasi

