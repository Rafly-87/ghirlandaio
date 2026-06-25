## Cek status apakah firewalld sudah aktif
```
systemctl status firewalld
```

## Cek semua list zone firewall yang ada
```
firewall-cmd --list-all-zone
```

## Hapus service pada zone work
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Hapus service zone public
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,http,https} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```
## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Menghapus service pada zone Internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Menghapus service home zone
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```














