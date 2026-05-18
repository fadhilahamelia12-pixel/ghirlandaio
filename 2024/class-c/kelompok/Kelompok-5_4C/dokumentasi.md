# Panduan Instalasi Arch Linux 

Hal pertama yang dilakukan adalah mendownload archiso 

## Cara download archiso

masuk ke website archlinux.org/download dan scroll ke bawah sampai ada indonesia lalu pencet citrahost.com

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-15 at 20 00 43" src="https://github.com/user-attachments/assets/88246516-ca2e-44e4-af35-25c4be2e60d7" />

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/c8afb423-76b7-486c-a2d1-db897269d7cb" />

Lalu pencet yang ke dua yang . Iso

Lalu kalau sudah mendowload, download rufus cari di google lalu masuk ke websitenya

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/a51a3c48-7fce-4c9d-9a00-149805dbf073" />

Sesuaikan dengan laptop (kalo saya si yang atas)

https://github.com/user-attachments/assets/acd78752-b682-45ee-8490-132e5b40a70e

Langkah awal ialah dengan cara memasukkan flashdisk ke laptop, lalu pencet tombol select dan pilih archiso yang telah didownload, untuk partition scheme, biasanya laptop baru menggunakan gpt.

- **USB Drive:** Minimal kapasitas 2GB (direkomendasikan 4GB atau lebih).
- **Peringatan:** Proses ini akan menghapus **seluruh data** di dalam USB drive.

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/c6cdf2fa-bfdc-4ba4-9240-5993030daca3" />
Setelah start pencet yang bawah


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/7f5258eb-f462-4b5b-a00c-c5013b2595c8" />
Ok, untuk menaruh archisonya. (Gunakan flashdisk yang tidak terpakai karenaakan menghapus seluruh file)

Close saja kalau sudah

Untuk partisi cek disk management (windows + x)

Cek berapa partisi yang kosong

Kalo udah pencet yang capacity paling banyak lalu klik kanan pakai shrink volume


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/ab5ee5bd-a7f4-4d55-bd09-ee20e37da779" />

Kalau masih banyak gunakan saja 50000, yaitu terbaca 50GB, Lalu shrink


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/938ada5e-6fac-49fb-9f66-4087ac2fa757" />

Kira kira seperti ini kalau sudah dipartisi, namun kita akan membuat partisi di arch

Masuk ke entering setup cari di google menyesuaikan laptop, Akses BIOS dengan menekan F1 saat logo Lenovo muncul, buka tab Security > Secure Boot, ubah menjadi disabled, lalu simpan dengan F10, Otomatis mereboot device. (Jika berbeda cari kembali di google menyesuaikan laptop)

Masuk boot menu untuk thinkpad yaitu f12(kondisi flashdisk tercolok dan terisi archiso)


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5a149ab2-6ed8-42c2-9fcb-051c2aa6c577" />

> Pencet usb


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/92555886-552a-4826-8e01-1026f12e737d" />

> Pencet enter



<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/94602b06-99fa-48d0-9708-41540148c7dc" />

> Tunggu sampai tampilan seperti ini lalu ketik iwctl

# Kondisi Khusus

Ketika device list di iwctl tidak terlihat wlan0 wlan1 dan semacamnya,ketik rfkill list


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/0e30f0bd-392d-434a-926d-051a9fdc5393" />

Contoh seperti ini, lalu ketik rfkill unblock wlan, Semisal ingin mengunblock bluetooth ketik saja unblock bluetooth

Ketik device list


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/25b6ef19-b6f1-49a0-8b40-269feecce651" />

Station wlan(0)(menyesuaikan name di tabel)

Contoh: 

```bash
Station wlan0 connect (nama wifi)
```

Kalo ada spasi dalam nama wifi memakai petik terlebih dahulu, Contoh "uin fah"


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/74161428-1d8f-429d-8d0d-db4d143604c7" />

Masukan password

lalu enter saja

Cek jaringan yaitu dengan mengetik 

exit

Terlebih dahulu lalu

```bash
ping 8.8.8.8
```

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5090e3f0-8037-4aff-b1b7-a37ba61bf03c" />

Kalau seperti ini connect ke wifi, jika ga konek network unreachable


habis ni ctrl+c

---


