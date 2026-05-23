# Instalasi Arch Linux dengan Desktop Environment KDE Plasma
## Preparation
- Siapkan bootable USB yang berisi ISO Arch Linux.
- Masuk ke mode BIOS dan nonaktifkan secure boot.
  ![alt text]()
- Lakukan boot ke USB dan reboot.
  ![alt text]()

## Live Environment
- Pilih yang pailing atas `Arch Linux install medium (x86_64, UEFI)`
  ![alt text]()

## Menghubungkan internet
![alt text]()
```
iwctl
```
Cek wifi
```
device list
```
menyambungkan koneksi wifi
```
station wlan0 connect "nama wifi"
```
tes koneksi
```
ping 8.8.8.8
```

## Sinkronisasi waktu
![alt text]()
```
timedatectl
```

## Partisi Disk
![alt text]()
lakukan cek partisi 
```
lsblk
```
buat partisi
![alt text]()
```
cfdsik /dev/the_disk_to_be_partitioned
```
buat partisi `EFI`, `swap`, `root`
cek kembali parisi yang telah dibuat dengan `lsblk`

## Format partisi
![alt text]()
format partisi root
```
mkfs.ext4 /dev/root_partition
```
format partisi swap
```
mkswap /dev/nvme0n1p2
```
aktifkan swap
```
swapon /dev/swap_partition
```
format partisi EFI
```
mkfs.fat -F 32 /dev/efi_system_partition
```

## Mount filesystem
![alt text]()
mouunt root
```
mount /dev/root_partition /mnt
```
mount EFI
```
mount --mkdir /dev/efi_system_partition /mnt/boot
```
cek kembali partisi yang telah di mounting dengan `lsblk`
![alt text]()

## Instalasi sistem
```
pacstrap -K /mnt base linux linux-firmware base base-devel networkmanager
```

## Membuat fstab, masuk ke arch-chroot, dan mengatur timezone
![alt text]()
membuat fstab 
```
genfstab -U /mnt >> /mnt/etc/fstab
```
masuk ke arch-chroot
```
arch-chroot /mnt
```
mengatur timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
