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
