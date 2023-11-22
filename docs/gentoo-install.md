# Gentoo install script

## Hello that is my gentoo installation

### index

1. Patitioning
2.
3.

---

# Partitioning

## Claning the disks

First u can zero overwrite entire disk with:

```bash
shred -vzf -n=2 /dev/sdX
```

or u only delete partition table or filesystem:

```bash
wipefs --force --all /dev/sdX
```

## advanced partitioning

```bash
parted -a optimal /dev/sdX

( mklabel gpt)
( unit mib )
( mkpart primary 1 513 )
( name 1 bios_grub )
( set 1 bios_grub on )
( mkpart [rimary 513 1537 )
( name 2 boot )
( set 2 BOOT on )
( mkpart primary 1537 -1 )
( name 3 gentoolvm )
( set 3 lvm on )
( p )
```

## formating filesystem

```bash
mkfs.vfat /dev/sdX1
mkfs.ext4 -T small -L "boot" /dev/sdX2
cryptsetup -v -y -c aes-xts -s 512 -h sha512 -i 5000 --use-random luksFormat /dev/sda3
cryptsetup luksDump /dev/sda3
cryptsetup luksOpen /dev/sda3 GentooPC
pvcreate /dev/mapper/GentooPC
pvdisplay
vgcreate gentoo /dev/mapper/GentooPC
vgdisplay
lvcreate -C y -L 8G gentoo -n swap
lvcreate -l +100%FREE gentoo -n root
lvdisplay
```
