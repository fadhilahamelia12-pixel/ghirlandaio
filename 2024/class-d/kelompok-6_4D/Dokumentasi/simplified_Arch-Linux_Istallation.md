## setelah masuk Arch install dari BIOS 

## masukan internet / koneksi internet
> iwctl <br>
> device list <br> 
> station [nama_device] scan <br>
> station [nama device] get-networks <br>
> station [nama_device] connect > masukan phasaprhharse / password : <br>
> station [nama_device} show <br>
> exit <br>

## lihat partisi 
> fdisk -l <br>
> lsblk <br>

## membuat partisi 

>cfdisk /dev/sda or /dev/nvme0n1 <br> 

| sda/nvme0n1 | format type | mount point | G |
|:------------|:------------|:------------|:---|
|sda1/nvme0n1p1    | mkfs.fat    | /mnt/boot   | 1G |
|sda2/nvme0n1p2    | mkswap      | swapon      | 4G |
|sda3/nvme0n1p3    | mkfs.ext4   | /mnt        | 50G |

## membuat format 

> mkfs.fat -F 32 /dev/sda1 or /dev/nvme0n1p1 <br>
> mkkswap /dev/sda2 or /dev/nvme0n1p2 <br>
> mkfs.ext4 sda/3 or /dev/nvme0n1p3 <br>

## me mounting point 
