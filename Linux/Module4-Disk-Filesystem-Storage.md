# 🟨 MODULE 4: Disk, Filesystem & Storage

> Complete interview preparation - Simple and Easy to Understand

---

## Q1. Difference Between df and du Commands.

### **Definition**
- **df**: Disk Free - Shows filesystem disk space usage
- **du**: Disk Usage - Shows file and directory space usage

### **Explain (Detail)**

**df Command:**
- Reports **entire filesystem** usage
- Shows **used and available** space per filesystem
- Displays at **mount point level**
- Shows **total, used, available, percentage**
- Measures **block allocation** (what's reserved)
- **Faster** - reads filesystem metadata only
- Good for: Checking overall disk space on system

**du Command:**
- Shows **specific file/directory** usage
- Reports actual **file sizes**
- Scans **directories recursively**
- Shows what's **consuming space**
- More **accurate** for finding large files
- Can take **longer** on large directories
- Good for: Finding which files/dirs use space

**Key Differences:**
```
df  → System-wide view (shows all filesystems)
du  → Detailed view (shows specific files/dirs)
```

**Real-World Example:**
```
Scenario: Disk is 100% full

Step 1: Use df to check which filesystem is full
df -h
Output: /dev/sda1  50G  50G  0G  100%  /

Step 2: Use du to find what's taking space
du -sh /* | sort -rh
Output:
/var    30G
/home   15G
/tmp    5G

Step 3: Drill down into /var
du -sh /var/* | sort -rh
Output:
/var/log    25G
/var/cache   5G

Now you know log files are causing the issue!
```

### **Use**
- **df**: Monitor overall disk space, check filesystem health
- **du**: Find large files/directories, clean up space
- **Both**: Together for complete disk management
- **Daily**: Check `df -h` to monitor disk usage
- **Troubleshooting**: Use `du` when disk is full

### **Command**
```bash
# df - view disk space
df -h                    # Human-readable (GB, MB)
df -Th                   # Show filesystem type
df -i                    # Show inode usage

# df examples
df -hT | grep -E '(Filesystem|/dev/sd)'
df -h --total             # Show total summary

# du - estimate directory size
du -h directory          # Human-readable
du -sh directory         # Summary (total only)
du -ah directory         # Show all files
du -h --max-depth=1 /   # Top-level directories
du -sh * | sort -h      # Sort by size

# du examples
du -sh /var/log/*        # Check log sizes
du -ah | grep -G '[0-9]+G'  # Find files over 1GB
du -sk * | sort -n | tail  # Top 10 largest

# Real-world troubleshooting
df -h                    # Check disk usage
du -sh /var/*           # Find large dirs
du -ah /var/log | sort -rh | head -10  # Largest log files
```

---

## Q2. How to Check if Disk is Full or Inode is Full?

### **Definition**
- **Disk Full**: No space available on storage device
- **Inode Full**: Reached maximum number of files allowed on filesystem

### **Explain (Detail)**

**Disk Full:**
- Means **no space left** for new data
- `df -h` shows **100% usage**
- Cannot create **any new file**
- Common causes: Large files, logs, backups
- Solution: Delete/move large files, clean up

**Inode Full:**
- Means **reached maximum file count**
- Each file/directory uses **1 inode**
- Shows disk space available but can't create files
- Same error message as disk full!
- Common causes: Too many small files
- Solution: Delete many small files

**How Inodes Work:**
```
Total Inodes = Fixed number (created at filesystem format)

Example: Filesystem has 1,000,000 inodes
- Create 1 file = Use 1 inode
- Create 1,000,000 files = Use all inodes (even if each file is 1KB!)
- Cannot create file #1,000,001 = Inode full error
```

**Difference in Output:**
```
Disk Full:
$ df -h
/dev/sda1    50G   50G   0G  100%  /
              ↑
              No space left

Inode Full:
$ df -h
/dev/sda1    50G   25G  25G   50%  /    ← Has space!

$ df -i
/dev/sda1    5M    5M   0M  100%  /    ← But inodes full!
                    ↑
                    All inodes used
```

**Common Causes:**

**Disk Full:**
- Large log files
- Backup files
- User downloads (videos, ISOs)
- Database growth
- Not cleaning up old files

**Inode Full:**
- Mail servers (millions of small emails)
- Web cache directories
- Application temp files
- Too many small files
- Not cleaning up old files

**Real-World Scenarios:**

**Scenario 1: Disk Full**
```
$ df -h
/dev/sda1    50G   50G   0G  100%  /

Error: No space left on device
Solution: Find and delete large files
```

**Scenario 2: Inode Full**
```
$ df -h
/dev/sda1    50G   25G  25G   50%  /  ← Space available!

Error: No space left on device
Check inodes:
$ df -i
/dev/sda1    5M    5M   0M  100%  /  ← Inodes full!

Solution: Delete many small files
```

### **Use**
- **Daily Monitoring**: Always check both `df -h` and `df -i`
- **Alert Setup**: Monitor both disk space and inode usage
- **Prevent Issues**: Set alerts at 80% for both
- **Troubleshooting**: Check both when "no space" error occurs
- **Filesystem Planning**: Plan inode count based on expected files

### **Command**
```bash
# Check disk space
df -h

# Check inode usage
df -i

# Check both together
df -hi

# Check specific filesystem
df -hi /var
df -hi /home

# Find filesystems at risk (>80%)
df -h | awk '$5 > 80 {print}'
df -i | awk '$5 > 80 {print}'

# Monitor with watch (live)
watch -n 5 'df -h && df -i'

# Find directories using most inodes
echo "Top directories by file count:"
sudo du -sh /* 2>/dev/null | sort -hr | head -5

# Count files in directory
find /path -type f | wc -l        # Count files
find /path -type d | wc -l        # Count directories

# Real-world troubleshooting
# Step 1: Check disk and inodes
df -hi

# Step 2: If inode full, find directories with most files
cd /
sudo du -sh */ 2>/dev/null | sort -hr | head -5

# Step 3: Drill into largest directory
cd /path/to/largest
sudo find . -type f | wc -l

# Step 4: Delete old files
sudo find . -name "*.tmp" -delete

# Step 5: Monitor improvement
watch -n 10 'df -i'
```

---

## Q3. How to Check Disk Partitions (lsblk, fdisk)?

### **Definition**
Commands to view and manage disk partitions on Linux system.

### **Explain (Detail)**

**Disk Partitions Concept:**
```
Physical Disk: /dev/sda (entire 500GB disk)
    ↓ Partition into sections
Partition 1:  /dev/sda1 (1GB - /boot)
Partition 2:  /dev/sda2 (50GB - /)
Partition 3:  /dev/sda3 (4GB - swap)
Partition 4:  /dev/sda4 (445GB - /home)
```

**lsblk Command:**
- Lists all **block devices** (disks, partitions, LVM, etc.)
- Shows **tree structure** (hierarchical view)
- Displays: Disk name, size, type, mount point
- **Read-only** - safe to use anytime
- Shows: LVM volumes, RAID, USB drives, etc.
- Very **user-friendly** output

**lsblk Output Explained:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk
├─sda1   8:1    0   1G  0 part /boot
├─sda2   8:2    0  50G  0 part /
├─sda3   8:3    0   4G  0 part [SWAP]
└─sda4   8:4    0  445G 0 part /home

NAME        → Device name (sda = disk, sda1 = partition)
MAJ:MIN     → Device numbers (system internal)
RM          → Removable? (1 = yes, 0 = no)
SIZE        → Disk/partition size
RO          → Read-only? (1 = yes, 0 = no)
TYPE        → disk/partition/lvm/crypt/rom
MOUNTPOINT  → Where mounted (if mounted)
```

**fdisk Command:**
- **Partition table editor** - create/delete/modify partitions
- **Interactive** - requires root, can modify disk
- Shows **detailed partition information**
- **DANGER**: Can erase data if used incorrectly
- Supports **MBR** and **GPT** partition tables

**Partition Table Types:**

**MBR (Master Boot Record):**
- Maximum **4 primary partitions**
- Maximum disk size: **2TB**
- Older standard
- Boot sector at start of disk
- Most older systems use this

**GPT (GUID Partition Table):**
- Up to **128 partitions** (theoretically unlimited)
- **No disk size limit**
- Modern standard
- Required for **UEFI boot**
- Better data integrity
- Recommended for new systems

**When to Use Each:**
```
lsblk:
✅ View current disk layout
✅ Check what's mounted
✅ Identify disks and partitions
✅ Safe, can be run by any user
✅ Quick overview

fdisk:
✅ Create new partitions
✅ Delete partitions
✅ Change partition types
⚠️  Requires root
⚠️  Can destroy data - use with caution!
```

**Block Device Naming:**
```
/dev/sda, /dev/sdb, /dev/sdc     → SATA/SCSI disks
/dev/nvme0n1, /dev/nvme1n1       → NVMe SSDs (modern)
/dev/vda, /dev/vdb                → Virtual disks (VMs)
/dev/mapper/vgname-lvname         → LVM logical volumes
```

**Real-World Use Cases:**

**Use Case 1: Add new disk**
```
Step 1: Identify new disk
lsblk

Step 2: Partition new disk
sudo fdisk /dev/sdb

Step 3: Format partition
sudo mkfs.ext4 /dev/sdb1

Step 4: Mount
sudo mount /dev/sdb1 /mnt/data
```

**Use Case 2: Check disk layout**
```
lsblk -f
Shows all disks, partitions, filesystems, and mount points
```

**Use Case 3: View partition details**
```
sudo fdisk -l /dev/sda
Shows partition table, start/end sectors, sizes
```

### **Use**
- **lsblk**: View disk layout, identify new disks, check mounts
- **fdisk**: Create/delete partitions, modify disk structure
- **Planning**: Before adding new disks or repartitioning
- **Troubleshooting**: When disk not showing or mount issues
- **System Administration**: Managing storage devices

### **Command**
```bash
# lsblk - list block devices
lsblk                    # Basic tree view
lsblk -f                 # Show filesystem types, UUIDs
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT  # Custom output
lsblk -m                 # Show permissions
lsblk -l                 # List format (no tree)

# lsblk examples
lsblk -f                 # Shows UUID, FSTYPE, LABEL
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
lsblk -d                 # Disks only (no partitions)

# fdisk - partition disk
sudo fdisk -l            # List all partitions
sudo fdisk /dev/sda      # Edit disk sda

# fdisk interactive commands:
# m  → Show help
# p  → Print partition table
# n  → Create new partition
# d  → Delete partition
# w  → Write changes (COMMIT!)
# q  → Quit without saving

# View partition table (read-only)
sudo fdisk -l /dev/sda
sudo fdisk -l | grep -E '(Disk /dev|/dev/sd)'

# Check specific disk details
sudo fdisk -l /dev/sda | grep -E '(Disk|Sector|cylinders)'

# Identify new disk
lsblk
lsblk -o NAME,SIZE,MOUNTPOINT,MODEL

# Real-world: Add new disk
# Step 1: Identify new disk
lsblk

# Step 2: Partition disk
sudo fdisk /dev/sdb
# Inside fdisk:
# n (new)
# p (primary)
# 1 (partition 1)
# Enter (default start)
# Enter (default end)
# w (write)

# Step 3: Verify
lsblk /dev/sdb

# Step 4: Format
sudo mkfs.ext4 /dev/sdb1

# Step 5: Mount
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
```

---

## Q4. What is /etc/fstab and How is It Used?

### **Definition**
**/etc/fstab** (File System TABle) - Configuration file that defines which filesystems mount at system boot.

### **Explain (Detail)**

**Purpose of /etc/fstab:**
- Stores **mount information** for all filesystems
- System **reads at boot** and automatically mounts everything listed
- Ensures **filesystems available** after system restart
- No need to manually mount every time

**fstab Structure (6 fields per line):**
```
/dev/sda1    /boot    ext4    defaults    0    1
   │            │        │          │         │    │
   │            │        │          │         │    └─ fsck check order
   │            │        │          │         └───── Dump (backup)
   │            │        │          └────────── Mount options
   │            │        └───────────────────── Filesystem type
   │            └────────────────────────────── Mount point
   └───────────────────────────────────────── Device/UUID
```

**Field 1: Device (What to mount)**
```
/dev/sda1           → By device name (can change - not recommended)
UUID=1234-5678      → By UUID (recommended - stable)
LABEL=mylabel        → By label (good, but can change)
/dev/mapper/vg0-lv1  → LVM logical volume
//server/share       → Network share (NFS, SMB)
```

**Field 2: Mount Point (Where to mount)**
```
/                    → Root filesystem
/home                → User home directories
/boot                → Boot files
/var                 → Variable data (logs, cache)
/tmp                 → Temporary files
/mnt/data            → Custom mount point
```

**Field 3: Filesystem Type**
```
ext4                 → Most common Linux filesystem
xfs                  → Enterprise filesystem
btrfs                → Advanced with snapshots
swap                 → Swap partition
auto                 → Auto-detect (CD-ROM, USB)
nfs                  → Network filesystem
cifs                 → Windows share (SMB)
tmpfs                → In RAM filesystem
```

**Field 4: Mount Options (Comma-separated)**
```
defaults             → rw, suid, dev, exec, auto, nouser, async
ro                   → Read-only
rw                   → Read-write
noexec               → Don't allow executing programs
nosuid               → Don't allow SUID/SGID
noatime              → Don't update access time (faster)
noauto               → Don't mount at boot
user                 → Allow normal users to mount
sync                 → Synchronous writes (safer, slower)
```

**Field 5: Dump (Backup)**
```
0 → Don't backup with dump (most common)
1 → Backup daily
2 → Backup less frequently
```

**Field 6: fsck (Check order at boot)**
```
0 → Don't check (swap, network, tmpfs)
1 → Check first (root filesystem /)
2 → Check after root (other local filesystems)
```

**Why Use UUID?**
```
/dev/sda1:
- Can change if you add/remove disks
- Not reliable across reboots

UUID=1234-5678:
- Unique identifier
- Never changes
- Recommended for /etc/fstab
```

**Example /etc/fstab:**
```
# <device>       <mount>    <type>  <options>            <dump> <pass>
UUID=abcd-1234   /          ext4    errors=remount-ro    0      1
UUID=efgh-5678   /boot      ext4    defaults            0      2
UUID=ijkl-9012   /home      ext4    defaults            0      2
UUID=mnop-3456   none       swap    sw                  0      0
/dev/sr0        /media/cd  iso9660  ro,user,noauto      0      0
```

**When System Reads fstab:**
```
1. Boot process starts
2. Kernel mounts root (/)
3. System reads /etc/fstab
4. Mounts all filesystems marked "auto"
5. System boot completes
```

**Best Practices:**
```
✅ Use UUID instead of device names
✅ Test mount manually before adding to fstab
✅ Use "mount -a" to test after editing
✅ Backup fstab before editing
✅ Set correct fsck order (1 for /, 2 for others)

❌ Don't use device names (/dev/sda1)
❌ Don't edit fstab without testing
❌ Don't use wrong filesystem type
```

### **Use**
- **System Boot**: Automatically mount filesystems on startup
- **Persistence**: Keep mounts across reboots
- **Organization**: Define all mounts in one place
- **Administration**: Centralized mount configuration
- **Backups**: Define mount points for backup scripts

### **Command**
```bash
# View /etc/fstab
cat /etc/fstab

# View with line numbers
cat -n /etc/fstab

# Test all fstab entries (no reboot needed)
sudo mount -a

# Get UUID for fstab
sudo blkid /dev/sdb1
lsblk -f

# Create mount point
sudo mkdir -p /mnt/newdisk

# Add entry to fstab
echo "UUID=$(sudo blkid -s UUID -o value /dev/sdb1) /mnt/newdisk ext4 defaults 0 2" | sudo tee -a /etc/fstab

# Test fstab entry
sudo mount -a

# Backup fstab before editing
sudo cp /etc/fstab /etc/fstab.backup

# Real-world: Add new disk to fstab
# Step 1: Get UUID
sudo blkid /dev/sdb1
# Output: UUID="abcd-1234-5678"

# Step 2: Create mount point
sudo mkdir -p /mnt/data

# Step 3: Test mount manually
sudo mount /dev/sdb1 /mnt/data
df -h | grep sdb1

# Step 4: Add to fstab
sudo nano /etc/fstab
# Add: UUID=abcd-1234-5678 /mnt/data ext4 defaults 0 2

# Step 5: Test
sudo mount -a

# Step 6: Verify
df -h | grep mnt/data
```

---

## Q5. How to Mount and Unmount Filesystems?

### **Definition**
- **mount**: Attach filesystem to directory (make accessible)
- **umount**: Detach filesystem from directory (make inaccessible)

### **Explain (Detail)**

**Mounting Process:**
```
Before mount:
/dev/sdb1 exists but not accessible
Files on /dev/sdb1 cannot be read

After mount to /mnt/data:
/dev/sdb1 files accessible at /mnt/data
Can read/write files at /mnt/data

Unmount /mnt/data:
Filesystem detached, no longer accessible
```

**How Mounting Works:**
```
Linux Directory Tree:
/ (root)
├── /home
├── /var
└── /mnt/data (empty directory)

After mounting /dev/sdb1 to /mnt/data:
/ (root)
├── /home
├── /var
└── /mnt/data (now shows /dev/sdb1 contents)
```

**Mount Point:**
- Directory where filesystem is attached
- Must **exist** before mounting
- Can be any empty directory
- **Empty directories recommended**
- Mounting hides original directory contents

**Common Mount Scenarios:**

**1. Mount Partition:**
```bash
mount /dev/sdb1 /mnt/data
```

**2. Mount with Filesystem Type:**
```bash
mount -t ext4 /dev/sdb1 /mnt/data
mount -t ntfs /dev/sdb1 /mnt/windows
mount -t vfat /dev/sdb1 /mnt/usb
```

**3. Mount with Options:**
```bash
mount -o ro /dev/sdb1 /mnt/data      # Read-only
mount -o rw /dev/sdb1 /mnt/data      # Read-write
mount -o noexec /dev/sdb1 /mnt/data  # No execute
mount -o noatime /dev/sdb1 /mnt/data # Performance
```

**4. Mount by UUID:**
```bash
mount UUID=abcd-1234 /mnt/data
```

**5. Mount ISO Image:**
```bash
mount -o loop image.iso /mnt/iso
```

**6. Mount Network Share:**
```bash
# NFS
mount server:/export /mnt/nfs

# CIFS (Windows share)
mount -t cifs //server/share /mnt/smb -o user=username
```

**Unmounting Issues:**
```
Error: "Device is busy"

Why:
- User has file open in that directory
- Process running from that directory
- User's current directory is the mount point
- Files are being used

Solution:
1. Check who's using it:
   fuser -m /mount/point
   lsof /mount/point

2. Change directory out of mount point:
   cd /

3. Kill processes using it (if needed):
   fuser -km /mount/point

4. Try umount again:
   umount /mount/point
```

**Lazy Unmount:**
```bash
umount -l /mount/point
# Detaches immediately, cleanup happens later
# Useful when device is busy
```

**Bind Mount:**
```bash
mount --bind /source /dest
# Make directory available at another location
# Useful for chroot, containers
```

### **Use**
- **Access Storage**: Make partitions/disks accessible
- **External Storage**: USB drives, external hard drives
- **Network Storage**: NFS, SMB/CIFS shares
- **ISO Images**: Mount CD/DVD/ISO files
- **Persistence**: Keep mounts across reboots (via fstab)
- **Troubleshooting**: Test filesystems, recovery

### **Command**
```bash
# Mount commands
sudo mount /dev/sdb1 /mnt/data                    # Basic
sudo mount -t ext4 /dev/sdb1 /mnt/data             # With type
sudo mount -o ro /dev/sdb1 /mnt/data               # Read-only
sudo mount -o noatime /dev/sdb1 /mnt/data         # Performance
sudo mount UUID=abcd-1234 /mnt/data                # By UUID

# Mount ISO
sudo mount -o loop image.iso /mnt/iso

# Mount network share
sudo mount server:/export /mnt/nfs                 # NFS
sudo mount -t cifs //server/share /mnt/smb -o user=username  # SMB

# Unmount commands
sudo umount /mnt/data
sudo umount /dev/sdb1
sudo umount -l /mnt/data                           # Lazy

# Check mount status
mount
df -h
lsblk
mountpoint /mnt/data

# Find what's using mount point
sudo lsof /mnt/data
sudo fuser -m /mnt/data

# Kill processes and unmount
sudo fuser -km /mnt/data
sudo umount /mnt/data

# Remount with different options
sudo mount -o remount,ro /mnt/data

# Real-world: Setup new disk
# Step 1: Identify disk
lsblk

# Step 2: Create mount point
sudo mkdir -p /mnt/data

# Step 3: Mount temporarily
sudo mount /dev/sdb1 /mnt/data

# Step 4: Test
df -h | grep sdb1
ls /mnt/data

# Step 5: Add to fstab for permanent mount
sudo blkid /dev/sdb1
# Add to /etc/fstab:
# UUID=xxx /mnt/data ext4 defaults 0 2

# Step 6: Test fstab
sudo mount -a
```

---

## Q6. What is LVM (Logical Volume Manager)?

### **Definition**
LVM - Abstraction layer over physical disks that allows flexible disk management with resizing and pooling.

### **Explain (Detail)**

**Why LVM Was Created:**
```
Traditional Partitions:
/dev/sda1, /dev/sda2, /dev/sda3
- Fixed sizes, hard to resize
- Limited to physical disk size
- Can't extend across disks

LVM:
- Resize volumes dynamically
- Combine multiple disks
- Create snapshots
- Easy management
```

**LVM Architecture (3 Layers):**
```
Layer 1: Physical Volumes (PV)
  ↓ Actual physical disks/partitions
  /dev/sda1, /dev/sdb1, /dev/nvme0n1p2

Layer 2: Volume Groups (VG)
  ↓ Pool of physical volumes
  vg_data, vg_system, vg_backup

Layer 3: Logical Volumes (LV)
  ↓ Usable volumes (like partitions)
  /dev/vg_data/lv_home, /dev/vg_system/lv_root
```

**3 Layers Explained:**

**1. Physical Volume (PV):**
- Physical disk or partition
- Must be initialized for LVM
- Can be entire disk or partition
- Shows as "PV" in commands

**2. Volume Group (VG):**
- Pool of one or more PVs
- Acts as single large storage pool
- Can span multiple physical disks
- Total space = sum of all PVs in VG

**3. Logical Volume (LV):**
- Partition-like volume created from VG
- Can be resized dynamically
- Can be striped across PVs
- Can have snapshots
- Acts like normal partition to OS

**LVM Benefits:**
```
1. Flexible Resizing
   - Enlarge LV when needed
   - Shrink LV when space available
   - Online (no downtime)

2. Disk Pooling
   - Combine multiple disks into one VG
   - Use space from all disks together

3. Snapshots
   - Point-in-time copy of LV
   - Backup without downtime
   - Rollback capability

4. Easy Management
   - Create/resize/delete easily
   - No reboot required
   - Logical names (not sda1, sda2)
```

**LVM Naming:**
```
PV: /dev/sda1, /dev/sdb1
VG: vg_system, vg_data
LV: lv_root, lv_home, lv_var
Full path: /dev/vg_system/lv_root
```

**Creating LVM (Step by Step):**
```
Step 1: Initialize PV
pvcreate /dev/sdb1

Step 2: Create VG from PV
vgcreate vg_data /dev/sdb1

Step 3: Create LV from VG
lvcreate -L 50G -n lv_home vg_data

Step 4: Format LV
mkfs.ext4 /dev/vg_data/lv_home

Step 5: Mount LV
mount /dev/vg_data/lv_home /home
```

**Extending LV (Add Space):**
```
Scenario: /home needs more space

1. Check VG free space:
   vgs

2. Extend LV by 20GB:
   lvextend -L +20G /dev/vg_data/lv_home

3. Resize filesystem:
   resize2fs /dev/vg_data/lv_home

Done! No reboot, no downtime
```

**Adding New Disk to LVM:**
```
1. Create PV on new disk:
   pvcreate /dev/sdb2

2. Add PV to VG:
   vgextend vg_data /dev/sdb2

3. Extend LV:
   lvextend -L +100G /dev/vg_data/lv_home

4. Resize filesystem:
   resize2fs /dev/vg_data/lv_home
```

**LVM Snapshots:**
```
Create snapshot:
lvcreate -L 10G -s -n snap_home /dev/vg_data/lv_home

Use for backup:
- Backup from snapshot
- No downtime

Delete snapshot:
lvremove /dev/vg_data/snap_home
```

**When to Use LVM:**
```
✅ Use LVM:
- Server with multiple disks
- Need flexible storage
- Future growth expected
- Want snapshot capability
- Production servers

❌ Don't Use LVM:
- Simple single-disk setup
- Boot partition (GRUB issues)
- Embedded systems (extra complexity)
```

### **Use**
- **Flexible Storage**: Resize partitions without reboot
- **Disk Pooling**: Combine multiple disks into one pool
- **Snapshots**: Create backups without downtime
- **Server Administration**: Easy volume management
- **Cloud/VM**: Dynamic disk sizing
- **Production Systems**: Reliable, flexible storage

### **Command**
```bash
# PV (Physical Volume) commands
sudo pvcreate /dev/sdb1           # Initialize PV
sudo pvdisplay                    # Display PV details
sudo pvs                          # List all PVs
sudo pvremove /dev/sdb1           # Remove PV

# VG (Volume Group) commands
sudo vgcreate vg_data /dev/sdb1   # Create VG
sudo vgdisplay                    # Display VG details
sudo vgs                          # List all VGs
sudo vgremove vg_data             # Remove VG
sudo vgextend vg_data /dev/sdb2   # Add PV to VG

# LV (Logical Volume) commands
sudo lvcreate -L 50G -n lv_home vg_data      # Create 50GB LV
sudo lvcreate -l 100%FREE -n lv_var vg_data  # Use all free space
sudo lvdisplay                               # Display LV details
sudo lvs                                     # List all LVs
sudo lvremove /dev/vg_data/lv_home           # Remove LV
sudo lvextend -L +20G /dev/vg_data/lv_home   # Extend by 20GB
sudo lvextend -L 100G /dev/vg_data/lv_home   # Extend to 100GB

# Filesystem resize
sudo resize2fs /dev/vg_data/lv_home          # Resize ext4
sudo xfs_growfs /dev/vg_data/lv_home         # Resize XFS

# Snapshots
sudo lvcreate -L 10G -s -n snap /dev/vg/lv   # Create snapshot
sudo lvremove /dev/vg/snap                   # Remove snapshot

# View LVM structure
sudo pvs
sudo vgs
sudo lvs

# Real-world: Create LVM from scratch
# Step 1: Create PV
sudo pvcreate /dev/sdb1

# Step 2: Create VG
sudo vgcreate vg_system /dev/sdb1

# Step 3: Create LVs
sudo lvcreate -L 50G -n lv_root vg_system
sudo lvcreate -l 100%FREE -n lv_home vg_system

# Step 4: Format
sudo mkfs.ext4 /dev/vg_system/lv_root
sudo mkfs.ext4 /dev/vg_system/lv_home

# Step 5: Mount
sudo mount /dev/vg_system/lv_root /mnt/root
sudo mount /dev/vg_system/lv_home /mnt/home

# Real-world: Extend LV
# Step 1: Check VG free space
sudo vgs

# Step 2: Extend LV
sudo lvextend -L +20G /dev/vg_data/lv_home

# Step 3: Resize filesystem
sudo resize2fs /dev/vg_data/lv_home

# Step 4: Verify
df -h | grep home
```

---

## Q7. Different Types of Filesystems (ext4, xfs, btrfs).

### **Definition**
Filesystem types - Methods of organizing and storing data on storage devices.

### **Explain (Detail)**

**What is a Filesystem:**
```
Filesystem provides rules for:
- How to create files
- How to delete files
- Where to store data on disk
- How to find files later
```

**ext4 (Fourth Extended Filesystem):**
```
Features:
- Journaling (prevents corruption on crash)
- Max file size: 16TB
- Max filesystem size: 1EB
- Max files: 4 billion
- Fast for small files
- Stable, mature (since 2008)
- Default on most Linux distros

Use Cases:
- Desktop systems
- Small servers
- Boot partitions
- General purpose

Pros:
✅ Stable and tested
✅ Good performance
✅ Can shrink and grow
✅ Wide compatibility
✅ Simple to manage

Cons:
❌ No built-in snapshots
❌ No native compression
❌ Large overhead for many small files
```

**xfs (XFS Filesystem):**
```
Features:
- Journaling
- Max file size: 8EB
- Max filesystem size: 8EB
- Excellent for large files
- High performance
- Can't shrink (only grow)
- Default on RHEL/CentOS

Use Cases:
- Large file servers
- Databases
- Enterprise systems
- High-performance needs

Pros:
✅ Excellent large file performance
✅ Handles huge filesystems
✅ Good scalability
✅ Minimal fragmentation

Cons:
❌ Can't shrink filesystem
❌ Higher overhead for small files
❌ More complex recovery
```

**btrfs (B-Tree Filesystem):**
```
Features:
- Copy-on-write (COW)
- Built-in snapshots
- Built-in compression
- Built-in subvolumes
- Built-in RAID
- Checksums (data integrity)
- Max file: 16EB
- Max filesystem: 16EB

Use Cases:
- Modern workstations
- Backup systems
- Development/testing
- Systems needing snapshots

Pros:
✅ Built-in snapshots
✅ Compression saves space
✅ Data integrity checksums
✅ Subvolumes (flexible organization)

Cons:
❌ Newer, less tested
❌ More complex
❌ Performance overhead
❌ Can be unstable on older kernels
```

**Comparison Table:**
```
Feature          ext4     xfs       btrfs
Max File Size   16TB      8EB       16EB
Max FS Size     1EB       8EB       16EB
Shrink          ✅        ❌        ✅
Grow            ✅        ✅        ✅
Snapshots       ❌        ❌        ✅
Compression     ❌        ❌        ✅
RAID            ❌        ❌        ✅
Maturity        ✅ High   ✅ High   ⚠️ Medium
Default         Ubuntu    RHEL      Fedora
```

**How to Choose:**

**Choose ext4 when:**
```
✅ Need stability (production)
✅ Need to shrink filesystem
✅ Small to medium files
✅ Simple setup
✅ Wide compatibility
✅ Boot partition
```

**Choose xfs when:**
```
✅ Large files (videos, databases)
✅ Need high performance
✅ RHEL/CentOS system
✅ Never need to shrink
✅ Enterprise environment
```

**Choose btrfs when:**
```
✅ Need built-in snapshots
✅ Want compression
✅ Data integrity critical
✅ Modern hardware
✅ Development/testing
```

**Performance Characteristics:**
```
Small Files (< 100KB):    ext4 (best) > btrfs > xfs
Large Files (> 100MB):    xfs (best) > btrfs > ext4
Mixed Workloads:          ext4 (best) > xfs > btrfs
```

### **Use**
- **ext4**: General use, boot partitions, stability
- **xfs**: Large files, databases, high performance
- **btrfs**: Snapshots, compression, modern features

### **Command**
```bash
# Create filesystems
sudo mkfs.ext4 /dev/sdb1              # Create ext4
sudo mkfs.ext4 -L "mylabel" /dev/sdb1 # With label
sudo mkfs.xfs /dev/sdb1               # Create xfs
sudo mkfs.btrfs /dev/sdb1            # Create btrfs

# View filesystem type
df -T
lsblk -f
blkid

# Resize filesystems
# ext4 (can grow and shrink)
sudo resize2fs /dev/vg0/lv_home              # Grow
sudo resize2fs /dev/vg0/lv_home 50G         # Shrink

# xfs (can only grow)
sudo xfs_growfs /mount/point                 # Grow to max
# Cannot shrink xfs!

# btrfs (can grow and shrink)
sudo btrfs filesystem resize max /mnt/data   # Grow
sudo btrfs filesystem resize -50G /mnt/data # Shrink

# btrfs snapshots
sudo btrfs subvolume create /mnt/data/subvol
sudo btrfs subvolume snapshot /mnt/data/subvol /mnt/data/snap
sudo btrfs subvolume delete /mnt/data/snap

# btrfs compression
sudo mount -o compress=lzo /dev/sdb1 /mnt/data

# Real-world: Format and use ext4
sudo mkfs.ext4 /dev/sdb1
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
df -hT | grep sdb1
```

---

## Q8. How to Find Large Files and Directories?

### **Definition**
Commands and techniques to identify files and directories consuming most disk space.

### **Explain (Detail)**

**Why Find Large Files:**
```
Common Scenarios:
- Disk full alerts
- Need to free space
- Identify space hogs
- Clean up old files
- Plan capacity
- Troubleshoot performance
```

**Using du Command:**
```
du (Disk Usage):
- Shows space used by files/directories
- Can scan recursively
- Can sort results
- Shows actual file sizes

Key Options:
-h           → Human-readable (K, M, G)
-s           → Summary only (total)
-a           → Show all files
--max-depth=1 → Top-level only
```

**Finding Largest Directories:**
```
Step 1: Check root directory
du -sh /*

Step 2: Find largest (top 10)
du -sh /* | sort -rh | head -10

Step 3: Drill down into largest
cd /var
du -sh * | sort -rh | head -10
```

**Finding Large Files:**
```
By size:
find /path -type f -size +100M      # Files > 100MB
find /path -type f -size +1G       # Files > 1GB

By type:
find /var/log -name "*.log" -size +50M
find /home -name "*.mp4" -size +500M

By time and size:
find /path -type f -mtime +30 -size +100M
# Files older than 30 days, larger than 100MB
```

**Sorting Files by Size:**
```
Find + sort:
find /path -type f -exec du -h {} + | sort -rh | head -20

Summary of directories:
du -sh /path/* | sort -rh | head -20
```

**Common Space Hogs:**
```
/var/log        → Log files (often large)
/var/cache      → Application cache
/tmp            → Temporary files
/home/user      → User files (downloads, videos)
/var/lib/mysql  → Database files
```

**Disk Cleanup Strategy:**
```
Step 1: Check disk usage
df -h

Step 2: Find largest directories
du -sh /* | sort -rh | head -5

Step 3: Drill down
cd /largest_directory
du -sh * | sort -rh | head -10

Step 4: Identify files
find . -size +100M -ls

Step 5: Review before deleting
ls -lh /path/to/large_file

Step 6: Clean up
rm /path/to/large_file

Step 7: Verify space freed
df -h
```

### **Use**
- **Troubleshooting**: Find disk space issues
- **Capacity Planning**: Identify growing directories
- **Cleanup**: Remove unnecessary large files
- **Monitoring**: Regular space audits
- **Optimization**: Reorganize files by size
- **Backup Planning**: Identify what to backup

### **Command**
```bash
# du - find large directories
du -sh /*                                   # Root directories
du -sh /var/*                               # /var subdirectories
du -ah /path | sort -rh | head -20           # Top 20 largest
du -sh * | sort -rh | head -10               # Current dir

# du with depth
du -h --max-depth=1 /path                    # Top-level
du -h --max-depth=2 /path                    # Two levels

# find - find large files
find /path -type f -size +100M               # Files > 100MB
find /path -type f -size +1G                # Files > 1GB
find /path -type f -size +500M              # Files > 500MB

# find + du - sorted large files
find /path -type f -exec du -h {} + | sort -rh | head -20

# find by type and size
find /var/log -name "*.log" -size +50M       # Large logs
find /home -name "*.mp4" -size +500M        # Large videos

# find by time and size
find /path -type f -mtime +7 -size +100M    # Old + large

# Check specific locations
du -sh /var/log/* | sort -rh               # Log files
du -sh /home/* | sort -rh                  # User homes

# Find files over 1GB system-wide
find / -size +1G -type f 2>/dev/null

# Count files by size
find /path -type f -size +100M | wc -l
find /path -type f -size +1G | wc -l

# Monitor disk usage in real-time
watch -n 5 'du -sh /path/*'

# Real-world troubleshooting
# Step 1: Check disk
df -h

# Step 2: Find largest top-level directories
sudo du -sh /* 2>/dev/null | sort -rh | head -5

# Step 3: Drill into largest
sudo du -sh /var/* 2>/dev/null | sort -rh | head -10

# Step 4: Find large log files
sudo find /var/log -name "*.log" -size +100M -ls

# Step 5: Verify space freed
df -h
```

---

## 📚 Quick Reference - Module 4 Commands

### Disk Usage (df, du)
```bash
df -h                        # Disk space
df -i                        # Inode usage
df -hT                       # With filesystem type
du -sh directory              # Directory size
du -sh * | sort -rh          # Sorted by size
```

### Partitions (lsblk, fdisk)
```bash
lsblk                        # List disks
lsblk -f                     # With filesystem
sudo fdisk -l                # List partitions
```

### Mount/Unmount
```bash
mount /dev/sdb1 /mnt/data    # Mount
umount /mnt/data             # Unmount
mount -a                     # Mount all from fstab
mount -o ro device mount     # Read-only
```

### LVM
```bash
pvcreate /dev/sdb1           # Create PV
vgcreate vg /dev/sdb1         # Create VG
lvcreate -L 50G -n lv vg    # Create LV
pvs, vgs, lvs               # List PV/VG/LV
lvextend -L +20G /dev/vg/lv  # Extend LV
resize2fs /dev/vg/lv        # Resize FS (ext4)
```

### Filesystem
```bash
mkfs.ext4 /dev/sdb1          # Create ext4
mkfs.xfs /dev/sdb1           # Create xfs
mkfs.btrfs /dev/sdb1        # Create btrfs
```

### Find Large Files
```bash
find /path -size +1G         # Files > 1GB
du -sh * | sort -rh          # Largest dirs
```

---

**Next:** Module 5 - Networking & Connectivity
