## Sambungkan Wifi
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect nama wifi
```
```
masukin pass
```
```
exit
```
## Memeriksa  partisi
```
lsblk
```
## Format Luks 
```
cryptsetup luksFormat /dev/nvme0n1p7
```
```
masukkan password
```
```
cryptsetup luksOpen /dev/nvme0n1p7(partisi root) stardust (nama device)
```
