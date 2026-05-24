# Instalasi Arch Linux dan KDE Plasma
## Persiapan 
- Pastikan _Device Encryption_ di laptop itu off
- Mengunduh ISO Arch Linux di situs https://archlinux.org/download/. Lalu scroll ke bawah hingga menemukan Indonesia dan klik citrahost.com
<img width="1600" height="1200" alt="arch indonesia" src="https://github.com/user-attachments/assets/ec33dbde-7d86-456f-af22-1b1b62102e49" />

- Klik archlinux-2026.05.01-x86_64.iso
<img width="1600" height="1200" alt="citrahost" src="https://github.com/user-attachments/assets/f1d02ce0-54ea-42fa-bb7a-4dc61d3ea50a" />

- Siapkan flashdisk dan unduh Rufus di situs https://rufus.ie/id. Lalu klik rufus-4.14.exe (disesuaikan laptop masing-masing)
<img width="1599" height="899" alt="rufus" src="https://github.com/user-attachments/assets/12351e75-a978-4cac-9462-2c8cea610225" />

- Buka CMD (windows + r). Lalu ketik diskpart
<img width="1200" height="1600" alt="diskpart" src="https://github.com/user-attachments/assets/1fb61d8a-57e0-4074-9e3c-7ed62ed61d68" />

- Kemudian ketik list disk. Muncul tampilannya dan cek indikator visual harddisknya itu GPT
<img width="1200" height="1600" alt="list disk" src="https://github.com/user-attachments/assets/f2e80b60-c6c5-4606-b19b-b11fa80d645d"/>
<img width="1600" height="1200" alt="gpt" src="https://github.com/user-attachments/assets/52edd743-5ac3-4111-9265-0fc46b62ffc7" />

## Proses Instalasi
- Sambungkan flashdisk ke Laptop lalu copypaste file ISO Arch Linux. Masuk ke file Rufus. Ubah pengaturan pada _Boot Selection_ pilih file Arch Linux yang tadi sudah dicopypaste. Ubah juga pada _Partition Scheme_ menjadi GPT. Lalu klik start
<img width="1200" height="1600" alt="drive properties" src="https://github.com/user-attachments/assets/3f2f6df6-020e-4bb7-93df-59e61287e399" />

- Masuk ke disk management, lalu klik kanan dan klik shrink volume. Kemudian atur sizenya sesuai keinginan masing-masing. Setelah itu, klik shrink
<img width="1600" height="1200" alt="disk mane" src="https://github.com/user-attachments/assets/aff8c83f-f9be-4922-bd49-ae0c34f541a8" />
<img width="1600" height="1200" alt="shrink" src="https://github.com/user-attachments/assets/4cffefd4-4cff-46d0-a848-7815276e300e" />

- Akses Bios dengan menekan tombol **esc** pada keyboard. Kemudian buka tab Security -> Secure Boot dan ubah statusnya menjadi **disable**. Lalu klik save & exit. Otomatis layar laptop akan mati.
<img width="1600" height="1200" alt="security" src="https://github.com/user-attachments/assets/a503beba-63db-47b7-834d-392fcedb6197" />
<img width="1600" height="1200" alt="save exit" src="https://github.com/user-attachments/assets/692dce9c-4c7c-4954-ad69-06e91dc2d8f7" />

- Setelah itu, masuk ke Boot Menu dan pilih yang USB. Otomatis masuk ke halaman awal installer Arch Linux. Dilanjutkan dengan mengetik **cat /sys/firmware/efi/fw_platform_size**. Kemudian akan muncul '64' (ini berarti tipe perangkat OS nya yaitu 64-bit)
<img width="1600" height="1200" alt="cat" src="https://github.com/user-attachments/assets/d90ec9f1-cbb1-4aea-a283-c8b71e2e2cbe" />

- Kemudian mengoneksikan internet dengan ketik **iwctl**. Dilanjutkan dengan mengecek perangkat wireless dengan mengetik **device list** (cek powerednya harus on)
<img width="1600" height="1200" alt="device list" src="https://github.com/user-attachments/assets/26ccddd9-5b22-44c9-85cb-adcbc9cbb170" />

- Lalu ketik **station wlan0 get-networks** untuk melihat jaringan wifi yang ada di lingkungan sekitar kita
<img width="1600" height="1200" alt="get net" src="https://github.com/user-attachments/assets/7b139706-e44c-4b86-afef-9719d30ee00f" />

- Untuk menscan jaringan ketik **station wlan0 scan**. Dilanjutkan dengan mengetik **station wlan0 connect "oppo a54"** dan masukan passwordnya
<img width="1600" height="1200" alt="sambungin" src="https://github.com/user-attachments/assets/fde04716-7bb7-421a-9f78-3a4eafb5fcb6" />

- Kemudian ketik **exit**. Setelah itu, ketik **ping google.com** untuk mengetes koneksi
<img width="1600" height="1200" alt="exit ping" src="https://github.com/user-attachments/assets/084dde8b-b34a-4088-a404-3d3fbb818149" />

- Sekarang cek partisi Linux dengan mengetik **lsblk -o name,fstype,size**
<img width="1200" height="1600" alt="fstype" src="https://github.com/user-attachments/assets/5cc531fa-f7f6-4675-a3fb-47dc6feab8a9" />
  
- Lalu ketik **cfdisk /dev/sdb**
<img width="1600" height="1200" alt="cfdisk" src="https://github.com/user-attachments/assets/0edc4732-89b5-4e69-b566-8a6ca46fcaa3" />

- Maka akan terbuka tampilan seperti ini. Lalu pilih **free space** kemudian pilih opsi **new**. Ubah partition size menjadi **1G**
<img width="1600" height="1200" alt="tampilan cfdisk" src="https://github.com/user-attachments/assets/8a3733d0-0731-45d3-8dcf-1cd2a690243f" />

- Kemudian pilih **/dev/sdb5**. Lalu pilih opsi **type**. Selanjutnya select partition type dengan memilih opsi **EFI System** (untuk boot)
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 37 25" src="https://github.com/user-attachments/assets/21dfbe19-9824-4f74-ae73-a589c2ca5cd0" />
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-24 at 11 37 26" src="https://github.com/user-attachments/assets/8cb35ec6-8930-4415-939c-f94924d703a6" />

- Selanjutnya lakukan hal yang sama. Pilih **free space** kemudian pilih opsi **new**. Ubah partition size menjadi **4G**. Kemudian pilih **/dev/sdb6**. Lalu pilih opsi **type**. Selanjutnya select partition type dengan memilih opsi **Linux Swap** (untuk swap)
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 51 45" src="https://github.com/user-attachments/assets/d4fd1a06-9339-485e-a09d-06c001a2cf40" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 51 45 (1)" src="https://github.com/user-attachments/assets/de16ac31-5736-4301-9f2d-1027ee0144fd" />
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-24 at 11 51 46" src="https://github.com/user-attachments/assets/c5881e52-f787-4569-b5ab-9bc74998eeb5" />

- Selanjutnya lakukan hal yang sama. Pilih **free space** kemudian pilih opsi **new**. Untuk partition size tidak perlu diubah karena itu sisa untuk root sekitar **24.3G**. Kemudian pilih **/dev/sdb7**. Lalu pilih opsi **write** dan ketik "yes". Lalu pilih opsi **quit**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 36" src="https://github.com/user-attachments/assets/bc334a9a-0afc-4223-bf41-880cc365366d" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 38" src="https://github.com/user-attachments/assets/81a70a66-2bad-4233-8544-4283d1af9572" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 38 (1)" src="https://github.com/user-attachments/assets/77877fd4-ae9d-472c-85c8-b9fc2637a62c" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 38 (2)" src="https://github.com/user-attachments/assets/6b778798-4ffd-4b77-8912-6fd4902ac6e4" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 39" src="https://github.com/user-attachments/assets/364ead0b-018a-4840-8517-1a4550569a7d" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 40" src="https://github.com/user-attachments/assets/eff8bb14-1949-4d1e-b178-ebb4cd90b40d" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 11 59 41" src="https://github.com/user-attachments/assets/f5457f1e-c4c2-4c2c-9252-dc3695b1aa63" />

- Ketik **lsblk -o name,fstype,size** untuk mengecek lagi partisi Linux (sdb5, sdb6, dan sdb7 merupakan partisi milik Linux)
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-24 at 12 08 49" src="https://github.com/user-attachments/assets/ec8a2c6f-34a6-4eef-a0dc-2cd004cd10b8" />

### Format partisi root
- Ketik **mkfs.ext4 /dev/sdb7**

### Format partisi boot
- Ketik **mkfs.fat -F 32 /dev/sdb5**

### Format partisi swap
- Ketik **mkswap /dev/sdb6**
- Lalu ketik **swapon /dev/sdb6**

- Ketik **lsblk -o name,fstype,size** untuk mengecek lagi partisi Linux (sdb5, sdb6, dan sdb7 merupakan partisi milik Linux)
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-24 at 12 21 33" src="https://github.com/user-attachments/assets/96e54cd7-10f3-4d86-bac9-399180d95d07" />

### Mount Filesystem
- Ketik **mount /dev/sdb7 /mnt** (untuk root)
- Lalu ketik **mount --mkdir /dev/sdb5 /mnt/boot** (untuk boot)
- Lalu ketik **lsblk** untuk mengecek
<img width="1200" height="1600" alt="WhatsApp Image 2026-05-24 at 12 29 27" src="https://github.com/user-attachments/assets/459261df-1772-4bda-a7e7-a5c11ad330c5" />

- Untuk menginstall sistem dasar ketik **pacstrap -K /mnt base linux linux-firmware base-devel neovim git networkmanager**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 34 02" src="https://github.com/user-attachments/assets/bf5e733e-772f-4246-8236-5fa132889e9e" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 34 03" src="https://github.com/user-attachments/assets/d7b18766-8554-447b-86cb-fb094dc2f831" />

- Untuk membuat fstab ketik **genfstab -U /mnt > /mnt/etc/fstab**, enter, lalu ketik lagi **arch-chroot /mnt**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 41 50" src="https://github.com/user-attachments/assets/50e2a879-b5d6-4bb8-923e-ab5e7f1ce21e" />

- Untuk mengatur timezone ketik **ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime**, enter, lalu ketik lagi **hwclock --systohc**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 47 12" src="https://github.com/user-attachments/assets/5d9b1de9-b597-4fd2-82af-9a71f59ac836" />

- Selanjutnya ketik **locale-gen**, enter, kemudian ketik **nvim /etc/locale.conf**, enter dan ketik 'i' pada keyboard dan akan muncul tampilan seperti ini
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 50 43" src="https://github.com/user-attachments/assets/2b2e433b-88bf-48c1-bc5f-240bde735e50" />

- Ketik:
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 52 53" src="https://github.com/user-attachments/assets/8067d5f6-796a-43c8-81ed-103a93c07dbf" />

- Lalu ketik:
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 52 53 (1)" src="https://github.com/user-attachments/assets/29e603ea-8ad0-4638-a1ae-3d704f44a5f3" />

- Untuk membuat hostname ketik:
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 56 07" src="https://github.com/user-attachments/assets/83559deb-6bcc-427c-a026-b827529c4f6e" />

- Untuk membuat user ketik:
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 12 57 30" src="https://github.com/user-attachments/assets/3736b2c9-87fd-4440-abf1-549c9bb6203b" />

 - Selanjutnya ketik **echo '(nama user yang dipilih) ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none**. Dianjutkan dengan mengetik **pacman -S grub efibootmgr os-prober**, enter kemudian ketik huruf **y**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 01 17" src="https://github.com/user-attachments/assets/dd3cdaf6-f2f4-4cfb-bf4f-e70186745f52" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 01 18" src="https://github.com/user-attachments/assets/2df7ab84-4cae-455a-bcbb-f0a11810fd71" />

- Selanjutnya ketik **grub-install --target=86_64-efi --efi-directory=/BOOT --bootloader-id=GRUB**. Kemudian ketik **nvim /etc/default/grub**
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 05 20" src="https://github.com/user-attachments/assets/9abe74aa-adba-4ff6-a22f-91760b1d3c6b" />

- Hapus '#' sehingga menjadi (GRUB_DISABLE_OS_PROBER=false), enter lalu ketik **:wq** 
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 07 53" src="https://github.com/user-attachments/assets/d8c2d892-ca6b-4974-90a0-f3e40d2c8a11" />

- Kemudian ketik:
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 10 19" src="https://github.com/user-attachments/assets/08dbda83-5b12-4248-ab3c-34fbbbbdce57" />

- Lalu ketik **exit**. Dilanjutkan dengan mengetik **umount -R /mnt**. Lalu ketik **reboot** dan enter
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-24 at 13 13 02" src="https://github.com/user-attachments/assets/5b1d8df3-9314-4761-a819-b531f9742249" />

### Proses Instalasi KDE Plasma
- 
