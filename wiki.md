# Programming
## C#
### Data Types
- byte: 0 - 255
- short = int16: -32.768 - 32.767
- int = int32: -2.1B - 2.1B
- long = int64:
- float = single/float: e.g.: 1.2f
- double = long float:
- decimal = long long float: e.g.: 1.2m
- char: unicode
- bool: Boolean

### loops

### if



# Windows
## Visual Studio
- it's HUGE! > 16GB
- can do C#, F#, Python and C++
# Linux
## Ansible
## git
## NixOS
### Installation
1. Partition and format disk
2. mount root partition under `/mnt` and boot under `/mnt/boot`
3. run `nixos-generate-config --root /mnt`
4. edit `/mnt/etc/nixos/configuration.nix`
5. run `nixos-install`

#### Install script
```bash
parted /dev/sdX -- mklabel gpt



```
### Caveats of root on ZFS
- zfs support is built in but for grub to read from zfs the `boot.supportedFilesystems = ["zfs"];` option needs to be set
- zfs on bios and a 4k sector drive will not work
- grub is a bitch
- a `networking.hostId = "";` needs to be set
- users need to be in the zfs group