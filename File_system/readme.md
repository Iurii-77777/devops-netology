# 1.
## **Использование Sparse File помогает оптимизировать объём свободного места в файловой системе. Правда есть недостатки, например нагрузка на вычислительную мощность компонентов системы при использовании таких файлов.**
# 2.
## **Создавая жесткую ссылку на файл, мы привязываемся к его индексному номеру (inode), получая тот же самый файл (с новым именем), на который указывает ссылка, но без физического создании копии.**
## **Соответственно файлы, являющиеся жесткой ссылкой на один объект, не могут иметь разные права доступа и владельца**.
# 3.
## **Делаем vagrant destroy и изменяем настройки vagrantfile.**
## **Проверяем результат:**
#### vagrant@vagrant:~$ lsblk
###### NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
###### sda                    8:0    0   64G  0 disk
###### ├─sda1                 8:1    0  512M  0 part /boot/efi
###### ├─sda2                 8:2    0    1K  0 part
###### └─sda5                 8:5    0 63.5G  0 part
######   ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
######   └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
###### sdb                    8:16   0  2.5G  0 disk
###### sdc                    8:32   0  2.5G  0 disk
# 4.
## **Проверяем диски (вывод команд привожу в сокращённом виде):**
#### vagrant@vagrant:~$ sudo -i
#### root@vagrant:~# fdisk -l
###### Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors
###### Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
###### Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
###### Disk /dev/mapper/vgvagrant-root: 62.55 GiB, 67150807040 bytes, 131153920 sectors
###### Disk /dev/mapper/vgvagrant-swap_1: 980 MiB, 1027604480 bytes, 2007040 sectors
#### root@vagrant:~# fdisk /dev/sdb
#### Command (m for help): m
#### Command (m for help): n
###### Partition type
######    p   primary (0 primary, 0 extended, 4 free)
######    e   extended (container for logical partitions)
###### Select (default p): p
###### Partition number (1-4, default 1): 1
###### First sector (2048-5242879, default 2048):
###### Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G
###### 
###### Created a new partition 1 of type 'Linux' and of size 2 GiB.
###### 
#### Command (m for help): n
###### Partition type
######    p   primary (1 primary, 0 extended, 3 free)
######    e   extended (container for logical partitions)
###### Select (default p): p
###### Partition number (2-4, default 2): 2
###### First sector (4196352-5242879, default 4196352):
###### Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):
###### 
###### Created a new partition 2 of type 'Linux' and of size 511 MiB.
###### 
#### Command (m for help): w
###### The partition table has been altered.
###### Calling ioctl() to re-read partition table.
###### Syncing disks.
#### root@vagrant:~# fdisk -l
###### Device     Boot   Start     End Sectors  Size Id Type
###### /dev/sdb1          2048 4196351 4194304    2G 83 Linux
###### /dev/sdb2       4196352 5242879 1046528  511M 83 Linux
# 5.
#### root@vagrant:~# sfdisk -d /dev/sdb|sfdisk --force /dev/sdc
###### Checking that no-one is using this disk right now ... OK
###### Device     Boot   Start     End Sectors  Size Id Type
###### /dev/sdc1          2048 4196351 4194304    2G 83 Linux
###### /dev/sdc2       4196352 5242879 1046528  511M 83 Linux
###### The partition table has been altered.
###### Calling ioctl() to re-read partition table.
###### Syncing disks.
# 6.
#### root@vagrant:~# mdadm --create --verbose /dev/md1 -l 1 -n 2 /dev/sd{b1,c1}
###### mdadm: Note: this array has metadata at the start and
######     may not be suitable as a boot device.  If you plan to
######     store '/boot' on this device please ensure that
######     your boot-loader understands md/v1.x metadata, or use
######     --metadata=0.90
###### mdadm: size set to 2094080K
###### Continue creating array? y
###### mdadm: Defaulting to version 1.2 metadata
###### mdadm: array /dev/md1 started.
# 7.
#### root@vagrant:~# mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b2,c2}
###### mdadm: Note: this array has metadata at the start and
######     may not be suitable as a boot device.  If you plan to
######     store '/boot' on this device please ensure that
######     your boot-loader understands md/v1.x metadata, or use
######     --metadata=0.90
###### mdadm: size set to 522240K
###### Continue creating array? y
###### mdadm: Defaulting to version 1.2 metadata
###### mdadm: array /dev/md0 started.
# 8.
#### root@vagrant:~# pvcreate /dev/md1 /dev/md0
######   Physical volume "/dev/md1" successfully created.
######   Physical volume "/dev/md0" successfully created.
# 9.
#### root@vagrant:~# vgcreate volgr_1 /dev/md1 /dev/md0
######   Volume group "volgr_1" successfully created
#### root@vagrant:~# vgdisplay
######   --- Volume group ---
######   VG Name               vgvagrant
######   System ID
######   Format                lvm2
######   Metadata Areas        1
######   Metadata Sequence No  3
######   VG Access             read/write
######   VG Status             resizable
######   MAX LV                0
######   Cur LV                2
######   Open LV               2
######   Max PV                0
######   Cur PV                1
######   Act PV                1
######   VG Size               <63.50 GiB
######   PE Size               4.00 MiB
######   Total PE              16255
######   Alloc PE / Size       16255 / <63.50 GiB
######   Free  PE / Size       0 / 0
######   VG UUID               PaBfZ0-3I0c-iIdl-uXKt-JL4K-f4tT-kzfcyE
###### 
######   --- Volume group ---
######   VG Name               volgr_1
######   System ID
######   Format                lvm2
######   Metadata Areas        2
######   Metadata Sequence No  1
######   VG Access             read/write
######   VG Status             resizable
######   MAX LV                0
######   Cur LV                0
######   Open LV               0
######   Max PV                0
######   Cur PV                2
######   Act PV                2
######   VG Size               2.49 GiB
######   PE Size               4.00 MiB
######   Total PE              638
######   Alloc PE / Size       0 / 0
######   Free  PE / Size       638 / 2.49 GiB
######   VG UUID               IfM0m3-VgfW-giYf-vobD-yrR8-kicp-45GIwK
# 10.
#### root@vagrant:~# lvcreate -L 100M volgr_1 /dev/md0
######   Logical volume "lvol0" created.
#### root@vagrant:~# vgs
######   VG        #PV #LV #SN Attr   VSize   VFree
######   vgvagrant   1   2   0 wz--n- <63.50g    0
######   volgr_1     2   1   0 wz--n-   2.49g 2.39g
#### root@vagrant:~# lvs
######   LV     VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
######   root   vgvagrant -wi-ao---- <62.54g
######   swap_1 vgvagrant -wi-ao---- 980.00m
######   lvol0  volgr_1   -wi-a----- 100.00m
# 11.
###### 
