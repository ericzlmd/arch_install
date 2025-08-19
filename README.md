# Arch UEFI Install - July 2025

## Table of Contents
   - Make Partitions
   - Format Partitions
   - Mount File Systems
   - Select Mirrors
   - Install Essential Packages
   - Generate fstab 
   - Change root
   - Set timezone
   - Set localization 
   - Set hostname

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

This section is mostly when you'll diverge and add your own most use apps, desktop environment, etc. The following below is mostly standard needed for other apps to be installed.

```# pacstrap -K /mnt base linux linux-firmware git base-devel networkmanager vi neovim efibootmgr```

### Generate fstab

Make sure that the entries on ```lsblk``` reflects what's appended on ``` cat/mnt/etc/fstab```. Mount/re-mount partitions and edit manually if necessary.

```# genfstab -U /mnt >> /mnt/etc/fstab```

### Change root

```# arch-chroot /mnt```

### Set localtime

```# ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime```

```# hwclock --systohc```


### Set localization

```# nvim /etc/locale.gen```

And uncomment the UTF-8 section:

```
en...
en_US.UTF..
en_US ISO..
...
```

Generate localization:

```# locale-gen```

Create locale.conf

```# nvim /etc/locale.conf```

Add the following:

```LANG=en_US.UTF-8```

# Credits
- [Arch Linux](https://wiki.archlinux.org/title/Main_page)
- [Bread on Penguins](https://www.youtube.com/@BreadOnPenguins)
