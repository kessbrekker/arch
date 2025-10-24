# Arch
### Installation Guidelines
- [Omarchy Dual Boot Setup](#omarchy-dual-boot-setup)
    - [Installation Guidelines](#installation-guidelines)
  - [Dual Boot](#dual-boot)
    - [Requirements](#requirements)
    - [Configurations and Settings](#configurations-and-settings)
    - [Boot Flash Disk](#boot-flash-disk)
    - [Dual Boot Configuration](#dual-boot-configuration)
  - [if you want "delete"](#if-you-want-delete)

## Dual Boot
### Requirements
- Download [omarchy.iso](https://iso.omarchy.org) or [Arch Linux](https://archlinux.org/download/) file (i prefer omarchy.iso)
- Download [Rufus](https://rufus.ie/) or [Balena Etcher](https://etcher.balena.io/)

### Configurations and Settings
- Create Restore Point (optional)
- Disable Bitlocker
  - Cmd
    - `manage-bde -status` (if Lock Status : Lock)
    - `manage-bde -unlock C: -password` (my disk is C)
- Secure Boot : Disabled
- Diskpart : Free 50GB+ space
  - Right Click `C:`
  - Select `Shrink Volume`
  - Enter Amount of Space to Shrink in MB : 60000 (i prefer 60GB)

### Boot Flash Disk
- When it is opened omarchy setup gui
  - Press ESC and opened arch terminal
```
setfont ter-132n (optional)
ping -c 5 github.com
- ping: github.com: Temporary Failure in name resolution
iwctl
```
```
device list
```
if dont see anything
```
device wlan0 set-property Powered on
```
```
station wlan0 get-networks
station wlan0 connect [Wi-Fi]
```
and type password
```
exit
```
retrying
```
ping -c 5 github.com
- Successfull
```
Update System
```
pacman -Sy
pacman -S archlinux-keyring
pacman -S archinstall
```
Disk Parting
```
lsblk
cfdisk /dev/nvme0n1
```
and opened disk parts and select my created free space
- Select New
- Partition Size : 1G 
- Select Created Partition
- Select Type
- Select EFI System
- Select Remaining Free Space
- Select New
- Partition Size : (Remaining Size, Usually auto dedected)
- Select Type (if Automaticly Selected Linux Filesystem dont touch)
- Select Linux Filesystem
- Select Write (in Linux Filesystem)
- Are you sure you want to write the partition table to disk? yes
- Select Quit

Check and formating
```
lsblk
mkfs.fat -F32 /dev/nvme0n1p4
("nvme0n1p5" anyway choose efi part)
mkfs.ext4 /dev/nvme0n1p5
("nvme0n1p6" anyway choose Linux Filesystem part)
```
```
lsblk
mount /dev/nvme0n1p5 /mnt
mkdir /mnt/boot
mount /dev/nvme0n1p4 /mnt/boot
```
and check
```
lsblk
```
if everythings is correct, type
```
archinstall
```
and opened install gui(almost)
- Archinstall Language : English (Press Enter to Change) -> Turkish
- Locales : + (if you want do change)
- Mirrors and repositories (skip)
- Disk configuration (Enter)
  - Partitioning
    - Pre-mounted configuration
      - Root mount directory : /mnt
  - Disk encryption (if you want do configurite)
- Swap : Enable
- Bootloder : Grub
- Unified kernel images (skip)
- Hostname (if you want do change)
- Authentication
  - Root Password : (your choose)
  - User Account
    - Add User
      - Username : (your choose)
      - Password : (your choose)
      - Sudo : Yes
      - Confirm and exit
- Profile
  - Type : (Select if you want, i prefer KDE Plasma or Gnome)
  - Graphic Driver : (Select if you want, i prefer all drivers)
  - Greeter : (skip)
- Applications 
  - Audio : Pipewire
- Network Configuration
  - Use Network Manager
- Additional Packages : (Select if you want, i prefer) nano git fastfetch htop
- Timezone (your choose)
- Automatic time sync (NTP) (optional)
- **Install**
- 
Installation Completed  
What would you like to do next  
- **chroot into installation for post-installation configurations**
And type
```
lsblk
pacman -S grub efibootmgr dosfstools mtools
lsblk
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
(if you want download apps)
pacman -S firefox zen-browser vlc libreoffice-fresh gwenview spectacle power-profiles-daemon gcc wger curl perl flatpak grub-customizer 
exit
```
```
umount -lR /mnt
shutdown now
```
Eject USB
Opened Omarchy check notifications and click Update System  
yeeeesssss its finish... no

- Select : Advanced options for Arch Linux
- Restart PC and press bios key
- Boot Options
  - OS Manager up the top and select GRUB 
- Open Arch Linux
 - Open Terminal
```
sudo nano /etc/default/grub
```
Change GRUB_TIMOUT=30
#GRUB_DISABLE_OS_PROBER=false
^ delete
CTRL + O | Enter | CTRL + X
```
sudo pacman -S os-prober
sudo grub-mkconfig -o /boot/grub/grub.cfg
sudo reboot now
```

## if you want "delete"
- Windows
  - Diskpart
    - 56GB Free Linuxsystem (i preferred 60GB)
      - `Right Click` and select `Delete Volume`
    - 4GB EFI (cannot be deleted)
      - `Windows + R` and type `cmd` and press `Enter` 
      ```
      diskpart
      ```
      ```
      DISKPART>list disk
      - disk 0 (Which disk installed Omarchy linux if choose it disk)
      DISKPART>select disk 0
      - Disk 0 is now the selected disk.
      DISKPART>list partition
      - Partition 4   System    4096 MB   ...
      (Be carefully deleting partition)
      DISKPART>select partition 4
      DISKPART>delete partition override
      ```
    - C:
      - `Right Click` and select `Extend Volume` and follow the instructions
- Successfully Deleted Arch and Dual boot
