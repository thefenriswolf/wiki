# Windows
## Visual Studio
- it's HUGE! > 16GB
- can do C#, F#, Python and C++
# Linux
## Ansible
## git
## ZFS
### recyle drives from a former zfs array
ZFS puts it's metadata in the first and last sectors of a drive. A drive with this metadata will show up as zfs_member for other utilities. The official way to wipe all traces of zfs from a drive is:

``` bash
zpool import
zpool destroy "POOL"
zpool labelclear -f /dev/sdX
```
This often fails to actually wipe all traces of zfs_member from the drive, insteal use `dd` to overwrite all metadata:

```bash
dd if=/dev/zero of=/dev/sdXX bs=512 count=10
dd if=/dev/zero of=/dev/sdXX bs=512 seek=$(( $(blockdev --getsz /dev/sdXX) - 4096 )) count=1M
```

## NixOS
### Installation
1. Partition and format disk
2. mount root partition under `/mnt` and boot under `/mnt/boot`
3. run `nixos-generate-config --root /mnt`
4. edit `/mnt/etc/nixos/configuration.nix`
5. run `nixos-install`

#### Install script (zfs)
```bash
DISK=/dev/disk/by-id/ata-XXX
# Partition 2 boot partition, legacy (BIOS) boot
sgdisk -a1 -n2:34:2047 -t2:EF02 $DISK
# EFI partition:
sgdisk -n3:1M:+512M -t3:EF00 $DISK
# Partition 1 main ZFS partition, using up the remaining space
sgdisk -n1:0:0 -t1:BF01 $DISK

zpool create \
 -O atime=off \
 -O compression=lz4 \
 -O xattr=sa \
 -O acltype=posixacl \
 -O mountpoint=none -R /mnt rpool $DISK-partX

zfs create -o mountpoint=none rpool/root
zfs create -o mountpoint=legacy rpool/root/nixos
zfs create -o mountpoint=legacy rpool/home

mount -t zfs rpool/root/nixos /mnt
mkdir /mnt/home
mount -t zfs rpool/home /mnt/home
```
### Caveats of root on ZFS
- zfs support is built in but for grub to read from zfs the `boot.supportedFilesystems = ["zfs"];` option needs to be set
- zfs on bios and a 4k sector drive will not work
- grub is a bitch
- a `networking.hostId = "";` needs to be set
- users need to be in the zfs group