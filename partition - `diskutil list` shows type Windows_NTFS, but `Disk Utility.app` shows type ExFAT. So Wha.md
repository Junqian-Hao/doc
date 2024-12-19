# partition - `diskutil list` shows type Windows_NTFS, but `Disk Utility.app` shows type ExFAT. So What is the type? - Ask Different
There is no mystery here. The volume is ExFAT formatted.

The drive is using the legacy Master Boot Record (MBR) partition scheme. The `diskutil` command displays the `TYPE` based on the single byte partition id. In your case, the partition id is a hexadecimal value of `07`. The `diskutil` has this hardcoded to the string `Windows_NTFS`. This partition id is also used for Microsoft's ExFAT format and IBM's HPFS format. The Disk Utility application actually looks at the contents of the volume to determine the format.

You can get the partition id by entering the command `fdisk /dev/disk1`. A list of MBR partition id's can be found [here](https://en.wikipedia.org/wiki/Partition_type#List_of_partition_IDs).

The `diskutil` command does know the partition is ExFAT formatted. If you enter the command `diskutil info /dev/disk1s1`, you can see for yourself. The `diskutil list /dev/disk1` command displayed the `Partition Type`, while the Disk Utility application displayed the `Name (User Visible)`.

Example
-------

Below is the `diskutil list /dev/disk3` output for a small mounted file named `exfat.dmg`.

```
/dev/disk3 (disk image):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        +100.0 MB   disk3
   1:               Windows_NTFS myexfat                 100.0 MB   disk3s1

```

Below is what is shown by the Disk Utility application.

[![](https://github.com/Junqian-Hao/doc/blob/main/image/2024-12-19%2015-33-34/16849e5e-58a4-4e8e-89df-d3169b633f3a.png?raw=true)
](https://i.sstatic.net/1Cb8s.png)

The output from `fdisk /dev/disk3` shows the partition id as `07`.

```
Disk: /dev/disk3    geometry: 775/4/63 [195376 sectors]
Signature: 0xAA55
         Starting       Ending
 #: id  cyl  hd sec -  cyl  hd sec [     start -       size]
------------------------------------------------------------------------
 1: 07 1023 254  63 - 1023 254  63 [         1 -     195375] HPFS/QNX/AUX
 2: 00    0   0   0 -    0   0   0 [         0 -          0] unused      
 3: 00    0   0   0 -    0   0   0 [         0 -          0] unused      
 4: 00    0   0   0 -    0   0   0 [         0 -          0] unused      

```

The output from the command `diskutil info /dev/disk3s1` is shown below.

```
   Device Identifier:        disk3s1
   Device Node:              /dev/disk3s1
   Whole:                    No
   Part of Whole:            disk3

   Volume Name:              myexfat
   Mounted:                  Yes
   Mount Point:              /Volumes/myexfat

   Partition Type:           Windows_NTFS
   File System Personality:  ExFAT
   Type (Bundle):            exfat
   Name (User Visible):      ExFAT

   OS Can Be Installed:      No
   Media Type:               Generic
   Protocol:                 Disk Image
   SMART Status:             Not Supported
   Volume UUID:              EE0D02D3-C2F6-3230-B97D-17C9C3AD7724
   Partition Offset:         512 Bytes (1 512-Byte-Device-Blocks)

   Disk Size:                100.0 MB (100032000 Bytes) (exactly 195375 512-Byte-Units)
   Device Block Size:        512 Bytes

   Volume Total Space:       99.8 MB (99831808 Bytes) (exactly 194984 512-Byte-Units)
   Volume Used Space:        24.6 KB (24576 Bytes) (exactly 48 512-Byte-Units) (0.0%)
   Volume Free Space:        99.8 MB (99807232 Bytes) (exactly 194936 512-Byte-Units) (100.0%)
   Allocation Block Size:    4096 Bytes

   Read-Only Media:          No
   Read-Only Volume:         No

   Device Location:          External
   Removable Media:          Removable
   Media Removal:            Software-Activated

```

I am using macOS High Sierra version 10.13.6.