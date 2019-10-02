# Windows
# Linux
## NixOS
### Installation
1. Partition and format disk
2. mount root partition under `/mnt` and boot under `/mnt/boot`
3. run `nixos-generate-config --root /mnt`
4. edit `/mnt/etc/nixos/configuration.nix`
5. run `nixos-install`