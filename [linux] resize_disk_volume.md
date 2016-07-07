# (작업 중) Disk partition volume 확장 (Centos7)

  * Disk 및 partition 정보 확인
```
fdisk -l
```

  * 1. fdisk - Target Disk 선택
```
fdisk /dev/sda2
```
```
Command (m for help):
> n 입력 (Add new Partition)
   
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
   
> p 입력 (primary)

Partition number (1-4, default 1):

> Enter 입력 (default)

First sector (2048-1023999, default 2048):
> Enter 입력 (default)

Last sector, +sectors or +size{K,M,G} (2048-1023999, default 1023999):
> Enter 입력 (default)

Command (m for help):
> w 입력 (Write table)


```

