# Linux Software RAID

[TOC]

## Introduction

mdadm - manage MD devices aka Linux Software RAID
md: Multiple Device driver aka Linux Software RAID

## Create array

Check available drives  
```sh
fdisl -l
```

```sh
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

```

## MD Management

### Show detail about MD

```sh
mdadm --detail /dev/md0
# OR
mdadm -D /dev/md0
```

### Add SPARE Disk

```sh
mdadm --manage /dev/md0 --add /dev/sdd
```

### Set disk as faulty

```sh
mdadm --fail /dev/md0 /dev/sdc
```

### Change a disk

```sh
mdadm --manage --remove /dev/md0 /dev/sdc
# Change the faulty disk PHYSICALY
mdadm --manage --add /dev/md0 /dev/sdc
```

## Set current configuration as persistant

```sh
mdadm --detail --scan >> /etc/mdadm/mdadm.conf
```

## Emergency case

Boot from a live CD (System Rescue CD, Hiren's Boot CD, ...)

```sh
mdadm --examine -scan >> /etc/mdadm/mdadm.conf
service mdadm-raid restart
mount /dev/mdx /mnt/raidx
```

