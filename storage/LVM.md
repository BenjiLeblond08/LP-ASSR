# Volumes Logiques

[TOC]

## Cours - Gestion de volume

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

### Logical Volume Manager (LVM)
 - Gestionnaire de périphérique mode bloc au niveau système
	- Partition ou tout type de périphérique de stockage
 - Vue système homogène
	- N Périphériques physiques vus comme un périphérique logique
 - Analogie entre volume et partition
	- Formatage et création d'un système de fichiers
 - Changements dynamiques de configuration
	- Snapshots, redimensionnement, extension, déplacements

### LVM Structure

- PV [Physical Volume]
	- VG [Volume Group]
		- LV [Logical Volume]
		- LV [Logical Volume]

![Various elements of the LVM - Wikipedia](https://upload.wikimedia.org/wikipedia/commons/e/e6/Lvm.svg)

## TP

### Commands
non-exhaustive list

|Physical Volume| |  Volume Group    | |Logical Volume |
|:-------------:|-|:----------------:|-|:-------------:|
|```pvcreate``` | |```vgcreate```    | |```lvcreate``` |
|```pvchange``` | |```vgchange```    | |```lvchange``` |
|```pvdata```   | |```vgrename```    | |```lvrename``` |
|```pvdisplay```| |```vgdisplay```   | |```lvdisplay```|
|```pvscan```   | |```vgscan```      | |```lvscan```   |
|```pvmove```   | |```vgsplit```     | |```lvresize``` |
|               | |```vgmerge```     | |```lvextend``` |
|               | |```vgremove```    | |```lvremove``` |
|               | |```vgcfgbackup``` | |```lvremove``` |
|               | |```vgcfgrestore```| |               |

### Physical Volume

#### Création

```sh
pvcreate [-f[f]] [-y] <device>
# Exemple:
pvcreate /dev/sdb
# If use over MD
pvcreate /dev/md0
```
Option args for ```pvcreate```:

|ShortOpt | LongOpt     | Description   |
|--------:|-------------|---------------|
|```-f``` |```--force```| Force         |
|```-ff```|             | Force Force ! |
|```-y``` |```--yes```  | Accept all    |
|```-q``` |```--quiet```| Quiet !       |


### Volume Group

#### Création

```sh
vgcreate <volume_group_name> <device>
# Exemple:
vgcreate vg0 /dev/md0
```

### Logical Volume

#### Création

```sh
lvcreate -n <logical_volume_name> -L <lv_size> <volume_q_name>
# Exemple:
lvcreate -n lv0 -L 256M vg0
```
> lvcreate - Create a logical volume  
```sh
lvcreate -L, --size Size[m|UNIT] VG
    [ -l, --extents Number[PERCENT] ]
    [ PV ... ]
```  
> 
> ```-L, --size Size[m|UNIT]```  
> Specifies the size of the new LV.
> 
> ```-l, --extents Number[PERCENT]```  
> Specifies the size of the new LV in logical extents.
> 
> ```-n, --name String```  
> Specifies the name of a new LV. Default when unspecified is "lvol#"
> 
> ```-v, --verbose ...```  
> Set verbose level. Repeat from 1 to 4. Output is stdout and stderr.
> 
> ```-d, --debug ...```  
> Set debug level. Repeat from 1 to 6. Output is log file / syslog.
> 
> ```-t, --test```  
> Run in test mode.
> 
> ```-q, --quiet ...```  
> Suppress output and log. Overrides ```--debug``` and ```--verbose```.  
>  Repeat once suppress any prompts with answer 'no'.

### Create the usable volume

#### Create the file system

```sh
mkfs -t ext4 /dev/<volume_group_name>/<logical_volume_name>
mkfs -t ext4 /dev/vg0/lv0
```

#### Mount volume

```sh
mount <volume_path> <mount_point>
mount /dev/vg0/lv0 /mnt/lv0
```

### Hot resize

Hot resize of the file system of LV0 to 512M

Resize the logical volume:
```sh
lvresize -L 512M /dev/vg0/lv0
```

Resize the file system:
```sh
resize2fs -p /dev/vg0/lv0 512M
```

> resize2fs - ext2/ext3/ext4 file system resizer  
> ```resize2fs [OPTION] device [ size ]```
> 
> ```-f``` Force  
> ```-p``` Prints percentage completion bars


Check the result:
```sh
df -h
```

> df - report file system disk space usage  
> ```df [OPTION]... [FILE]...```
> 
> ```-h```, ```--human-readable```  
> print sizes in powers of 1024 (e.g., 1023M)
