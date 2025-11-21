# How do you find the top 10 largest files in a directory?
## Answer: find /path/to/dir -type f -exec du -h {} + | sort -rh | head -n 10
---
# find /boot -type f -exec du -h {} + | sort -rh | head -n 10
## output
 ###  . 72M     /boot/initramfs-0-rescue-f47ea5a99ddd4cef9d9a42dabfd28e74.img
###  . 31M     /boot/initramfs-4.18.0-553.el8_10.x86_64.img
###  . 27M     /boot/initramfs-4.18.0-553.el8_10.x86_64kdump.img
###  . 11M     /boot/vmlinuz-4.18.0-553.el8_10.x86_64
###  . 11M     /boot/vmlinuz-0-rescue-f47ea5a99ddd4cef9d9a42dabfd28e74
###  . 4.3M    /boot/System.map-4.18.0-553.el8_10.x86_64
###  . 2.5M    /boot/grub2/fonts/unicode.pf2
###  . 200K    /boot/config-4.18.0-553.el8_10.x86_64
###  . 156K    /boot/grub2/i386-pc/normal.mod
###  . 100K    /boot/grub2/i386-pc/net.mod


##

