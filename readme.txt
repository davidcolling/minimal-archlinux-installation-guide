Minimal ArchLinux Installation Guide
"I just want to be p u r e."

The installation detailed is useful for troubleshooting installation on a unfamiliar system or as a template for a more application-specific installation. Once installed, the system will have a configured network, be persistent, and contain vim. 

This guide assumes the user understands each step of the procedure. This purpose of this guide is to be faster than skimming the whole Archlinux Installation Guide for those who have read it many times and who are installing on commonly-available systems.

This for sure works on
    VirtualBox on macOS,
    Hetzner Cloud.

Some choices made:
    MBR is used, not EFI.
    The bootloader is GRUB.
    The network manager is NetworkManager.
    The filesystem is ext4.

Procedure:

    Boot into the archlinux installation medium.
    $ timedatectl set-ntp true
    $ fdisk /dev/sda
        interactive commands:
            M : Make sure fdisk is in MBR mode. If the command isn't found, it already is in MBR mode.
            d : Do this until all there are no partitions.
            n : Make a new partition
              : Keep pressing enter to choose the default options: primary partition, type Linux, the whole disk is a root partition.
            w : Write the changes.
    $ mkfs.ext4 /dev/sda1
    $ mount /dev/sda1 /mnt
    $ pacstrap /mnt base linux linux-firmware vim resolvconf networkmanager grub
    $ genfstab -U /mnt >> /mnt/etc/fstab
    $ arch-chroot /mnt
    $ grub-install --target=i386-pc /dev/sda
    $ grub-mkconfig -o /boot/grub/grub.cfg
    
    $ echo "en_US ISO-8859-1
    en_US.UTF-8 UTF-8" > /etc/locale.gen
    
    $ locale-gen
    $ echo "LANG=en-US.UTF-8" > /etc/locale.conf 
    $ echo "<hostname>" > /etc/hostname
    $ systemctl enable NetworkManager
    $ passwd
    $ exit
    $ shutdown now
    Remove the installation medium now.
