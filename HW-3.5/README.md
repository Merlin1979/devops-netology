# Домашняя работа к занятию "3.5. Файловые системы"

1. **Sparse file** (раазрежённый файл) — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях (список дыр).  
   **Дыра** (англ. hole) — последовательность нулевых байт внутри файла, не записанная на диск. Информация о дырах (смещение от начала файла в байтах и количество байт) хранится в метаданных ФС.  
   Разреженные файлы поддерживаются во всех современных файловых системах. 
   Преимущества:
   - экономия дискового пространства. Использование разрежённых файлов считается одним из способов сжатия данных на уровне файловой системы;
   - отсутствие временных затрат на запись нулевых байт;
   - увеличение срока службы запоминающих устройств.
   
   Недостатки:
   - накладные расходы на работу со списком дыр;
   - фрагментация файла при частой записи данных в дыры;
   - невозможность записи данных в дыры при отсутствии свободного места на диске;
   - невозможность использования других индикаторов дыр, кроме нулевых байт.
   
   Самое распространенное применение — образы жестких дисков для ВМ и загрузка торрентов.

2. Жесткие ссылки (хард-линки) на один объект, не могут иметь разные права доступа и владельца, т.к. по сути, отдельными файлами не являются, все хард-линки одного объекта по сути являются разными именами одного и того же файла (имеют один inode), а изменение атрибутов по одному из имен приведет к изменению атрибутов по всем хард-линками. 

3. ```fdisk --list
   ...
   Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
   Disk model: VBOX HARDDISK
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 512 bytes / 512 bytes


   Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
   Disk model: VBOX HARDDISK
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 512 bytes / 512 bytes
   ...
   ```

4. `sudo fdisk /dev/sdb`
   ```fdisk
   Command (m for help): 
   Partition type
      p   primary (0 primary, 0 extended, 4 free)
      e   extended (container for logical partitions)
   Select (default p): p
   Partition number (1-4, default 1):
   First sector (2048-5242879, default 2048):
   Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G
   
   Created a new partition 1 of type 'Linux' and of size 2 GiB.
   
   Command (m for help): n
   Partition type
      p   primary (1 primary, 0 extended, 3 free)
      e   extended (container for logical partitions)
   Select (default p): p
   Partition number (2-4, default 2):
   First sector (4196352-5242879, default 4196352):
   Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):
   
   Created a new partition 2 of type 'Linux' and of size 511 MiB.
   
   Command (m for help): w
   The partition table has been altered.
   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```

5. `sudo sfdisk --dump /dev/sdb | sudo sfdisk /dev/sdc`
   ```sfdisk
   Checking that no-one is using this disk right now ... OK
   
   Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
   Disk model: VBOX HARDDISK
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 512 bytes / 512 bytes
   
   >>> Script header accepted.
   >>> Script header accepted.
   >>> Script header accepted.
   >>> Script header accepted.
   >>> Created a new DOS disklabel with disk identifier 0x849d6882.
   /dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
   /dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
   /dev/sdc3: Done.
   
   New situation:
   Disklabel type: dos
   Disk identifier: 0x849d6882
   
   Device     Boot   Start     End Sectors  Size Id Type
   /dev/sdc1          2048 4196351 4194304    2G 83 Linux
   /dev/sdc2       4196352 5242879 1046528  511M 83 Linux
   
   The partition table has been altered.
   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```
6. `sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[bc]1`
   ```mdadm
   mdadm: Note: this array has metadata at the start and
      may not be suitable as a boot device.  If you plan to
      store '/boot' on this device please ensure that
      your boot-loader understands md/v1.x metadata, or use
      --metadata=0.90
   Continue creating array? y
   mdadm: Defaulting to version 1.2 metadata
   mdadm: array /dev/md0 started.
   ```

7. `sudo mdadm --create /dev/md1 --level=0 --raid-devices=2 /dev/sd[bc]2`
   ```mdadm
   mdadm: Defaulting to version 1.2 metadata
   mdadm: array /dev/md1 started.
   ```

8. `sudo pvcreate /dev/md0 /dev/md1`
   ```
   Physical volume "/dev/md0" successfully created.
   Physical volume "/dev/md1" successfully created.
   ```

9. `sudo vgcreate VG01 /dev/md0 /dev/md1`
   ```
   Volume group "VG01" successfully created
   ```

10. `sudo lvcreate -l 10%PVS VG01 /dev/md1`
    ```
    Logical volume "lvol0" created.
    ```
    
11. `sudo mkfs.ext4 /dev/VG01/lvol0`
    ```
    mke2fs 1.45.5 (07-Jan-2020)
    Creating filesystem with 25600 4k blocks and 25600 inodes

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (1024 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```
    
12. ```
    mkdir /tmp/new
    sudo mount /dev/VG01/lvol0 /tmp/new
    ```

13. ```
    --2022-03-26 21:18:54--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
    Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
    Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 22380644 (21M) [application/octet-stream]
    Saving to: ‘/tmp/new/test.gz’
    
    /tmp/new/test.gz        100%[=============================>]  21.34M  22.4MB/s    in 1.0s
    
    2022-03-26 21:18:56 (21.2 MB/s) - ‘/tmp/new/test.gz’ saved [22380644/22380644]`
    ```

14. ```
    NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    loop0                       7:0    0 67.8M  1 loop  /snap/lxd/22753
    loop1                       7:1    0 55.4M  1 loop  /snap/core18/2128
    loop3                       7:3    0 43.6M  1 loop  /snap/snapd/15177
    loop4                       7:4    0 55.5M  1 loop  /snap/core18/2344
    loop5                       7:5    0 61.9M  1 loop  /snap/core20/1376
    loop6                       7:6    0 67.9M  1 loop  /snap/lxd/22526
    sda                         8:0    0   64G  0 disk
    ├─sda1                      8:1    0    1M  0 part
    ├─sda2                      8:2    0    1G  0 part  /boot
    └─sda3                      8:3    0   63G  0 part
      └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
    sdb                         8:16   0  2.5G  0 disk
    ├─sdb1                      8:17   0    2G  0 part
    │ └─md0                     9:0    0    2G  0 raid1
    └─sdb2                      8:18   0  511M  0 part
      └─md1                     9:1    0 1018M  0 raid0
        └─VG01-lvol0          253:1    0  100M  0 lvm   /tmp/new
    sdc                         8:32   0  2.5G  0 disk
    ├─sdc1                      8:33   0    2G  0 part
    │ └─md0                     9:0    0    2G  0 raid1
    └─sdc2                      8:34   0  511M  0 part
      └─md1                     9:1    0 1018M  0 raid0
        └─VG01-lvol0          253:1    0  100M  0 lvm   /tmp/new
    ```

15. ```
    vagrant@vagrant:~$ gzip -t /tmp/new/test.gz
    vagrant@vagrant:~$ echo $?
    0
    ```

16. `sudo pvmove /dev/md1`
    ```
    /dev/md1: Moved: 12.00%
    /dev/md1: Moved: 100.00%
    ```

17. `sudo mdadm /dev/md0 --fail /dev/sdc1`
    ```
    mdadm: set /dev/sdc1 faulty in /dev/md0
    ```

18. `dmesg -T`
    ```
    ...
    [Sat Mar 26 22:34:52 2022] md/raid1:md0: Disk failure on sdc1, disabling device.
    md/raid1:md0: Operation continuing on 1 devices.
    ```

19. ```
    vagrant@vagrant:~$ gzip -t /tmp/new/test.gz
    vagrant@vagrant:~$ echo $?
    0
    ```

20. ```
        default: Are you sure you want to destroy the 'default' VM? [y/N] y
    ==> default: Forcing shutdown of VM...
    ==> default: Destroying VM and associated drives...
    ```