# Linux LVM Storage Management

## Project Overview
Configured a 10 GiB virtual disk on Red Hat Enterprise Linux
using Logical Volume Manager (LVM).

Created a Physical Volume, Volume Group, Logical Volume,
and XFS filesystem. Configured persistent mounting and
expanded storage from 5 GiB to 8 GiB without downtime
or data loss.

## Environment
- OS: Red Hat Enterprise Linux
- Virtualization: Oracle VirtualBox
- Remote Access: PuTTY (SSH)
- Practice Disk: /dev/sdb (10 GiB)
- Volume Group: vg_lab
- Logical Volume: lv_data
- Filesystem: XFS
- Mount Point: /mnt/labdata

## LVM Architecture
Physical Disk -> Physical Volume -> Volume Group
-> Logical Volume -> XFS Filesystem -> Mount Point

## Implementation

### Create Physical Volume
sudo pvcreate /dev/sdb

### Create Volume Group
sudo vgcreate vg_lab /dev/sdb

### Create Logical Volume
sudo lvcreate -L 5G -n lv_data vg_lab

### Create XFS Filesystem
sudo mkfs.xfs /dev/vg_lab/lv_data

### Mount Storage
sudo mkdir -p /mnt/labdata
sudo mount /dev/vg_lab/lv_data /mnt/labdata

### Persistent Mounting
Configured /etc/fstab using the filesystem UUID.
Verified automatic mounting after reboot.

### Expand Logical Volume
sudo lvextend -L 8G /dev/vg_lab/lv_data

### Grow XFS Filesystem
sudo xfs_growfs /mnt/labdata

## Verification Commands
sudo pvs
sudo vgs
sudo lvs
lsblk -f
df -hT /mnt/labdata
findmnt /mnt/labdata
sha256sum /mnt/labdata/testfile.txt

## Results
- Created 10 GiB LVM storage pool.
- Created 5 GiB logical volume.
- Configured persistent XFS mounting.
- Successfully expanded storage to 8 GiB.
- Verified file integrity using SHA-256.
- No reboot required during storage expansion.

## Key Learnings
- Linux disk management
- Physical Volumes (PV)
- Volume Groups (VG)
- Logical Volumes (LV)
- XFS filesystem administration
- Persistent mounting with /etc/fstab
- Online storage expansion
- Data integrity verification
