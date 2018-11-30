# Volumes Logiques

[TOC]

## Introduction

### Types d'operations
- Ajout/Retrai d'unités de disque
- Augemntation/Diminution capacité
- Redimensionnement dynamique
- Déplacement de données entr unités de disque

### Solutions
- Logical Volume Manager (LVM)
- Système de fichier ZFS
- Système de fichier btrfs

### Logical Volume Manager (LVM)
Gestionnaire de peripherique mode bloc eu niveau ssys
vu sys homogene
analogie entre volume et partition

### LVM Structure

- PV [Physical Volume]
	- VG [Volume Groupe]
		- LV [Logical Volume]
		- LV [Logical Volume]

## Physical Volume

### Création

```SH
pvcreate [-f[f]] [-y] /dev/sdb
```
### Voire aussi

|               |               |
|---------------|---------------|
|```pvchange``` ||
|```pvdata```   ||
|```pvdisplay```||
|```pvscan```   ||
|```pvmove```   ||

## Volume Groupe

### Création



## Logical Volume

### Création

