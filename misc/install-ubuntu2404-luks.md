# Install Ubuntu 24.04 with LUKS partition 

## Why?

They insist using flutter installer without enough testing, which ends up with this shit!

https://bugs.launchpad.net/ubuntu-desktop-provision/+bug/2060678

### 0. Disk partition

| Partation | Mount point | Size | File System | Desciption |
| --- | --- | --- | --- |
| /sda1 | /boot/efi | 128M | FAT32 | UEFI files, 128M is enough |
| /sda2 | /boot| 1024M | EXT4 | Kernel images |
| /sda3 | / | >= 20GB | EXT4 | System root |

### 1. Install ubuntu as usual

### 2. install cryptsetup on the new system

```bash
apt install cryptsetup
```

### 3. Start system with liveCD/USB

### 4. Backup the root directory to another disk/partition

```bash
mount /dev/sda3 /data/root
mount /dev/sdx1 /data/backup

rsync -axHAWXS --numeric-ids --info=progress2 /data/root /data/backup

umount /data/root
```

### 5. Format the original root partition into LUKS encrypted device

```bash
cryptsetup luksFormat /dev/sda3
cryptsetup luksOpen /dev/sda3 sda3_crypt 

mkfs.ext4 /dev/mapper/sda3_crypt 
```

### 6. Restore backup files to the newly encrypted partition 

```bash
mount /dev/mapper/sda3_crypt /data/root

rsync -axHAWXS --numeric-ids --info=progress2 /data/backup/ /data/root
```

### 7. Update `/etc/crypttab`

```bash
# get the UUID of the LUKS root partition
blkid | grep /dev/sda3

# update the file 
vim /data/root/etc/crypttab
```

`/etc/crypttab`

```
sda3_crypt UUID=(LUKS physical partition UUID) none luks,discard
```

### 8. Update `/etc/fstab`

```bash
vim /data/root/etc/fstab
```

`/etc/fstab`
```
/dev/mapper/sda3_crypt /               ext4    errors=remount-ro 0       1
```

### 9. Update /etc/default/cryptdisks

```bash
vim /data/root/etc/default/cryptdisks
```

```
CRYPTDISKS_ENABLE=Yes
CRYPTDISKS_MOUNT=""
CRYPTDISKS_CHECK=blkid
```



### 10. Create chroot and update the boot images 

Mount /boot, /boot/efi, /proc, /sys, /dev, /dev/pts, /run

```bash
mount /dev/sda2 ./root/boot
mount /dev/sda1 ./root/boot/efi
mount -t proc none /data/root/proc
mount -t sysfs none /data/root/sys
mount --bind /dev /data/root/dev
mount --bind /dev/pts /data/root/dev/pts
mount --bind /run /data/root/run
```

chroot and update the images

```bash
chroot /data/root/

update-initramfs -u -k all
update-grub
```

