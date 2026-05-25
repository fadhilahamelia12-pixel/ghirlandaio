# Instalasi linux with cis layout disk
## menghubungkan ke internet

```
iwctl
```
lalu ketik

```
station wlan0 connect "namawifi"
```
lalu cek
```
ping 8.8.8.8
```
jika 


## Membuat Physical Volume
Partisi yang paling besar digunakan sebagai Physical Volume.

```pvcreate /dev/sda6 ```⁠

---
## Membuat Volume Group
Volume Group Bernama ``system``

``vgcreate system /dev/sda6``

---
## Membuat Logical Volume
### Root
Fungsi: menyimpan file konfigurasi dan package Linux.
``lvcreate -L 10G system -n root``
### Var
Fungsi: menyimpan data dinamis yang bisa dibuat, dimodifikasi, atau dihapus.

``lvcreate -L 10G system -n vars``
### Var Log
Fungsi: menyimpan log jurnal dan aktivitas aplikasi Linux.

``lvcreate -L 1G system -n vlog``
### Var Audit
Fungsi: mencatat log yang bersifat kritikal dan aktivitas kernel.

``lvcreate -L 512M system -n vaud``
### Var Tmp
Fungsi: menyimpan data sementara yang tetap ada setelah reboot.

``lvcreate -L 4G system -n vtm``
### Home Public
Fungsi: menyimpan data user seperti PDF, gambar, foto, dan dokumen umum.

``lvcreate -L 1G system -n home``
### Home Pribadi
Menggunakan sisa ruang kosong.

``lvcreate -l 50%FREE system -n crab``

---
# Format Partisi
## EFI System Partition
Partisi EFI menggunakan filesystem FAT32.

``mkfs.vfat -F32 /dev/sda5`` ⁠

Keterangan:

•⁠  ⁠⁠ ``mkfs`` ⁠ = membuat filesystem

•⁠  ⁠⁠ ``vfat`` ⁠ = filesystem FAT32

•⁠  ⁠Digunakan oleh UEFI

---
## Format Logical Volume
### Root
``
mkfs.ext4 /dev/system/root
 ⁠``

### Var

``mkfs.ext4 /dev/system/vars``
⁠

### Var Log

``mkfs.ext4 /dev/system/vlog``
 ⁠

### Var Audit

``mkfs.ext4 /dev/system/vaud``
 ⁠

### Var Tmp

``mkfs.ext4 /dev/system/vtmp``
 ⁠

### Home Public

``mkfs.ext4 /dev/system/home``

---
## Enkripsi Home Pribadi

``cryptsetup luksFormat /dev/system/crab``
 ⁠
 
Jawab:

⁠ text
YES
 ⁠

Masukkan password dan verifikasi dua kali.

---
# Mounting Filesystem

## Root
``
mount /dev/system/root /mnt
 ⁠``

---

## EFI
``
mount --mkdir \
-o uid=0,gid=0,fmask=0077,dmask=0077 \
/dev/sda5 /mnt/boot
 ⁠``

Keterangan:

•⁠  ⁠⁠ ``--mkdir`` ⁠ membuat direktori otomatis

•⁠  ⁠⁠ ``uid=0 ``⁠ hanya root yang memiliki file

•⁠  ⁠⁠ ``gid=0 ``⁠ grup root

•⁠  ⁠⁠ ``fmask=0077`` ⁠ file hanya dapat diakses root

•⁠  ⁠⁠`` dmask=0077 ⁠`` direktori hanya dapat diakses root

---
## Var Log

⁠``
mount --mkdir 
-o rw,nodev,noexec,nosuid 
/dev/system/vlog /mnt/var/log
`` ⁠

---

## Var Audit

⁠``
mount --mkdir 
-o rw,nodev,noexec,nosuid 
/dev/system/vaud /mnt/var/log/audit
 ⁠``

---

## Var Tmp

⁠``
mount --mkdir 
-o rw,nodev,noexec,nosuid 
/dev/system/vtmp /mnt/var/tmp
 ⁠``

---

## Home Public

⁠``
mount --mkdir 
-o rw,nodev,noexec,nosuid 
/dev/system/home /mnt/home
 ⁠``

Partisi ⁠ crab ⁠ tidak di-mount karena akan dibuka otomatis menggunakan ⁠ ``pam_mount ⁠.``

---
# Instalasi Package

⁠``
pacstrap /mnt base base-devel 
intel-ucode linux-lts linux-firmware \linux-lts-headers lvm2 neovim openssh superfile podman podman-desktop iptables mpd mpc mpv 
keepassxc secrets booster networkmanager pam_mount
 ⁠``

## Keterangan Package

| Package | Fungsi |
|----------|---------|
| base | Sistem dasar Arch Linux |
| base-devel | Tools build package |
| intel-ucode | Microcode Intel |
| linux-lts | Kernel Linux LTS |
| linux-firmware | Firmware perangkat keras |
| linux-lts-headers | Header kernel |
| lvm2 | Dukungan LVM |
| neovim | Editor teks CLI |
| openssh | Remote access SSH |
| superfile | File manager CLI |
| podman | Container engine |
| podman-desktop | GUI Podman |
| iptables | Firewall |
| mpd | Music Player Daemon |
| mpc | Client MPD |
| mpv | Pemutar video |
| keepassxc | Password manager |
| secrets | Password manager GNOME |
| booster | Initramfs generator |
| networkmanager | Manajemen jaringan |
| pam_mount | Mount otomatis saat login |

---
# Generate FSTAB

⁠``
genfstab -U /mnt > /mnt/etc/fstab
 ⁠``

Tambahkan tmpfs:

⁠``
echo "tmpfs /tmp tmpfs defaults,nodev,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
 ``⁠

---

# Masuk Chroot

⁠ ``
arch-chroot /mnt
 ⁠``

---

# Hostname

⁠``
nvim /etc/hostname
`` ⁠

Masukkan hostname lalu simpan.

---
# Timezone

⁠ ``
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
 ⁠``

⁠`` 
hwclock --systohc
 ⁠``

---

# Locale

⁠``
nvim /etc/locale.gen
 ⁠``

Aktifkan:

⁠ ``text
en_US.UTF-8 UTF-8
 ⁠``

Generate:

⁠``
locale-gen
⁠``

``
locale > /etc/locale.conf
 ⁠``

Edit:

⁠``
nvim /etc/locale.conf
``

---

# Setup Home Terenkripsi

Buka partisi:

⁠``
cryptsetup luksOpen /dev/system/crab capit
 ⁠``

Masukkan password.

Format:

⁠``
mkfs.ext4 /dev/mapper/capit
 ⁠``

Buat direktori:

⁠``
mkdir /home/user
 ⁠``

Buat user:

⁠``
useradd -d /home/user mutia
 ⁠``

Atur kepemilikan:

⁠``
chown -R mutia:mutia /home/user
 ⁠``

Password user:

⁠``
passwd mutia
 ⁠``

Hak sudo:

⁠``
echo "mutia ALL=(ALL:ALL) ALL" > /etc/sudoers.d/mutia
 ``⁠

---
# Edit dile PAM
``
nvim /etc/pam.d/system-login
 ⁠``

Tambahkan:

⁠`` text
session optional pam_mount.so
 ⁠``

Sesuaikan dengan konfigurasi pada gambar referensi.

---

# Konfigurasi Booster

Edit file:

⁠``
nvim /etc/booster.yaml
 ⁠``

Isi sesuai konfigurasi pada gambar referensi.

---

# Generate Booster Image

Lihat versi kernel yang terpasang:

⁠``
ls /usr/lib/modules
 ⁠``

Contoh output:

⁠`` text
6.18.32-2-lts
 ⁠``

Generate image Booster:

⁠``
booster build --kernel-version 6.18.32-2-lts /boot/booster-linux-lts-new.img
 ⁠``

Hapus image lama:

⁠``
rm -fr booster-linux-lts.img
 ⁠``

---

# Install systemd-boot

Install bootloader:

⁠``
bootctl --path=/boot install
 ⁠``

Verifikasi instalasi:

⁠ ``
bootctl --graceful
 ⁠``

---

# Membuat Entry Boot

Buat file konfigurasi:

⁠``
nvim /boot/loader/entries/booster.conf
 ⁠``

Isi sesuai konfigurasi pada gambar referensi.

Contoh struktur:

⁠ ``text
title archlinux with booster
linux /vmlinuz-linux-lts
initrd /intel-ucode.img
initrd /booster-linux-lts-new.img
options root=/dev/system/root rw
 ⁠``

Simpan dan keluar.

---

# Konfigurasi Loader

Edit file:

⁠ ``
nvim /boot/loader/loader.conf
 ⁠``

Isi:

⁠ ``text
default booster.conf
 ⁠``

Simpan dan keluar.

---

# Selesai Instalasi

Keluar dari chroot:

⁠`` 
exit
 ``⁠

Unmount seluruh partisi:

⁠ ``
umount -R /mnt
 ⁠``

Restart sistem:

⁠ ``
reboot
``
