# Volumes Logiques

[TOC]

### Types d'operations
 - Ajout/Retrait d'unités de disques
	- Maintenance avec utilisation temporaire de disques
 - Augmentation/Diminution de la capacité de stockage
	- Transferts entre volumes logiques sur un même système
 - Redimensionnement dynamique
	- Extension d'un système de fichiers en ligne
 - Déplacements de données entre unités de disque
	- Préparation à l'extraction d'unités de disque
 - Snapshots
	- Copie instantanée de l'état d'un volume logique
 - Réplication
	- Copie entre volumes logiques

### Solutions
- Logical Volume Manager (LVM)
- Système de fichier ZFS
- Système de fichier btrfs

## Logical Volume Manager (LVM)
 - Gestionnaire de périphérique mode bloc au niveau système
	- Partition ou tout type de périphérique de stockage
 - Vue système homogène
	- N Périphériques physiques vus comme un périphérique logique
 - Analogie entre volume et partition
	- Formatage et création d'un système de fichiers
 - Changements dynamiques de configuration
	- Snapshots, redimensionnement, extension, déplacements

## LVM Structure

- PV [Physical Volume]
	- VG [Volume Group]
		- LV [Logical Volume]
		- LV [Logical Volume]

![Various elements of the LVM - Wikipedia](https://upload.wikimedia.org/wikipedia/commons/e/e6/Lvm.svg)

## TP

## Commands
non-exhaustive list

|Physical Volume| |Volume Group  | |Logical Volume|
|:-------------:|-|:------------:|-|:------------:|
|`pvs`          | |`vgs`         | |`lvs`         |
|`pvdisplay`    | |`vgdisplay`   | |`lvdisplay`   |
|`pvcreate`     | |`vgcreate`    | |`lvcreate`    |
|`pvchange`     | |`vgchange`    | |`lvchange`    |
|`pvdata`       | |`vgrename`    | |`lvrename`    |
|`pvscan`       | |`vgscan`      | |`lvscan`      |
|`pvmove`       | |`vgsplit`     | |`lvresize`    |
|               | |`vgmerge`     | |`lvextend`    |
|               | |`vgremove`    | |`lvreduce`    |
|               | |`vgcfgbackup` | |`lvremove`    |
|               | |`vgcfgrestore`| |              |

## Physical Volume

### Création

```sh
pvcreate [-f[f]] [-y] <device_PV_name>
# Exemple:
pvcreate /dev/sda5 # Use partition
pvcreate /dev/sdb # Use whole disk
pvcreate /dev/md0 # Use Linux Software RAID
```
Option args for ```pvcreate```:

|ShortOpt| LongOpt | Description   |
|-------:|---------|---------------|
|`-f`    |`--force`| Force         |
|`-ff`   |         | Force Force ! |
|`-y`    |`--yes`  | Accept all    |
|`-q`    |`--quiet`| Quiet !       |


## Volume Group

### Création

```sh
vgcreate <volume_group_name> <device_PV_name>
# Exemple:
vgcreate VG1 /dev/sdb
```

### Add new Physical Volume to existing VG

```sh
vgextend <volume_group_name> <device_PV_name>
# Exemple:
vgextend VG0 /dev/sdb
```


## Logical Volume

### Création

```sh
lvcreate -n <logical_volume_name> -L <lv_size> <volume_group_name>
# Exemple:
lvcreate -n lv0 -L 256M VG0
```

### Extend

```sh
lvextend -l <logical_volume_name> -L <lv_size> <volume_group_name>
# Exemple:
lvcreate -n lv0 -L 256M VG0
```

### Common options for `lvcreate` and `lvresize`

lvcreate: create a logical volume in an existing volume group  
lvresize: resize a logical volume

`-n, --name LogicalVolume{Name|Path}`  
> ___Only for `lvcreate`___  
> Specifies the name of a new LV. Default when unspecified is "lvol#"

`-L, --size [+|-]LogicalVolumeSize[b|B|s|S|k|K|m|M|g|G|t|T|p|P|e|E]`  
> Set the logical volume size in units of megabytes. Size suffix of B for bytes, S for sectors as 512 bytes,
> K for kilobytes, M for megabytes, G for gigabytes, T for terabytes, P for petabytes or E for exabytes is optional.  
> Default unit is megabytes.

`-l, --extents [+|-]LogicalExtentsNumber[%{VG|PVS|FREE|ORIGIN}]`  
> Change or set the logical volume size in units of logical extents. With the + or - sign the value is added to or subtracted
> from the actual size of the logical volume and without it, the value is taken as an absolute one. The total number of physical
> extents affected will be greater than this if, for example, the volume is mirrored. The number can also be expressed as a
> percentage of the total space in the Volume Group with the suffix %VG, relative to the existing size of the Logical Volume with
> the suffix %LV, as a percentage of the remaining free space of the PhysicalVolumes on the command line with the suffix %PVS,
> as a percentage of the remaining free space in the Volume  Group with the suffix %FREE, or (for a snapshot) as a percentage
> of the total space in the Origin Logical Volume  with the suffix %ORIGIN. The resulting value is rounded downward for the
> subtraction otherwise it is rounded upward. N.B. In a future release, when expressed as a percentage with PVS, VG or FREE, the
> number will be treated as an approximate total number of physical extents to be allocated or freed (including extents used by
> any mirrors, for example). The code may currently allocate or remove more space than you might otherwise expect.

`-v, --verbose ...`  
> Set verbose level. Repeat from 1 to 4. Output is stdout and stderr.

`-d, --debug ...`  
> Set debug level. Repeat from 1 to 6. Output is log file / syslog.

`-t, --test`  
> Run in test mode.


## Create the usable volume

### Create the file system

```sh
mkfs -t ext4 /dev/<volume_group_name>/<logical_volume_name>
mkfs -t ext4 /dev/VG0/lv0
```

### Mount volume

```sh
mount <volume_path> <mount_point>
mount /dev/VG0/lv0 /mnt/lv0
```

## Hot resize

Hot resize of the file system of LV0 to 512M

Resize the logical volume:
```sh
lvresize -L 512M /dev/VG0/lv0
```

Resize the file system:
```sh
resize2fs -p /dev/VG0/lv0 512M
```

> resize2fs - ext2/ext3/ext4 file system resizer  
> `resize2fs [OPTION] device [ size ]`
> 
> `-f` Force  
> `-p` Prints percentage completion bars


Check the result:
```sh
df -h
```

> df - report file system disk space usage  
> `df [OPTION]... [FILE]...`
> 
> `-h`, `--human-readable`  
> print sizes in powers of 1024 (e.g., 1023M)
