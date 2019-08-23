# Linux Tips

[TOC]

## Reset Root Password

Edit GRUB Menu : press `e` on first item

On line starting by `linux /boot/...`, edit to include read-write mode `rw` and `init=/bin/bash`

Example:

```
linux     /boot/vmlinuz-4-4.0-22-generic root=UUID=43ad24d3-e\
c5b-44ee-a099-a88eb9520989 ro
```

CHANGE TO:

```
linux     /boot/vmlinuz-4-4.0-22-generic root=UUID=43ad24d3-e\
c5b-44ee-a099-a88eb9520989 rw init=/bin/bash
```

Check `/` is mounted in read/write

```
root@(none):/# mount | grep -w /
```

Reset password with `passwd`

