# Cara Install Linux
## Nama: Finka Suci Safitri
## Kelas: 4D
## NIM:12402051030091
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum

Jika sudah terinstall, masukan flashdisk, restart laptop, pilih usb flashdisk, pilih arch linux

Step 1: 
- iwctl

Step 2: 
- station wlan0 get-networks (untuk sambungin ke wifi)

Step 3: 
- station wlan0 connect (nama wifi)

Step 4: masukin password wifi

Step 5: exit

Step 6: 
- lsblk

Step 7: 
- cfdisk /dev/sda

Step 8: delete semua

Step 9: ‘new’

Step 10: 
1G untuk boot type efi system
4G untuk swap, type linux swap
14H root, type linux filesystem (memo)

Step 11: 
- pencet 'write’
- yes
- quit

Step 12: 
- lsblk

Step 13: 
format root ke ext4
- mkfs.ext4 /dev/sda3 (nvme0n1p7 jadi sda3)
- tulis ‘y’

Step 14:
format partisi tambahan
- mkfs.ext4 /dev/sda2
- tulis ‘y’

Step 15:
aktifkan swap jika cadangan ram penuh
- mkswap /dev/sda2
swapon /dev/sda2

Step 16: 
format boot
- mkfs.fat -F 32 /dev/sda1

Step 17:
mount partisi
- mount /dev/sda3 /mnt

Step 18: 
- mount --mkdir /dev/sda1 /mnt/boot

Step 19: 
- pacstrap -K /mnt base-devel linux linux-firmware nvim intel(sesuai laptop masing)-ucode

download

Step 20:
generate fstab
- genfstab -U /mnt >> /mnt/etc/fstab

Step 21: 
masuk ke sistem arch
- arch-chroot /mnt

Step 22: 
mengatur timezone
- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

Step 23: 
sinkronkan dengan hardware clock
- hwclock --systohc

Step 24:
- locale-gen
- nvim /etc/locale.conf
- pencet i
- LANG=en_US.UTF-8
- pencet esc
- :wq

Step 25: 
- nvim /etc/isi nama kita sbg host
- pencet i

Step 26: 
- mkinitcpio -P

Step 27: 
- nvim /etc/locale.gen
cari #en_US. UTF dan #en_US.ISO hapus pagernya (tambahin huruf i didepan nya supaya bisa hapus pagernya)
- pencet esc
- :wq

Step 28:
- useradd -m -G wheel -s /bin/bash (nama user)
- passwd (user)
- bikin password

Step 29:
- nvim visudo /etc/sudoers.d/administrator
- pencet i
- user ALL=(ALL:ALL)ALL
- pencet esc
- :wq

Step 30:
- pacman -S kitty dolphin sddm networkmanager plasma pipewire

enter sampai pilihannya y/n, tulis ‘y’
 
Step 31: 
- systemctl enable NetworkManager.service
- systemctl enable sddm.service

Step 32:
- pacman -S efibootmgr grub os-prober
- tulis ‘y’

Step 33:
- grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=GRUB

Step 34: 
- grub-mkconfig -o /boot/grub/grub.cfg

Step 35:
- exit

Step 36:
- umount -R /mnt

Step 37:
- reboot
- lepas flashdisk

Step 38:
- login arch linux
- masukin password user

Selesai

Dokumentasi Penginstall-an Arch Linux

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/10808e59-3433-4672-b4eb-4c5a08ab88ff" />


<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/cca58c72-4f0e-4c3b-9be7-144b68d487e1" />


<img width="585" height="1040" alt="image" src="https://github.com/user-attachments/assets/1cce3c61-dc5e-4722-9332-d12e8cec38b4" />

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/53d58136-9da7-4862-916c-6cfdf10ddbd6" />

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/0fce3d57-9e3b-49ad-bb64-8e7964c84ca8" />




