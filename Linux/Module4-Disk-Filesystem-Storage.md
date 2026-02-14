# 🟨 MODULE 4: Disk, Filesystem & Storage

> Complete interview preparation for Disk, Filesystem & Storage

---

## Q1. Difference Between df and du Commands.

### **Definition**
- **df**: Disk Free - reports filesystem disk space usage
- **du**: Disk Usage - estimates file and directory space usage

### **Explain (Detailed)**

**df Command:**
- Shows **entire filesystem** disk usage
- Reports space **used and available**
- Shows information **per filesystem** (mount points)
- Displays **block-level usage** (blocks allocated)
- Shows **total, used, available, and percentage**
- Measures at **filesystem level**
- **Faster** as it reads filesystem metadata
- Good for: Checking overall disk space on system

**du Command:**
- Shows **file and directory** usage
- Reports space **consumed by specific files/dirs**
- Can scan **subdirectories recursively**
- Shows **actual file sizes** (not block allocation)
- More **accurate** for specific directories
- Can take **longer** on large directories
- Good for: Finding which files/dirs consume space

**Key Differences:**
```
df  → System-wide view (filesystem level)
du  → Detailed view (file/directory level)
```

**Example Scenario:**
- Use `df` when: System alerts "disk full" - check overall space
- Use `du` when: Know disk is full but don't know which directory

**Output Differences:**
- `df` shows: /dev/sda1, /dev/sda2, mount points
- `du` shows: /var/log, /home/user, actual file sizes

### **Use**
- **df**: Monitor overall disk space, check filesystem health
- **du**: Find large files/directories, clean up space
- **Both**: Together for complete disk management
- **Daily monitoring**: Check `df` for available space
- **Troubleshooting**: Use `du` to find space hogs

### **Command**
```bash
# df - view all filesystems disk space
df -h                    # Human-readable format (GB, MB)
df -Th                   # Show filesystem type
df -i                    # Show inode usage
df -hT /path             # Check specific filesystem

# df examples
df -hT | grep -E '(Filesystem|/dev/sd)'
df -h --total             # Show total summary
df -H                    # 1000-based (not 1024)

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
du -cxh /home/user       # Count and show sizes

# Find largest directories
du -h --max-depth=2 / | sort -hr | head -20

# Combine df and du for troubleshooting
df -h                    # Check disk usage
du -sh /var/*           # Find large dirs
du -ah /var/log | sort -rh | head -10  # Largest log files
```

---

## Q2. How to Check if Disk is Full or Inode is Full?

### **Definition**
- **Disk Full**: No space available on storage
- **Inode Full**: No more file entries available (max files reached)

### **Explain (Detailed)**

**Disk Full:**
- Means **no space left** for new files
- Shows as **100% usage** in `df`
- Cannot create new files even if few files exist
- Common causes: Large files, logs, backups
- Solution: Delete/move large files, clean logs

**Inode Full:**
- Means **reached maximum number of files**
- Each file/directory uses **1 inode**
- Shows disk space available but can't create files
- **Error message**: "No space left on device" (even if space available!)
- Common causes: Too many small files, temp files, cache
- Solution: Delete many small files, increase inode count

**How Inodes Work:**
```
Total Inodes = Fixed number (created at filesystem creation)
Each file = 1 inode
Each directory = 1 inode (directory entries don't count)

If you have 1 million files = 1 million inodes used
Even if each file is 1KB = still 1 million inodes
```

**Checking Methods:**
```
1. Disk Space: df -h
2. Inode Usage: df -i
3. Look for: Use% column near 100%
```

**Symptoms of Each:**
```
Disk Full:
- df -h shows 100%
- Cannot create ANY file
- Errors: "No space left on device"

Inode Full:
- df -h shows space available
- df -i shows 100%
- Cannot create new files
- Same error: "No space left on device"
- But df -h shows free space!
```

**Real-World Scenarios:**

**Scenario 1: Disk Full**
```
df -h
/dev/sda1     50G   50G   0G  100% /
Can't create file → Delete large files
```

**Scenario 2: Inode Full**
```
df -h
/dev/sda1     50G   25G  25G   50% /  ← Has space

df -i
/dev/sda1     5M   5M   0M  100% /  ← Inodes full!

Can't create file → Delete many small files
```

**Common Causes of Inode Exhaustion:**
- Too many small files (mail servers, web caches)
- Temporary directories with millions of files
- Backup systems creating millions of small files
- Application creating many small temp files
- Not cleaning up old files

### **Use**
- **Daily Monitoring**: Check both `df -h` and `df -i`
- **Alert Setup**: Monitor both disk space and inode usage
- **Prevent Issues**: Set alerts at 80% for both
- **Troubleshooting**: Always check both when "no space" error
- **Filesystem Creation**: Plan inode count based on expected files

### **Command**
```bash
# Check disk space
df -h
# Output shows:
# Filesystem  Size  Used  Avail  Use%  Mounted on
# /dev/sda1    50G   25G   25G   50%   /

# Check inode usage
df -i
# Output shows:
# Filesystem  Inodes IUsed IFree IUse% Mounted on
# /dev/sda1   5M    5M     0M   100%  /

# Check both disk and inodes together
df -hi
# -h: human-readable
# -i: inode information

# Find filesystems at risk
df -h | awk '5 > 80 {print}'           # >80% disk used
df -i | awk '5 > 80 {print}'           # >80% inodes used

# Check specific filesystem
df -hi /var
df -hi /home

# Monitor with watch (live updates)
watch -n 5 'df -h && df -i'

# Find directories using most inodes
echo "Detailed inode usage by directory:"
for d in /*; do
  echo "$d: $(sudo find "$d" -xdev | wc -l) files"
done | sort -k2 -rn | head -10

# Find files using inodes in current directory
find . -printf '%i\n' | sort -u | wc -l

# Count files in directory (finds inode usage)
find /path -type f | wc -l        # Count files
find /path -type d | wc -l        # Count directories

# Real-world troubleshooting
# 1. Check disk and inodes
df -hi

# 2. If inode full, find directories with most files
sudo du -sh /* 2>/dev/null | sort -hr | head -5

# 3. Drill down into largest directory
cd /path/to/largest
find . -type f | wc -l

# 4. Delete old files or increase inode (requires format)
sudo find . -name "*.tmp" -delete

# 5. Monitor improvement
watch -n 10 'df -i'
```

---

## Q3. How to Check Disk Partitions (lsblk, fdisk)?

### **Definition**
Commands to view and manage disk partitions on Linux system.

### **Explain (Detailed)**

**Disk Partitions - Basic Concept:**
```
Physical Disk → /dev/sda (entire disk)
    ↓
Partition 1 → /dev/sda1 (first partition)
Partition 2 → /dev/sda2 (second partition)
Partition 3 → /dev/sda3 (third partition)
```

**lsblk Command:**
- **Lists Block Devices** - shows all storage devices
- **Tree structure** - hierarchical view
- **Shows**: Disk, partitions, mount points, size, type
- **Read-only** - safe to view anytime
- **Shows**: LVM, RAID, USB drives, etc.
- **User-friendly** - easy to read output

**lsblk Output Explained:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk
├─sda1   8:1    0   1G  0 part /boot
├─sda2   8:2    0  50G  0 part /
├─sda3   8:3    0   4G  0 part [SWAP]
└─sda4   8:4    0  445G 0 part /home

NAME     → Device name (sda = disk, sda1 = partition)
MAJ:MIN  → Major:Minor device numbers
RM       → Removable (1 = yes, 0 = no)
SIZE     → Disk/partition size
RO       → Read-only (1 = yes, 0 = no)
TYPE     → disk/partition/lvm/crypt/rom
MOUNTPOINT → Where mounted (if mounted)
```

**fdisk Command:**
- **Partition table editor** - create, delete, modify partitions
- **Interactive** - requires root, can modify disk
- **Shows**: Detailed partition information
- **DANGER**: Can erase data if used incorrectly
- **Supports**: MBR and GPT partition tables

**Partition Table Types:**
```
MBR (Master Boot Record):
- Maximum 4 primary partitions
- Maximum disk size: 2TB
- Older standard
- Boot sector at start of disk

GPT (GUID Partition Table):
- Up to 128 partitions (theoretically unlimited)
- No disk size limit
- Modern standard
- Required for UEFI boot
- Better data integrity
```

**When to Use Each:**
```
lsblk:
- View current disk layout
- Check what's mounted
- Identify disks and partitions
- Safe, can be run by any user

fdisk:
- Create new partitions
- Delete partitions
- Change partition types
- Requires root, can destroy data
- Use with caution!
```

**Practical Use Cases:**
- Add new disk → lsblk to identify, fdisk to partition
- Check available space → lsblk to see unpartitioned space
- Expand disk → fdisk to modify, resize partition
- Troubleshoot mount → lsblk to check device name

**Block Device Naming:**
```
/dev/sda, /dev/sdb, /dev/sdc     → SATA/SCSI disks
/dev/nvme0n1, /dev/nvme1n1       → NVMe SSDs
/dev/vda, /dev/vdb                → Virtual disks (VMs)
/dev/mapper/vgname-lvname          → LVM logical volumes
```

### **Use**
- **lsblk**: View disk layout, identify new disks, check mounts
- **fdisk**: Create/delete partitions, modify disk structure
- **Planning**: Before adding new disks or repartitioning
- **Troubleshooting**: When disk not showing or mount issues
- **System Administration**: Managing storage devices

### **Command**
```bash
# lsblk - list all block devices
lsblk                    # Basic tree view
lsblk -f                 # Show filesystem types, UUIDs
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT  # Custom output
lsblk -m                 # Show permissions
lsblk -l                 # List format (no tree)

# lsblk examples
lsblk -f                 # Shows UUID, FSTYPE, LABEL, MOUNTPOINT
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
lsblk -d                 # Show only disks (no partitions)

# fdisk - partition disk
sudo fdisk -l            # List all disks and partitions
sudo fdisk /dev/sda      # Start fdisk on disk sda

# fdisk interactive commands (inside fdisk):
# m  → Show help/menu
# p  → Print partition table
# n  → Create new partition
# d  → Delete partition
# t  → Change partition type
# w  → Write changes to disk (COMMIT!)
# q  → Quit without saving

# View partition table (read-only)
sudo fdisk -l /dev/sda
sudo fdisk -l | grep -E '(Disk /dev|/dev/sd)'

# Check specific disk details
sudo fdisk -l /dev/sda | grep -E '(Disk|Sector| cylinders)'

# Identify new disk (after plugging in)
lsblk                    # Shows all disks
lsblk -o NAME,SIZE,MOUNTPOINT,MODEL

# Real-world scenario: Add new disk
# 1. Identify new disk
lsblk
# Output shows new /dev/sdb with no partitions

# 2. Partition new disk
sudo fdisk /dev/sdb
# Inside fdisk:
# n (new partition)
# p (primary)
# 1 (partition 1)
# Enter (default first sector)
# Enter (default last sector)
# w (write changes)

# 3. Verify partition created
lsblk /dev/sdb

# 4. Format partition
sudo mkfs.ext4 /dev/sdb1

# 5. Mount (see next question)
```

---

## Q4. What is /etc/fstab and How is It Used?

### **Definition**
**/etc/fstab** (File System TABle) - configuration file that defines disk drives, partitions, and other storage devices to be automatically mounted at boot.

### **Explain (Detailed)**

**Purpose of /etc/fstab:**
- Stores **mount information** for all filesystems
- System **reads at boot** and mounts everything listed
- Ensures **filesystems available** after system restart
- Simplifies mounting - no need to manually mount every time

**fstab Structure (6 fields per line):**
```
/dev/sda1    /boot    ext4    defaults    0    1
   │            │        │          │         │    │
   │            │        │          │         │    └─ Dump (backup order)
   │            │        │          │         └───── fsck check order
   │            │        │          └────────── Mount options
   │            │        └───────────────────── Filesystem type
   │            └────────────────────────────── Mount point
   └───────────────────────────────────────── Device/UUID
```

**Field 1: Device (What to mount)**
```
/dev/sda1           → By device name (not recommended - can change)
UUID=1234-5678      → By UUID (recommended - stable)
LABEL=mylabel        → By label (good, but labels can change)
/dev/mapper/vg0-lv1  → LVM logical volume
/path/to/file.img     → Disk image file (loop mount)
//server/share       → Network share (NFS, SMB)
```

**Field 2: Mount Point (Where to mount)**
```
/                    → Root filesystem
/home                → User home directories
/boot                → Boot files
/var                 → Variable data
/tmp                 → Temporary files
/mnt/data             → Custom mount point
/media/usb           → Removable media
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
Common Options:
defaults             → rw, suid, dev, exec, auto, nouser, async
ro                   → Read-only
rw                   → Read-write
noauto               → Don't mount automatically at boot
noexec               → Don't allow executing programs
nosuid               → Don't allow SUID/SGID
nodev                → Don't interpret device files
sync                 → Write synchronously (safer, slower)
async                → Write asynchronously (faster)
user                 → Allow normal users to mount
users                → Any user can mount/umount
noatime              → Don't update file access time (performance)
nodiratime           → Don't update dir access time

Combinations:
ro,noexec,nosuid     → Secure read-only mount
defaults,noatime      → Better performance
rw,users,noauto      → User-mountable USB drive
```

**Field 5: Dump (Backup)**
```
0 → Don't backup with dump utility
1 → Backup daily
2 → Backup less frequently

Most modern systems: Use 0 (dump rarely used)
```

**Field 6: fsck (Check order)**
```
0 → Don't check at boot (swap, network, tmpfs)
1 → Check first (root filesystem /)
2 → Check after root (other local filesystems)

Example:
/          → 1 (check first)
/boot      → 2 (check after root)
/home      → 2 (check after root)
/var       → 2 (check after root)
swap       → 0 (don't check)
```

**Example /etc/fstab:**
```
# <device>       <mount>    <type>  <options>            <dump> <pass>
UUID=abcd-1234   /          ext4    errors=remount-ro    0      1
UUID=efgh-5678   /boot      ext4    defaults            0      2
UUID=ijkl-9012   /home      ext4    defaults            0      2
UUID=mnop-3456   none       swap    sw                  0      0
/dev/sr0        /media/cd  iso9660  ro,user,noauto      0      0
//server/share  /mnt/share cifs    credentials=/etc/.smb,uid=1000 0 0
tmpfs            /tmp       tmpfs   defaults,noatime    0      0
```

**Why Use UUID Instead of Device Name?**
```
/dev/sda1:
- Can change if you add/remove disks
- Not reliable across reboots

UUID=1234-5678:
- Unique identifier for partition
- Never changes
- Recommended for /etc/fstab

How to get UUID:
blkid /dev/sda1  → Shows UUID, TYPE, LABEL
lsblk -f           → Shows UUID, FSTYPE, MOUNTPOINT
```

**When System Reads fstab:**
```
1. Boot process starts
2. Kernel mounts root (/)
3. System reads /etc/fstab
4. Mounts all filesystems marked "auto"
5. System boot completes

If fstab has error:
- System may not boot
- Drops to maintenance mode
- Need to fix fstab manually
```

**Troubleshooting fstab Issues:**
```
Error at boot: "Failed to mount /home"
- Check fstab syntax
- Verify device exists (lsblk, blkid)
- Verify mount point exists (mkdir)
- Test mount manually first!

Safe testing:
mount -a    → Test all fstab entries without reboot
```

**Best Practices:**
```
✅ Use UUID instead of device names
✅ Test mount manually before adding to fstab
✅ Use "mount -a" to test after editing
✅ Backup fstab before editing
✅ Use "noauto" for removable media
✅ Set correct fsck order (1 for /, 2 for others, 0 for swap)

❌ Don't use device names (/dev/sda1) in fstab
❌ Don't edit fstab without testing
❌ Don't use wrong filesystem type
❌ Don't forget to create mount point directory
```

### **Use**
- **System Boot**: Automatically mount filesystems on startup
- **Persistence**: Keep mounts across reboots
- **Organization**: Define all mounts in one place
- **Administration**: Centralized mount configuration
- **Troubleshooting**: Reference for mounted filesystems
- **Backups**: Define mount points for backup scripts

### **Command**
```bash
# View /etc/fstab
cat /etc/fstab

# View fstab with line numbers
cat -n /etc/fstab

# Test all fstab entries (no reboot needed)
sudo mount -a

# Test specific mount
mount /mount/point

# Get UUID for fstab
sudo blkid /dev/sda1
sudo blkid | grep sda1
lsblk -f /dev/sda1

# Get filesystem type
lsblk -f
df -T
blkid

# Create mount point (before adding to fstab)
sudo mkdir -p /mnt/newdisk

# Add entry to fstab (example)
echo "UUID=$(sudo blkid -s UUID -o value /dev/sdb1) /mnt/newdisk ext4 defaults 0 2" | sudo tee -a /etc/fstab

# Real-world: Add new disk to fstab
# 1. Get UUID
sudo blkid /dev/sdb1
# Output: UUID="abcd-1234-5678"

# 2. Create mount point
sudo mkdir -p /mnt/data

# 3. Test mount manually
sudo mount /dev/sdb1 /mnt/data
df -h | grep sdb1

# 4. If works, add to fstab
sudo nano /etc/fstab
# Add line: UUID=abcd-1234-5678 /mnt/data ext4 defaults 0 2

# 5. Test fstab entry
sudo mount -a

# 6. Verify
df -h | grep mnt/data

# 7. Reboot to test (or use mount -a)

# Common fstab examples
# Standard partition:
# UUID=xxx /home ext4 defaults 0 2

# Swap partition:
# UUID=xxx none swap sw 0 0

# NFS mount:
# server:/export /mnt/nfs nfs defaults 0 0

# Windows share:
# //server/share /mnt/smb cifs credentials=/etc/.smb,uid=1000,gid=1000 0 0

# Temporary filesystem (in RAM):
# tmpfs /tmp tmpfs defaults,noatime,size=2G 0 0

# Backup fstab before editing
sudo cp /etc/fstab /etc/fstab.backup

# Check fstab syntax after editing
sudo mount -a --fake --verbose

# Unmount and remove from fstab (cleanup)
sudo umount /mnt/data
sudo nano /etc/fstab
# Comment out or remove the line
```

---

## Q5. How to Mount and Unmount Filesystems?

### **Definition**
- **mount**: Attach a filesystem to directory tree (make accessible)
- **umount**: Detach filesystem from directory tree (make inaccessible)

### **Explain (Detailed)**

**Mounting Process:**
```
Before mount:
/dev/sdb1 exists but not accessible
Files on /dev/sdb1 cannot be read

After mount /mnt/data:
/dev/sdb1 files accessible at /mnt/data
Can read/write files at /mnt/data

Unmount /mnt/data:
/filesystem no longer accessible
/directory becomes empty (or shows original contents)
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

**Mount Command Syntax:**
```bash
mount [options] device directory
```

**Common Mount Scenarios:**

**1. Mount Partition:**
```bash
mount /dev/sdb1 /mnt/data
```

**2. Mount with Filesystem Type:**
```bash
mount -t ext4 /dev/sdb1 /mnt/data
mount -t xfs /dev/sdb1 /mnt/data
mount -t ntfs /dev/sdb1 /mnt/windows
mount -t vfat /dev/sdb1 /mnt/usb
```

**3. Mount with Options:**
```bash
mount -o ro /dev/sdb1 /mnt/data       # Read-only
mount -o rw /dev/sdb1 /mnt/data       # Read-write
mount -o noexec /dev/sdb1 /mnt/data   # No execute
mount -o nosuid /dev/sdb1 /mnt/data    # No SUID/SGID
mount -o noatime /dev/sdb1 /mnt/data   # Performance
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

**Unmounting Process:**
```
Before umount:
Filesystem is accessible
Files can be read/written

After umount:
Filesystem is detached
Cannot access files anymore
Directory shows original contents (or is empty)
```

**Unmount Command:**
```bash
umount /mount/point
# or
umount /dev/sdb1
```

**Unmount Issues - "Device is busy":**
```
Cannot unmount if:
- User has file open in that directory
- Process running from that directory
- User's current directory is the mount point
- Files in use

Solution:
fuser -m /mount/point         # See who's using it
lsof /mount/point            # See open files
cd /                          # Change out of directory
kill processes using it
Then try umount again
```

**Lazy Unmount:**
```bash
umount -l /mount/point
# Detaches immediately, cleanup happens later
# Useful when device is busy but not critical
```

**Force Unmount:**
```bash
umount -f /mount/point
# Force unmount (NFS only)
# Can cause data corruption - use with caution
```

**Mount at Boot (fstab):**
```
Add to /etc/fstab:
UUID=abcd-1234 /mnt/data ext4 defaults 0 2

System automatically mounts at boot
Or test with: mount -a
```

**Bind Mount:**
```
Make directory available at another location:
mount --bind /source /dest
mount --bind /var/www/html /mnt/www

Useful for:
- Chroot environments
- Container filesystem isolation
- Accessing files in multiple places
```

**Temporary Mount (tmpfs):**
```
Mount filesystem in RAM:
mount -t tmpfs -o size=1G tmpfs /mnt/ram

Useful for:
- Temporary fast storage
- Cache directories
- Testing without disk I/O
```

**Mount Options (Common):**
```
ro           → Read-only
rw           → Read-write
exec         → Allow execution
noexec       → Prevent execution
suid         → Allow SUID
nosuid       → Prevent SUID
dev          → Allow device files
nodev        → Prevent device files
sync         → Synchronous writes (safer)
async        → Asynchronous writes (faster)
atime        → Update access time
noatime      → Don't update access time (faster)
auto         → Mount at boot (fstab)
noauto       → Don't mount at boot
user         → Allow normal users to mount
nouser       → Only root can mount
defaults     → Common options set
```

### **Use**
- **Access Storage**: Make partitions/disks accessible
- **System Configuration**: Mount required filesystems
- **External Storage**: USB drives, external hard drives
- **Network Storage**: NFS, SMB/CIFS shares
- **ISO Images**: Mount CD/DVD/ISO files
- **Persistence**: Keep mounts across reboots (via fstab)
- **Troubleshooting**: Test filesystems, recovery

### **Command**
```bash
# Mount - basic usage
sudo mount /dev/sdb1 /mnt/data

# Mount with filesystem type
sudo mount -t ext4 /dev/sdb1 /mnt/data
sudo mount -t xfs /dev/sdb1 /mnt/data
sudo mount -t ntfs-3g /dev/sdb1 /mnt/windows

# Mount with options
sudo mount -o ro /dev/sdb1 /mnt/data              # Read-only
sudo mount -o rw /dev/sdb1 /mnt/data              # Read-write
sudo mount -o noexec /dev/sdb1 /mnt/data          # No execute
sudo mount -o noatime /dev/sdb1 /mnt/data        # Performance

# Mount by UUID
sudo mount UUID=abcd-1234 /mnt/data

# Get UUID
sudo blkid /dev/sdb1
lsblk -f

# View mounted filesystems
mount
df -h
lsblk

# Mount specific entries from fstab
sudo mount -a                    # All fstab entries
sudo mount /home                 # From fstab /home

# Mount ISO image
sudo mount -o loop image.iso /mnt/iso

# Mount loop device (disk image)
sudo losetup /dev/loop0 disk.img
sudo mount /dev/loop0 /mnt/image

# Unmount
sudo umount /mnt/data
sudo umount /dev/sdb1

# Unmount all filesystems
sudo umount -a

# Lazy unmount (detach even if busy)
sudo umount -l /mnt/data

# Force unmount (NFS only)
sudo umount -f /mnt/nfs

# Check if mount point is busy
sudo lsof /mnt/data
sudo fuser -m /mnt/data

# Kill processes using mount point
sudo fuser -km /mnt/data
sudo umount /mnt/data

# Bind mount (mount same directory elsewhere)
sudo mount --bind /var/www/html /mnt/www

# Temporary filesystem in RAM
sudo mount -t tmpfs -o size=1G tmpfs /mnt/ram

# Mount network share (NFS)
sudo mount server:/export /mnt/nfs

# Mount Windows share (CIFS)
sudo mount -t cifs //server/share /mnt/smb \
  -o user=username,password=pass,uid=1000

# Remount with different options
sudo mount -o remount,ro /mnt/data

# View mount details
mount | grep sdb1
findmnt /mnt/data
mountpoint /mnt/data

# Create mount point before mounting
sudo mkdir -p /mnt/newdisk
sudo mount /dev/sdb1 /mnt/newdisk

# Real-world: Setup new disk
# 1. Identify disk
lsblk

# 2. Create partition (see fdisk section)
sudo fdisk /dev/sdb

# 3. Format
sudo mkfs.ext4 /dev/sdb1

# 4. Create mount point
sudo mkdir -p /mnt/data

# 5. Mount temporarily
sudo mount /dev/sdb1 /mnt/data

# 6. Test
df -h | grep sdb1
ls /mnt/data

# 7. Add to fstab for permanent mount
sudo blkid /dev/sdb1
# Add to /etc/fstab:
# UUID=xxx /mnt/data ext4 defaults 0 2

# 8. Test fstab
sudo mount -a

# 9. Done - will mount at boot

# Cleanup - unmount and remove
sudo umount /mnt/data
sudo rm -r /mnt/data
```

---

## Q6. What is LVM (Logical Volume Manager)?

### **Definition**
LVM - abstraction layer over physical disks, allows flexible disk management with resizing and pooling.

### **Explain (Detailed)**

**Why LVM Was Created:**
```
Traditional Partitioning:
/dev/sda1, /dev/sda2, /dev/sda3
- Fixed sizes
- Hard to resize
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

**Physical Volume (PV):**
- Physical disk or partition
- Must be initialized for LVM
- Can be entire disk or partition
- Shows as "PV" in commands

**Volume Group (VG):**
- Pool of one or more PVs
- Acts as single large storage pool
- Can span multiple physical disks
- Total space = sum of all PVs in VG

**Logical Volume (LV):**
- Partition-like volume created from VG
- Can be resized dynamically
- Can be striped across PVs
- Can have snapshots
- Acts like normal partition to OS

**LVM Benefits:**
```
1. Flexible Resizing
   - Enlarge LV when space needed
   - Shrink LV when space available
   - Online (no downtime)

2. Disk Pooling
   - Combine multiple disks into one VG
   - Use space from all disks together

3. Snapshots
   - Point-in-time copy of LV
   - Backup without downtime
   - Rollback capability

4. Better Management
   - Create/resize/delete easily
   - No reboot required
   - Logical names (not sda1, sda2)

5. Disk Replacement
   - Move data between disks
   - Replace failing disk easily
```

**LVM Naming Convention:**
```
PV: /dev/sda1, /dev/sdb1
VG: vg_system, vg_data
LV: lv_root, lv_home, lv_var
Full path: /dev/vg_system/lv_root
```

**Creating LVM Structure:**
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

**Extending LV (Adding Space):**
```
Scenario: /home needs more space

1. Check VG free space
vgs

2. Extend LV
lvextend -L +20G /dev/vg_data/lv_home

3. Resize filesystem
resize2fs /dev/vg_data/lv_home

Done! No reboot, no downtime
```

**Adding New Disk to LVM:**
```
1. Create PV on new disk
pvcreate /dev/sdb2

2. Add PV to VG
vgextend vg_data /dev/sdb2

3. Extend LV to use new space
lvextend -L +100G /dev/vg_data/lv_home

4. Resize filesystem
resize2fs /dev/vg_data/lv_home
```

**LVM Snapshots:**
```
Create snapshot:
lvcreate -L 10G -s -n snapshot_name /dev/vg_data/lv_home

Use snapshot for backup
Backup完成后删除:
lvremove /dev/vg_data/snapshot_name

Snapshots take space!
- Must be large enough for changes
- Deleted when full (unless configured otherwise)
```

**LVM Metadata:**
- Stored at start of each PV
- Tracks PV, VG, LV configuration
- Can be backed up
- Can be restored if corrupted

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
- Need maximum performance (small overhead)
```

**LVM vs Traditional Partitions:**
```
              Partitions          LVM
Flexibility   ❌ Low             ✅ High
Resize        ❌ Difficult        ✅ Easy
Snapshots     ❌ No              ✅ Yes
Complexity    ✅ Low             ❌ Higher
Performance  ✅ Best            ⚠️ Slight overhead
Boot          ✅ Simple         ⚠️ Needs initramfs
```

**LVM Commands Summary:**
```
PV Commands:
pvcreate, pvdisplay, pvs, pvremove, pvscan

VG Commands:
vgcreate, vgdisplay, vgs, vgremove, vgextend, vgreduce

LV Commands:
lvcreate, lvdisplay, lvs, lvremove, lvextend, lvreduce
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
# LVM - Physical Volume (PV) commands
sudo pvcreate /dev/sdb1              # Initialize PV
sudo pvdisplay                        # Display PV details
sudo pvs                             # List all PVs
sudo pvremove /dev/sdb1              # Remove PV (WARNING!)
sudo pvscan                           # Scan for PVs

# LVM - Volume Group (VG) commands
sudo vgcreate vg_data /dev/sdb1        # Create VG
sudo vgdisplay                        # Display VG details
sudo vgs                              # List all VGs
sudo vgremove vg_data                 # Remove VG (WARNING!)
sudo vgextend vg_data /dev/sdb2       # Add PV to VG
sudo vgreduce vg_data /dev/sdb1       # Remove PV from VG

# LVM - Logical Volume (LV) commands
sudo lvcreate -L 50G -n lv_home vg_data    # Create LV (50GB)
sudo lvcreate -l 100%FREE -n lv_var vg_data  # Use all free space
sudo lvdisplay                                 # Display LV details
sudo lvs                                        # List all LVs
sudo lvremove /dev/vg_data/lv_home             # Remove LV
sudo lvextend -L +20G /dev/vg_data/lv_home    # Extend by 20GB
sudo lvextend -L 100G /dev/vg_data/lv_home    # Extend to 100GB
sudo lvreduce -L 40G /dev/vg_data/lv_home     # Reduce to 40GB
sudo lvresize -L 60G /dev/vg_data/lv_home     # Resize to 60GB

# LVM Snapshots
sudo lvcreate -L 10G -s -n snap_home /dev/vg_data/lv_home   # Create snapshot
sudo lvdisplay /dev/vg_data/snap_home                        # View snapshot
sudo lvremove /dev/vg_data/snap_home                           # Remove snapshot

# Filesystem resize (after LV resize)
sudo resize2fs /dev/vg_data/lv_home      # Resize ext4
sudo xfs_growfs /dev/vg_data/lv_home     # Resize XFS (can't shrink)

# View LVM structure
sudo pvs
sudo vgs
sudo lvs

# Detailed LVM info
sudo pvdisplay /dev/sdb1
sudo vgdisplay vg_data
sudo lvdisplay /dev/vg_data/lv_home

# Check free space in VG
sudo vgs
# Look at "VFree" column

# Real-world: Setup LVM from scratch
# Step 1: Create PV
sudo pvcreate /dev/sdb1
sudo pvs

# Step 2: Create VG
sudo vgcreate vg_system /dev/sdb1
sudo vgs

# Step 3: Create LV for root (50GB)
sudo lvcreate -L 50G -n lv_root vg_system
sudo lvs

# Step 4: Create LV for home (use remaining space)
sudo lvcreate -l 100%FREE -n lv_home vg_system
sudo lvs

# Step 5: Format LVs
sudo mkfs.ext4 /dev/vg_system/lv_root
sudo mkfs.ext4 /dev/vg_system/lv_home

# Step 6: Mount LVs
sudo mkdir -p /mnt/root /mnt/home
sudo mount /dev/vg_system/lv_root /mnt/root
sudo mount /dev/vg_system/lv_home /mnt/home

# Step 7: Add to fstab
echo "/dev/vg_system/lv_root / ext4 defaults 0 1" | sudo tee -a /etc/fstab
echo "/dev/vg_system/lv_home /home ext4 defaults 0 2" | sudo tee -a /etc/fstab

# Real-world: Extend existing LV
# Scenario: /home needs more space

# Step 1: Check VG free space
sudo vgs

# Step 2: Extend LV by 20GB
sudo lvextend -L +20G /dev/vg_system/lv_home

# Step 3: Resize filesystem (ext4)
sudo resize2fs /dev/vg_system/lv_home

# Step 4: Verify
df -h | grep home

# Real-world: Add new disk to LVM
# Step 1: Create PV on new disk
sudo pvcreate /dev/sdc1

# Step 2: Extend VG
sudo vgextend vg_system /dev/sdc1

# Step 3: Extend LV
sudo lvextend -L +100G /dev/vg_system/lv_home

# Step 4: Resize filesystem
sudo resize2fs /dev/vg_system/lv_home

# Real-world: Create snapshot before update
# Step 1: Create snapshot
sudo lvcreate -L 20G -s -n snap_before_update /dev/vg_system/lv_root

# Step 2: Do your updates
sudo apt update && sudo apt upgrade

# Step 3: Test system
# If everything OK, remove snapshot
sudo lvremove /dev/vg_system/snap_before_update

# Step 4: If problem, rollback
# Mount snapshot and restore files
```

---

## Q7. Different Types of Filesystems (ext4, xfs, btrfs).

### **Definition**
Filesystem types - methods of organizing and storing data on storage devices.

### **Explain (Detailed)**

**What is a Filesystem:**
```
Operating System needs to know:
- How to create files
- How to delete files
- Where to store data on disk
- How to find files later

Filesystem provides these rules and structure
```

**ext4 (Fourth Extended Filesystem):**
```
Features:
- Journaling (prevents corruption on crash)
- Maximum file size: 16TB
- Maximum filesystem size: 1EB (exabyte)
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
❌ Large overhead for small files
❌ No built-in snapshots
❌ No native compression
❌ No built-in encryption
```

**xfs (XFS Filesystem):**
```
Features:
- Journaling
- Maximum file size: 8EB
- Maximum filesystem size: 8EB
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
✅ Online defragmentation

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
- Self-healing (with RAID)
- Maximum file: 16EB
- Maximum filesystem: 16EB

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
✅ Built-in RAID

Cons:
❌ Newer, less tested
❌ More complex
❌ Performance overhead
❌ RAID1/RAID10 not recommended for production
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
Maturity        ✅ High    ✅ High    ⚠️ Medium
Default         Ubuntu    RHEL      Fedora
Performance     Good      Excellent Variable
```

**How to Choose Filesystem:**

**Choose ext4 when:**
```
✅ Need stability (production)
✅ Need to shrink filesystem
✅ Small to medium files
✅ Simple setup
✅ Wide compatibility needed
✅ Boot partition
```

**Choose xfs when:**
```
✅ Large files (videos, databases)
✅ Need high performance
✅ RHEL/CentOS system
✅ Never need to shrink
✅ Enterprise environment
✅ Large filesystems needed
```

**Choose btrfs when:**
```
✅ Need built-in snapshots
✅ Want compression
✅ Data integrity critical
✅ Modern hardware
✅ Development/testing
✅ Can handle complexity
```

**Journaling Explained:**
```
Before Journaling:
Write data to disk → System crash → Files corrupted

With Journaling:
Write to journal → Commit to disk
System crash → Replay journal → Recover data

ext4, xfs, btrfs all support journaling
```

**Copy-on-Write (btrfs):**
```
ext4/xfs:
Copy file = Write new copy (uses 2x space)

btrfs COW:
Copy file = Reference same data (uses minimal space)
Changes only when file modified
```

**Performance Characteristics:**

**Small Files (< 100KB):**
```
Best: ext4
Good: btrfs
Slow: xfs
```

**Large Files (> 100MB):**
```
Best: xfs
Good: btrfs
Slow: ext4
```

**Mixed Workloads:**
```
Best: ext4 (consistent)
Good: xfs
Variable: btrfs (depends on workload)
```

### **Use**
- **ext4**: General use, boot partitions, stability
- **xfs**: Large files, databases, high performance
- **btrfs**: Snapshots, compression, modern features

### **Command**
```bash
# Create ext4 filesystem
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.ext4 -L "mylabel" /dev/sdb1     # With label

# Create xfs filesystem
sudo mkfs.xfs /dev/sdb1
sudo mkfs.xfs -L "mylabel" /dev/sdb1      # With label

# Create btrfs filesystem
sudo mkfs.btrfs /dev/sdb1
sudo mkfs.btrfs -L "mylabel" /dev/sdb1   # With label

# View filesystem type
df -T
lsblk -f
blkid

# Check filesystem
sudo fsck /dev/sdb1              # ext4
sudo xfs_repair /dev/sdb1         # xfs
sudo btrfs check /dev/sdb1        # btrfs

# Resize filesystem
# ext4 (can grow and shrink)
sudo resize2fs /dev/vg0/lv_home           # Grow
sudo resize2fs /dev/vg0/lv_home 50G      # Shrink

# xfs (can only grow)
sudo xfs_growfs /mount/point               # Grow to max
# Cannot shrink xfs!

# btrfs (can grow and shrink)
sudo btrfs filesystem resize max /mnt/data     # Grow
sudo btrfs filesystem resize -50G /mnt/data   # Shrink

# btrfs snapshots
sudo btrfs subvolume create /mnt/data/subvol
sudo btrfs subvolume snapshot /mnt/data/subvol /mnt/data/snap
sudo btrfs subvolume delete /mnt/data/snap

# btrfs compression
sudo mount -o compress=lzo /dev/sdb1 /mnt/data
sudo btrfs filesystem defrag -r -c /mnt/data

# Real-world: Format and use ext4
sudo mkfs.ext4 /dev/sdb1
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
df -hT | grep sdb1

# Real-world: Format and use xfs
sudo mkfs.xfs /dev/sdb1
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
df -hT | grep sdb1

# Real-world: Format and use btrfs
sudo mkfs.btrfs /dev/sdb1
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
df -hT | grep sdb1

# Create btrfs subvolume
sudo btrfs subvolume create /mnt/data/subvol1
sudo btrfs subvolume list /mnt/data

# Create snapshot
sudo btrfs subvolume snapshot /mnt/data/subvol1 /mnt/data/snap1
sudo btrfs subvolume list /mnt/data

# Delete snapshot
sudo btrfs subvolume delete /mnt/data/snap1
```

---

## Q8. How to Find Large Files and Directories?

### **Definition**
Commands and techniques to identify files and directories consuming most disk space.

### **Explain (Detailed)**

**Why Find Large Files:**
```
Common Scenarios:
- Disk full alerts
- Need to free space
- Identify space hogs
- Clean up old files
- Plan capacity
- Troubleshooting performance
```

**Using du Command:**
```
du (Disk Usage):
- Shows space used by files/directories
- Can scan recursively
- Can sort results
- Shows actual file sizes (not block allocation)

Syntax:
du [options] [path]
```

**Key du Options:**
```
-h           → Human-readable (K, M, G)
-s           → Summary only (total)
-a           → Show all files
--max-depth=1 → Top-level only
--max-depth=2 → Two levels deep
-sh           → Summary + human-readable
-c           → Grand total
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

Step 4: Continue drilling
cd /var/log
du -sh * | sort -rh | head -10
```

**Finding Large Files:**
```
By size:
find /path -type f -size +100M      # Files > 100MB
find /path -type f -size +1G       # Files > 1GB
find /path -type f -size +500M      # Files > 500MB

By type:
find /path -name "*.log" -size +100M
find /path -name "*.iso" -size +1G
find /path -name "*.tar.gz" -size +500M

By time and size:
find /path -type f -mtime +30 -size +100M
# Files older than 30 days, larger than 100MB
```

**Sorting Files by Size:**
```
Find + sort:
find /path -type f -exec du -h {} + | sort -rh | head -20

With size limit:
find /path -type f -size +100M -exec du -h {} + | sort -rh | head -20

du only (faster):
du -ah /path | sort -rh | head -20

Summary of directories only:
du -sh /path/* | sort -rh | head -20
```

**Using ncdu (NCurses Disk Usage):**
```
ncdu:
- Interactive interface
- Browse directories
- Delete files from interface
- Easy to use
- Not installed by default

Install:
sudo apt install ncdu     # Debian/Ubuntu
sudo yum install ncdu     # RHEL/CentOS

Usage:
ncdu /path
# Navigate with arrows
# Press d to delete
# Press q to quit
```

**Common Space Hogs:**
```
Locations to check:
/var/log        → Log files (often large)
/var/cache      → Application cache
/tmp            → Temporary files
/home/user      → User files (downloads, videos)
/var/lib/mysql  → Database files (if using MySQL)
/opt            → Application files
/root           → Root user files
```

**Finding by File Type:**
```
Log files:
find /var/log -name "*.log" -size +50M

Database files:
find /var/lib/mysql -type f -size +1G

Video files:
find /home/user -name "*.mp4" -size +500M

Archive files:
find /path -name "*.tar.gz" -size +500M
find /path -name "*.zip" -size +500M

ISO images:
find /path -name "*.iso" -size +1G
```

**Finding Old Large Files:**
```
Older than 7 days:
find /path -type f -mtime +7 -size +100M

Older than 30 days:
find /path -type f -mtime +30 -size +100M

Older than 90 days:
find /path -type f -mtime +90 -size +100M
```

**Finding Duplicate Files:**
```
Using fdupes:
fdupes -r /path

Using find + md5sum:
find /path -type f -exec md5sum {} \; | sort | uniq -d

Using diff:
diff file1 file2
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

Step 6: Clean up (with care)
rm /path/to/large_file
# or move first
mv /path/to/large_file /backup/

Step 7: Verify space freed
df -h
```

**Automated Monitoring:**
```
Check disk usage daily:
df -h | awk '$5 > 80 {print}'

Find large files and email:
find /path -size +1G -exec ls -lh {} \; | mail -s "Large files" admin@domain.com

Cron job (daily check):
0 2 * * * /root/scripts/check-large-files.sh
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
du -sh * | sort -rh | head -10                # Current dir

# du with depth
du -h --max-depth=1 /path                    # Top-level only
du -h --max-depth=2 /path                    # Two levels deep

# find - find large files
find /path -type f -size +100M               # Files > 100MB
find /path -type f -size +1G                # Files > 1GB
find /path -type f -size +500M              # Files > 500MB

# find + du - sorted large files
find /path -type f -exec du -h {} + | sort -rh | head -20

# find by type and size
find /var/log -name "*.log" -size +50M        # Large log files
find /home -name "*.mp4" -size +500M         # Large videos
find / -name "*.iso" -size +1G               # Large ISOs

# find by time and size
find /path -type f -mtime +7 -size +100M      # Old + large
find /path -type f -mtime +30 -size +100M     # Older + larger

# Find and list details
find /path -type f -size +100M -exec ls -lh {} \;

# ncdu - interactive disk usage
sudo apt install ncdu                       # Install
ncdu /path                                 # Run
ncdu /                                     # Interactive browse

# Check specific common locations
du -sh /var/log/* | sort -rh               # Log files
du -sh /var/cache/* | sort -rh             # Cache
du -sh /home/* | sort -rh                  # User homes
du -sh /tmp/* | sort -rh                   # Temp files

# Find top 10 largest in system
du -a / | sort -rn | head -20

# Find files over 1GB system-wide
find / -size +1G -type f 2>/dev/null

# Find and delete old large files (BE CAREFUL!)
find /var/log -name "*.log.*" -mtime +30 -delete

# Monitor disk usage in real-time
watch -n 5 'du -sh /path/*'

# Find files by extension and size
find /path -name "*.tar.gz" -size +500M
find /path -name "*.zip" -size +500M
find /path -name "*.sql" -size +1G

# Count files by size
find /path -type f -size +100M | wc -l
find /path -type f -size +1G | wc -l

# Find space usage by user
find /home -type f -user username -exec du -ch {} + | tail -1

# Real-world troubleshooting workflow
# Step 1: Check disk
df -h

# Step 2: Find largest top-level directories
sudo du -sh /* 2>/dev/null | sort -rh | head -5

# Step 3: Drill into largest
sudo du -sh /var/* 2>/dev/null | sort -rh | head -10

# Step 4: Drill into logs
sudo du -sh /var/log/* 2>/dev/null | sort -rh | head -10

# Step 5: Find large log files
sudo find /var/log -name "*.log" -size +100M -ls

# Step 6: Check and clean
sudo ls -lh /var/log/large-file.log
sudo gzip /var/log/large-file.log
# or
sudo rm /var/log/large-file.log

# Step 7: Verify space freed
df -h

# Automated report (save to file)
echo "=== Large Files Report ===" > report.txt
echo "Date: $(date)" >> report.txt
echo "" >> report.txt
echo "=== Top 10 Directories ===" >> report.txt
du -sh /* 2>/dev/null | sort -rh | head -10 >> report.txt
echo "" >> report.txt
echo "=== Files > 1GB ===" >> report.txt
find / -size +1G -type f 2>/dev/null -exec ls -lh {} \; >> report.txt
```

---

## 📚 Quick Reference - Module 4 Commands

### Disk Usage (df, du)
```bash
df -h                        # Disk space
df -i                        # Inode usage
df -hT                       # With filesystem type
du -sh directory              # Directory size
du -ah directory              # All files size
du -sh * | sort -rh         # Sorted by size
```

### Partitions (lsblk, fdisk)
```bash
lsblk                        # List block devices
lsblk -f                     # With filesystem
sudo fdisk -l                # List partitions
sudo fdisk /dev/sdb          # Partition disk
```

### Mount/Unmount
```bash
mount /dev/sdb1 /mnt/data    # Mount
umount /mnt/data             # Unmount
mount -a                     # Mount all from fstab
mount -o ro device mountpoint # Read-only
```

### LVM
```bash
pvcreate /dev/sdb1           # Create PV
vgcreate vg /dev/sdb1         # Create VG
lvcreate -L 50G -n lv vg    # Create LV
pvs, vgs, lvs               # List PVs, VGs, LVs
lvextend -L +20G /dev/vg/lv  # Extend LV
resize2fs /dev/vg/lv        # Resize FS (ext4)
```

### Filesystem
```bash
mkfs.ext4 /dev/sdb1          # Create ext4
mkfs.xfs /dev/sdb1           # Create xfs
mkfs.btrfs /dev/sdb1        # Create btrfs
fsck /dev/sdb1               # Check ext4
xfs_repair /dev/sdb1          # Check xfs
```

### Find Large Files
```bash
find /path -size +1G         # Files > 1GB
du -sh * | sort -rh          # Largest directories
find /path -size +100M -exec du -h {} + | sort -rh | head -20
```

---

**Next:** Module 5 - Networking & Connectivity
