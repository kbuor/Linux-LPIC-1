# LINUX LVM MANAGEMENT
## I. CREATE `NEW MOUNT POINT` FROM `NEW LOGICAL VOLUME` FROM `NEW VOLUME GROUP` FROM `NEW PHYSICAL VOLUME` FROM `NEW PARTITION` FROM `NEW PHYSICAL DISK`

### 1. Check current physical disk:

```console
fdisk -l
```
   
```shell
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: AD4DACFB-2E03-4B35-8AF7-1A5A27DCB856

Device       Start       End   Sectors  Size Type
/dev/sda1     2048   1230847   1228800  600M EFI System
/dev/sda2  1230848   3327999   2097152    1G Linux filesystem
/dev/sda3  3328000 104855551 101527552 48.4G Linux LVM


Disk /dev/mapper/rl-root: 44.47 GiB, 47752151040 bytes, 93265920 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rl-swap: 3.94 GiB, 4227858432 bytes, 8257536 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
```console
lsblk
```
```shell
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   50G  0 disk 
├─sda1        8:1    0  600M  0 part /boot/efi
├─sda2        8:2    0    1G  0 part /boot
└─sda3        8:3    0 48.4G  0 part 
  ├─rl-root 253:0    0 44.5G  0 lvm  /
  └─rl-swap 253:1    0  3.9G  0 lvm  [SWAP]
sr0          11:0    1 1024M  0 rom
```
2. Add new /dev/sdb hard disk
```console
fdisk -l
```
```shell
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: AD4DACFB-2E03-4B35-8AF7-1A5A27DCB856

Device       Start       End   Sectors  Size Type
/dev/sda1     2048   1230847   1228800  600M EFI System
/dev/sda2  1230848   3327999   2097152    1G Linux filesystem
/dev/sda3  3328000 104855551 101527552 48.4G Linux LVM


Disk /dev/mapper/rl-root: 44.47 GiB, 47752151040 bytes, 93265920 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rl-swap: 3.94 GiB, 4227858432 bytes, 8257536 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
```console
lsblk
```
```shell
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   50G  0 disk 
├─sda1        8:1    0  600M  0 part /boot/efi
├─sda2        8:2    0    1G  0 part /boot
└─sda3        8:3    0 48.4G  0 part 
  ├─rl-root 253:0    0 44.5G  0 lvm  /
  └─rl-swap 253:1    0  3.9G  0 lvm  [SWAP]
sdb           8:16   0   20G  0 disk 
sr0          11:0    1 1024M  0 rom
```
3. Create new LVM partition
```console
fdisk /dev/sdb
```
```shell
Welcome to fdisk (util-linux 2.37.4).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x6b42e800.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-41943039, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-41943039, default 41943039): 

Created a new partition 1 of type 'Linux' and of size 20 GiB.

Command (m for help): t
Selected partition 1
Hex code or alias (type L to list all): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
```console
fdisk -l
```
```shell
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: AD4DACFB-2E03-4B35-8AF7-1A5A27DCB856

Device       Start       End   Sectors  Size Type
/dev/sda1     2048   1230847   1228800  600M EFI System
/dev/sda2  1230848   3327999   2097152    1G Linux filesystem
/dev/sda3  3328000 104855551 101527552 48.4G Linux LVM


Disk /dev/mapper/rl-root: 44.47 GiB, 47752151040 bytes, 93265920 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rl-swap: 3.94 GiB, 4227858432 bytes, 8257536 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x6b42e800

Device     Boot Start      End  Sectors Size Id Type
/dev/sdb1        2048 41943039 41940992  20G 8e Linux LVM
```
```console
lsblk
```
```shell
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   50G  0 disk 
├─sda1        8:1    0  600M  0 part /boot/efi
├─sda2        8:2    0    1G  0 part /boot
└─sda3        8:3    0 48.4G  0 part 
  ├─rl-root 253:0    0 44.5G  0 lvm  /
  └─rl-swap 253:1    0  3.9G  0 lvm  [SWAP]
sdb           8:16   0   20G  0 disk 
└─sdb1        8:17   0   20G  0 part 
sr0          11:0    1 1024M  0 rom
```
4. Create new *physical volume*
```console
pvdisplay
```
```shell
--- Physical volume ---
  PV Name               /dev/sda3
  VG Name               rl
  PV Size               48.41 GiB / not usable 2.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              12393
  Free PE               0
  Allocated PE          12393
  PV UUID               FeJgxJ-UAcw-qHch-lVUA-miKt-pUNx-OOIXvi
```
```console
pvs
```
```shell
  PV         VG Fmt  Attr PSize  PFree
  /dev/sda3  rl lvm2 a--  48.41g    0
```
```console
pvcreate /dev/sdb1
```
```shell
  Physical volume "/dev/sdb1" successfully created.
```
```console
pvdisplay
```
```shell
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               rl
  PV Size               48.41 GiB / not usable 2.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              12393
  Free PE               0
  Allocated PE          12393
  PV UUID               FeJgxJ-UAcw-qHch-lVUA-miKt-pUNx-OOIXvi
   
  "/dev/sdb1" is a new physical volume of "<20.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name               
  PV Size               <20.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               kltrUf-uG2l-1jzg-jmEa-Bz67-4e7h-xXNE0q
```
```console
pvs
```
```shell
  PV         VG Fmt  Attr PSize   PFree  
  /dev/sda3  rl lvm2 a--   48.41g      0 
  /dev/sdb1     lvm2 ---  <20.00g <20.00g
```

5. Create new *volume group*
```console
vgdisplay
```
```shell
  --- Volume group ---
  VG Name               rl
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               48.41 GiB
  PE Size               4.00 MiB
  Total PE              12393
  Alloc PE / Size       12393 / 48.41 GiB
  Free  PE / Size       0 / 0   
  VG UUID               d9GCGF-80hN-iXOd-MQY1-cUcP-j171-pFZd3a
```
```console
vgs
```
```shell
  VG #PV #LV #SN Attr   VSize  VFree
  rl   1   2   0 wz--n- 48.41g    0
```
```console
vgcreate kbuor /dev/sdb1
```
```shell
  Volume group "kbuor" successfully created
```
```console
vgdisplay
```
```shell
  --- Volume group ---
  VG Name               kbuor
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <20.00 GiB
  PE Size               4.00 MiB
  Total PE              5119
  Alloc PE / Size       0 / 0   
  Free  PE / Size       5119 / <20.00 GiB
  VG UUID               ZgaqFh-ow1j-n2gj-H5di-GtcX-WaXI-QtPwxa
   
  --- Volume group ---
  VG Name               rl
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               48.41 GiB
  PE Size               4.00 MiB
  Total PE              12393
  Alloc PE / Size       12393 / 48.41 GiB
  Free  PE / Size       0 / 0   
  VG UUID               d9GCGF-80hN-iXOd-MQY1-cUcP-j171-pFZd3a
```
```console
vgs
```
```shell
  VG    #PV #LV #SN Attr   VSize   VFree  
  kbuor   1   0   0 wz--n- <20.00g <20.00g
  rl      1   2   0 wz--n-  48.41g      0
```

6. Create new *Logical Volume*
```console
lvdisplay
```
```shell
  --- Logical volume ---
  LV Path                /dev/rl/swap
  LV Name                swap
  VG Name                rl
  LV UUID                7Guo8Y-gHIL-1JyK-Wz7f-c2hP-Q5yi-ZbrLzd
  LV Write Access        read/write
  LV Creation host, time web.kbuor.tech, 2024-01-06 13:44:20 +0700
  LV Status              available
  # open                 2
  LV Size                <3.94 GiB
  Current LE             1008
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
   
  --- Logical volume ---
  LV Path                /dev/rl/root
  LV Name                root
  VG Name                rl
  LV UUID                sxA2oL-fYzf-W17T-4E6M-Oxx4-fSX1-Qq6GL0
  LV Write Access        read/write
  LV Creation host, time web.kbuor.tech, 2024-01-06 13:44:20 +0700
  LV Status              available
  # open                 1
  LV Size                44.47 GiB
  Current LE             11385
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```
```console
lvs
```
```shell
  LV   VG Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root rl -wi-ao---- 44.47g                                                    
  swap rl -wi-ao---- <3.94g
```
```console
lvcreate -n lv_kbuor -l 100%FREE kbuor
```
```shell
  Logical volume "lv_kbuor" created.
```
```console
lvdisplay
```
```shell
  --- Logical volume ---
  LV Path                /dev/kbuor/lv_kbuor
  LV Name                lv_kbuor
  VG Name                kbuor
  LV UUID                B6VBRi-4rYv-kVOT-p3V2-XevD-JurB-rcpyOi
  LV Write Access        read/write
  LV Creation host, time web.kbuor.tech, 2024-01-06 22:02:17 +0700
  LV Status              available
  # open                 0
  LV Size                <20.00 GiB
  Current LE             5119
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:2
   
  --- Logical volume ---
  LV Path                /dev/rl/swap
  LV Name                swap
  VG Name                rl
  LV UUID                7Guo8Y-gHIL-1JyK-Wz7f-c2hP-Q5yi-ZbrLzd
  LV Write Access        read/write
  LV Creation host, time web.kbuor.tech, 2024-01-06 13:44:20 +0700
  LV Status              available
  # open                 2
  LV Size                <3.94 GiB
  Current LE             1008
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
   
  --- Logical volume ---
  LV Path                /dev/rl/root
  LV Name                root
  VG Name                rl
  LV UUID                sxA2oL-fYzf-W17T-4E6M-Oxx4-fSX1-Qq6GL0
  LV Write Access        read/write
  LV Creation host, time web.kbuor.tech, 2024-01-06 13:44:20 +0700
  LV Status              available
  # open                 1
  LV Size                44.47 GiB
  Current LE             11385
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```
```console
lvs
```
```shell
  LV       VG    Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_kbuor kbuor -wi-a----- <20.00g                                                    
  root     rl    -wi-ao----  44.47g                                                    
  swap     rl    -wi-ao----  <3.94g
```

7. Format New Logical Volume
```console
mkfs -t ext4 /dev/kbuor/lv_kbuor
```
```shell
mke2fs 1.46.5 (30-Dec-2021)
Discarding device blocks: done                            
Creating filesystem with 5241856 4k blocks and 1310720 inodes
Filesystem UUID: 02f93a79-835f-438e-be42-7ec0cff44e75
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

8. Mount New Logical Volume to use
```console
mkdir -p /kbuor
```
```console
vi /etc/fstab
```
```shell
#
# /etc/fstab
# Created by anaconda on Sat Jan  6 06:44:25 2024
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/rl-root     /                       xfs     defaults        0 0
UUID=5348ec08-97bd-420a-bad7-78e1636866ea /boot                   xfs     defaults        0 0
UUID=6C5A-94D9          /boot/efi               vfat    umask=0077,shortname=winnt 0 2
/dev/mapper/rl-swap     none                    swap    defaults        0 0
/dev/kbuor/lv_kbuor     /kbuor                  ext4    defaults        0 0
```
```console
mount -a
```
```shell
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```
```console
systemctl daemon-reload
```
```console
df -hT
```
```shell
Filesystem                 Type      Size  Used Avail Use% Mounted on
devtmpfs                   devtmpfs  4.0M     0  4.0M   0% /dev
tmpfs                      tmpfs     1.8G     0  1.8G   0% /dev/shm
tmpfs                      tmpfs     730M  8.6M  721M   2% /run
/dev/mapper/rl-root        xfs        45G  2.3G   43G   6% /
/dev/sda2                  xfs       960M  269M  692M  28% /boot
/dev/sda1                  vfat      599M  7.0M  592M   2% /boot/efi
tmpfs                      tmpfs     365M     0  365M   0% /run/user/0
/dev/mapper/kbuor-lv_kbuor ext4       20G   24K   19G   1% /kbuor
```
