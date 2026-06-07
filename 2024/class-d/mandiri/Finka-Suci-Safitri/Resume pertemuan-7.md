# Resume Kelompok 7
## NIM: 12402051030091
## Mata Kuliah: Perpustakaan dan Arsip Digital

### 1. LVM on LUKS dan LUKS on LVM
LVM on LUKS adalah saat disk dienkripsi dulu dengan LUKS, lalu LVM dibuat di atasnya. Semua logical volume cukup dibuka sekali dengan satu kunci. Cocok untuk penggunaan sederhana dengan akses data serentak.
LUKS on LVM adalah saat LVM dibuat dulu, lalu tiap logical volume dienkripsi terpisah dengan LUKS. Lebih aman karena tiap volume punya kunci sendiri, cocok untuk data sensitif.

### 2. KeePassXC dan Secret Service
KeePassXC adalah password manager lokal terenkripsi. Secret Service (GNOME Keyring/KDE Wallet) adalah layanan sistem untuk menyimpan kredensial agar bisa dipakai aplikasi lain. Keduanya bisa terintegrasi untuk penyimpanan dan penggunaan password secara aman.

### 3. Podman
Podman adalah container engine tanpa daemon dan mendukung rootless mode. Digunakan untuk menjalankan aplikasi dalam container terisolasi, lebih aman dan bisa diotomasi dengan systemd.

### 4. XFCE4
XFCE4 adalah desktop environment Linux yang ringan dan hemat resource, cocok untuk komputer dengan spesifikasi rendah atau server dengan GUI.

### 5. Superfile
Superfile adalah file manager berbasis terminal (TUI) untuk memudahkan pengelolaan file di server tanpa GUI.

### 6. iptables
iptables adalah firewall Linux untuk mengatur lalu lintas jaringan, membuka atau menutup port tertentu, dan melindungi server dari akses tidak sah.

### 7. CIS (Center for Internet Security)
CIS menyediakan standar dan panduan keamanan sistem (benchmark) untuk hardening server dan infrastruktur IT sesuai praktik terbaik.

### 8. OpenSSH
OpenSSH adalah tool untuk akses remote aman ke server melalui SSH, mendukung login terenkripsi, transfer file, dan SSH key.

### 9. Booster
Booster adalah tool pembuatan initramfs yang cepat dan ringan, digunakan saat booting terutama untuk sistem dengan enkripsi LUKS.

### 10. MPD, MPV, MPC
MPD adalah server musik di background, MPC adalah client untuk mengontrol MPD via terminal, dan MPV adalah media player serbaguna untuk audio/video.
