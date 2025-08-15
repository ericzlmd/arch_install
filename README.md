# Arch UEFI Install - July 2025

## Table of Contents
   - Make Partitions
   - Format Partitions
   - Mount File Systems
   - Select Mirrors
   - Install Essential Packages
   - Generate fstab 
   - Set timezone
   - Set localization 

### Make Paritions

Use ```gdisk```

```# gdisk /dev/sda```

Press ```n``` to make a new and every partition below.

EFI:  
   - [Enter] 2x
   - Last Sector: +1G
   - FS code: EF00

SWAP:
   - [Enter] 2x
   - Last Sector: +64G (equal to your RAM)
   - FS code: 8200

/root:
   - [Enter] 2x
   - Last Sector: +40G
   - FS code: 8300 (or leave blank)

/home:
   - [Enter] 2x
   - Last Sector: (leave blank to use rest)
   - FS code: 8300 (or leave blank)

Enter ```p``` to print all the partition made. Then ```w``` to write the partition on disk permanently.

### Format Partitions

EFI:

```# mkfs.fat -F 32 /dev/sda1```

SWAP:

```# mkswap /dev/sda2```

/root

```# mkfs.ext4 /dev/sda3```

/home

```# mkfs.ext4 /dev/sda4```


### Mount File Systems

EFI:

```# mkdir /boot/efi```

```# mount /dev/sda1 /boot/efi```


SWAP:

```# swapon /dev/sda2```

/root:

```# mount /dev/sda3 /mnt```

/home:

```# mkdir /mnt/home```

```# mount /dev/sda4 /mnt/home```

### Select Mirrors

```# reflector --country '[your country]' --sort rate --latest 5 >> /etc/pacman.d/mirrorlist```


### Install base & essential packages

```# pacstrap -K /mnt base linux linux-firmware git base-devel vi neovim efibootmgr```

# Credits
- [Arch Linux](https://wiki.archlinux.org/title/Main_page)
- [Bread on Penguins](https://www.youtube.com/@BreadOnPenguins)
