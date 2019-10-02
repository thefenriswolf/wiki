# Windows
# Linux
## NixOS
### Installation
1. Partition and format disk
2. mount root partition under `/mnt` and boot under `/mnt/boot`
3. run `nixos-generate-config --root /mnt`
4. edit `/mnt/etc/nixos/configuration.nix`
5. run `nixos-install`
### caveats
- zfs support is built in but for grub to read from zfs the `boot.supportedFilesystems = [""];` option needs to be set
- zfs on bios and a 4k sector drive will not work
- grub is a bitch
- a `networking.hostId = "";` needs to be set
- users need to be in the zfs group