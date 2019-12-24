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
