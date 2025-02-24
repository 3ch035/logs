<details><summary>root@Echoes:~# lsblk</summary>


```bash
  NAME                         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
  sda                            8:0    0   1.8T  0 disk
  ├─sda1                         8:1    0   1.8T  0 part
  └─sda9                         8:9    0     8M  0 part
  sdb                            8:16   0   1.8T  0 disk
  ├─sdb1                         8:17   0   1.8T  0 part
  └─sdb9                         8:25   0     8M  0 part
  sdc                            8:32   0   1.8T  0 disk
  ├─sdc1                         8:33   0   1.8T  0 part
  └─sdc9                         8:41   0     8M  0 part
  sdd                            8:48   0   1.8T  0 disk
  ├─sdd1                         8:49   0   1.8T  0 part
  └─sdd9                         8:57   0     8M  0 part
  sde                            8:64   0   1.8T  0 disk
  ├─sde1                         8:65   0   1.8T  0 part
  └─sde9                         8:73   0     8M  0 part
  sdf                            8:80   0   1.8T  0 disk
  ├─sdf1                         8:81   0   1.8T  0 part
  └─sdf9                         8:89   0     8M  0 part
  sdg                            8:96   0   1.8T  0 disk
  ├─sdg1                         8:97   0   1.8T  0 part
  └─sdg9                         8:105  0     8M  0 part
  sdh                            8:112  0   1.8T  0 disk
  ├─sdh1                         8:113  0   1.8T  0 part
  └─sdh9                         8:121  0     8M  0 part
  sdi                            8:128  0   1.8T  0 disk
  ├─sdi1                         8:129  0   1.8T  0 part
  └─sdi9                         8:137  0     8M  0 part
  nvme0n1                      259:0    0 953.9G  0 disk
  ├─nvme0n1p1                  259:1    0  1007K  0 part
  ├─nvme0n1p2                  259:2    0     1G  0 part
  └─nvme0n1p3                  259:3    0 952.9G  0 part
  ├─pve-swap                 252:0    0     8G  0 lvm  [SWAP]
  ├─pve-root                 252:1    0    96G  0 lvm  /
  ├─pve-data_tmeta           252:2    0   8.3G  0 lvm
  │ └─pve-data-tpool         252:4    0 816.2G  0 lvm
  │   ├─pve-data             252:5    0 816.2G  1 lvm
  │   ├─pve-vm--101--disk--0 252:6    0     4M  0 lvm
  │   └─pve-vm--101--disk--1 252:7    0   500G  0 lvm
  └─pve-data_tdata           252:3    0 816.2G  0 lvm
  └─pve-data-tpool         252:4    0 816.2G  0 lvm
  ├─pve-data             252:5    0 816.2G  1 lvm
  ├─pve-vm--101--disk--0 252:6    0     4M  0 lvm
  └─pve-vm--101--disk--1 252:7    0   500G  0 lvm

```
</details>

<details><summary>root@Echoes:~# pvesm status</summary>


```bash
  Name             Type     Status           Total            Used       Available        %
  Remnants      zfspool     active     13245939712            1965     13245937746    0.00%
  local             dir     active        98497780         7127216        86321016    7.24%
  local-lvm     lvmthin     active       855855104        72234170       783620933    8.44%

```
</details>

<details><summary>root@Echoes:~# pvscan</summary>


```bash
  PV /dev/nvme0n1p3   VG pve             lvm2 [<952.87 GiB / 16.00 GiB free]
  Total: 1 [<952.87 GiB] / in use: 1 [<952.87 GiB] / in no VG: 0 [0   ]

```
</details>

<details><summary>root@Echoes:~# cat /etc/pve/storage.cfg</summary>


```bash
  dir: local
  path /var/lib/vz
  content backup,iso,vztmpl

  lvmthin: local-lvm
  thinpool data
  vgname pve
  content images,rootdir

  zfspool: Remnants
  pool Remnants
  content rootdir,images
  mountpoint /Remnants
  nodes Echoes

```
</details>

<details><summary>root@Echoes:~# vgscan</summary>


```bash
  Found volume group "pve" using metadata type lvm2

```
</details>

<details><summary>root@Echoes:~# lvscan</summary>


```bash
  ACTIVE            '/dev/pve/data' [<816.21 GiB] inherit
  ACTIVE            '/dev/pve/swap' [8.00 GiB] inherit
  ACTIVE            '/dev/pve/root' [96.00 GiB] inherit
  ACTIVE            '/dev/pve/vm-101-disk-0' [4.00 MiB] inherit
  ACTIVE            '/dev/pve/vm-101-disk-1' [500.00 GiB] inherit

```
</details>

<details><summary>root@Echoes:~# pvs</summary>


```bash
  PV             VG  Fmt  Attr PSize    PFree
  /dev/nvme0n1p3 pve lvm2 a--  <952.87g 16.00g

```
</details>

<details><summary>root@Echoes:~# vgs</summary>


```bash
  VG  #PV #LV #SN Attr   VSize    VFree
  pve   1   5   0 wz--n- <952.87g 16.00g

```
</details>

<details><summary>root@Echoes:~# lvs</summary>


```bash
  LV            VG  Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  data          pve twi-aotz-- <816.21g             8.44   0.51
  root          pve -wi-ao----   96.00g
  swap          pve -wi-ao----    8.00g
  vm-101-disk-0 pve Vwi-aotz--    4.00m data        14.06
  vm-101-disk-1 pve Vwi-aotz--  500.00g data        13.77

```
</details>

<details><summary>root@Echoes:~# zpool status</summary>


```bash
  pool: Remnants
  state: ONLINE
  config:

  NAME                                             STATE     READ WRITE CKSUM
  Remnants                                         ONLINE       0     0     0
  raidz2-0                                       ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X705040M  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X318373T  ONLINE       0     0     0
  ata-Samsung_SSD_870_QVO_2TB_S5VWNJ0R910332E  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X306414W  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X215660V  ONLINE       0     0     0
  ata-Samsung_SSD_870_QVO_2TB_S6R4NJ0T210035R  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0W924347N  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NL0X900603T  ONLINE       0     0     0
  ata-Samsung_SSD_870_EVO_2TB_S753NS0W907688M  ONLINE       0     0     0

  errors: No known data errors

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sda -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0X705040M
  LU WWN Device Id: 5 002538 f3470a9e8
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       184
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       5
  177 Wear_Leveling_Count     0x0013   100   100   000    Pre-fail  Always       -       0
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   058   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       2
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       449322073
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%         0         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdb -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0X318373T
  LU WWN Device Id: 5 002538 f3432f6fc
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       185
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       5
  177 Wear_Leveling_Count     0x0013   100   100   000    Pre-fail  Always       -       0
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   078   058   000    Old_age   Always       -       22
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       2
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       449731681
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%         0         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdc -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 QVO 2TB
  Serial Number:    S5VWNJ0R910332E
  LU WWN Device Id: 5 002538 f31903099
  Firmware Version: SVQ02B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   098   098   000    Old_age   Always       -       9410
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       398
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       1
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   037   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       128
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       3171401619

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  No self-tests have been logged.  [To run self-tests, use: smartctl -t]

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdd -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0X306414W
  LU WWN Device Id: 5 002538 f3431e293
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       1293
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       119
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       4
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   051   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       109
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       15324260204
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%        10         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sde -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0X215660V
  LU WWN Device Id: 5 002538 f34223840
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       722
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       49
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       2
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   054   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       37
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       7989972995
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%         5         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdf -T permissive</summary>


```bash

  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 QVO 2TB
  Serial Number:    S6R4NJ0T210035R
  LU WWN Device Id: 5 002538 f32209967
  Firmware Version: SVQ02B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   097   097   000    Old_age   Always       -       10758
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       561
  177 Wear_Leveling_Count     0x0013   100   100   000    Pre-fail  Always       -       0
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   051   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       278
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       43985757

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  No self-tests have been logged.  [To run self-tests, use: smartctl -t]

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdg -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0W924347N
  LU WWN Device Id: 5 002538 f339147b0
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       2458
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       100
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       1
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   052   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   001   001   000    Old_age   Always       -       760979
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       88
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       4216526307
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%         8         -
  # 2  Short offline       Completed without error       00%         1         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdh -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NL0X900603T
  LU WWN Device Id: 5 002538 f54908bca
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       749
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       60
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       2
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   062   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       29
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       11721510777
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%        25         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# smartctl -a /dev/sdi -T permissive</summary>


```bash
  smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.8.4-2-pve] (local build)
  Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

  === START OF INFORMATION SECTION ===
  Model Family:     Samsung based SSDs
  Device Model:     Samsung SSD 870 EVO 2TB
  Serial Number:    S753NS0W907688M
  LU WWN Device Id: 5 002538 f3390277b
  Firmware Version: SVT03B6Q
  User Capacity:    2,000,398,934,016 bytes [2.00 TB]
  Sector Size:      512 bytes logical/physical
  Rotation Rate:    Solid State Device
  Form Factor:      2.5 inches
  TRIM Command:     Available, deterministic, zeroed
  Device is:        In smartctl database 7.3/5319
  ATA Version is:   ACS-4 T13/BSR INCITS 529 revision 5
  SATA Version is:  SATA 3.3, 6.0 Gb/s (current: 6.0 Gb/s)
  Local Time is:    Mon Feb 24 12:42:37 2025 HST
  SMART support is: Available - device has SMART capability.
  SMART support is: Enabled

  === START OF READ SMART DATA SECTION ===
  SMART overall-health self-assessment test result: PASSED

  General SMART Values:
  Offline data collection status:  (0x00) Offline data collection activity
  was never started.
  Auto Offline Data Collection: Disabled.
  Self-test execution status:      (   0) The previous self-test routine completed
  without error or no self-test has ever
  been run.
  Total time to complete Offline
  data collection:                (    0) seconds.
  Offline data collection
  capabilities:                    (0x53) SMART execute Offline immediate.
  Auto Offline data collection on/off support.
  Suspend Offline collection upon new
  command.
  No Offline surface scan supported.
  Self-test supported.
  No Conveyance Self-test supported.
  Selective Self-test supported.
  SMART capabilities:            (0x0003) Saves SMART data before entering
  power-saving mode.
  Supports SMART auto save timer.
  Error logging capability:        (0x01) Error logging supported.
  General Purpose Logging supported.
  Short self-test routine
  recommended polling time:        (   2) minutes.
  Extended self-test routine
  recommended polling time:        ( 160) minutes.
  SCT capabilities:              (0x003d) SCT Status supported.
  SCT Error Recovery Control supported.
  SCT Feature Control supported.
  SCT Data Table supported.

  SMART Attributes Data Structure revision number: 1
  Vendor Specific SMART Attributes with Thresholds:
  ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   099   099   000    Old_age   Always       -       2461
  12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       149
  177 Wear_Leveling_Count     0x0013   099   099   000    Pre-fail  Always       -       1
  179 Used_Rsvd_Blk_Cnt_Tot   0x0013   100   100   010    Pre-fail  Always       -       0
  181 Program_Fail_Cnt_Total  0x0032   100   100   010    Old_age   Always       -       0
  182 Erase_Fail_Count_Total  0x0032   100   100   010    Old_age   Always       -       0
  183 Runtime_Bad_Block       0x0013   100   100   010    Pre-fail  Always       -       0
  187 Uncorrectable_Error_Cnt 0x0032   100   100   000    Old_age   Always       -       0
  190 Airflow_Temperature_Cel 0x0032   079   054   000    Old_age   Always       -       21
  195 ECC_Error_Rate          0x001a   200   200   000    Old_age   Always       -       0
  199 CRC_Error_Count         0x003e   100   100   000    Old_age   Always       -       0
  235 POR_Recovery_Count      0x0012   099   099   000    Old_age   Always       -       115
  241 Total_LBAs_Written      0x0032   099   099   000    Old_age   Always       -       4037149717
  252 Unknown_Attribute       0x0032   100   100   000    Old_age   Always       -       0

  SMART Error Log Version: 1
  No Errors Logged

  SMART Self-test log structure revision number 1
  Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
  # 1  Short offline       Completed without error       00%         2         -

  SMART Selective self-test log data structure revision number 1
  SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
  1        0        0  Not_testing
  2        0        0  Not_testing
  3        0        0  Not_testing
  4        0        0  Not_testing
  5        0        0  Not_testing
  256        0    65535  Read_scanning was never started
  Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
  If Selective self-test is pending on power-up, resume after 0 minute delay.

```
</details>

<details><summary>root@Echoes:~# zfs get all</summary>


```bash
  NAME      PROPERTY              VALUE                  SOURCE
  Remnants  type                  filesystem             -
  Remnants  creation              Sat Feb 15 17:48 2025  -
  Remnants  used                  1.92M                  -
  Remnants  available             12.3T                  -
  Remnants  referenced            238K                   -
  Remnants  compressratio         1.00x                  -
  Remnants  mounted               yes                    -
  Remnants  quota                 none                   default
  Remnants  reservation           none                   default
  Remnants  recordsize            128K                   default
  Remnants  mountpoint            /Remnants              default
  Remnants  sharenfs              off                    default
  Remnants  checksum              on                     default
  Remnants  compression           lz4                    local
  Remnants  atime                 on                     default
  Remnants  devices               on                     default
  Remnants  exec                  on                     default
  Remnants  setuid                on                     default
  Remnants  readonly              off                    default
  Remnants  zoned                 off                    default
  Remnants  snapdir               hidden                 default
  Remnants  aclmode               discard                default
  Remnants  aclinherit            restricted             default
  Remnants  createtxg             1                      -
  Remnants  canmount              on                     default
  Remnants  xattr                 on                     default
  Remnants  copies                1                      default
  Remnants  version               5                      -
  Remnants  utf8only              off                    -
  Remnants  normalization         none                   -
  Remnants  casesensitivity       sensitive              -
  Remnants  vscan                 off                    default
  Remnants  nbmand                off                    default
  Remnants  sharesmb              off                    default
  Remnants  refquota              none                   default
  Remnants  refreservation        none                   default
  Remnants  guid                  5100276654684546440    -
  Remnants  primarycache          all                    default
  Remnants  secondarycache        all                    default
  Remnants  usedbysnapshots       0B                     -
  Remnants  usedbydataset         238K                   -
  Remnants  usedbychildren        1.69M                  -
  Remnants  usedbyrefreservation  0B                     -
  Remnants  logbias               latency                default
  Remnants  objsetid              54                     -
  Remnants  dedup                 off                    default
  Remnants  mlslabel              none                   default
  Remnants  sync                  standard               default
  Remnants  dnodesize             legacy                 default
  Remnants  refcompressratio      1.02x                  -
  Remnants  written               238K                   -
  Remnants  logicalused           284K                   -
  Remnants  logicalreferenced     47K                    -
  Remnants  volmode               default                default
  Remnants  filesystem_limit      none                   default
  Remnants  snapshot_limit        none                   default
  Remnants  filesystem_count      none                   default
  Remnants  snapshot_count        none                   default
  Remnants  snapdev               hidden                 default
  Remnants  acltype               off                    default
  Remnants  context               none                   default
  Remnants  fscontext             none                   default
  Remnants  defcontext            none                   default
  Remnants  rootcontext           none                   default
  Remnants  relatime              on                     default
  Remnants  redundant_metadata    all                    default
  Remnants  overlay               on                     default
  Remnants  encryption            off                    default
  Remnants  keylocation           none                   default
  Remnants  keyformat             none                   default
  Remnants  pbkdf2iters           0                      default
  Remnants  special_small_blocks  0                      default

```
</details>

<details><summary>root@Echoes:~# zpool history</summary>


```bash
  History for 'Remnants':
  2025-02-15.17:48:05 zpool create -o ashift=12 Remnants raidz2 /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0X705040M /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0X318373T /dev/disk/by-id/ata-Samsung_SSD_870_QVO_2TB_S5VWNJ0R910332E /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0X306414W /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0X215660V /dev/disk/by-id/ata-Samsung_SSD_870_QVO_2TB_S6R4NJ0T210035R /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0W924347N /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NL0X900603T /dev/disk/by-id/ata-Samsung_SSD_870_EVO_2TB_S753NS0W907688M
  2025-02-15.17:48:05 zfs set compression=lz4 Remnants
  2025-02-24.10:54:18 zpool import -N -d /dev/disk/by-id -o cachefile=none Remnants
  2025-02-24.12:51:41 zpool scrub Remnants

```
</details>

<details><summary>root@Echoes:~# zpool iostat -v</summary>


```bash
  capacity     operations     bandwidth
  pool                                             alloc   free   read  write   read  write
  -----------------------------------------------  -----  -----  -----  -----  -----  -----
  Remnants                                         2.73M  16.4T      0      0  1.85K  3.28K
  raidz2-0                                       2.73M  16.4T      0      0  1.85K  3.28K
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X705040M      -      -      0      0    215    377
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X318373T      -      -      0      0    218    371
  ata-Samsung_SSD_870_QVO_2TB_S5VWNJ0R910332E      -      -      0      0    222    370
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X306414W      -      -      0      0    213    370
  ata-Samsung_SSD_870_EVO_2TB_S753NS0X215660V      -      -      0      0    199    375
  ata-Samsung_SSD_870_QVO_2TB_S6R4NJ0T210035R      -      -      0      0    208    374
  ata-Samsung_SSD_870_EVO_2TB_S753NS0W924347N      -      -      0      0    202    373
  ata-Samsung_SSD_870_EVO_2TB_S753NL0X900603T      -      -      0      0    212    374
  ata-Samsung_SSD_870_EVO_2TB_S753NS0W907688M      -      -      0      0    205    375
  -----------------------------------------------  -----  -----  -----  -----  -----  -----

```
</details>

<details><summary>root@Echoes:~# journalctl -k | grep -i "ata\|sd\|zfs"</summary>


```bash
  Feb 24 10:54:13 Echoes kernel: ACPI: RSDP 0x00000000DE4F0000 000024 (v02 ALASKA)
  Feb 24 10:54:13 Echoes kernel: ACPI: XSDT 0x00000000DE4F0090 00009C (v01 ALASKA A M I    01072009 AMI  00010013)
  Feb 24 10:54:13 Echoes kernel: ACPI: DSDT 0x00000000DE4F01C0 0128DB (v02 ALASKA A M I    00000023 INTL 20120711)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0x00000000DE502DB8 000C7D (v02 Ther_R Ther_Rvp 00001000 INTL 20120711)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0x00000000DE503A38 000539 (v02 PmRef  Cpu0Ist  00003000 INTL 20051117)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0x00000000DE503F78 000B74 (v02 CpuRef CpuSsdt  00003000 INTL 20051117)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0x00000000DE504B68 00036D (v01 SataRe SataTabl 00001000 INTL 20120711)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0x00000000DE504ED8 005835 (v02 SaSsdt SaSsdt   00003000 INTL 20120711)
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving DSDT table memory at [mem 0xde4f01c0-0xde502a9a]
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving SSDT table memory at [mem 0xde502db8-0xde503a34]
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving SSDT table memory at [mem 0xde503a38-0xde503f70]
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving SSDT table memory at [mem 0xde503f78-0xde504aeb]
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving SSDT table memory at [mem 0xde504b68-0xde504ed4]
  Feb 24 10:54:13 Echoes kernel: ACPI: Reserving SSDT table memory at [mem 0xde504ed8-0xde50a70c]
  Feb 24 10:54:13 Echoes kernel: NODE_DATA(0) allocated [mem 0x81efd5000-0x81effffff]
  Feb 24 10:54:13 Echoes kernel: [Firmware Bug]: TSC_DEADLINE disabled due to Errata; please update microcode to version: 0x22 (or later)
  Feb 24 10:54:13 Echoes kernel: Memory: 32680072K/33499152K available (20480K kernel code, 3638K rwdata, 13412K rodata, 4796K init, 5716K bss, 818820K reserved, 0K cma-reserved)
  Feb 24 10:54:13 Echoes kernel: MMIO Stale Data: Unknown: No mitigations
  Feb 24 10:54:13 Echoes kernel: core: PMU erratum BJ122, BV98, HSD29 workaround disabled, HT off
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0xFFFF9A6841657800 0003D3 (v02 PmRef  Cpu0Cst  00003001 INTL 20051117)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0xFFFF9A68416DF000 0005AA (v02 PmRef  ApIst    00003000 INTL 20051117)
  Feb 24 10:54:13 Echoes kernel: ACPI: SSDT 0xFFFF9A68401CA400 000119 (v02 PmRef  ApCst    00003000 INTL 20051117)
  Feb 24 10:54:13 Echoes kernel: libata version 3.00 loaded.
  Feb 24 10:54:13 Echoes kernel: Write protecting the kernel read-only data: 34816k
  Feb 24 10:54:13 Echoes kernel: Freeing unused kernel image (rodata/data gap) memory: 924K
  Feb 24 10:54:13 Echoes kernel: ahci 0000:00:1f.2: AHCI 0001.0300 32 slots 4 ports 6 Gbps 0xd impl SATA mode
  Feb 24 10:54:13 Echoes kernel: ata1: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15100 irq 40 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata2: DUMMY
  Feb 24 10:54:13 Echoes kernel: ata3: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15200 irq 40 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata4: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15280 irq 40 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ahci 0000:03:00.0: AHCI 0001.0200 32 slots 2 ports 6 Gbps 0x3 impl SATA mode
  Feb 24 10:54:13 Echoes kernel: ata5: SATA max UDMA/133 abar m512@0xf7c00000 port 0xf7c00100 irq 47 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata6: SATA max UDMA/133 abar m512@0xf7c00000 port 0xf7c00180 irq 47 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ahci 0000:0b:00.0: AHCI 0001.0301 32 slots 24 ports 6 Gbps 0xffff0f impl SATA mode
  Feb 24 10:54:13 Echoes kernel: ahci 0000:0b:00.0: flags: 64bit ncq sntf stag pm led only pio slum part deso sadm sds apst
  Feb 24 10:54:13 Echoes kernel: ata7: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980100 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata8: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980180 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata9: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980200 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata10: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980280 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata11: DUMMY
  Feb 24 10:54:13 Echoes kernel: ata12: DUMMY
  Feb 24 10:54:13 Echoes kernel: ata13: DUMMY
  Feb 24 10:54:13 Echoes kernel: ata14: DUMMY
  Feb 24 10:54:13 Echoes kernel: ata15: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980500 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata16: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980580 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata17: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980600 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata18: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980680 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata19: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980700 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata20: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980780 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata21: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980800 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata22: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980880 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata23: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980900 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata24: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980980 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata25: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980a00 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata26: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980a80 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata27: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980b00 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata28: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980b80 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata29: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980c00 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata30: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980c80 irq 48 lpm-pol 0
  Feb 24 10:54:13 Echoes kernel: ata1: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata5: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata3: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata4: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata4.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata4.00: ATA-11: Samsung SSD 870 QVO 2TB, SVQ02B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata4.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata1.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata1.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata3.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata3.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata1.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata3.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata4.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata4.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata1.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata3.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata1.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata3.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata7: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata7.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata7.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata7.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata4.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata1.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 0:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: ata3.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: Attached scsi generic sg0 type 0
  Feb 24 10:54:13 Echoes kernel: ata1.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: scsi 2:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: ata7.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: Attached scsi generic sg1 type 0
  Feb 24 10:54:13 Echoes kernel: ata3.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: ata1.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: scsi 3:0:0:0: Direct-Access     ATA      Samsung SSD 870  2B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata7.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata3.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: ata4.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: Attached scsi generic sg2 type 0
  Feb 24 10:54:13 Echoes kernel: ata4.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sda: sda1 sda9
  Feb 24 10:54:13 Echoes kernel:  sdb: sdb1 sdb9
  Feb 24 10:54:13 Echoes kernel:  sdc: sdc1 sdc9
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 0:0:0:0: [sda] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 2:0:0:0: [sdb] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 3:0:0:0: [sdc] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata7.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata6: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: scsi 6:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: Attached scsi generic sg3 type 0
  Feb 24 10:54:13 Echoes kernel: ata7.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata7.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sdd: sdd1 sdd9
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 6:0:0:0: [sdd] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata8: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata9: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata10: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata10.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata10.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata10.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata10.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata10.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata10.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 9:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: ata10.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: Attached scsi generic sg4 type 0
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata10.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sde: sde1 sde9
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 9:0:0:0: [sde] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata15: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata16: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata17: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata18: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata19: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata20: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata21: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata22: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata23: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata24: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata25: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata26: SATA link down (SStatus 0 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata27: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata27.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata27.00: ATA-11: Samsung SSD 870 QVO 2TB, SVQ02B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata27.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata27.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata27.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata27.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 26:0:0:0: Direct-Access     ATA      Samsung SSD 870  2B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: Attached scsi generic sg5 type 0
  Feb 24 10:54:13 Echoes kernel: ata27.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata27.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sdf: sdf1 sdf9
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 26:0:0:0: [sdf] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata28: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata28.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata28.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata28.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata28.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata28.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata28.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 27:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: Attached scsi generic sg6 type 0
  Feb 24 10:54:13 Echoes kernel: ata28.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata28.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sdg: sdg1 sdg9
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 27:0:0:0: [sdg] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata29: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata29.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata29.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata29.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata29.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata29.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata29.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 28:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: Attached scsi generic sg7 type 0
  Feb 24 10:54:13 Echoes kernel: ata29.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata29.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sdh: sdh1 sdh9
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 28:0:0:0: [sdh] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: ata30: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  Feb 24 10:54:13 Echoes kernel: ata30.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata30.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  Feb 24 10:54:13 Echoes kernel: ata30.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  Feb 24 10:54:13 Echoes kernel: ata30.00: Features: Trust Dev-Sleep NCQ-sndrcv
  Feb 24 10:54:13 Echoes kernel: ata30.00: supports DRM functions and may not be fully accessible
  Feb 24 10:54:13 Echoes kernel: ata30.00: configured for UDMA/133
  Feb 24 10:54:13 Echoes kernel: scsi 29:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: Attached scsi generic sg8 type 0
  Feb 24 10:54:13 Echoes kernel: ata30.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] Write Protect is off
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] Mode Sense: 00 3a 00 00
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] Preferred minimum I/O size 512 bytes
  Feb 24 10:54:13 Echoes kernel: ata30.00: Enabling discard_zeroes_data
  Feb 24 10:54:13 Echoes kernel:  sdi: sdi1 sdi9
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] supports TCG Opal
  Feb 24 10:54:13 Echoes kernel: sd 29:0:0:0: [sdi] Attached SCSI disk
  Feb 24 10:54:13 Echoes kernel: EXT4-fs (dm-1): mounted filesystem 82c3aa13-f45d-477b-b91c-4089b49c4443 ro with ordered data mode. Quota mode: none.
  Feb 24 10:54:13 Echoes systemd[1]: Created slice system-zfs\x2dimport.slice - Slice /system/zfs-import.
  Feb 24 10:54:13 Echoes kernel: zfs: module license 'CDDL' taints kernel.
  Feb 24 10:54:13 Echoes kernel: zfs: module license taints kernel.
  Feb 24 10:54:15 Echoes kernel: ZFS: Loaded module v2.2.3-pve2, ZFS pool version 5000, ZFS filesystem version 5

```
</details>

<details><summary>root@Echoes:~# dmesg | grep -i "ata\|sd\|zfs"</summary>


```bash
  [    0.008262] ACPI: RSDP 0x00000000DE4F0000 000024 (v02 ALASKA)
  [    0.008265] ACPI: XSDT 0x00000000DE4F0090 00009C (v01 ALASKA A M I    01072009 AMI  00010013)
  [    0.008276] ACPI: DSDT 0x00000000DE4F01C0 0128DB (v02 ALASKA A M I    00000023 INTL 20120711)
  [    0.008293] ACPI: SSDT 0x00000000DE502DB8 000C7D (v02 Ther_R Ther_Rvp 00001000 INTL 20120711)
  [    0.008296] ACPI: SSDT 0x00000000DE503A38 000539 (v02 PmRef  Cpu0Ist  00003000 INTL 20051117)
  [    0.008299] ACPI: SSDT 0x00000000DE503F78 000B74 (v02 CpuRef CpuSsdt  00003000 INTL 20051117)
  [    0.008308] ACPI: SSDT 0x00000000DE504B68 00036D (v01 SataRe SataTabl 00001000 INTL 20120711)
  [    0.008311] ACPI: SSDT 0x00000000DE504ED8 005835 (v02 SaSsdt SaSsdt   00003000 INTL 20120711)
  [    0.008324] ACPI: Reserving DSDT table memory at [mem 0xde4f01c0-0xde502a9a]
  [    0.008330] ACPI: Reserving SSDT table memory at [mem 0xde502db8-0xde503a34]
  [    0.008330] ACPI: Reserving SSDT table memory at [mem 0xde503a38-0xde503f70]
  [    0.008331] ACPI: Reserving SSDT table memory at [mem 0xde503f78-0xde504aeb]
  [    0.008334] ACPI: Reserving SSDT table memory at [mem 0xde504b68-0xde504ed4]
  [    0.008335] ACPI: Reserving SSDT table memory at [mem 0xde504ed8-0xde50a70c]
  [    0.008403] NODE_DATA(0) allocated [mem 0x81efd5000-0x81effffff]
  [    0.065333] [Firmware Bug]: TSC_DEADLINE disabled due to Errata; please update microcode to version: 0x22 (or later)
  [    0.163614] Memory: 32680072K/33499152K available (20480K kernel code, 3638K rwdata, 13412K rodata, 4796K init, 5716K bss, 818820K reserved, 0K cma-reserved)
  [    0.181689] MMIO Stale Data: Unknown: No mitigations
  [    0.311270] core: PMU erratum BJ122, BV98, HSD29 workaround disabled, HT off
  [    0.326984] ACPI: SSDT 0xFFFF9A6841657800 0003D3 (v02 PmRef  Cpu0Cst  00003001 INTL 20051117)
  [    0.327962] ACPI: SSDT 0xFFFF9A68416DF000 0005AA (v02 PmRef  ApIst    00003000 INTL 20051117)
  [    0.328818] ACPI: SSDT 0xFFFF9A68401CA400 000119 (v02 PmRef  ApCst    00003000 INTL 20051117)
  [    0.361376] libata version 3.00 loaded.
  [    0.722313] Write protecting the kernel read-only data: 34816k
  [    0.722629] Freeing unused kernel image (rodata/data gap) memory: 924K
  [    0.920593] ahci 0000:00:1f.2: AHCI 0001.0300 32 slots 4 ports 6 Gbps 0xd impl SATA mode
  [    0.942370] ata1: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15100 irq 40 lpm-pol 0
  [    0.942372] ata2: DUMMY
  [    0.942374] ata3: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15200 irq 40 lpm-pol 0
  [    0.942376] ata4: SATA max UDMA/133 abar m2048@0xf7f15000 port 0xf7f15280 irq 40 lpm-pol 0
  [    0.942648] ahci 0000:03:00.0: AHCI 0001.0200 32 slots 2 ports 6 Gbps 0x3 impl SATA mode
  [    0.943275] ata5: SATA max UDMA/133 abar m512@0xf7c00000 port 0xf7c00100 irq 47 lpm-pol 0
  [    0.943278] ata6: SATA max UDMA/133 abar m512@0xf7c00000 port 0xf7c00180 irq 47 lpm-pol 0
  [    0.953852] ahci 0000:0b:00.0: AHCI 0001.0301 32 slots 24 ports 6 Gbps 0xffff0f impl SATA mode
  [    0.953855] ahci 0000:0b:00.0: flags: 64bit ncq sntf stag pm led only pio slum part deso sadm sds apst
  [    0.957463] ata7: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980100 irq 48 lpm-pol 0
  [    0.957467] ata8: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980180 irq 48 lpm-pol 0
  [    0.957470] ata9: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980200 irq 48 lpm-pol 0
  [    0.957473] ata10: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980280 irq 48 lpm-pol 0
  [    0.957474] ata11: DUMMY
  [    0.957475] ata12: DUMMY
  [    0.957475] ata13: DUMMY
  [    0.957476] ata14: DUMMY
  [    0.957478] ata15: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980500 irq 48 lpm-pol 0
  [    0.957481] ata16: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980580 irq 48 lpm-pol 0
  [    0.957484] ata17: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980600 irq 48 lpm-pol 0
  [    0.957487] ata18: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980680 irq 48 lpm-pol 0
  [    0.957490] ata19: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980700 irq 48 lpm-pol 0
  [    0.957493] ata20: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980780 irq 48 lpm-pol 0
  [    0.957496] ata21: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980800 irq 48 lpm-pol 0
  [    0.957499] ata22: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980880 irq 48 lpm-pol 0
  [    0.957502] ata23: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980900 irq 48 lpm-pol 0
  [    0.957505] ata24: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980980 irq 48 lpm-pol 0
  [    0.957508] ata25: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980a00 irq 48 lpm-pol 0
  [    0.957511] ata26: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980a80 irq 48 lpm-pol 0
  [    0.957514] ata27: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980b00 irq 48 lpm-pol 0
  [    0.957517] ata28: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980b80 irq 48 lpm-pol 0
  [    0.957520] ata29: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980c00 irq 48 lpm-pol 0
  [    0.957523] ata30: SATA max UDMA/133 abar m8192@0xf7980000 port 0xf7980c80 irq 48 lpm-pol 0
  [    1.253909] ata1: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    1.253990] ata5: SATA link down (SStatus 0 SControl 300)
  [    1.253991] ata3: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    1.254019] ata4: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    1.257141] ata4.00: supports DRM functions and may not be fully accessible
  [    1.257144] ata4.00: ATA-11: Samsung SSD 870 QVO 2TB, SVQ02B6Q, max UDMA/133
  [    1.257556] ata4.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    1.258107] ata1.00: supports DRM functions and may not be fully accessible
  [    1.258108] ata1.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    1.258415] ata3.00: supports DRM functions and may not be fully accessible
  [    1.258417] ata3.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    1.258615] ata1.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    1.258862] ata3.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    1.260015] ata4.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    1.260332] ata4.00: supports DRM functions and may not be fully accessible
  [    1.260991] ata1.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    1.261127] ata3.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    1.261314] ata1.00: supports DRM functions and may not be fully accessible
  [    1.261545] ata3.00: supports DRM functions and may not be fully accessible
  [    1.261853] ata7: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    1.262103] ata7.00: supports DRM functions and may not be fully accessible
  [    1.262104] ata7.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    1.262521] ata7.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    1.263370] ata4.00: configured for UDMA/133
  [    1.264221] ata1.00: configured for UDMA/133
  [    1.264370] scsi 0:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    1.264442] ata3.00: configured for UDMA/133
  [    1.264601] sd 0:0:0:0: Attached scsi generic sg0 type 0
  [    1.264627] ata1.00: Enabling discard_zeroes_data
  [    1.264637] sd 0:0:0:0: [sda] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    1.264689] sd 0:0:0:0: [sda] Write Protect is off
  [    1.264690] sd 0:0:0:0: [sda] Mode Sense: 00 3a 00 00
  [    1.264698] sd 0:0:0:0: [sda] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    1.264714] sd 0:0:0:0: [sda] Preferred minimum I/O size 512 bytes
  [    1.264745] scsi 2:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    1.264872] ata7.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    1.264944] sd 2:0:0:0: Attached scsi generic sg1 type 0
  [    1.265006] ata3.00: Enabling discard_zeroes_data
  [    1.265009] ata1.00: Enabling discard_zeroes_data
  [    1.265013] sd 2:0:0:0: [sdb] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    1.265018] sd 2:0:0:0: [sdb] Write Protect is off
  [    1.265020] sd 2:0:0:0: [sdb] Mode Sense: 00 3a 00 00
  [    1.265026] scsi 3:0:0:0: Direct-Access     ATA      Samsung SSD 870  2B6Q PQ: 0 ANSI: 5
  [    1.265027] sd 2:0:0:0: [sdb] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    1.265043] sd 2:0:0:0: [sdb] Preferred minimum I/O size 512 bytes
  [    1.265191] ata7.00: supports DRM functions and may not be fully accessible
  [    1.265211] ata3.00: Enabling discard_zeroes_data
  [    1.265235] ata4.00: Enabling discard_zeroes_data
  [    1.265242] sd 3:0:0:0: [sdc] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    1.265246] sd 3:0:0:0: [sdc] Write Protect is off
  [    1.265248] sd 3:0:0:0: [sdc] Mode Sense: 00 3a 00 00
  [    1.265256] sd 3:0:0:0: [sdc] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    1.265270] sd 3:0:0:0: [sdc] Preferred minimum I/O size 512 bytes
  [    1.265364] sd 3:0:0:0: Attached scsi generic sg2 type 0
  [    1.265408] ata4.00: Enabling discard_zeroes_data
  [    1.266059]  sda: sda1 sda9
  [    1.266220]  sdb: sdb1 sdb9
  [    1.266466]  sdc: sdc1 sdc9
  [    1.267020] sd 0:0:0:0: [sda] supports TCG Opal
  [    1.267022] sd 0:0:0:0: [sda] Attached SCSI disk
  [    1.267274] sd 2:0:0:0: [sdb] supports TCG Opal
  [    1.267276] sd 2:0:0:0: [sdb] Attached SCSI disk
  [    1.267527] sd 3:0:0:0: [sdc] supports TCG Opal
  [    1.267528] sd 3:0:0:0: [sdc] Attached SCSI disk
  [    1.268045] ata7.00: configured for UDMA/133
  [    1.573898] ata6: SATA link down (SStatus 0 SControl 300)
  [    1.574049] scsi 6:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    1.574235] sd 6:0:0:0: Attached scsi generic sg3 type 0
  [    1.574259] ata7.00: Enabling discard_zeroes_data
  [    1.574281] sd 6:0:0:0: [sdd] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    1.574286] sd 6:0:0:0: [sdd] Write Protect is off
  [    1.574296] sd 6:0:0:0: [sdd] Mode Sense: 00 3a 00 00
  [    1.574302] sd 6:0:0:0: [sdd] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    1.574338] sd 6:0:0:0: [sdd] Preferred minimum I/O size 512 bytes
  [    1.574591] ata7.00: Enabling discard_zeroes_data
  [    1.579738]  sdd: sdd1 sdd9
  [    1.581532] sd 6:0:0:0: [sdd] supports TCG Opal
  [    1.581534] sd 6:0:0:0: [sdd] Attached SCSI disk
  [    1.885911] ata8: SATA link down (SStatus 0 SControl 300)
  [    2.189925] ata9: SATA link down (SStatus 0 SControl 300)
  [    2.493911] ata10: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    2.494163] ata10.00: supports DRM functions and may not be fully accessible
  [    2.494164] ata10.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    2.494589] ata10.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    2.496898] ata10.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    2.497236] ata10.00: supports DRM functions and may not be fully accessible
  [    2.500208] ata10.00: configured for UDMA/133
  [    2.500290] scsi 9:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    2.500487] ata10.00: Enabling discard_zeroes_data
  [    2.500493] sd 9:0:0:0: Attached scsi generic sg4 type 0
  [    2.500495] sd 9:0:0:0: [sde] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    2.500499] sd 9:0:0:0: [sde] Write Protect is off
  [    2.500501] sd 9:0:0:0: [sde] Mode Sense: 00 3a 00 00
  [    2.500507] sd 9:0:0:0: [sde] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    2.500530] sd 9:0:0:0: [sde] Preferred minimum I/O size 512 bytes
  [    2.500740] ata10.00: Enabling discard_zeroes_data
  [    2.501880]  sde: sde1 sde9
  [    2.503433] sd 9:0:0:0: [sde] supports TCG Opal
  [    2.503435] sd 9:0:0:0: [sde] Attached SCSI disk
  [    2.805904] ata15: SATA link down (SStatus 0 SControl 300)
  [    3.109904] ata16: SATA link down (SStatus 0 SControl 300)
  [    3.413903] ata17: SATA link down (SStatus 0 SControl 300)
  [    3.717921] ata18: SATA link down (SStatus 0 SControl 300)
  [    4.021916] ata19: SATA link down (SStatus 0 SControl 300)
  [    4.325896] ata20: SATA link down (SStatus 0 SControl 300)
  [    4.629898] ata21: SATA link down (SStatus 0 SControl 300)
  [    4.933899] ata22: SATA link down (SStatus 0 SControl 300)
  [    5.237899] ata23: SATA link down (SStatus 0 SControl 300)
  [    5.541899] ata24: SATA link down (SStatus 0 SControl 300)
  [    5.845901] ata25: SATA link down (SStatus 0 SControl 300)
  [    6.149898] ata26: SATA link down (SStatus 0 SControl 300)
  [    6.453933] ata27: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    6.457071] ata27.00: supports DRM functions and may not be fully accessible
  [    6.457073] ata27.00: ATA-11: Samsung SSD 870 QVO 2TB, SVQ02B6Q, max UDMA/133
  [    6.457593] ata27.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    6.460241] ata27.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    6.460659] ata27.00: supports DRM functions and may not be fully accessible
  [    6.464099] ata27.00: configured for UDMA/133
  [    6.464204] scsi 26:0:0:0: Direct-Access     ATA      Samsung SSD 870  2B6Q PQ: 0 ANSI: 5
  [    6.464422] sd 26:0:0:0: Attached scsi generic sg5 type 0
  [    6.464422] ata27.00: Enabling discard_zeroes_data
  [    6.464448] sd 26:0:0:0: [sdf] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    6.464459] sd 26:0:0:0: [sdf] Write Protect is off
  [    6.464461] sd 26:0:0:0: [sdf] Mode Sense: 00 3a 00 00
  [    6.464477] sd 26:0:0:0: [sdf] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    6.464510] sd 26:0:0:0: [sdf] Preferred minimum I/O size 512 bytes
  [    6.464718] ata27.00: Enabling discard_zeroes_data
  [    6.465850]  sdf: sdf1 sdf9
  [    6.467517] sd 26:0:0:0: [sdf] supports TCG Opal
  [    6.467519] sd 26:0:0:0: [sdf] Attached SCSI disk
  [    6.773913] ata28: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    6.774229] ata28.00: supports DRM functions and may not be fully accessible
  [    6.774241] ata28.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    6.774741] ata28.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    6.777151] ata28.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    6.777541] ata28.00: supports DRM functions and may not be fully accessible
  [    6.780667] ata28.00: configured for UDMA/133
  [    6.780767] scsi 27:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    6.780993] sd 27:0:0:0: Attached scsi generic sg6 type 0
  [    6.781012] ata28.00: Enabling discard_zeroes_data
  [    6.781033] sd 27:0:0:0: [sdg] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    6.781051] sd 27:0:0:0: [sdg] Write Protect is off
  [    6.781053] sd 27:0:0:0: [sdg] Mode Sense: 00 3a 00 00
  [    6.781069] sd 27:0:0:0: [sdg] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    6.781090] sd 27:0:0:0: [sdg] Preferred minimum I/O size 512 bytes
  [    6.781272] ata28.00: Enabling discard_zeroes_data
  [    6.782478]  sdg: sdg1 sdg9
  [    6.784101] sd 27:0:0:0: [sdg] supports TCG Opal
  [    6.784104] sd 27:0:0:0: [sdg] Attached SCSI disk
  [    7.085907] ata29: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    7.086224] ata29.00: supports DRM functions and may not be fully accessible
  [    7.086226] ata29.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    7.086735] ata29.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    7.089167] ata29.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    7.089572] ata29.00: supports DRM functions and may not be fully accessible
  [    7.092651] ata29.00: configured for UDMA/133
  [    7.092752] scsi 28:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    7.092983] sd 28:0:0:0: Attached scsi generic sg7 type 0
  [    7.093007] ata29.00: Enabling discard_zeroes_data
  [    7.093028] sd 28:0:0:0: [sdh] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    7.093034] sd 28:0:0:0: [sdh] Write Protect is off
  [    7.093035] sd 28:0:0:0: [sdh] Mode Sense: 00 3a 00 00
  [    7.093042] sd 28:0:0:0: [sdh] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    7.093078] sd 28:0:0:0: [sdh] Preferred minimum I/O size 512 bytes
  [    7.093291] ata29.00: Enabling discard_zeroes_data
  [    7.094277]  sdh: sdh1 sdh9
  [    7.095831] sd 28:0:0:0: [sdh] supports TCG Opal
  [    7.095833] sd 28:0:0:0: [sdh] Attached SCSI disk
  [    7.397905] ata30: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
  [    7.398231] ata30.00: supports DRM functions and may not be fully accessible
  [    7.398233] ata30.00: ATA-11: Samsung SSD 870 EVO 2TB, SVT03B6Q, max UDMA/133
  [    7.398732] ata30.00: 3907029168 sectors, multi 1: LBA48 NCQ (depth 32), AA
  [    7.401168] ata30.00: Features: Trust Dev-Sleep NCQ-sndrcv
  [    7.401573] ata30.00: supports DRM functions and may not be fully accessible
  [    7.404652] ata30.00: configured for UDMA/133
  [    7.404787] scsi 29:0:0:0: Direct-Access     ATA      Samsung SSD 870  3B6Q PQ: 0 ANSI: 5
  [    7.405019] sd 29:0:0:0: Attached scsi generic sg8 type 0
  [    7.405021] ata30.00: Enabling discard_zeroes_data
  [    7.405028] sd 29:0:0:0: [sdi] 3907029168 512-byte logical blocks: (2.00 TB/1.82 TiB)
  [    7.405033] sd 29:0:0:0: [sdi] Write Protect is off
  [    7.405048] sd 29:0:0:0: [sdi] Mode Sense: 00 3a 00 00
  [    7.405054] sd 29:0:0:0: [sdi] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
  [    7.405080] sd 29:0:0:0: [sdi] Preferred minimum I/O size 512 bytes
  [    7.405235] ata30.00: Enabling discard_zeroes_data
  [    7.406284]  sdi: sdi1 sdi9
  [    7.407845] sd 29:0:0:0: [sdi] supports TCG Opal
  [    7.407847] sd 29:0:0:0: [sdi] Attached SCSI disk
  [    8.338333] EXT4-fs (dm-1): mounted filesystem 82c3aa13-f45d-477b-b91c-4089b49c4443 ro with ordered data mode. Quota mode: none.
  [    8.559180] systemd[1]: Created slice system-zfs\x2dimport.slice - Slice /system/zfs-import.
  [    8.634743] zfs: module license 'CDDL' taints kernel.
  [    8.634764] zfs: module license taints kernel.
  [   10.098591] ZFS: Loaded module v2.2.3-pve2, ZFS pool version 5000, ZFS filesystem version 5
```
</details>

