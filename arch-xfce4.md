# My Build for Arch Linux + XFCE4

This is current as of 2019-12-24

## Prep

I am targeting two different laptops: A thinkpad e530 and a thinkpad twist s230u. Each has their own special quirks. Flash drive prepped with dd.

## Base Install Steps

On bootup, hit Enter to get to boot menu, then F12 to choose startup device.

**For Thinkpad Twist s230u**, once in the bootloader, hit 'e' and add 'i8042.reset=1' to the end of the kernel params. This prevents the keyboard and trackpad from freezing randomly. (will provide links/docs soon)

`wifi-menu` to get wifi connected (skip if wired)

`ip a` to check for an ip

`ping archlinux.org`

`passwd` temp password so we can ssh

`systemctl start sshd`

`ssh root@192.168.?.?` obviously put in your IP

`timedatectl set-ntp true`

`dd if=/dev/zero of=/dev/sdb bs-4096 count=1000` I wipe out the front of the drive I plan to use as all install issues I have seem to go away with a completely *blank* drive. **NOTE** I am installing on /dev/sdb, please adjust accordingly!

`cfdisk /dev/sdb`, select `gpt`

Select **New**, size is **300M**, Enter. Select **Type**, select **EFI System**, Enter. Move to `Free space`, select **New**, use up the rest of the space, Enter. Don't forget to **Write** :)

> I skip swap here and will add it later on as a file as opposed to a separate partition.

`mkfs.fat -F32 /dev/sdb1`

`mkfs.ext4 /dev/sdb2`

`mount /dev/sdb2 /mnt`

`mkdir -p /mnt/boot/efi`

`mount /dev/sdb1 /mnt/boot/efi`

At this point I'll often look at `lsblk` just to make sure I got things right.

`nano /etc/pacman.d/mirrorlist`, and go find a good mirror. Copy line with `Alt+6`, up-arrow or PgUp to the top, then `Ctrl+U` to paste. Quick exit (for me) is `Ctrl+X`, `Y`, and `Enter`.

`pacstrap /mnt base base-devel linux linux-firmware vim git`

* Base is required
* linux is required
* base-devel tells the system to download all build tools
* linux-firmware pulls all device firmware
* Vim and git are other tools that I go to right away.

`genfstab -U /mnt` I check this before writing it with `genfstab -U /mnt >> /mnt/etc/fstab`

`arch-chroot /mnt`

Set timezone with `ln -sf /usr/share/zoneinfo/XXX/YYY /etc/localtime` replacing XXX and YYY with appropriate location information.

`hwclock --systohc`

Edit `/etc/locale.gen` for the correct locale, then run `locale-gen`, then create the locale.conf file with `echo LANG=en_US.UTF-8 >> /etc/locale.conf` (change to match your locale)

create hostname in `/etc/hostname`

edit `/etc/mkinitcpio.conf` to add `i915` as a module for intel gfx.

run `mkinitcpio -p linux`

set root password with `passwd`

