# DOKUMENTASI DISABLE MODULE KERNEL

## Blacklist modul kernel via Modprobe
```
nvim /etc/modprobe.d/modprobe.conf
```
Masukan ini semua
```
blacklist hfs

install hfsplus /bin/false
blacklist hfsplus

install jffs2 /bin/false
blacklist jffs2

install overlay /bin/false
blacklist overlay

install squashfs /bin/false
blacklist squashfs

install udf /bin/false
blacklist udf

install firewire_core /bin/false
blacklist firewire_core

install usb_storage /bin/false
blacklist usb_storage

install ceph /bin/false
blacklist ceph

install cifs /bin/false
blacklist cifs

install exfat /bin/falsae
blacklist exfat

install fscache /bin/false
blacklist fscache

install fuse /bin/false
blacklist fuse

install gfs2 /bin/false
blacklist gfs2

install nfs_common /bin/false
```


        
> Biarkan seperti itu, lalu esc :wq

Mencopot Modul yang sedang aktif secara Instan
```
modprobe -r usb-storage 2>/dev/null
```
```
modprobe -r bluetooth 2>/dev/null
```
Memperbarui Ramdisk Kernel
```
mkinitcpio -P
```
Pre-check status module
```
lsmod | grep usb-storage
```
```
lsmod | grep bluetooth
```

---
## Hardening Jaringan Berstandar CIS
Konfigurasi Parameter Kernel
```
nvim /etc/sysctl.d/99-cis.conf
```
Buat seperti ini
```
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.log_martians = 1
kernel.randomize_va_space = 2
kernel.sysrq = o
kernel.dmesg_restrict = 1
fs.suid_dumpable = 0
net.ipv6.conf.all.disable_ipv6 = 1
```
Instalasi Tools dan Penerapan Konfigurasi Sysctl
```
pacman -S procps-ng
```
```
sysctl -p /etc/sysctl.d/99-cis.conf
```
Pengecekan hasil Sysctl
```
sysctl net.ipv4.ip_forward
```
```
sysctl kernel.randomize_va_space
```
```
sysctl kernel.sysrq
```

---
## Konfigurasi Firewall Hardening
Mengaktifkan Firewall
```
systemctl enable --now firewalld
```
Set Default Zone to Drop
```
firewall-cmd --set-default-zone=drop
```
Membuka akses untuk layanan web
```
firewall-cmd --zone=drop --add-service=http --permanent
```
Cek alamat IP
```
ip addr show wlan0
```
Mengizinkan koneksi dari jaringan sendiri
```
firewall-cmd --zone=drop --add-source=(alamat IP sendiri) --permanent
```
> ubah angka terakhir sebelum tanda garis miring menjadi 0.
> contoh : alamat Ip 10.252.10.248/24, maka menjadi 10.252.10.0/24.
```
firewall-cmd --reload
```
```
firewall-cmd --list-all
```
