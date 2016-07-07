[Back](https://github.com/songagi/study-linux/blob/master/README.md)

# Partition 확장 (Centos7)

========================================================

  * Partition 정보 확인
```
$ fdisk -l

 Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
 ...
   Device Boot      Start         End      Blocks   Id  System
 /dev/sda1   *        2048     1026047      512000   83  Linux
 /dev/sda2         1026048    62914559    30944256   8e  Linux LVM
...
```

* Mount된 용량 확인
```
$ df -h

Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   28G  2.8G   25G  11% /
devtmpfs                 918M     0  918M   0% /dev
tmpfs                    927M     0  927M   0% /dev/shm
tmpfs                    927M  8.5M  919M   1% /run
tmpfs                    927M     0  927M   0% /sys/fs/cgroup
/dev/sda1                497M  165M  332M  34% /boot
tmpfs                    186M     0  186M   0% /run/user/0
```

 (테스트 환경) 기존 30G --> 100G로 확장 (70G 공간 free 상태)


========================================================



 * fdisk 접속 (/dev/sda)
```
$ fdisk /dev/sda
```

 * 신규 Partition 추가
```
[신규 파티션 추가]
Command (m for help):

>> n 입력 (Add new Partition)
   
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
   
>> p 입력 (primary)

Partition number (1-4, default 3):

>> 3 입력 (default)

First sector (62914560-209715199, default 62914560):
>> Enter 입력 (default)

Last sector, +sectors or +size{K,M,G} (62914560-209715199, default 209715199):
>> Enter 입력 (default)

[파티션 확인]
Command (m for help):
>> p 입력

 ...
   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048    62914559    30944256   8e  Linux LVM
/dev/sda3        62914560   209715199    73400320   83  Linux

[Type 변경 - Linux LVM]

Command (m for help): 
>> t

Partition number (1-4): 
>> 3

Hex code (type L to list codes):
>> 8e 입력  (8e: Linux LVM)

Command (m for help):
> w 입력 (Write)
```

 * Partition 생성 확인
```
$ fdisk -l
```
```
...
   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048    62914559    30944256   8e  Linux LVM
/dev/sda3        62914560   209715199    73400320   8e  Linux LVM
...
```

 --> /dev/sda3 - Linux LVM 생성 확인


 * 시스템 Rebooting
```
$ shutdown -r now
```

========================================================

 * Phsical Volume 생성

 - 확인 전
```
$ pvscan

  PV /dev/sda2   VG centos   lvm2 [29.51 GiB / 44.00 MiB free]
  Total: 1 [29.51 GiB] / in use: 1 [29.51 GiB] / in no VG: 0 [0   ]
```

 - 생성
```
$ pvcreate /dev/sda3

  Physical volume "/dev/sda3" successfully created
```

 - 생성 후 확인 (PV /dev/sda3)
```
$ pvscan

  PV /dev/sda2   VG centos   lvm2 [29.51 GiB / 44.00 MiB free]
  PV /dev/sda3               lvm2 [70.00 GiB]
  Total: 2 [99.51 GiB] / in use: 1 [29.51 GiB] / in no VG: 1 [70.00 GiB]
```

 * VG Name 확인 ( VG Name = centos, New VG Name = 없음 )
```
$ pvdisplay

  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               29.51 GiB / not usable 3.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              7554
  Free PE               11
  Allocated PE          7543
  PV UUID               6GTNq2-FlAz-CbUv-yLbX-3Fm4-u3vR-zgP2KI
   
  "/dev/sda3" is a new physical volume of "70.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sda3
  VG Name               
  PV Size               70.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               UTmbVI-zMO2-BJlz-diXd-0b2A-nC4j-ABMaEg
```

 * Volume Group을 하나로 확장 (vgextend)

```
$ vgextend centos /dev/sda3

  Volume group "centos" successfully extended
```

 * vgdisplay (VG가 하나로 합쳐짐)

```
$ vgdisplay

  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               99.50 GiB
  PE Size               4.00 MiB
  Total PE              25473
  Alloc PE / Size       7543 / 29.46 GiB
  Free  PE / Size       17930 / 70.04 GiB
  VG UUID               4QECzt-30dd-WvkG-PFoM-U1Pa-SOQ6-vLpL3L
```
 
 --> 합쳐진 후, FREE PE 생김 (Free PE = 17930)
 
 
 * Logical 파티션 확장

```
$ lvextend /dev/centos/root -l +17930

  Size of logical volume centos/root changed from 27.46 GiB (7031 extents) to 70.04 GiB (17930 extents).
  Logical volume root successfully resized.
```

```
$ vgdisplay

  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               99.50 GiB
  PE Size               4.00 MiB
  Total PE              25473
  Alloc PE / Size       25473 / 99.50 GiB
  Free  PE / Size       0 / 0   
  VG UUID               4QECzt-30dd-WvkG-PFoM-U1Pa-SOQ6-vLpL3L
```

 --> Alloc 공간이 늘어나고, Free 공간이 0으로 바뀜.
 
 
========================================================
 
 * 파일 시스템 확장 반영 (xfs_growfs 또는 resize2fs)

```
$ xfs_growfs /dev/centos/root
```
또는
```
$ resize2fs /dev/centos/root
```

[Back](https://github.com/songagi/study-linux/blob/master/README.md)
