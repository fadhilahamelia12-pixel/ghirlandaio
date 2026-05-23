# Instalasi Arch Linux 
## Preparation
- Siapkan bootable USB yang berisi ISO Arch Linux.
- Masuk ke mode BIOS dan nonaktifkan secure boot.
  ![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.04.jpeg?token=GHSAT0AAAAAAD5BPMY43PQAWDNWTPE5LBA42QREMGQ)
- Lakukan boot ke USB dan reboot.

## Live Environment
Pilih yang pailing atas `Arch Linux install medium (x86_64, UEFI)`

## Menghubungkan internet
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.03%20(2).jpeg?token=GHSAT0AAAAAAD5BPMY5WIWYGY6LEOVCFZ562QREOZQ)
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
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.02.jpeg?token=GHSAT0AAAAAAD5BPMY4OKQCLXJBD4MBGMI62QREPQQ)
```
timedatectl
```

## Partisi Disk
lakukan cek partisi 
```
lsblk
```
buat partisi
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01.jpeg?token=GHSAT0AAAAAAD5BPMY4UPW5A6ARYFBMVUMG2QREQKA)
```
cfdsik /dev/the_disk_to_be_partitioned
```
buat partisi `EFI`, `swap`, `root`
cek kembali parisi yang telah dibuat dengan `lsblk`

## Format partisi
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.02%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY4DQ6FBWDE7F7JHET22QREQ4Q)
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
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY5AQXBQBZ22WMN5PEQ2QRFEEA)
mount root
```
mount /dev/root_partition /mnt
```
mount EFI
```
mount --mkdir /dev/efi_system_partition /mnt/boot
```
cek kembali partisi yang telah di mounting dengan `lsblk`
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.01%20(2).jpeg?token=GHSAT0AAAAAAD5BPMY4RED7IBAG6KIWC32Y2QRFE5A)

## Instalasi sistem
```
pacstrap -K /mnt base linux linux-firmware base base-devel networkmanager
```

## Membuat fstab, masuk ke arch-chroot, dan mengatur timezone
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.00.jpeg?token=GHSAT0AAAAAAD5BPMY4ZFS3QUXVBM27CSWS2QRFF7A)
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
sinkronkan hardware clock
```
hwclock --systohc
```

## Localization
Sebelumnya instal dulu teks editor seperti neovim
```
pacman -S neovim
```
uncomenting en_US di /etc/locale.gen
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.00%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY5SAGTPJFTXXCTSTGW2QRFJ3A)
```
nvim /etc/locale.gen
``` 
generate locale
```
locale-gen
```
```
locale > /etc/locale.conf
```
config locale
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.59.jpeg?token=GHSAT0AAAAAAD5BPMY5S5OVVNHSE2PIZ5GA2QRFK7Q)
```
nvim /etc/locale.conf
```
Isi `LANG=` dan `LC_ALL=` dengan `en_US.UTF-8`

## Generate Initramfs, membuat hostname, password, dan user
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.59%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY5G5MZWCX37XA7BRH62QRFL4A)
```
mkinitcpio -P
```
isi file /etc/hostname dengan nama hostname yang ingin dibuat
```
nvim /etc/hostname
```
buat password root
```
passwd
```
buat user dan password user
```
useradd -m -G wheel -s /bin/bash "nama user"
```
```
passwd user
```

## Install bootloader GRUB
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.58.jpeg?token=GHSAT0AAAAAAD5BPMY4KFSSKQBUCNVWNE3I2QRE2NQ)
```
pacman -S grub efibootmgr
```
instalasi bootloader ke partisi EFI
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB2
```
membuat file konfigurasi
```
grub-mkconfig -o /boot/grub/grub.cfg
```
Install os prober untuk melakukan dual boot di GRUB
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.58%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY56WMCKG3EDO4HF2NW2QRE3CQ)
```
pacman -S os-prober
```
aktifkan os prober
```
echo "GRUB_DISABLE_OS_PROBER=false" >> /etc/default/grub
```
update kembali
```
grub-mkconfig -o /boot/grub/grub.cfg
```

## Reboot
keluar dari arch-chroot
```
exit
```
umount
```
umount -R /mnt
```
reboot
```
reboot
```
lepas usb ketika layar sudah mati.

---

# Instalasi Desktop Environment KDE Plasma
## Memberikan hak akses sudo ke user
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.56.jpeg?token=GHSAT0AAAAAAD5BPMY5OZ3KV6I6TT7PYPIK2QRE3ZA)
```
sudo EDITOR=nvim visudo
```
uncometing `# %wheel ALL=(ALL:ALL) ALL`

## Aktifkan network manager dan sambungkan koneksi internet
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.56%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY5KGYPCKYYGRDYBB2W2QRE4KQ)
```
systemctl enable --now NetworkManager
```
sambungkan koneksi internet
```
nmtui
```
pilih `Activate a connection`

## Install plasma
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.55.jpeg?token=GHSAT0AAAAAAD5BPMY5CEQSFO6XIQ37IFCW2QRE43A)
```
sudo pacman -S plasma sddm pipewire dolphin kitty
```
masuk ke KDE plasma
```
sudo systemctl enable --now sddm
```
Akan mucul tampilan seperti ini dan masukkan password
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.55%20(1).jpeg?token=GHSAT0AAAAAAD5BPMY5KTBIS6QDK5ZGXCWM2QRE5IA)

Tampilan filemanager dolphin
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54.jpeg?token=GHSAT0AAAAAAD5BPMY4T6DGMQ2NZLQY4PYC2QRE5TQ)

Tampilan network Network Manager
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54%20(2).jpeg?token=GHSAT0AAAAAAD5BPMY46GKPVKR7POCKD5PY2QRE57A)

Tampilan audio pipewire
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.34.54%20(2).jpeg?token=GHSAT0AAAAAAD5BPMY5VBJHDCX2MFB3DSAQ2QRE6YQ)
