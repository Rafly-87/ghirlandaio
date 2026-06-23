# Instalasi Slims


## . Cek kernel aktif

```bash
uname -r
```

Karena yang digunakan adalah hardened maka yang muncul harus:

```bash
7.0.xx-hardened
```

---

## . Sambungkan internet

```bash
iwctl
```

```bash
station wlan0 get-networks
```

```bash
station wlan0 connect nama wifi
```

Masukkan password Wi-Fi lalu:

```bash
exit
```

Cek koneksi:

```bash
ping 8.8.8.8
```

## . Pastikan podman sudah aktif

```bash
podman --version
```

Jika belum terinstall silahkan install dulu (Cek pada bagian install podman)

---

## . Download slims

```bash
wget https://github.com/slims/slims9_bulian/archive/refs/heads/master.zip
unzip master.zip
```

Kemudian Ekstrak file yang sudah diunduh

```bash
unzip master.zip
```
