# Arch UEFI Install - 2025

## Table of Contents
   - Make Partitions
   - Make File Filesystems
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

# Credits
- [Arch Linux](https://wiki.archlinux.org/title/Main_page)
- [Bread on Penguins](https://www.youtube.com/@BreadOnPenguins)