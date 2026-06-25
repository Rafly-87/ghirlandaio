```
system                                                                                                                                                                          
systemctl status podman                                                                                                                                
systemctl enable --global podman                                                                                                                       
systemctl aktif --global podman                                                                                                                        systemctl start --global podman                                                                                                                        
systemctl status --global podman                                                                                                                       
systemctl status podman                                                                                                                                
systemctl status --global podman                                                                                                                       
systemctl status podman                                                                                                                                
systemctl status --status podman                                                                                                                       
systemctl status podman
```
```                                                                                                                               
cd /etc/containers                                                                                                                                     
ls                                                                                                                                               
cd ..                                                                                                                                            
cd /etc/containers/                                                                                                                                     
nvim /registries.conf 
ls
```
```
nvim /registries.conf
wq + enter
nvim /etc/containers/registries.conf

Cari baris berikut 
# unqualified-search-registries = ["example.com"]

Ubah menjadi (tulisan akan berubah mejadi warna biru)
unqualified-search-registries= ["docker.io"]
```
```
nvim /etc/sysctl.d/novaria.conf

Ketik paling atas
kernel.unprivileged_userns_clone=1
```
```
sysctl --system   
nvim /etc/sysctl.d/novaria.conf
pacman -S podman-compose 
```
```
Iwctl
station wlan0 connect 1`                                                                                                                                          
station wlan0 connect 11                                                                                                                                          
(masukkan pass)
Exit
```
       
