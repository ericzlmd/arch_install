# Arch UEFI Install - July 2025

# Table of Contents
   - [Create Partitions](#create-partions)
   - Format Partitions
   - Mount File Systems
   - Select Mirrors
   - Install Essential Packages
   - Generate fstab 
   - Change root
   - Set timezone
   - Set localization 
   - Set hostname
   - Set root/user password
   - Enable system services on boot
   - Install GRUB
   - Reboot
   - Post Install

## Create Partions

Use ```gdisk```

```# gdisk /dev/sda```

Press ```n``` to make a new and every partition below.

EFI:  
   - [Enter] 2x
   - Last Sector: +1G
   - FS code: EF00

SWAP:
   - [Enter] 2x
   - Last Sector: +8G (equal to your RAM)
   - FS code: 8200

/root:
   - [Enter] 2x
   - Last Sector: +60G
   - FS code: 8300 (or leave blank)

/home:
   - [Enter] 2x
   - Last Sector: (leave blank to use remaining disk space)
   - FS code: 8300 (or leave blank)

Enter ```p``` to print all the partition made. Then ```w``` to write the partition on disk permanently.

## Format Partitions

EFI:

```# mkfs.fat -F 32 /dev/sda1```

SWAP:

```# mkswap /dev/sda2```

/root

```# mkfs.ext4 /dev/sda3```

/home

```# mkfs.ext4 /dev/sda4```


## Mount File Systems

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

## Select Mirrors

```# reflector --country '[your country]' --sort rate --latest 5 >> /etc/pacman.d/mirrorlist```


## Install base & essential packages

This section is mostly when you'll diverge and add your own most use apps, desktop environment, etc. The following below is mostly standard needed for other apps to be installed.

```# pacstrap -K /mnt base linux linux-firmware git base-devel networkmanager vi neovim efibootmgr pacman-contrib```

Nvidia Drivers - Wayland
```nvidia-open nvidia-settings lib32-opencl-nvidia opencl-nvidia lib32-nvidia-utils nvidia-utils egl-wayland```

Apps
```konsole, dolphine firefox gwenview kwrite kcalc```

Audio
```pipewire lib32-pipewire pipewire-alsa pipewire-pulse```

## Generate fstab

Make sure that the entries on ```lsblk``` reflects what's appended on ``` cat/mnt/etc/fstab```. Mount/re-mount partitions and edit manually if necessary.

```# genfstab -U /mnt >> /mnt/etc/fstab```

## Change root

```# arch-chroot /mnt```

## Set localtime

```# ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime```

```# hwclock --systohc```


## Set localization

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

## Set hostname

```# echo "[hostname]" >> /etc/hostname```

## Change root password

Enter new password twice:

```# passwd```

## Add user

```# useradd -m -G wheel,users [username]```

## Change user password

```# passwd [username]```

## Enable system servcies

```# systemctl enable bluetooth.service```

```# systemctl enable NetworkManager```

```# systemctl enable sddm```

```# systemctl enable fstrim.timer```

```# systemctl enable paccache.timer```

## Install GRUB

```# pacman -S grub efibootmgr```

```# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB```

Generate GRUB config

```# grub-mkconfig -o /boot/grub/grub.cfg```

## Reboot

```# exit```

```# umount -R /mnt```

```# reboot```

# Post Install

# Credits
- [Arch Linux](https://wiki.archlinux.org/title/Main_page)
- [Bread on Penguins](https://www.youtube.com/@BreadOnPenguins)
