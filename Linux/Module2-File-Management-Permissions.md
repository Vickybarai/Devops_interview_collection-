# 🟨 MODULE 2: File Management & Permissions

> **Essential for DevOps Engineers** - Master file operations, permissions, and access control in Linux systems.

## Table of Contents
- [Q19. File Permissions (rwx) - chmod 755 vs 777](#q19-what-are-file-permissions-rwx-explain-chmod-755-vs-777)
- [Q20. Types of Permissions (Read, Write, Execute)](#q20-types-of-permissions-read-write-execute)
- [Q21. Soft Link vs Hard Link](#q21-what-is-the-difference-between-a-soft-link-and-a-hard-link)
- [Q22. Change File Ownership (chown, chgrp)](#q22-how-do-you-change-file-ownership-and-group-chown-chgrp)
- [Q23. Umask and Its Control](#q23-what-is-umask-what-does-it-control)
- [Q24. Sticky Bit (/tmp example)](#q24-what-is-the-sticky-bit-and-where-is-it-used-tmp-example)
- [Q25. /etc/passwd, /etc/shadow, /etc/group](#q25-what-are-etcpasswd-etcshadow-and-etcgroup-used-for)
- [Q26. su vs sudo](#q26-what-is-the-difference-between-su-and-sudo)
- [Q27. Edit sudoers File (visudo)](#q27-how-do-you-safely-edit-the-sudoers-file-visudo)
- [Q28. Find Recently Modified Files (find -mtime)](#q28-how-do-you-find-recently-modified-files-find--mtime)
- [Q29. Hidden Files (ls -a)](#q29-what-are-hidden-files-and-how-to-view-them-ls--a)
- [Q30. Change File Permissions (chmod)](#q30-change-file-permissions-using-chmod)
- [Q31. Execute Permissions for Scripts](#q31-permissions-to-execute-a-script)
- [Q32. Create/Manage Symbolic Links](#q32-create-manage-symbolic-links)

---

## Q19. What are File Permissions (rwx)? Explain chmod 755 vs 777.

### **Answer:**

In Linux, file permissions control who can **read**, **write**, and **execute** files. Permissions are represented by a 10-character string (or octal numbers).

### **Permission Structure:**

```
-rwxr-xr--
││││││││││
│││││││││└─ Others: Read, Write, Execute
│││││││└────── Group: Read, Write, Execute
││││││└───────── User (Owner): Read, Write, Execute
││││└──────────── File type (- = file, d = directory)
││└───────────── Read permission
│└─────────────── Write permission
└──────────────── Execute permission
```

### **Permission Types:**

| Symbol | Octal | Description                      | File Action                        | Directory Action                  |
|--------|--------|----------------------------------|-----------------------------------|----------------------------------|
| **r**  | 4      | Read                             | View file contents                 | List directory contents            |
| **w**  | 2      | Write                            | Modify file contents               | Add/remove files in directory     |
| **x**  | 1      | Execute                          | Run as program/script              | Enter/access directory            |
| **-**  | 0      | No permission                    | No access                        | No access                        |

### **User Categories:**

| Category | Who it Applies To                   | Position |
|----------|------------------------------------|----------|
| **User (u)** | File owner                          | 1st group (positions 2-4)        |
| **Group (g)** | Users in file's group               | 2nd group (positions 5-7)        |
| **Others (o)** | Everyone else                       | 3rd group (positions 8-10)       |
| **All (a)** | All three categories (u, g, o)    | N/A                              |

### **chmod 755 vs chmod 777:**

```bash
# chmod 755
# Owner: rwx (7) = Read + Write + Execute
# Group: r-x (5) = Read + Execute
# Others: r-x (5) = Read + Execute

$ chmod 755 script.sh
$ ls -l script.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 script.sh
```

```bash
# chmod 777
# Owner: rwx (7) = Read + Write + Execute
# Group: rwx (7) = Read + Write + Execute
# Others: rwx (7) = Read + Write + Execute

$ chmod 777 script.sh
$ ls -l script.sh
-rwxrwxrwx 1 user user 1024 Jan 15 10:00 script.sh
```

### **Comparison: chmod 755 vs 777**

| Aspect        | chmod 755                                    | chmod 777                                    |
|---------------|----------------------------------------------|----------------------------------------------|
| **Owner**     | Full access (rwx)                             | Full access (rwx)                             |
| **Group**     | Read + Execute only (r-x)                    | Full access (rwx)                             |
| **Others**    | Read + Execute only (r-x)                    | Full access (rwx)                             |
| **Security**  | ✅ More secure                               | ⚠️ Less secure                               |
| **Use Case**  | Web files, scripts, shared executables        | Highly discouraged, temporary use only         |
| **Risk**      | Others cannot modify files                     | Anyone can modify/delete files                |

### **Common Permission Patterns:**

| Octal | Symbolic | Use Case                           | Security |
|-------|-----------|------------------------------------|----------|
| **644** | `rw-r--r--` | Regular files (config, scripts)     | Standard  |
| **755** | `rwxr-xr-x` | Executables, programs, directories  | Secure    |
| **600** | `rw-------` | Private files (keys, secrets)       | High      |
| **700** | `rwx------` | Private directories (SSH, .ssh)    | High      |
| **777** | `rwxrwxrwx` | ⚠️ Avoid (security risk)            | Risky     |

### **chmod Usage Examples:**

```bash
# Symbolic mode
$ chmod u+x script.sh      # Add execute for owner
$ chmod g-w file.txt      # Remove write from group
$ chmod o-r file.txt      # Remove read from others
$ chmod a+rw file.txt     # Add read+write for all
$ chmod u=rwx,g=rx,o=rx script.sh  # Set specific permissions

# Octal mode
$ chmod 644 file.txt      # rw-r--r--
$ chmod 755 script.sh     # rwxr-xr-x
$ chmod 700 private/      # rwx------
$ chmod 640 config.conf   # rw-r-----

# Recursive (apply to directory and contents)
$ chmod -R 755 /var/www/html
$ chmod -R 644 /etc/myapp/config
```

### **Security Best Practices:**

```bash
# ❌ AVOID: World-writable files
$ chmod 777 script.sh      # Anyone can modify/delete

# ❌ AVOID: World-writable directories
$ chmod 777 /var/www      # Security vulnerability

# ✅ RECOMMENDED: Proper permissions
$ chmod 755 script.sh     # Owner can write, others execute
$ chmod 644 config.conf   # Owner can write, others read only
$ chmod 600 private.key   # Only owner can read/write

# ✅ RECOMMENDED: Secure web directories
$ find /var/www -type f -exec chmod 644 {} \;    # Files
$ find /var/www -type d -exec chmod 755 {} \;    # Directories
```

### **Checking Permissions:**

```bash
# Detailed listing
$ ls -la
total 20
-rw-r--r--  1 user user  1024 Jan 15 10:00 config.conf
-rwxr-xr-x  1 user user  2048 Jan 15 10:00 script.sh
drwx------  5 user user  4096 Jan 15 10:00 private/

# Check specific file
$ stat file.txt
  File: file.txt
  Size: 1024       Blocks: 8          IO Block: 4096   regular file
Access: (0644/-rw-r--r--)  Uid: ( 1000/   user)   Gid: ( 1000/   user)

# Numeric view of permissions
$ stat -c "%a %n" file.txt
644 file.txt
```

---

## Q20. Types of Permissions (Read, Write, Execute)

### **Answer:**

Linux has three fundamental types of permissions: **Read (r)**, **Write (w)**, and **Execute (x)**. Each permission has different effects on files versus directories.

### **Read Permission (r = 4)**

#### **On Files:**
- Allows viewing file contents
- Can copy file if read permission is granted
- Cannot modify or execute without additional permissions

```bash
# Read permission on file
$ chmod 644 file.txt      # rw-r--r--
-rw-r--r-- 1 user user 1024 Jan 15 10:00 file.txt

# Can view file
$ cat file.txt
Hello World

# Can copy file
$ cp file.txt file_copy.txt

# Cannot modify (no write permission for others)
$ echo "New content" >> file.txt
bash: file.txt: Permission denied
```

#### **On Directories:**
- Allows listing directory contents (`ls`)
- Cannot access files inside without execute permission on directory
- Cannot see file details (size, permissions) without read permission

```bash
# Read permission on directory
$ chmod 755 mydir/
drwxr-xr-x 2 user user 4096 Jan 15 10:00 mydir/

# Can list directory
$ ls mydir/
file1.txt  file2.txt

# Without read permission
$ chmod 711 mydir/
drwx--x--x 2 user user 4096 Jan 15 10:00 mydir/

$ ls mydir/
ls: cannot open directory 'mydir': Permission denied
```

### **Write Permission (w = 2)**

#### **On Files:**
- Allows modifying file contents
- Allows truncating or deleting file (if directory permission also allows)
- Does NOT allow renaming or moving (requires directory write permission)

```bash
# Write permission on file
$ chmod 666 file.txt      # rw-rw-rw-

# Can modify file
$ echo "New content" >> file.txt
$ vim file.txt

# Cannot delete if directory has no write permission
$ chmod 555 /dir/    # r-xr-xr-x
$ rm /dir/file.txt
rm: cannot remove '/dir/file.txt': Permission denied
```

#### **On Directories:**
- Allows creating, deleting, and renaming files within directory
- Does NOT allow reading directory contents (requires read permission)
- Can delete files even without write permission on the files themselves

```bash
# Write permission on directory
$ chmod 777 shared/
drwxrwxrwx 2 user user 4096 Jan 15 10:00 shared/

# Can create files
$ touch shared/newfile.txt

# Can delete files (even without file write permission)
$ chmod 000 shared/protected.txt    # No permissions on file
$ rm shared/protected.txt          # Can delete (directory has write)
```

### **Execute Permission (x = 1)**

#### **On Files:**
- Allows executing file as a program or script
- Required for binary executables and shell scripts
- Shebang (`#!/bin/bash`) determines interpreter

```bash
# Script without execute permission
$ echo 'echo "Hello"' > script.sh
$ chmod 644 script.sh
-rw-r--r-- 1 user user 17 Jan 15 10:00 script.sh

$ ./script.sh
bash: ./script.sh: Permission denied

# Add execute permission
$ chmod +x script.sh    # or chmod 755 script.sh
-rwxr-xr-x 1 user user 17 Jan 15 10:00 script.sh

$ ./script.sh
Hello

# Explicitly specify interpreter
$ bash script.sh       # Works without execute permission
```

#### **On Directories:**
- Allows entering/accessing the directory (`cd` command)
- Required to access files inside directory
- Without execute, cannot `cd` into directory even with read permission

```bash
# Directory with read but no execute
$ chmod 644 mydir/
drw-r--r-- 2 user user 4096 Jan 15 10:00 mydir/

$ cd mydir/
bash: cd: mydir/: Permission denied

# Add execute permission
$ chmod 755 mydir/
drwxr-xr-x 2 user user 4096 Jan 15 10:00 mydir/

$ cd mydir/
mydir$ pwd
/path/mydir
```

### **Permission Summary Table:**

| Permission | On File                             | On Directory                           |
|------------|-------------------------------------|---------------------------------------|
| **Read (r)** | View contents, copy file            | List directory contents                |
| **Write (w)** | Modify contents, truncate            | Create, delete, rename files          |
| **Execute (x)** | Run as program/script              | Enter directory (cd), access files    |
| **None (-)** | No access                           | No access                             |

### **Permission Effects Matrix:**

```
File Permission Effects:
┌─────────┬─────────┬─────────┬──────────┐
│         │ Read    │ Write   │ Execute  │
├─────────┼─────────┼─────────┼──────────┤
│ Read    │ ✓       │ ✗       │ ✗        │
│ Write   │ ✗       │ ✓       │ ✗        │
│ Execute │ ✗       │ ✗       │ ✓        │
│ Read+   │ ✓       │ ✗       │ ✗        │
│ Execute │         │         │          │
│ Write+  │ ✗       │ ✓       │ ✗        │
│ Execute │         │         │          │
│ All     │ ✓       │ ✓       │ ✓        │
└─────────┴─────────┴─────────┴──────────┘

Directory Permission Effects:
┌─────────┬─────────┬─────────┬──────────┐
│         │ Read    │ Write   │ Execute  │
├─────────┼─────────┼─────────┼──────────┤
│ Read    │ List    │ ✗       │ ✗        │
│ Write   │ ✗       │ ✗       │ ✗        │
│ Execute │ ✗       │ ✗       │ Enter    │
│ Read+   │ List    │ ✗       │ Enter    │
│ Execute │         │         │          │
│ Write+  │ ✗       │ Delete  │ Enter    │
│ Execute │         │         │          │
│ All     │ List    │ Delete  │ Enter    │
└─────────┴─────────┴─────────┴──────────┘
```

### **Practical Examples:**

```bash
# Web server files: read for all, execute for scripts
$ find /var/www -type f -exec chmod 644 {} \;
$ find /var/www -type f -name "*.cgi" -exec chmod 755 {} \;
$ find /var/www -type d -exec chmod 755 {} \;

# SSH directory: private, only owner access
$ mkdir -p ~/.ssh
$ chmod 700 ~/.ssh/
$ chmod 600 ~/.ssh/id_rsa
$ chmod 644 ~/.ssh/id_rsa.pub

# Log files: append-only for application, read for others
$ touch /var/log/app.log
$ chown appuser:appgroup /var/log/app.log
$ chmod 640 /var/log/app.log      # rw-r-----

# Executable scripts
$ chmod +x deploy.sh
$ ls -l deploy.sh
-rwxr-xr-x 1 user user 2048 Jan 15 10:00 deploy.sh

# Check effective permissions for different users
$ su - user1 -c "cat /etc/shadow"
cat: /etc/shadow: Permission denied

$ sudo cat /etc/shadow    # Works with sudo
```

### **Permission Testing:**

```bash
# Test if current user has read permission
if [ -r file.txt ]; then
    echo "Can read file"
fi

# Test write permission
if [ -w file.txt ]; then
    echo "Can write to file"
fi

# Test execute permission
if [ -x script.sh ]; then
    echo "Can execute script"
fi

# Test directory access
if [ -d mydir/ ]; then
    echo "Is a directory"
    if [ -x mydir/ ]; then
        echo "Can access directory"
    fi
fi
```

---

## Q21. What is the Difference Between a Soft Link and a Hard Link?

### **Answer:**

Linux supports two types of links: **Soft Links (Symbolic Links)** and **Hard Links**. They behave differently in terms of inode usage, cross-filesystem support, and behavior when original is deleted.

### **Inode Fundamentals:**

Every file in Linux has a unique **inode** number that contains metadata and pointers to data blocks.

```bash
# View inode numbers
$ ls -li
total 20
123456 -rw-r--r-- 1 user user 1024 Jan 15 10:00 file.txt
123457 -rw-r--r-- 1 user user 2048 Jan 15 10:00 data.txt

# inode = 123456, 123457
```

### **Hard Link:**

#### **Characteristics:**
- Points to the same inode as the original file
- Is essentially another name for the same file
- Cannot span different filesystems/partitions
- Deleting original file does NOT delete the data (link count decrements)
- Cannot link to directories (except root)
- Same permissions and ownership as original

```bash
# Create a hard link
$ ln original.txt hardlink.txt

# Both files have same inode
$ ls -li original.txt hardlink.txt
123456 -rw-r--r-- 2 user user 1024 Jan 15 10:00 original.txt
123456 -rw-r--r-- 2 user user 1024 Jan 15 10:00 hardlink.txt
        │                                      │
        └─ Same inode number                   └─ Link count = 2

# Modify through hard link
$ echo "New data" >> hardlink.txt

# Original also modified (same data)
$ cat original.txt
New data

# Delete original (link count decrements)
$ rm original.txt

# Data still accessible through hard link
$ cat hardlink.txt
New data

$ ls -l hardlink.txt
-rw-r--r-- 1 user user 1024 Jan 15 10:00 hardlink.txt
                   │
                   └─ Link count = 1 now
```

### **Soft Link (Symbolic Link):**

#### **Characteristics:**
- Contains path to the original file (like a shortcut)
- Has its own inode (different from original)
- Can span different filesystems/partitions
- Deleting original file breaks the link (dangling link)
- Can link to directories and files
- Can point to non-existent files
- Permissions affect the link itself, not target

```bash
# Create a soft link
$ ln -s original.txt softlink.txt

# Different inode numbers
$ ls -li original.txt softlink.txt
123456 -rw-r--r-- 1 user user 1024 Jan 15 10:00 original.txt
123458 lrwxrwxrwx 1 user user   12 Jan 15 10:00 softlink.txt -> original.txt
        │                                   │
        │                                   └─ Points to original
        └─ Different inode

# Access through soft link
$ cat softlink.txt
Hello World

# View link target
$ readlink softlink.txt
original.txt

# Delete original file
$ rm original.txt

# Soft link becomes dangling
$ cat softlink.txt
cat: softlink.txt: No such file or directory

$ ls -l softlink.txt
lrwxrwxrwx 1 user user 12 Jan 15 10:00 softlink.txt -> original.txt
                                    │
                                    └─ Target no longer exists
```

### **Comparison: Hard Link vs Soft Link**

| Aspect                | Hard Link                                      | Soft Link                                      |
|-----------------------|------------------------------------------------|------------------------------------------------|
| **Inode**             | Same inode as original                          | Different inode                                |
| **Content**           | Points to same data blocks                     | Contains path to target file                   |
| **Cross Filesystem**   | ❌ Cannot cross filesystems                    | ✅ Can span filesystems                       |
| **Delete Original**    | ✅ Data remains (link count decrements)         | ❌ Link becomes broken (dangling)             |
| **Link to Directory** | ❌ Cannot link to directories                 | ✅ Can link to directories                    |
| **Size**              | Same size as original                          | Size of target path (small)                   |
| **Permissions**       | Same as original                              | May differ (affects link itself)               |
| **Creation Command**   | `ln original link`                            | `ln -s original link`                        |
| **Link Count**        | Increments with each link                      | Does not affect link count                     |
| **Point to Non-existent** | Impossible                             | ✅ Possible (dangling link)                   |

### **Visual Representation:**

```
Hard Link Structure:
┌──────────────┐
│  Inode 12345 │◄───────────┐
│              │            │
│  Data Blocks │            │
│              │            │
└──────────────┘            │
     ▲          ▲          │
     │          │          │
     │          │          │
original.txt  link1.txt   link2.txt
    │           │           │
    └───────────┴───────────┘
     All point to same inode and data


Soft Link Structure:
┌──────────────┐         ┌──────────────┐
│ Inode 12345  │         │ Inode 12346  │
│ Data Blocks  │◄────────│ Path Pointer  │
└──────────────┘         └──────────────┘
      ▲                        │
      │                        ▼
original.txt           softlink.txt -> original.txt

(Deleting original breaks the soft link)
```

### **Practical Use Cases:**

#### **Hard Links:**
```bash
# Backup strategy - multiple references to same data
$ ln important_data.txt backup_data.txt

# Version control - save space with hard links
$ cp -l original.txt version1.txt
$ cp -l original.txt version2.txt

# Deduplication - same file, multiple locations
$ ln /data/large_file.bin ~/shortcut/large_file.bin
```

#### **Soft Links:**
```bash
# Configuration management
$ ln -s /etc/nginx/nginx.conf ~/nginx.conf

# Application versioning
$ ln -s /opt/app/v2.0 /opt/app/current

# Cross-filesystem links
$ ln -s /var/log/app.log ~/logs/app.log

# Library linking
$ ln -s /usr/local/lib/libmylib.so.1.0 /usr/lib/libmylib.so.1

# Directory shortcuts
$ ln -s /var/www/html ~/www
```

### **Advanced Link Operations:**

```bash
# Create relative soft link (better for portability)
$ ln -s ../data/file.txt link.txt

# Find all hard links to a file
$ find / -inum 123456
/home/user/original.txt
/home/user/link1.txt
/home/user/backup/link2.txt

# Find broken soft links
$ find / -xtype l
/home/user/dangling_link.txt -> /nonexistent/file.txt

# Update soft link
$ ln -sf new_target.txt existing_link.txt

# Link directory
$ ln -s /var/log ~/logs
$ ls -l ~/logs
lrwxrwxrwx 1 user user 8 Jan 15 10:00 logs -> /var/log

# Follow symbolic link
$ readlink -f softlink.txt
/absolute/path/to/target.txt
```

### **Security Considerations:**

```bash
# Check if file is symbolic link
if [ -L file.txt ]; then
    echo "Symbolic link"
fi

# Check link target before following
if [ -L suspicious_link ]; then
    target=$(readlink -f suspicious_link)
    if [[ "$target" == /sensitive/path/* ]]; then
        echo "Warning: Link points to sensitive area"
    fi
fi

# Symbolic link permissions (lrwxrwxrwx is default)
$ ls -l symlink.txt
lrwxrwxrwx 1 user user 10 Jan 15 10:00 symlink.txt -> target.txt
# The rwxrwxrwx on symbolic links is misleading
# Actual access depends on target file permissions
```

### **Monitoring and Maintenance:**

```bash
# Check link count on a file
$ stat -c "%h" file.txt
3

# Show all hard links
$ find /path -samefile file.txt

# Identify dangling symbolic links
$ find / -type l -exec test ! -e {} \; -print

# Fix broken symbolic links
$ find / -type l -xtype l -delete

# Create symbolic link with backup
$ ln -sb original.txt link.txt  # Backup existing link
```

---

## Q22. How Do You Change File Ownership and Group (chown, chgrp)?

### **Answer:**

Linux allows changing file ownership (user) and group using `chown` (change owner) and `chgrp` (change group) commands. Only root user can change ownership.

### **chown (Change Owner):**

#### **Basic Syntax:**
```bash
$ chown [OPTIONS] USER[:GROUP] FILE
```

#### **Change Owner Only:**
```bash
# Change owner of single file
$ sudo chown newuser file.txt
$ ls -l file.txt
-rw-r--r-- 1 newuser user 1024 Jan 15 10:00 file.txt

# Change owner of multiple files
$ sudo chown newuser file1.txt file2.txt file3.txt

# Change owner recursively (directory and contents)
$ sudo chown -R newuser /path/to/directory
```

#### **Change Owner and Group:**
```bash
# Change both owner and group
$ sudo chown newuser:newgroup file.txt
$ ls -l file.txt
-rw-r--r-- 1 newuser newgroup 1024 Jan 15 10:00 file.txt

# Using period instead of colon
$ sudo chown newuser.newgroup file.txt
```

#### **Change Group Only:**
```bash
# Change group only (keep owner same)
$ sudo chown :newgroup file.txt
$ ls -l file.txt
-rw-r--r-- 1 user newgroup 1024 Jan 15 10:00 file.txt

# Using user:group syntax to change group
$ sudo chown user:newgroup file.txt
```

#### **Reference Other File's Owner:**
```bash
# Copy ownership from reference file
$ sudo chown --reference=reference.txt target.txt

# This is useful when multiple files should have same owner
$ sudo chown --reference=template.conf config1.conf config2.conf
```

### **chgrp (Change Group):**

#### **Basic Syntax:**
```bash
$ chgrp [OPTIONS] GROUP FILE
```

#### **Change Group:**
```bash
# Change group of file
$ sudo chgrp developers file.txt
$ ls -l file.txt
-rw-r--r-- 1 user developers 1024 Jan 15 10:00 file.txt

# Change group recursively
$ sudo chgrp -R developers /path/to/directory

# Change group for multiple files
$ sudo chgrp developers file1.txt file2.txt file3.txt
```

### **User and Group Information:**

```bash
# List all users
$ cut -d: -f1 /etc/passwd
root
daemon
bin
user1
user2

# List all groups
$ cut -d: -f1 /etc/group
root
daemon
developers
docker

# Check user's groups
$ groups user1
user1 sudo docker developers

# Check user ID and group ID
$ id user1
uid=1000(user1) gid=1000(user1) groups=1000(user1),27(sudo),1001(docker)

# Check which user owns a file
$ ls -l file.txt
-rw-r--r-- 1 user1 user1 1024 Jan 15 10:00 file.txt
         │  │     │
         │  │     └─ Group
         │  └────── Owner
         └───────── Link count
```

### **Advanced chown Options:**

```bash
# Verbose mode (show changes)
$ sudo chown -v newuser file.txt
changed ownership of 'file.txt' from user to newuser

# Preserve root ownership (when using tar extract)
$ sudo tar --preserve-permissions -xzf archive.tar.gz

# Change ownership only for files (not directories)
$ find /path -type f -exec chown user:group {} \;

# Change ownership only for directories
$ find /path -type d -exec chown user:group {} \;

# Change ownership for files matching pattern
$ find /var/log -name "*.log" -exec chown syslog:adm {} \;

# Use with xargs (more efficient)
$ find /path -type f -print0 | xargs -0 sudo chown user:group
```

### **Common Scenarios:**

#### **1. Transferring File Ownership:**
```bash
# User leaves company, transfer files to new user
$ sudo chown -R newuser:team /home/olduser/projects

# Verify transfer
$ ls -la /home/newuser/projects
total 20
-rw-r--r-- 1 newuser team 1024 Jan 15 10:00 file1.txt
-rw-r--r-- 1 newuser team 2048 Jan 15 10:00 file2.txt
```

#### **2. Web Server Files:**
```bash
# Set Apache/Nginx ownership
$ sudo chown -R www-data:www-data /var/www/html

# Set proper permissions along with ownership
$ sudo chown -R www-data:www-data /var/www/html
$ sudo find /var/www/html -type f -exec chmod 644 {} \;
$ sudo find /var/www/html -type d -exec chmod 755 {} \;
```

#### **3. Application Deployment:**
```bash
# Application user
$ sudo adduser --system --group appuser

# Deploy application with proper ownership
$ sudo chown -R appuser:appuser /opt/myapp

# Ensure logs are writable
$ sudo chown appuser:appuser /var/log/myapp
```

#### **4. Shared Team Directory:**
```bash
# Create shared directory
$ sudo mkdir /shared/team

# Set group ownership
$ sudo chown :team /shared/team

# Set group write permission
$ sudo chmod 775 /shared/team

# Set SGID bit for automatic group inheritance
$ sudo chmod g+s /shared/team

# New files automatically belong to team group
$ touch /shared/team/newfile.txt
$ ls -l /shared/team/newfile.txt
-rw-r--r-- 1 user1 team 0 Jan 15 10:00 newfile.txt
```

### **Batch Ownership Changes:**

```bash
# Find files owned by olduser, change to newuser
$ sudo find /home/olduser -user olduser -exec chown -h newuser:newgroup {} \;

# Change ownership for files modified in last 7 days
$ find /path -mtime -7 -exec chown user:group {} \;

# Change ownership for files larger than 100MB
$ find /path -size +100M -exec chown user:group {} \;

# Change ownership excluding symbolic links
$ find /path ! -type l -exec chown user:group {} \;
```

### **Security and Permissions After Ownership Change:**

```bash
# After changing ownership, verify permissions
$ ls -l file.txt
-rw-r--r-- 1 newuser newgroup 1024 Jan 15 10:00 file.txt

# Check if new owner can access
$ su - newuser -c "cat file.txt"
Hello World

# Group member access
$ sudo -u user1 groups
user1 team developers

$ sudo -u user1 cat /shared/team/file.txt
Hello from team
```

### **Ownership Change Best Practices:**

```bash
# Use -R carefully (recursive)
# Know exactly what will be affected
$ sudo chown -R user:group /specific/path

# Test first with --dry-run equivalent (ls first)
$ ls -lR /path/to/change

# Use verbose mode to see changes
$ sudo chown -vR user:group /path
changed ownership of '/path/file1.txt' from olduser to user
changed ownership of '/path/dir1' from olduser to user

# Document ownership changes
$ echo "$(date): Changed ownership of /app/data to appuser" >> /var/log/chown.log
```

### **Special Cases:**

```bash
# Change ownership of symbolic link (not target)
$ chown -h user:group symlink.txt

# Preserving root when extracting archives
$ tar --preserve-permissions -xzf archive.tar.gz

# Change ownership only for specific user's files
$ find / -user olduser -exec chown -h newuser {} \;

# Undo ownership change (requires knowing previous owner)
# Keep logs of ownership changes
$ find /path -printf "%u:%g %p\n" > ownership_backup.txt
```

### **Monitoring Ownership:**

```bash
# Find files not owned by valid users (orphaned files)
$ find /path -nouser

# Find files not belonging to any group
$ find /path -nogroup

# Check for world-writable files (security risk)
$ find /path -perm -002 -type f

# Check for world-writable directories
$ find /path -perm -002 -type d

# Generate ownership report
$ find /home -printf "%u %g %p\n" | sort | uniq -c
```

---

## Q23. What is Umask? What Does It Control?

### **Answer:**

**umask** (user file-creation mask) is a command that determines the default permissions for newly created files and directories. It works by **subtracting** permissions from the maximum allowed permissions.

### **How Umask Works:**

```bash
# Maximum permissions:
# Files: 666 (rw-rw-rw-) - No execute by default
# Directories: 777 (rwxrwxrwx) - Full permissions

# umask SUBTRACTS from maximum

# Example: umask 022
Files:       666 (max)
            - 022 (umask)
            ----
            644 (rw-r--r--)

Directories: 777 (max)
            - 022 (umask)
            ----
            755 (rwxr-xr-x)
```

### **Umask Values and Effects:**

| Umask | File Permissions | Directory Permissions | Effect                              |
|-------|-----------------|----------------------|--------------------------------------|
| **022** | 644 (rw-r--r--) | 755 (rwxr-xr-x) | Owner: rw, Group/Other: r (common default) |
| **002** | 664 (rw-rw-r--) | 775 (rwxrwxr-x) | Owner/Group: rw, Other: r (shared)      |
| **027** | 640 (rw-r-----) | 750 (rwxr-x---) | Owner: rw, Group: r, Other: none (private)|
| **077** | 600 (rw-------) | 700 (rwx------) | Owner only (strict security)           |
| **000** | 666 (rw-rw-rw-) | 777 (rwxrwxrwx) | All permissions (very permissive)       |

### **Viewing Current Umask:**

```bash
# View current umask
$ umask
0022

# View symbolic mode
$ umask -S
u=rwx,g=rx,o=rx

# View octal value
$ umask
022

# Check umask in specific shell
$ bash -c 'umask'
0022

# View all processes' umask (system-wide check)
$ sudo cat /etc/login.defs | grep UMASK
UMASK           022
```

### **Setting Umask:**

#### **Temporary (Current Session Only):**
```bash
# Set umask for current session
$ umask 022

# Verify
$ umask
022

# Create file to test
$ touch testfile.txt
$ ls -l testfile.txt
-rw-r--r-- 1 user user 0 Jan 15 10:00 testfile.txt
            │││ │││ ││
            │││ │││ └─ Others: read only (4)
            │││ ││└───── Group: read only (4)
            │││ │└─────── Owner: read+write (6)
            │││ └───────── Type: regular file
```

#### **Permanent (User-specific):**
```bash
# Add to ~/.bashrc or ~/.bash_profile
$ echo "umask 022" >> ~/.bashrc

# Reload shell configuration
$ source ~/.bashrc

# Verify
$ umask
022
```

#### **System-wide Default:**
```bash
# Edit /etc/login.defs
$ sudo nano /etc/login.defs

# Find and modify UMASK line
UMASK           022

# Edit /etc/profile (system-wide for all users)
$ sudo nano /etc/profile
umask 022

# Edit /etc/bash.bashrc (bash-specific)
$ sudo nano /etc/bash.bashrc
umask 022
```

### **Common Umask Values by Use Case:**

#### **Standard Server (umask 022):**
```bash
$ umask 022

$ touch config.conf
-rw-r--r-- 1 user user 0 Jan 15 10:00 config.conf

$ mkdir newdir
drwxr-xr-x 2 user user 4096 Jan 15 10:00 newdir

# Web servers typically use this
# Files: 644, Directories: 755
```

#### **Shared Development (umask 002):**
```bash
$ umask 002

$ touch shared_file.txt
-rw-rw-r-- 1 user group 0 Jan 15 10:00 shared_file.txt

$ mkdir shared_dir
drwxrwxr-x 2 user group 4096 Jan 15 10:00 shared_dir

# Team collaboration
# Group members can write
```

#### **Secure Environment (umask 077):**
```bash
$ umask 077

$ touch private_key.pem
-rw------- 1 user user 0 Jan 15 10:00 private_key.pem

$ mkdir private_dir
drwx------ 2 user user 4096 Jan 15 10:00 private_dir

# Only owner can access
# Used for sensitive data, SSH keys
```

#### **Mixed Security (umask 027):**
```bash
$ umask 027

$ touch project_file.txt
-rw-r----- 1 user group 0 Jan 15 10:00 project_file.txt

$ mkdir project_dir
drwxr-x--- 2 user group 4096 Jan 15 10:00 project_dir

# Owner: full, Group: read, Others: none
# Project directories with team access
```

### **Umask Calculation Examples:**

```bash
# Example 1: umask 077
File:     666
        - 077 (remove owner write, group all, other all)
        ----
        600 (rw-------)

Directory: 777
        - 077
        ----
        700 (rwx------)


# Example 2: umask 027
File:     666
        - 027 (remove group write, other all)
        ----
        640 (rw-r-----)

Directory: 777
        - 027
        ----
        750 (rwxr-x---)


# Example 3: umask 002
File:     666
        - 002 (remove other write)
        ----
        664 (rw-rw-r--)

Directory: 777
        - 002
        ----
        775 (rwxrwxr-x)
```

### **Practical Umask Usage:**

#### **1. Application Deployment:**
```bash
# In startup script for application
#!/bin/bash
umask 022    # Standard permissions
touch /var/log/app.log    # Creates with 644
mkdir /var/lib/app/data   # Creates with 755
```

#### **2. Secure File Creation:**
```bash
#!/bin/bash
# Temporary stricter umask
OLD_UMASK=$(umask)
umask 077

# Create sensitive files
openssl genrsa -out private.key 2048

# Restore original umask
umask $OLD_UMASK
```

#### **3. Team Project Setup:**
```bash
# /etc/profile.d/team-umask.sh
umask 002

# All team members can write to shared files
# Files created in project: 664
# Directories created: 775
```

#### **4. Web Application:**
```bash
# Application user configuration
# /home/www-data/.bashrc
umask 022

# Nginx creates log files with 644
# Directories with 755
```

### **Checking Umask Effects:**

```bash
# Test different umask values
$ umask 022 && touch test_022.txt && ls -l test_022.txt
-rw-r--r-- 1 user user 0 Jan 15 10:00 test_022.txt

$ umask 002 && touch test_002.txt && ls -l test_002.txt
-rw-rw-r-- 1 user user 0 Jan 15 10:00 test_002.txt

$ umask 077 && touch test_077.txt && ls -l test_077.txt
-rw------- 1 user user 0 Jan 15 10:00 test_077.txt

# Test directory creation
$ umask 022 && mkdir dir_022 && ls -ld dir_022
drwxr-xr-x 2 user user 4096 Jan 15 10:00 dir_022

$ umask 077 && mkdir dir_077 && ls -ld dir_077
drwx------ 2 user user 4096 Jan 15 10:00 dir_077
```

### **Umask and Specific Commands:**

```bash
# Some commands override umask
$ touch file.txt              # Respects umask
$ install -m 755 file.txt dest/  # Forces 755, ignores umask
$ mkdir -m 777 newdir         # Forces 777, ignores umask

# cp preserves original permissions by default
$ cp -a source.txt dest.txt   # Preserves all attributes

# tar extract respects umask unless specified
$ tar -xzf archive.tar.gz     # Uses current umask
$ tar --preserve-permissions -xzf archive.tar.gz  # Preserves original
```

### **Symbolic Umask Mode:**

```bash
# Set umask symbolically
$ umask u=rwx,g=rx,o=rx    # Same as 022
$ umask u=rwx,g=rx,o=        # Same as 027
$ umask u=rwx,g=,o=         # Same as 077

# Remove specific permissions
$ umask u=rwx,g-wx,o-rwx    # Removes group write+execute, other all
```

### **Umask Security Best Practices:**

```bash
# ❌ BAD: Too permissive
$ umask 000    # Files: 666, Dirs: 777

# ✅ GOOD: Standard secure default
$ umask 022    # Files: 644, Dirs: 755

# ✅ GOOD: Private/secure files
$ umask 077    # Files: 600, Dirs: 700

# ✅ GOOD: Team collaboration
$ umask 002    # Files: 664, Dirs: 775

# Set umask before creating sensitive files
OLD_UMASK=$(umask)
umask 077
openssl genrsa -out private.pem 2048
umask $OLD_UMASK

# Document umask settings in /etc/login.defs
```

### **Diagnosing Permission Issues:**

```bash
# Check umask if files have unexpected permissions
$ umask
0022

# If files created with wrong permissions, check:
# 1. Umask setting
$ grep -r "umask" ~/.bashrc ~/.bash_profile /etc/profile

# 2. PAM configuration
$ cat /etc/login.defs | grep UMASK

# 3. Application-specific umask
$ ps aux | grep appname
$ cat /proc/PID/environ | tr '\0' '\n' | grep UMASK

# Test file creation manually
$ umask 022
$ touch testfile && stat -c "%a %n" testfile
644 testfile
```

---

## Q24. What is the Sticky Bit and Where is It Used (/tmp example)?

### **Answer:**

The **sticky bit** is a special permission bit that can be set on directories. When set, it ensures that **only the owner of a file can delete or rename that file**, even if other users have write permissions on the directory.

### **How Sticky Bit Works:**

```
Directory with sticky bit:
drwxrwxrwt  10 root root 4096 Jan 15 10:00 /tmp
               │
               └─ t = sticky bit set

Without sticky bit:
drwxrwxrwx  10 root root 4096 Jan 15 10:00 /tmp
               │
               └─ x = normal execute permission
```

### **Sticky Bit Behavior:**

| Scenario | Without Sticky Bit | With Sticky Bit |
|----------|-------------------|----------------|
| **User A creates file** | User B can delete it (has write on dir) | Only User A can delete it |
| **User A creates file** | User B can rename it | Only User A can rename it |
| **User A creates file** | User B can modify it (if file permits) | User B can modify it (if file permits) |
| **Directory permissions** | Write permission allows delete any file | Write allows delete own files only |

### **Viewing Sticky Bit:**

```bash
# List /tmp directory (has sticky bit)
$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 Jan 15 10:00 /tmp
              │
              └─ t = sticky bit

# Check using stat
$ stat -c "%A %n" /tmp
drwxrwxrwt /tmp

# Find directories with sticky bit
$ find / -type d -perm -1000
/tmp
/var/tmp
```

### **Setting Sticky Bit:**

#### **Symbolic Mode:**
```bash
# Add sticky bit
$ sudo chmod +t /path/to/directory

# Remove sticky bit
$ sudo chmod -t /path/to/directory

# Verify
$ ls -ld /path/to/directory
drwxrwxrwt 2 root root 4096 Jan 15 10:00 /path/to/directory
```

#### **Octal Mode:**
```bash
# 1 = sticky bit
# 1777 = rwxrwxrwt
$ sudo chmod 1777 /path/to/directory

# 1755 = rwxr-xr-t
$ sudo chmod 1755 /path/to/directory

# Verify
$ ls -ld /path/to/directory
drwxr-xr-t 2 root root 4096 Jan 15 10:00 /path/to/directory
```

### **Sticky Bit Examples:**

#### **Example 1: /tmp Directory:**

```bash
# Check /tmp permissions
$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 Jan 15 10:00 /tmp

# User A creates file
$ touch /tmp/usera_file.txt
$ ls -l /tmp/usera_file.txt
-rw-r--r-- 1 usera usera 0 Jan 15 10:00 usera_file.txt

# User B tries to delete User A's file
$ su - userb
$ rm /tmp/usera_file.txt
rm: cannot remove '/tmp/usera_file.txt': Operation not permitted

# User B can create own file
$ touch /tmp/userb_file.txt
-rw-r--r-- 1 userb userb 0 Jan 15 10:00 userb_file.txt

# User B can delete own file
$ rm /tmp/userb_file.txt
# Success
```

#### **Example 2: Shared Upload Directory:**

```bash
# Create shared upload directory
$ sudo mkdir /shared/uploads

# Set group ownership and permissions
$ sudo chown root:team /shared/uploads
$ sudo chmod 1770 /shared/uploads    # rwxrwx--T

# Verify
$ ls -ld /shared/uploads
drwxrwx---T 2 root team 4096 Jan 15 10:00 /shared/uploads

# Users can upload files
$ su - user1
$ touch /shared/uploads/user1_file.txt

# Other users cannot delete user1's file
$ su - user2
$ rm /shared/uploads/user1_file.txt
rm: cannot remove '/shared/uploads/user1_file.txt': Operation not permitted
```

#### **Example 3: /var/tmp Directory:**

```bash
# /var/tmp also has sticky bit
$ ls -ld /var/tmp
drwxrwxrwt 5 root root 4096 Jan 15 10:00 /var/tmp

# Similar behavior to /tmp
# Preserved across reboots (unlike /tmp)
```

### **Comparison: With and Without Sticky Bit:**

#### **Without Sticky Bit:**
```bash
# Create directory without sticky bit
$ sudo mkdir /no_sticky
$ sudo chmod 1777 /no_sticky    # Actually has sticky bit

# Remove sticky bit for comparison
$ sudo chmod -t /no_sticky
$ ls -ld /no_sticky
drwxrwxrwx 2 root root 4096 Jan 15 10:00 /no_sticky

# User A creates file
$ su - usera
$ touch /no_sticky/usera.txt

# User B can delete User A's file
$ su - userb
$ rm /no_sticky/usera.txt
# Success - file deleted!

# This is a security issue
```

#### **With Sticky Bit:**
```bash
# Create directory with sticky bit
$ sudo mkdir /with_sticky
$ sudo chmod 1777 /with_sticky
$ ls -ld /with_sticky
drwxrwxrwt 2 root root 4096 Jan 15 10:00 /with_sticky

# User A creates file
$ su - usera
$ touch /with_sticky/usera.txt

# User B cannot delete User A's file
$ su - userb
$ rm /with_sticky/usera.txt
rm: cannot remove '/with_sticky/usera.txt': Operation not permitted
```

### **Common Directories with Sticky Bit:**

| Directory | Purpose | Why Sticky Bit? |
|-----------|---------|-----------------|
| **/tmp** | Temporary files for all users | Users can't delete others' temp files |
| **/var/tmp** | Persistent temporary files | Security for shared temp storage |
| **/public/uploads** | User upload directory | Prevent accidental/malicious deletion |

### **Setting Up Shared Directory with Sticky Bit:**

```bash
# Create shared directory structure
$ sudo mkdir -p /shared/team

# Set ownership and permissions
$ sudo chown root:team /shared/team
$ sudo chmod 1770 /shared/team

# Verify permissions
$ ls -ld /shared/team
drwxrwx---T 2 root team 4096 Jan 15 10:00 /shared/team

# Explanation:
# 1 = sticky bit
# 770 = rwx for owner and group
# T = sticky bit (capital when x is missing)
```

### **Finding and Managing Sticky Bit:**

```bash
# Find all directories with sticky bit
$ find / -type d -perm -1000
/tmp
/var/tmp

# Find directories without sticky bit that should have it
$ find /tmp /var/tmp -type d ! -perm -1000

# Remove sticky bit
$ sudo chmod -t /path/to/directory

# Set sticky bit and verify
$ sudo chmod +t /path/to/directory
$ ls -ld /path/to/directory
drwxrwxrwt 2 root root 4096 Jan 15 10:00 /path/to/directory

# Check sticky bit numerically
$ stat -c "%a" /path/to/directory
1777
#        ││
#        │└─ Sticky bit (1)
#        └─ 7 = rwx (owner)
```

### **Sticky Bit and Other Permissions:**

```bash
# Octal values with sticky bit:
# 1000 = sticky bit only
# 1777 = rwxrwxrwt
# 1775 = rwxr-xr-t
# 1770 = rwxrwx---T
# 1755 = rwxr-xr-t

# Symbolic representation:
# t = sticky bit + execute permission
# T = sticky bit WITHOUT execute permission

# Examples:
$ chmod 1777 dir    # rwxrwxrwt
$ chmod 1755 dir    # rwxr-xr-t
$ chmod 1770 dir    # rwxrwx---T
```

### **Sticky Bit Security Benefits:**

```bash
# Prevents accidental deletion
# Users can't delete files created by others

# Prevents malicious deletion
# Even if directory is world-writable, only owners can delete

# Common in multi-user systems
# /tmp directory is world-writable but uses sticky bit

# Useful for shared spaces
# Upload directories, temporary storage, collaboration areas
```

### **Testing Sticky Bit:**

```bash
# Create test directory
$ sudo mkdir /test_sticky
$ sudo chmod 1777 /test_sticky

# Verify sticky bit is set
$ ls -ld /test_sticky
drwxrwxrwt 2 root root 4096 Jan 15 10:00 /test_sticky

# Test from user1
$ su - user1 -c "touch /test_sticky/file1.txt"

# Test from user2 (try to delete user1's file)
$ su - user2 -c "rm /test_sticky/file1.txt"
rm: cannot remove '/test_sticky/file1.txt': Operation not permitted

# User2 can delete own file
$ su - user2 -c "touch /test_sticky/file2.txt && rm /test_sticky/file2.txt"
# Success
```

### **Best Practices for Sticky Bit:**

```bash
# ✅ USE sticky bit for:
# - /tmp directories
# - Shared upload directories
# - World-writable directories
# - Multi-user collaboration spaces

# ❌ DON'T use sticky bit for:
# - Private directories (use proper ownership instead)
# - Application directories (unnecessary)
# - Directories where admin manages all files

# Always verify:
$ ls -ld /your/directory
# Check for 't' in permissions
```

---

## Q25. What are /etc/passwd, /etc/shadow, and /etc/group Used For?

### **Answer:**

These three files are critical for user and group management in Linux systems. They store account information, encrypted passwords, and group memberships.

### **File Overview:**

| File | Purpose | Readable By | Contains |
|------|----------|-------------|-----------|
| **/etc/passwd** | User account information | All users | Username, UID, GID, home, shell |
| **/etc/shadow** | Encrypted passwords | Root only | Encrypted password, expiry info |
| **/etc/group** | Group information | All users | Group name, GID, members |

### **1. /etc/passwd**

#### **File Structure:**
```bash
username:password:UID:GID:GECOS:home:shell
          │        │   │   │    │      │    │
          │        │   │   │    │      │    └─ Login shell
          │        │   │   │    │      └───── Home directory
          │        │   │   │    └────────── Comment/GECOS field
          │        │   │   └─────────────── Primary GID
          │        │   └───────────────── UID (User ID)
          │        └───────────────────── x (shadow password)
          └─────────────────────────────── Username
```

#### **Example /etc/passwd:**
```bash
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
user:x:1000:1000:User Name:/home/user:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

#### **Field Explanations:**

| Field | Example | Description |
|-------|----------|-------------|
| **Username** | `user` | Login name (1-32 characters) |
| **Password** | `x` | Placeholder (actual in /etc/shadow) |
| **UID** | `1000` | User ID (0 = root, 1-999 = system, 1000+ = regular) |
| **GID** | `1000` | Primary Group ID |
| **GECOS** | `User Name` | Comment, real name, contact info |
| **Home** | `/home/user` | User's home directory |
| **Shell** | `/bin/bash` | Login shell (/bin/nologin = no shell) |

#### **Viewing User Information:**
```bash
# View entire file
$ cat /etc/passwd

# View specific user
$ grep user /etc/passwd
user:x:1000:1000:User Name:/home/user:/bin/bash

# Extract specific fields
$ awk -F: '{print $1, $3, $6, $7}' /etc/passwd
root 0 /root /bin/bash
user 1000 /home/user /bin/bash

# Find all users with bash shell
$ grep ':/bin/bash$' /etc/passwd
user:x:1000:1000:User Name:/home/user:/bin/bash

# Find all system users (UID < 1000)
$ awk -F: '$3 < 1000 {print $1, $3}' /etc/passwd
root 0
daemon 1
bin 2
```

### **2. /etc/shadow**

#### **File Structure:**
```bash
username:hash:last_change:min_age:max_age:warn:inactive:expire:reserved
          │     │      │           │        │     │        │      │
          │     │      │           │        │     │        │      └─ Reserved
          │     │      │           │        │     └────────── Account expiration date
          │     │      │           │        └───────────── Days of inactivity before lock
          │     │      │           └───────────────────── Days before warning
          │     │      └─────────────────────────────────── Maximum password age
          │     └─────────────────────────────────────────── Minimum password age
          └────────────────────────────────────────────────── Encrypted password hash
```

#### **Example /etc/shadow:**
```bash
$ sudo cat /etc/shadow
root:$6$rounds=656000$xyz...:18923:0:99999:7:::
daemon:*:18923:0:99999:7:::
bin:*:18923:0:99999:7:::
user:$6$rounds=656000$abc...:18923:0:99999:7:::
```

#### **Field Explanations:**

| Field | Meaning | Example |
|-------|---------|----------|
| **Username** | Login name | `user` |
| **Hash** | Encrypted password (SHA-512) | `$6$...` |
| **Last Change** | Days since Jan 1, 1970 password was changed | `18923` |
| **Min Age** | Minimum days before password change | `0` |
| **Max Age** | Maximum days password is valid | `99999` |
| **Warn** | Days before expiration to warn user | `7` |
| **Inactive** | Days after expiration until account lock | (empty) |
| **Expire** | Date when account expires (days since epoch) | (empty) |

#### **Password Hash Types:**

| Prefix | Algorithm | Description |
|--------|-----------|-------------|
| `$1$` | MD5 | Legacy, not recommended |
| `$5$` | SHA-256 | Secure |
| `$6$` | SHA-512 | Most common, default on modern systems |
| `$y$` | yescrypt | Very secure, newer systems |

#### **Viewing Password Information:**
```bash
# Root only
$ sudo cat /etc/shadow

# View specific user
$ sudo grep '^user:' /etc/shadow
user:$6$rounds=...:18923:0:99999:7:::

# Check password status (locked, expired)
$ sudo passwd -S user
user P 2024-01-15 0 99999 7 -1 (Password set, SHA512 crypt)

# Check last password change date
$ chage -l user
Last password change                                : Jan 15, 2024
Password expires                                    : never
Password inactive                                   : never
Account expires                                     : never
Minimum number of days between password change        : 0
Maximum number of days between password change        : 99999
```

### **3. /etc/group**

#### **File Structure:**
```bash
groupname:password:GID:members
           │        │     │    │
           │        │     │    └─ Comma-separated member list
           │        │     └─────── Group ID (GID)
           │        └───────────── x (placeholder, rarely used)
           └─────────────────────── Group name
```

#### **Example /etc/group:**
```bash
$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
user:x:1000:
sudo:x:27:user,admin
docker:x:1001:user,developer
www-data:x:33:
```

#### **Field Explanations:**

| Field | Description | Example |
|-------|-------------|----------|
| **Group Name** | Group identifier | `sudo` |
| **Password** | Group password (rarely used, usually x) | `x` |
| **GID** | Group ID | `27` |
| **Members** | Comma-separated list of users | `user,admin` |

#### **Viewing Group Information:**
```bash
# View all groups
$ cat /etc/group

# View specific group
$ grep sudo /etc/group
sudo:x:27:user,admin

# View groups for a specific user
$ groups user
user sudo docker

# Find all groups a user belongs to
$ id user
uid=1000(user) gid=1000(user) groups=1000(user),27(sudo),1001(docker)

# Find users in a group
$ grep -E ':$|^docker:' /etc/group | cut -d: -f4
user,developer

# Show group ID only
$ getent group sudo
sudo:x:27:user,admin
```

### **File Permissions:**

```bash
# Check file permissions
$ ls -l /etc/passwd /etc/shadow /etc/group
-rw-r--r-- 1 root root   2564 Jan 15 10:00 /etc/passwd
-r-------- 1 root shadow  1456 Jan 15 10:00 /etc/shadow
-rw-r--r-- 1 root root    892 Jan 15 10:00 /etc/group

# /etc/passwd: readable by all, writable only by root
# /etc/shadow: readable/writable only by root (shadow group)
# /etc/group: readable by all, writable only by root
```

### **User Management Commands:**

```bash
# Add user
$ sudo useradd -m -s /bin/bash newuser

# Set password
$ sudo passwd newuser

# Modify user
$ sudo usermod -aG docker newuser    # Add to docker group
$ sudo usermod -s /bin/zsh newuser    # Change shell

# Delete user
$ sudo userdel -r newuser              # Remove with home directory

# Lock user account
$ sudo usermod -L newuser

# Unlock user account
$ sudo usermod -U newuser

# View user details
$ id newuser
uid=1001(newuser) gid=1001(newuser) groups=1001(newuser),27(sudo)
```

### **Group Management Commands:**

```bash
# Add group
$ sudo groupadd developers

# Add user to group
$ sudo usermod -aG developers newuser

# Create group with specific GID
$ sudo groupadd -g 2000 mygroup

# Modify group
$ sudo groupmod -n newname oldname

# Delete group
$ sudo groupdel mygroup

# View group details
$ getent group developers
developers:x:1002:newuser,user1
```

### **Security Considerations:**

```bash
# /etc/passwd should be world-readable
# This allows applications to look up usernames

# /etc/shadow must be restricted (root only)
$ ls -l /etc/shadow
-r-------- 1 root shadow

# Check for world-readable shadow (security issue)
$ find /etc -name "shadow" -perm /o+r
# Should return nothing

# Check for world-writable /etc/passwd (security issue)
$ find /etc -name "passwd" -perm /o+w
# Should return nothing

# Monitor for suspicious changes
$ sudo apt install aide
$ sudo aide --init
$ sudo aide --check
```

### **Querying User/Group Information:**

```bash
# Get user information from /etc/passwd
$ getent passwd user
user:x:1000:1000:User Name:/home/user:/bin/bash

# Get group information from /etc/group
$ getent group sudo
sudo:x:27:user,admin

# Check if user exists
$ id user && echo "User exists" || echo "User not found"

# Check if user in specific group
$ groups user | grep -q sudo && echo "In sudo group"

# Find user's home directory
$ awk -F: '/^user:/ {print $6}' /etc/passwd
/home/user

# Find user's shell
$ awk -F: '/^user:/ {print $7}' /etc/passwd
/bin/bash
```

---

## Q26. What is the Difference Between su and sudo?

### **Answer:**

`su` (switch user) and `sudo` (superuser do) are both used for privilege escalation in Linux, but they work differently and serve different purposes.

### **Quick Comparison:**

| Aspect | su | sudo |
|--------|-----|------|
| **Full Name** | Switch User | Superuser Do |
| **Requires** | Target user's password | Current user's password (with sudo access) |
| **Privileges** | Complete switch to target user | Single command with elevated privileges |
| **Logs** | Limited logging | Comprehensive logging |
| **Session** | New shell session | Single command or shell |
| **Configuration** | /etc/passwd | /etc/sudoers |
| **Granularity** | All-or-nothing access | Fine-grained control |

### **su (Switch User):**

#### **Basic Usage:**
```bash
# Switch to root user
$ su
Password:  # Enter root password

# Verify
# whoami
root

# Exit back to original user
# exit
$ whoami
user
```

#### **Switch to Another User:**
```bash
# Switch to specific user
$ su - username
Password:

# Verify
$ whoami
username

# '-' (dash) loads target user's environment
# Without dash, keeps current user's environment
```

#### **Execute Single Command as Another User:**
```bash
# Execute command as root
$ su -c "apt update"
Password:  # root password

# Execute command as another user
$ su - username -c "ls ~username"
```

#### **su Examples:**
```bash
# Switch to root with environment
$ su -
root@server:~# pwd
/root

# Switch to root without loading environment
$ su
root@server:/home/user# pwd
/home/user

# Switch to another user
$ su - www-data
www-data@server:/var/www$ pwd
/var/www

# Run single command as root
$ su -c "systemctl restart nginx"
```

### **sudo (Superuser Do):**

#### **Basic Usage:**
```bash
# Execute single command with elevated privileges
$ sudo apt update
[sudo] password for user:  # Enter YOUR password

# Verify command ran with elevated privileges
$ sudo whoami
root

# Run command as another user
$ sudo -u username command
```

#### **Start Shell with sudo:**
```bash
# Start root shell
$ sudo -i
root@server:~# whoami
root

# Or use sudo su
$ sudo su -
root@server:~#

# Exit root shell
# exit
```

#### **Common sudo Commands:**
```bash
# Update system
$ sudo apt update && sudo apt upgrade

# Edit system file
$ sudo nano /etc/nginx/nginx.conf

# Restart service
$ sudo systemctl restart nginx

# Create directory in /opt
$ sudo mkdir /opt/myapp

# Change ownership
$ sudo chown user:group /path/to/file
```

### **Key Differences in Detail:**

#### **1. Password Requirement:**
```bash
# su: Requires target user's password
$ su
Password:  # Enter ROOT password

# sudo: Requires current user's password (if in sudoers)
$ sudo command
[sudo] password for user:  # Enter YOUR password

# sudo can be configured to not ask for password
# In /etc/sudoers:
user ALL=(ALL) NOPASSWD: ALL
```

#### **2. Environment and Shell:**
```bash
# su: Starts new shell with target user's environment
$ su -
root@server:~# echo $HOME
/root
root@server:~# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# sudo: Can preserve or modify environment
$ sudo -i     # Similar to su - (root environment)
$ sudo -s      # Preserve current environment
$ sudo -H      # Set HOME environment variable
```

#### **3. Logging and Auditing:**
```bash
# sudo logs all commands
# Check sudo log:
$ sudo grep sudo /var/log/auth.log
Jan 15 10:00 user sudo:  user : TTY=pts/0 ; PWD=/home/user ; USER=root ; COMMAND=/bin/cat /etc/shadow

# su has limited logging
# May not track individual commands

# View sudo access attempts
$ sudo journalctl -u sudo
```

#### **4. Granular Access Control:**

```bash
# /etc/sudoers configuration
# Allow specific commands only
user ALL=(ALL) /usr/bin/systemctl restart nginx

# Allow group members to run specific commands
%developers ALL=(ALL) /usr/bin/docker

# Allow running without password
user ALL=(ALL) NOPASSWD: /usr/bin/apt update

# Allow specific user full access
admin ALL=(ALL:ALL) ALL

# Allow user to run as another user
user ALL=(username) ALL
```

### **sudo vs su - Practical Examples:**

#### **Example 1: Restart Service:**

```bash
# Using su (requires root password)
$ su -c "systemctl restart nginx"
Password:  # root password

# Using sudo (requires your password)
$ sudo systemctl restart nginx
[sudo] password for user:  # your password
```

#### **Example 2: System Update:**

```bash
# Using su
$ su -
# apt update && apt upgrade
# exit

# Using sudo (single command)
$ sudo apt update && sudo apt upgrade

# Using sudo shell
$ sudo -i
# apt update && apt upgrade
# exit
```

#### **Example 3: Edit System File:**

```bash
# Using su
$ su -
# nano /etc/nginx/nginx.conf
# exit

# Using sudo
$ sudo nano /etc/nginx/nginx.conf

# Or
$ sudoedit /etc/nginx/nginx.conf  # Uses temporary file with security
```

### **sudo Features:**

#### **1. Run as Different User:**
```bash
# Run as www-data user
$ sudo -u www-data whoami
www-data

# Run as postgres user
$ sudo -u postgres psql

# Run with specific group
$ sudo -g developers command
```

#### **2. Preserved Environment Variables:**
```bash
# List environment variables
$ sudo env | grep HOME
HOME=/root

# Preserve user's HOME
$ sudo -H env | grep HOME
HOME=/home/user

# List preserved variables
$ sudo -V | grep "env_file"
```

#### **3. Sudo -l (List Permissions):**
```bash
# List user's sudo permissions
$ sudo -l
Matching Defaults entries for user on server:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User user may run the following commands on server:
    (ALL : ALL) ALL
    (root) /usr/bin/systemctl
```

### **sudo Configuration (/etc/sudoers):**

```bash
# View sudoers file
$ sudo visudo

# Basic syntax:
# user    HOST=(USER:GROUP) COMMANDS

# Examples:

# Full root access
root    ALL=(ALL:ALL) ALL

# User can run any command as root
user     ALL=(ALL) ALL

# User can run specific commands only
user     ALL=(ALL) /usr/bin/systemctl, /usr/bin/apt update

# Group permissions
%sudo    ALL=(ALL) ALL
%admin   ALL=(ALL) ALL

# No password required
user     ALL=(ALL) NOPASSWD: /usr/bin/systemctl

# Run as specific user
user     ALL=(postgres) /usr/bin/psql

# Aliases for commands
Cmnd_Alias WEB = /usr/sbin/nginx, /usr/sbin/apache2
user     ALL=(ALL) WEB
```

### **Security Best Practices:**

#### **For su:**
```bash
# ❌ Avoid frequent use of su
# Requires sharing root password

# ✅ Use su only for:
# - Administrative tasks
# - Emergency recovery
# - When sudo is not available

# Lock root account when using sudo
$ sudo passwd -l root
```

#### **For sudo:**
```bash
# ✅ Use sudo for regular admin tasks
# - Better logging
# - Fine-grained control
# - No root password sharing

# Limit sudo access to specific commands
%developers ALL=(ALL) /usr/bin/docker, /usr/bin/kubectl

# Require password for elevated commands
Defaults timestamp_timeout=15  # Re-ask password after 15 minutes

# Log all sudo usage
Defaults logfile="/var/log/sudo.log"
Defaults log_output
```

### **Choosing Between su and sudo:**

| Use Case | Recommended Command |
|----------|-------------------|
| Single command with elevated access | `sudo command` |
| Extended administrative session | `sudo -i` or `sudo su -` |
| Switch to another user account | `su - username` |
| Run command as specific user | `sudo -u username command` |
| System with multiple admins | `sudo` (better logging) |
| Recovery from sudo lockout | `su` (requires root password) |

### **Troubleshooting:**

```bash
# Check if user has sudo access
$ sudo -v
# Returns nothing if access granted

# View sudo rules
$ sudo -l

# Check sudo logs
$ sudo grep sudo /var/log/auth.log
$ sudo journalctl _COMM=sudo

# Reset sudo timestamp (will ask password again)
$ sudo -k
$ sudo ls
[sudo] password for user:

# If sudo is locked, use su
$ su -
# Check /etc/sudoers for syntax errors
# visudo -c
```

### **Summary Table:**

| Feature | su | sudo |
|---------|-----|------|
| **Password** | Target user's | Current user's (if authorized) |
| **Session** | New shell | Single command or shell |
| **Logging** | Minimal | Comprehensive |
| **Control** | All-or-nothing | Fine-grained |
| **Config** | System files | /etc/sudoers |
| **Use Case** | User switching, recovery | Administrative tasks |

---

## Q27. How Do You Safely Edit the sudoers File (visudo)?

### **Answer:**

The `visudo` command is the **only safe way** to edit the `/etc/sudoers` file. It provides syntax checking and prevents locking yourself out of sudo access.

### **Why Use visudo?**

```bash
# ❌ DANGEROUS: Editing directly
$ sudo nano /etc/sudoers
# If you make a syntax error:
# - You won't be able to use sudo
# - May need root access to fix
# - Could lock yourself out completely

# ✅ SAFE: Using visudo
$ sudo visudo
# - Validates syntax before saving
# - Prevents concurrent edits
# - Locks file during editing
# - Warns of syntax errors
```

### **Basic visudo Usage:**

```bash
# Open sudoers file in default editor
$ sudo visudo

# Edit file, then save and exit
# If syntax is valid, changes are saved
# If syntax is invalid, visudo will warn and not save
```

### **Choosing Editor:**

```bash
# View current default editor
$ sudo update-alternatives --config editor

# Set default editor for visudo
$ export EDITOR=nano
$ sudo visudo  # Uses nano

# One-time with specific editor
$ sudo EDITOR=nano visudo
$ sudo EDITOR=vim visudo
$ sudo EDITOR=mcedit visudo
```

### **visudo with Different Editors:**

```bash
# Using nano (simple, recommended for beginners)
$ sudo EDITOR=nano visudo

# Using vim (powerful)
$ sudo EDITOR=vim visudo

# Check which editor visudo will use
$ sudo visudo -c
/etc/sudoers: parsed OK
```

### **Syntax Checking:**

```bash
# Check sudoers file syntax without editing
$ sudo visudo -c
/etc/sudoers: parsed OK

# If there's an error:
$ sudo visudo -c
/etc/sudoers:12:12: syntax error near line 12
>>> /etc/sudoers: syntax error near line 12 <<<
```

### **Common visudo Workflow:**

```bash
# Step 1: Check current sudoers
$ sudo cat /etc/sudoers

# Step 2: Make backup before editing
$ sudo cp /etc/sudoers /etc/sudoers.bak

# Step 3: Edit with visudo
$ sudo visudo

# Step 4: Verify syntax
$ sudo visudo -c
/etc/sudoers: parsed OK

# Step 5: Test sudo access
$ sudo -v

# Step 6: Remove backup if everything works
$ sudo rm /etc/sudoers.bak
```

### **visudo Error Handling:**

```bash
# When you try to save with syntax error:
visudo: >>> /etc/sudoers: syntax error near line 12 <<<
visudo: What now?
Options are:
  (e)dit sudoers file again
  (x)it without saving changes to sudoers file
  (Q)uit and save changes to sudoers file (DANGER!)

# Choose 'e' to fix error
# Choose 'x' to discard changes
# NEVER choose 'Q' with syntax errors
```

### **Common Sudoers Syntax:**

```bash
# Basic format:
# username    host=(run_as_user:run_as_group) commands

# Examples:

# Full root access for user
user     ALL=(ALL:ALL) ALL

# Specific commands only
user     ALL=(ALL) /usr/bin/systemctl restart nginx

# Run as specific user
user     ALL=(postgres) /usr/bin/psql

# Group permissions
%wheel   ALL=(ALL) ALL
%admin   ALL=(ALL) ALL

# No password required
user     ALL=(ALL) NOPASSWD: /usr/bin/apt update

# Command aliases
Cmnd_Alias WEB = /usr/sbin/nginx, /usr/sbin/apache2
user     ALL=(ALL) WEB

# Multiple commands
user     ALL=(ALL) /usr/bin/systemctl, /usr/bin/docker, /usr/bin/kubectl
```

### **Practical Examples with visudo:**

#### **Example 1: Grant User Full Sudo Access:**

```bash
$ sudo visudo

# Add this line at the end:
newuser ALL=(ALL:ALL) ALL

# Save and exit
# Test:
$ sudo -u newuser sudo -l
newuser may run the following commands on server:
    (ALL : ALL) ALL
```

#### **Example 2: Grant Limited Command Access:**

```bash
$ sudo visudo

# Allow user to restart nginx only
developer ALL=(ALL) /usr/bin/systemctl restart nginx
developer ALL=(ALL) /usr/bin/systemctl reload nginx
developer ALL=(ALL) /usr/bin/systemctl status nginx

# Test:
$ sudo systemctl restart nginx  # Works
$ sudo systemctl restart mysql  # Denied
```

#### **Example 3: Grant Group Access:**

```bash
$ sudo visudo

# Allow all members of 'devops' group to use docker
%devops ALL=(ALL) /usr/bin/docker

# Allow 'admin' group full access
%admin ALL=(ALL:ALL) ALL

# Add user to group
$ sudo usermod -aG devops user
```

#### **Example 4: Passwordless Sudo for Specific Commands:**

```bash
$ sudo visudo

# Allow passwordless docker commands
user ALL=(ALL) NOPASSWD: /usr/bin/docker

# Allow passwordless systemctl for service management
user ALL=(ALL) NOPASSWD: /usr/bin/systemctl

# Test:
$ sudo docker ps  # No password prompt
$ sudo apt update  # Still asks for password
```

### **Advanced visudo Usage:**

#### **Viewing Current Rules:**

```bash
# Using sudo -l to list permissions
$ sudo -l
Matching Defaults entries for user on server:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User user may run the following commands on server:
    (ALL : ALL) ALL
    (root) /usr/bin/systemctl

# List permissions for another user (requires sudo)
$ sudo -l -U otheruser
```

#### **Using sudoers.d Directory:**

```bash
# Better approach: Create files in sudoers.d
$ sudo visudo -f /etc/sudoers.d/custom_rules

# Add rules to this file instead of main sudoers
# This keeps main sudoers clean

# Files in sudoers.d are automatically included
$ ls /etc/sudoers.d/
custom_rules  developers  monitoring
```

### **visudo Best Practices:**

```bash
# ✅ DO:
# - Always use visudo
# - Keep backups before editing
# - Test syntax after changes
# - Use specific command grants when possible
# - Group users by permissions
# - Use sudoers.d for custom rules

# ❌ DON'T:
# - Edit /etc/sudoers directly
# - Grant ALL to everyone
# - Ignore syntax errors
# - Skip testing after changes
# - Grant wildcard access unnecessarily
```

### **Recovering from visudo Errors:**

#### **If sudo is locked out:**

```bash
# Option 1: Use su to become root
$ su -
Password:  # Enter root password
# visudo  # Fix the error

# Option 2: Boot into single-user mode
# 1. Reboot
# 2. At GRUB, select 'Advanced options'
# 3. Select 'recovery mode'
# 4. Drop to root shell
# 5. Remount root as read-write
# mount -o remount,rw /
# 6. visudo to fix

# Option 3: Use live CD/USB
# 1. Boot from live media
# 2. Mount your filesystem
# 3. Edit /etc/sudoers
# 4. Reboot
```

#### **If syntax error detected:**

```bash
$ sudo visudo -c
/etc/sudoers:15:12: syntax error near line 15

# Fix by editing specific line
$ sudo visudo
# Go to line 15 and fix syntax
# Save and test again
```

### **Sudoers File Security:**

```bash
# Check sudoers file permissions
$ ls -l /etc/sudoers
-r--r----- 1 root root 1234 Jan 15 10:00 /etc/sudoers

# Should be readable only by root and group root
# If permissions are wrong:
$ sudo chmod 440 /etc/sudoers

# Check sudoers.d directory permissions
$ ls -ld /etc/sudoers.d
drwxr-xr-x 2 root root 4096 Jan 15 10:00 /etc/sudoers.d

# Files in sudoers.d should also be 440
$ ls -l /etc/sudoers.d/
-r--r----- 1 root root 123 Jan 15 10:00 custom_rules
```

### **Monitoring visudo Changes:**

```bash
# Track changes to sudoers
$ sudo grep visudo /var/log/auth.log

# Enable logging for sudo
# Add to /etc/sudoers:
Defaults logfile="/var/log/sudo.log"
Defaults log_host, log_year, log_pid

# View sudo logs
$ sudo tail -f /var/log/sudo.log
Jan 15 10:00: user : HOST=server : command=/usr/bin/cat /etc/shadow

# Track who edited sudoers
$ sudo ausearch -m avc -ts recent | grep sudoers
```

---

## Q28. How Do You Find Recently Modified Files (find -mtime)?

### **Answer:**

The `find` command with `-mtime`, `-atime`, and `-ctime` options allows locating files based on modification time. These options are essential for system administration, troubleshooting, and maintenance tasks.

### **Time-Based Find Options:**

| Option | Description | Unit |
|--------|-------------|-------|
| **-mtime** | File content modification time | Days |
| **-atime** | File access (read) time | Days |
| **-ctime** | File metadata change time | Days |
| **-mmin** | File content modification time | Minutes |
| **-amin** | File access time | Minutes |
| **-cmin** | File metadata change time | Minutes |

### **Time Value Syntax:**

```bash
-n  # Less than n units ago (recent)
+n  # More than n units ago (older)
 n  # Exactly n units ago
```

### **Basic Examples:**

#### **1. Find Files Modified in Last 7 Days:**
```bash
# Modified less than 7 days ago
$ find /path -mtime -7

# Example:
$ find /var/log -mtime -7
/var/log/syslog.1
/var/log/auth.log
/var/log/nginx/access.log
```

#### **2. Find Files Modified More Than 30 Days Ago:**
```bash
# Modified more than 30 days ago
$ find /path -mtime +30

# Example:
$ find /tmp -mtime +30
/tmp/old_file.txt
/tmp/abandoned_cache/
```

#### **3. Find Files Modified Exactly 1 Day Ago:**
```bash
# Modified exactly 1 day ago
$ find /path -mtime 1

# Example:
$ find /home/user/documents -mtime 1
```

### **Detailed Time-Based Examples:**

#### **Find Modified in Last 24 Hours:**
```bash
# Using -mmin for precision
$ find /path -mmin -1440    # 24 hours = 1440 minutes

# Or using -mtime (less precise)
$ find /path -mtime -1

# Example:
$ find /var/www/html -mmin -1440
/var/www/html/index.html
/var/www/html/style.css
```

#### **Find Modified Today:**
```bash
# Files modified today (less than 1 day)
$ find /path -mtime -1

# With time details
$ find /path -mtime -1 -ls

# Example:
$ find /home/user -mtime -1 -ls
123456    0 -rw-r--r--   1 user  user     0 Jan 15 10:00 /home/user/newfile.txt
```

#### **Find Modified Between Specific Dates:**
```bash
# Modified between 7 and 14 days ago
$ find /path -mtime +7 -mtime -14

# Example:
$ find /var/log -mtime +7 -mtime -14
/var/log/syslog.2.gz
/var/log/auth.log.2.gz
```

### **Access Time Examples:**

#### **Find Files Not Accessed in Last 90 Days:**
```bash
# Not accessed in more than 90 days
$ find /path -atime +90

# Example:
$ find /usr/share/doc -atime +90

# Safe to delete (if backup exists)
$ find /usr/share/doc -atime +90 -delete
```

#### **Find Frequently Accessed Files:**
```bash
# Accessed less than 1 day ago
$ find /path -atime -1

# Example:
$ find /var/www/html -atime -1
/var/www/html/index.html
```

### **Metadata Change Time Examples:**

#### **Find Files with Recent Permission Changes:**
```bash
# Permissions changed less than 1 day ago
$ find /path -ctime -1

# Example:
$ find /etc -ctime -1
/etc/passwd
/etc/group
```

#### **Find Files with Old Metadata:**
```bash
# Metadata unchanged for more than 365 days
$ find /path -ctime +365

# Example:
$ find /archive -ctime +365
/archive/old_project/
```

### **Combining Time with Other Criteria:**

#### **Find Recently Modified Log Files:**
```bash
# Log files (*.log) modified in last 7 days
$ find /var/log -name "*.log" -mtime -7

# With details
$ find /var/log -name "*.log" -mtime -7 -ls
```

#### **Find Recently Modified Large Files:**
```bash
# Files larger than 100MB modified in last 30 days
$ find /path -size +100M -mtime -30

# Example:
$ find /home -size +100M -mtime -30
/home/user/Videos/movie.mp4
/home/user/Downloads/software.iso
```

#### **Find Recently Modified by Specific User:**
```bash
# Files modified by 'nginx' user in last 24 hours
$ find /path -user nginx -mtime -1

# Example:
$ find /var/log -user nginx -mtime -1
/var/log/nginx/access.log
```

### **Minute-Based Precision:**

#### **Find Modified in Last Hour:**
```bash
# Modified less than 60 minutes ago
$ find /path -mmin -60

# Example:
$ find /tmp -mmin -60
/tmp/temp_file_1234
```

#### **Find Modified Between Specific Minutes:**
```bash
# Modified between 30 and 60 minutes ago
$ find /path -mmin +30 -mmin -60

# Example:
$ find /var/www/html -mmin +30 -mmin -60
/var/www/html/updated_page.html
```

### **Practical Use Cases:**

#### **1. Find and Clean Old Temporary Files:**
```bash
# Find files older than 7 days in /tmp
$ find /tmp -type f -mtime +7

# Delete them
$ find /tmp -type f -mtime +7 -delete

# Or remove manually after review
$ find /tmp -type f -mtime +7 -exec rm {} \;
```

#### **2. Find Recently Modified Configuration Files:**
```bash
# Config files modified in last 7 days
$ find /etc -name "*.conf" -mtime -7

# Example:
$ find /etc -name "*.conf" -mtime -7
/etc/nginx/nginx.conf
/etc/ssh/sshd_config
```

#### **3. Find Recently Accessed Web Files:**
```bash
# Files accessed in last 24 hours
$ find /var/www/html -atime -1

# Example:
$ find /var/www/html -type f -atime -1
/var/www/html/index.html
/var/www/html/about.html
```

#### **4. Find Large Old Files:**
```bash
# Files larger than 500MB older than 30 days
$ find / -size +500M -mtime +30 2>/dev/null

# Example:
$ find /home -size +500M -mtime +30
/home/user/Downloads/ubuntu.iso
/home/user/backup.tar.gz
```

#### **5. Backup Recently Modified Files:**
```bash
# Find files modified in last 24 hours and copy to backup
$ find /var/www/html -mtime -1 -exec cp --parents {} /backup/today/ \;

# Or use rsync with find
$ find /path -mtime -1 -print0 | rsync -av --from0 --files-from=- / backup/
```

### **Advanced Find Usage:**

#### **With Time Details:**
```bash
# Show file details with time
$ find /path -mtime -7 -ls

# Format output
$ find /path -mtime -7 -printf "%T+ %p\n"
2024-01-15+10:00:00.0000000000 /path/file.txt

# Human-readable time
$ find /path -mtime -7 -printf "%TY-%Tm-%Td %TH:%TM %p\n"
2024-01-15 10:00 /path/file.txt
```

#### **Combine with Actions:**
```bash
# List recently modified files
$ find /path -mtime -7 -type f -exec ls -lh {} \;

# Count recently modified files
$ find /path -mtime -7 -type f | wc -l

# Calculate total size
$ find /path -mtime -7 -type f -exec du -ch {} + | grep total

# Copy to another location
$ find /path -mtime -7 -exec cp {} /destination/ \;
```

### **Performance Tips:**

```bash
# Limit search depth
$ find /path -maxdepth 3 -mtime -7

# Search only specific filesystem
$ find /path -xdev -mtime -7

# Use -print0 for filenames with spaces
$ find /path -mtime -7 -print0 | xargs -0 ls -lh

# Exclude specific directories
$ find /path -mtime -7 -not -path "*/node_modules/*"
```

### **Find and Monitor Patterns:**

```bash
# Monitor directory for new files (last 1 minute)
$ find /path -mmin -1

# Find files that haven't been modified recently (potential issues)
$ find /var/log/app -name "*.log" -mtime +1
# If application log not updated in 1+ day, check if app is running

# Find old user files (cleanup candidates)
$ find /home -type f -mtime +365
```

### **Combining Multiple Time Criteria:**

```bash
# Modified recently but accessed long ago
$ find /path -mtime -7 -atime +30

# Accessed recently but modified long ago
$ find /path -atime -7 -mtime +30

# Permission changes but content unchanged
$ find /path -ctime -7 -mtime +30
```

---

## Q29. What Are Hidden Files and How to View Them (ls -a)?

### **Answer:**

In Linux, **hidden files** are files and directories whose names start with a dot (`.`). They are used for configuration files, user-specific settings, and system data that shouldn't clutter regular listings.

### **What Makes a File Hidden?**

```bash
# Hidden files start with dot (.)
.hidden_file.txt
.config/
.ssh/
.bashrc
.profile
```

### **Viewing Hidden Files:**

#### **1. Using ls -a (All Files):**
```bash
# Regular ls (doesn't show hidden files)
$ ls
documents  downloads  projects

# ls -a (shows hidden files)
$ ls -a
.  ..  .bashrc  .config  .profile  documents  downloads  projects

# Explanation:
# .  = Current directory
# .. = Parent directory
# .* = Hidden files and directories
```

#### **2. Using ls -A (Almost All):**
```bash
# Show hidden files except . and ..
$ ls -A
.bashrc  .config  .profile  documents  downloads  projects

# Useful for scripts
```

#### **3. ls -la (Long Format with Hidden):**
```bash
# Show hidden files with details
$ ls -la
total 20
drwxr-xr-x  5 user user 4096 Jan 15 10:00 .
drwxr-xr-x 20 root root 4096 Jan 10 09:00 ..
-rw-------  1 user user 220 Jan 10 00:00 .bash_history
-rw-r--r--  1 user user 220 Jan 10 00:00 .bashrc
drwx------  2 user user 4096 Jan 15 10:00 .ssh
-rw-r--r--  1 user user 1024 Jan 15 10:00 config.txt
```

#### **4. ls -ld (Directory Itself):**
```bash
# View directory's permissions (including hidden)
$ ls -ld
drwxr-xr-x 5 user user 4096 Jan 15 10:00 .

# With hidden count
$ ls -ldA /home/user
drwxr-xr-x 5 user user 4096 Jan 15 10:00 /home/user
```

### **Common Hidden Files and Their Purpose:**

| File | Purpose |
|------|---------|
| **.bashrc** | Bash shell configuration |
| **.bash_profile** | Login shell configuration |
| **.bash_history** | Command history |
| **.profile** | Generic shell configuration |
| **.ssh/** | SSH keys and config |
| **.git/** | Git configuration |
| **.config/** | Application configs |
| **.local/** | User-specific data |
| **.cache/** | Application cache |
| **.vimrc** | Vim editor settings |
| **.screenrc** | Screen terminal settings |
| **.viminfo** | Vim state |
| **.sudo_as_user/** | Sudo timestamp files |

### **Practical Examples:**

#### **View SSH Directory:**
```bash
$ ls -la ~/.ssh/
total 12
-rw------- 1 user user 2602 Jan 15 10:00 id_rsa
-rw-r--r-- 1 user user  578 Jan 15 10:00 id_rsa.pub
-rw-r--r-- 1 user user  150 Jan 15 10:00 config
-rw-r--r-- 1 user user  420 Jan 15 10:00 known_hosts
```

#### **View Configuration Files:**
```bash
$ ls -la ~ | grep "^\."
total 24
drwx------  2 user user 4096 Jan 15 10:00 .ssh
-rw-------  1 user user 1024 Jan 15 10:00 .bash_history
-rw-r--r--  1 user user  220 Jan 15 10:00 .bashrc
drwx------  5 user user 4096 Jan 15 10:00 .cache
```

### **Hidden Directories:**

```bash
# View hidden directories
$ ls -lad .* 2>/dev/null
drwxr-xr-x 5 user user 4096 Jan 15 10:00 .
drwxr-xr-x 20 root root 4096 Jan 10 09:00 ..
drwx------  2 user user 4096 Jan 15 10:00 .ssh
drwx------  5 user user 4096 Jan 15 10:00 .cache
drwxr-xr-x  3 user user 4096 Jan 15 10:00 .config
```

### **Creating Hidden Files:**

```bash
# Create hidden file
$ touch .hidden_file.txt

# Verify
$ ls -la .hidden_file.txt
-rw-r--r-- 1 user user 0 Jan 15 10:00 .hidden_file.txt

# Create hidden directory
$ mkdir .secret_dir

# Verify
$ ls -lad .secret_dir
drwxr-xr-x 2 user user 4096 Jan 15 10:00 .secret_dir
```

### **Finding Hidden Files:**

```bash
# Find all hidden files in current directory
$ find . -maxdepth 1 -name '.*'

# Find all hidden files recursively
$ find ~ -name '.*'

# Find hidden files only (not directories)
$ find ~ -name '.*' -type f

# Find hidden directories
$ find ~ -name '.*' -type d

# Exclude . and ..
$ find ~ -name '.*' -not -name '.' -not -name '..'
```

### **Managing Hidden Files:**

#### **Copy Hidden Files:**
```bash
# Copy specific hidden file
$ cp ~/.bashrc ~/.bashrc.backup

# Copy all hidden files (including . and .. - be careful!)
$ cp -a .* /destination/

# Better: specific hidden files
$ cp -a .bashrc .profile /destination/
```

#### **Move Hidden Files:**
```bash
# Move specific hidden file
$ mv .old_config.txt ~/archive/

# Move directory
$ mv .config/ ~/backup/
```

#### **Delete Hidden Files:**
```bash
# Delete specific hidden file
$ rm .temporary_file.txt

# Delete all hidden files in current directory (careful!)
$ rm .[^.]* .??

# Delete hidden directory
$ rm -rf .old_temp/

# CAUTION: This deletes ALL hidden files!
# rm -rf .*   # DON'T DO THIS
```

### **Viewing Specific Types of Hidden Files:**

```bash
# View hidden directories only
$ ls -lad .*/

# View hidden regular files only
$ ls -la | grep "^-"

# View hidden configuration files
$ ls -la | grep "\.conf$"
ls: cannot access: No such file or directory

# Better:
$ find ~ -maxdepth 1 -name '.*' -type f
```

### **Hidden Files in Home Directory:**

```bash
# Count hidden files/directories
$ ls -la | grep "^\." | wc -l

# List size of hidden directories
$ du -sh ~/.config ~/.cache ~/.local ~/.ssh
4.0K    .ssh
12K     .local
64K     .cache
128K    .config
```

### **Application-Specific Hidden Files:**

```bash
# Vim configuration
$ ls -la ~/.vimrc ~/.viminfo

# Git configuration
$ ls -la ~/.gitconfig
-rw-r--r-- 1 user user 220 Jan 15 10:00 .gitconfig

# Docker configuration
$ ls -la ~/.docker/
-rw------- 1 user user 1024 Jan 15 10:00 config.json

# npm/yarn cache
$ ls -la ~/.npm/
drwx------ 10 user user 4096 Jan 15 10:00 _cacache
```

### **Security Considerations for Hidden Files:**

```bash
# Check permissions on sensitive hidden files
$ ls -la ~/.ssh/
-rw------- 1 user user 2602 Jan 15 10:00 id_rsa
-rw-r--r-- 1 user user  578 Jan 15 10:00 id_rsa.pub

# Private key should be 600 (rw-------)
# Public key should be 644 (rw-r--r--)

# Fix incorrect permissions
$ chmod 600 ~/.ssh/id_rsa
$ chmod 644 ~/.ssh/id_rsa.pub

# Check for world-writable hidden files (security issue)
$ find ~ -name '.*' -perm -o+w
```

### **Searching Hidden File Contents:**

```bash
# Search for pattern in hidden files
$ grep -r "keyword" ~/.config/
~/.config/app/settings.conf:keyword=value

# Search specific hidden file
$ grep "alias" ~/.bashrc

# Search all hidden files
$ grep -r "search_term" ~/.[^.]*
```

### **System Hidden Files:**

```bash
# System-wide hidden files
$ ls -la /
total 20
drwxr-xr-x 24 root root 4096 Jan 15 10:00 .
drwxr-xr-x 24 root root 4096 Jan 15 10:00 ..
drwx------  2 root root 4096 Jan 10 00:00 .cache
drwxr-xr-x  5 root root 4096 Jan 15 10:00 .system

# /etc hidden configs
$ ls -la /etc | grep "^\."
```

### **Best Practices for Hidden Files:**

```bash
# ✅ DO use hidden files for:
# - Application configuration
# - User preferences
# - Temporary files (like .swp for vim)
# - Security-sensitive files (SSH keys)

# ❌ DON'T use hidden files for:
# - Regular documents that users need to see
# - Important data that might be forgotten
# - System-critical files without documentation

# ALWAYS check for hidden files when troubleshooting
$ ls -la

# ALWAYS verify permissions on sensitive hidden files
$ ls -la ~/.ssh/
```

---

## Q30. Change File Permissions Using chmod

### **Answer:**

`chmod` (change mode) modifies file and directory permissions in Linux. Permissions control read, write, and execute access for owner, group, and others.

### **Permission Basics:**

```bash
# Permission format: rwxrwxrwx
# Groups: owner | group | others
# Permissions: read | write | execute

# Octal values:
r = 4
w = 2
x = 1
- = 0
```

### **chmod Syntax:**

```bash
# Symbolic mode
$ chmod [who][operator][permissions] file

# Octal mode
$ chmod [octal_number] file

# Recursive
$ chmod -R [mode] directory
```

### **Symbolic Mode Examples:**

#### **1. Add Permissions:**
```bash
# Add execute for owner
$ chmod u+x script.sh

# Add read for group
$ chmod g+r file.txt

# Add write for others
$ chmod o+w shared_file.txt

# Add multiple permissions
$ chmod u+rx file.txt
$ chmod go+r file.txt  # group and others read

# Add for all users
$ chmod a+rw file.txt
```

#### **2. Remove Permissions:**
```bash
# Remove execute from owner
$ chmod u-x script.sh

# Remove write from group
$ chmod g-w file.txt

# Remove read from others
$ chmod o-r file.txt

# Remove multiple permissions
$ chmod go-wx file.txt  # remove write+execute from group+others

# Remove from all users
$ chmod a-w file.txt
```

#### **3. Set Specific Permissions:**
```bash
# Set owner permissions
$ chmod u=rwx file.txt

# Set group permissions
$ chmod g=rx file.txt

# Set other permissions
$ chmod o=r file.txt

# Set for all
$ chmod u=rwx,g=rx,o=r file.txt

# Same as: chmod 754 file.txt
```

### **Octal Mode Examples:**

#### **Common Permission Values:**
```bash
# 644 = rw-r--r-- (owner: rw, group: r, others: r)
$ chmod 644 file.txt

# 755 = rwxr-xr-x (owner: rwx, group: rx, others: rx)
$ chmod 755 script.sh

# 600 = rw------- (owner only)
$ chmod 600 private_key.pem

# 700 = rwx------ (owner only)
$ chmod 700 private_dir/

# 666 = rw-rw-rw- (all read/write)
$ chmod 666 shared_file.txt

# 777 = rwxrwxrwx (all permissions)
$ chmod 777 world_writable.txt  # DANGEROUS - avoid!
```

#### **Octal Calculation:**
```bash
# Owner permissions (first digit)
rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-- = 4+0+0 = 4
r-x = 4+0+1 = 5

# Group permissions (second digit)
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4

# Other permissions (third digit)
r-- = 4+0+0 = 4
--- = 0+0+0 = 0

# Example: 754
7 = rwx (owner)
5 = r-x (group)
4 = r-- (others)
```

### **Practical chmod Examples:**

#### **1. Make Script Executable:**
```bash
# Create script
$ echo 'echo "Hello"' > script.sh

# Add execute permission
$ chmod +x script.sh

# Verify
$ ls -l script.sh
-rwxr-xr-x 1 user user 17 Jan 15 10:00 script.sh

# Execute
$ ./script.sh
Hello
```

#### **2. Set Web File Permissions:**
```bash
# Set standard web file permissions
$ chmod 644 /var/www/html/index.html

# Set web directory permissions
$ chmod 755 /var/www/html

# Verify
$ ls -l /var/www/html/
total 4
-rw-r--r-- 1 www-data www-data 1024 Jan 15 10:00 index.html
```

#### **3. Secure Configuration Files:**
```bash
# Make config readable by owner only
$ chmod 600 /etc/myapp/config.conf

# SSH keys should be restrictive
$ chmod 600 ~/.ssh/id_rsa
$ chmod 644 ~/.ssh/id_rsa.pub

# Verify
$ ls -l ~/.ssh/
-rw------- 1 user user 2602 Jan 15 10:00 id_rsa
-rw-r--r-- 1 user user  578 Jan 15 10:00 id_rsa.pub
```

#### **4. Shared Team Directory:**
```bash
# Directory with group write access
$ chmod 775 /shared/team

# Files in shared directory
$ chmod 664 /shared/team/file.txt

# Verify
$ ls -ld /shared/team
drwxrwxr-x 2 root team 4096 Jan 15 10:00 /shared/team
```

### **Recursive chmod:**

#### **Apply to Directory Tree:**
```bash
# Set permissions recursively
$ chmod -R 755 /var/www/html

# Verify a few files
$ ls -l /var/www/html/
total 8
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 index.html
-rwxr-xr-x 1 user user 2048 Jan 15 10:00 style.css
```

#### **Mix Files and Directories:**
```bash
# Files: 644, Directories: 755
$ find /var/www/html -type f -exec chmod 644 {} \;
$ find /var/www/html -type d -exec chmod 755 {} \;

# More efficient:
$ find /var/www/html -type f -exec chmod 644 {} +
$ find /var/www/html -type d -exec chmod 755 {} +

# Or:
$ chmod -R 755 /var/www/html
$ find /var/www/html -type f -exec chmod 644 {} \;
```

### **chmod with find:**

#### **Find and Change Specific Files:**
```bash
# Change permissions of all .log files
$ find /var/log -name "*.log" -exec chmod 644 {} \;

# Change all scripts to executable
$ find /home/user/scripts -name "*.sh" -exec chmod +x {} \;

# Make all files in directory read-only
$ find /shared/docs -type f -exec a-w {} \;
```

#### **Complex Find Conditions:**
```bash
# Large files older than 30 days
$ find /data -size +100M -mtime +30 -exec chmod 600 {} \;

# Files owned by specific user
$ find /path -user olduser -exec chmod 644 {} \;

# Directories with SGID bit
$ find /shared -type d -exec chmod g+s {} \;
```

### **Reference Other Files:**

```bash
# Copy permissions from reference file
$ chmod --reference=config.conf new_config.conf

# Apply to directory
$ chmod -R --reference=template/ project/

# Verify
$ ls -l new_config.conf
-rw-r--r-- 1 user user 1024 Jan 15 10:00 new_config.conf
```

### **Batch Permission Changes:**

```bash
# Make all scripts executable
$ chmod +x *.sh

# Make all configuration files readable by all
$ chmod a+r *.conf

# Remove write permission from all
$ chmod a-w *.txt

# Specific pattern
$ chmod u+x,g+w,o+r *.{sh,py}
```

### **Special Permissions:**

#### **Set UID (Set User ID):**
```bash
# Set UID bit (runs as owner)
$ chmod u+s /usr/bin/passwd

# Verify
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 59040 Jan 15 10:00 /usr/bin/passwd
         │
         └─ s = UID bit set
```

#### **Set GID (Set Group ID):**
```bash
# Set GID bit on directory
$ chmod g+s /shared/project

# Verify
$ ls -ld /shared/project
drwxrwsr-x 2 root team 4096 Jan 15 10:00 /shared/project
               │
               └─ s = GID bit set
```

#### **Sticky Bit:**
```bash
# Set sticky bit
$ chmod +t /tmp
# or
$ chmod 1777 /tmp

# Verify
$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 Jan 15 10:00 /tmp
               │
               └─ t = sticky bit
```

### **Octal for Special Permissions:**

```bash
# 1777 = sticky bit + rwxrwxrwx
$ chmod 1777 /tmp

# 2755 = GID bit + rwxr-xr-x
$ chmod 2755 /shared/project

# 4755 = UID bit + rwxr-xr-x
$ chmod 4755 /usr/local/bin/app
```

### **Testing Permissions:**

```bash
# Test if file is executable
if [ -x script.sh ]; then
    ./script.sh
fi

# Test if file is readable
if [ -r file.txt ]; then
    cat file.txt
fi

# Test if file is writable
if [ -w file.txt ]; then
    echo "content" >> file.txt
fi
```

### **chmod Verbose Mode:**

```bash
# Show changes made
$ chmod -v 644 *.txt
mode of 'file1.txt' changed from 0644 (rw-r--r--) to 0644 (rw-r--r--)
mode of 'file2.txt' preserved as 0644 (rw-r--r--)

# Recursive with verbose
$ chmod -Rv 755 /var/www/html
```

### **Best Practices:**

```bash
# ✅ GOOD: Minimum necessary permissions
$ chmod 644 config.conf
$ chmod 755 script.sh

# ❌ BAD: Too permissive
$ chmod 777 file.txt      # Anyone can read/write/execute
$ chmod 666 secret.txt    # Anyone can read/write

# ✅ GOOD: Secure defaults
$ chmod 600 ~/.ssh/id_rsa
$ chmod 644 ~/.ssh/id_rsa.pub

# ✅ GOOD: Appropriate web permissions
$ find /var/www/html -type f -exec chmod 644 {} \;
$ find /var/www/html -type d -exec chmod 755 {} \;
```

---

## Q31. Permissions to Execute a Script

### **Answer:**

To execute a script in Linux, it must have **execute permission** (`x`). The execute permission allows the file to be run as a program.

### **Setting Execute Permission:**

#### **1. Using chmod +x:**
```bash
# Create a simple script
$ echo '#!/bin/bash' > script.sh
$ echo 'echo "Hello World"' >> script.sh

# Check initial permissions
$ ls -l script.sh
-rw-r--r-- 1 user user 24 Jan 15 10:00 script.sh
         │││
         ││└─ No execute permission
         │└─── No execute permission
         └──── No execute permission

# Add execute permission
$ chmod +x script.sh

# Verify
$ ls -l script.sh
-rwxr-xr-x 1 user user 24 Jan 15 10:00 script.sh
         │││
         ││└─ x = execute for others
         │└─── x = execute for group
         └──── x = execute for owner
```

#### **2. Using chmod 755:**
```bash
# Set specific permissions
$ chmod 755 script.sh

# Verify
$ ls -l script.sh
-rwxr-xr-x 1 user user 24 Jan 15 10:00 script.sh

# 7 = rwx (owner)
# 5 = r-x (group)
# 5 = r-x (others)
```

#### **3. Using chmod u+x (Owner Only):**
```bash
# Add execute for owner only
$ chmod u+x script.sh

# Verify
$ ls -l script.sh
-rwxr--r-- 1 user user 24 Jan 15 10:00 script.sh
         │││││
         ││││└─ Others: read only
         │││└────── Group: read only
         ││└─────── Owner: rwx (execute added)
         │└──────── File type (-)
```

### **Executing the Script:**

#### **1. Direct Execution:**
```bash
# Execute using ./ (current directory)
$ ./script.sh
Hello World

# Must have execute permission
# Shebang (#!/bin/bash) determines interpreter
```

#### **2. Specifying Interpreter:**
```bash
# Execute without execute permission
$ bash script.sh
Hello World

# Or
$ sh script.sh
Hello World

# Works even if execute permission is not set
```

### **Shebang Line:**

```bash
#!/bin/bash
#  │  │
#  │  └─ Path to interpreter
#  └─ Shebang indicator

# Common shebangs:
#!/bin/bash      # Bash shell
#!/bin/sh        # POSIX shell
#!/usr/bin/python3  # Python
#!/usr/bin/perl  # Perl
#!/usr/bin/env python3  # Use PATH to find python3
```

### **Complete Script Creation Example:**

```bash
# Step 1: Create script file
$ cat > deploy.sh << 'EOF'
#!/bin/bash
echo "Starting deployment..."
echo "Deploying application..."
echo "Deployment complete!"
EOF

# Step 2: Set execute permission
$ chmod +x deploy.sh

# Step 3: Verify
$ ls -l deploy.sh
-rwxr-xr-x 1 user user 84 Jan 15 10:00 deploy.sh

# Step 4: Execute
$ ./deploy.sh
Starting deployment...
Deploying application...
Deployment complete!
```

### **Execution Methods:**

#### **Method 1: Direct Path:**
```bash
# If script is in current directory
$ ./script.sh

# If script is in PATH
$ script.sh

# Full path
$ /home/user/scripts/script.sh

# All require execute permission
```

#### **Method 2: With Interpreter:**
```bash
# Using bash explicitly
$ bash script.sh

# Using sh
$ sh script.sh

# Doesn't require execute permission
# Shebang line is ignored
```

#### **Method 3: Source Script:**
```bash
# Source script (runs in current shell)
$ source script.sh
# or
$ . script.sh

# Variables/functions defined in script
# become available in current shell
```

### **Directory Execute Permission:**

```bash
# To execute script in directory:
# - Script needs execute permission
# - All directories in path need execute permission

# Example:
$ ls -l /home/user/scripts/myscript.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 myscript.sh

# Directory path needs execute permission
$ ls -ld /home/user/scripts/
drwxr-xr-x 2 user user 4096 Jan 15 10:00 scripts/
         │
         └─ x = execute permission (required to access)
```

### **Execute Permission by User Category:**

```bash
# Owner execute only
$ chmod u+x script.sh
-rwxr--r-- 1 user user 1024 Jan 15 10:00 script.sh
# Only owner can execute

# Group execute only
$ chmod g+x script.sh
-rw-r-xr-- 1 user user 1024 Jan 15 10:00 script.sh
# Owner and group can execute

# All users execute
$ chmod +x script.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 script.sh
# Everyone can execute
```

### **Multiple Scripts:**

```bash
# Make all shell scripts executable
$ chmod +x *.sh

# Verify
$ ls -l *.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 script1.sh
-rwxr-xr-x 1 user user 2048 Jan 15 10:00 script2.sh
-rwxr-xr-x 1 user user 512 Jan 15 10:00 script3.sh

# Make scripts in directory executable
$ chmod +x /home/user/scripts/*.sh
```

### **Finding Non-Executable Scripts:**

```bash
# Find shell scripts without execute permission
$ find /home/user/scripts -name "*.sh" ! -perm -111

# Find scripts without owner execute
$ find /path -name "*.sh" ! -perm -100

# Find scripts without any execute permission
$ find /path -name "*.sh" ! -perm /111
```

### **Setting Execute Permission Recursively:**

```bash
# Make all scripts in directory tree executable
$ find /scripts -name "*.sh" -exec chmod +x {} \;

# Verify
$ find /scripts -name "*.sh" -ls

# Make all files in directory executable (careful!)
$ chmod -R +x /scripts/  # NOT RECOMMENDED
```

### **Security Considerations:**

#### **Check Script Content:**
```bash
# Always review script before making it executable
$ cat script.sh
#!/bin/bash
# Script content here

# Check for malicious commands
$ grep -E "(rm -rf|dd if=|mkfs)" script.sh
```

#### **Verify Shebang:**
```bash
# Check if script has proper shebang
$ head -n 1 script.sh
#!/bin/bash

# Script without shebang:
# Will try to execute with current shell
# May produce unexpected results
```

#### **Set Appropriate Permissions:**

```bash
# Secure script (owner execute only)
$ chmod 700 script.sh
-rwx------ 1 user user 1024 Jan 15 10:00 script.sh

# Shared script (owner+group execute)
$ chmod 750 script.sh
-rwxr-x--- 1 user group 1024 Jan 15 10:00 script.sh

# Public script (all execute)
$ chmod 755 script.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 script.sh
```

### **PATH Variable and Execution:**

```bash
# Add directory to PATH
$ export PATH=$PATH:/home/user/scripts

# Execute without ./
$ script.sh

# Execute from any directory
$ cd /tmp
$ script.sh

# Find script location
$ which script.sh
/home/user/scripts/script.sh
```

### **Testing Script Execution:**

```bash
# Check if script is executable
if [ -x script.sh ]; then
    echo "Script is executable"
    ./script.sh
else
    echo "Script is not executable"
    chmod +x script.sh
    ./script.sh
fi
```

### **Common Issues and Solutions:**

#### **Issue 1: Permission Denied:**
```bash
$ ./script.sh
bash: ./script.sh: Permission denied

# Solution: Add execute permission
$ chmod +x script.sh
$ ./script.sh
```

#### **Issue 2: Bad Interpreter:**
```bash
$ ./script.sh
bash: ./script.sh: /bin/bash: bad interpreter

# Solution: Check shebang line
$ head -n 1 script.sh
#!/bin/bash

# Fix path if needed
$ sed -i '1s|#!/bin/sh|#!/bin/bash|' script.sh
```

#### **Issue 3: Windows Line Endings:**
```bash
$ ./script.sh
bash: ./script.sh: /bin/bash^M: bad interpreter

# Solution: Convert to Unix line endings
$ dos2unix script.sh
# or
$ sed -i 's/\r$//' script.sh
```

---

## Q32. Create/Manage Symbolic Links

### **Answer:**

**Symbolic links** (soft links) are files that point to other files or directories. They act like shortcuts in Windows, allowing you to reference files from different locations.

### **Creating Symbolic Links:**

#### **Basic Syntax:**
```bash
$ ln -s target_file link_name
#  │  │     │           │
#  │  │     │           └─ Name of the link to create
#  │  │     └────────────── File/directory to point to
#  │  └─────────────────── -s = symbolic link
#  └────────────────────── ln command
```

#### **Simple Example:**
```bash
# Create a symbolic link
$ ln -s /var/log/nginx/error.log ~/nginx_error.log

# Verify
$ ls -l ~/nginx_error.log
lrwxrwxrwx 1 user user 25 Jan 15 10:00 ~/nginx_error.log -> /var/log/nginx/error.log
                                     │
                                     └─ Points to target
```

#### **Link to Directory:**
```bash
# Create link to directory
$ ln -s /var/www/html ~/www

# Verify
$ ls -ld ~/www
lrwxrwxrwx 1 user user 13 Jan 15 10:00 ~/www -> /var/www/html

# Access through link
$ cd ~/www
$ pwd
/var/www/html
```

### **Viewing Symbolic Links:**

#### **1. Using ls -l:**
```bash
$ ls -l *.sh
lrwxrwxrwx 1 user user 25 Jan 15 10:00 link.sh -> /path/to/target.sh
-rwxr-xr-x 1 user user 1024 Jan 15 10:00 script.sh

# l = symbolic link
```

#### **2. Using readlink:**
```bash
# Show link target
$ readlink ~/nginx_error.log
/var/log/nginx/error.log

# Show absolute path
$ readlink -f ~/nginx_error.log
/var/log/nginx/error.log

# Show all links in directory
$ readlink -f *
```

#### **3. Using find:**
```bash
# Find all symbolic links in current directory
$ find . -type l

# Find all symbolic links recursively
$ find /path -type l

# Find broken symbolic links
$ find /path -type l -xtype l
```

### **Managing Symbolic Links:**

#### **1. Updating/Overwriting Link:**
```bash
# Create link
$ ln -s /path/to/old target

# Update link to point to new target
$ ln -sf /path/to/new target

# Verify
$ readlink target
/path/to/new
```

#### **2. Checking Link Status:**

```bash
# Working link
$ ls -l working_link
lrwxrwxrwx 1 user user 20 Jan 15 10:00 working_link -> /existing/file

# Broken (dangling) link
$ ls -l broken_link
lrwxrwxrwx 1 user user 20 Jan 15 10:00 broken_link -> /nonexistent/file

# Test if link is broken
if [ ! -e link ]; then
    echo "Link is broken"
fi
```

#### **3. Removing Symbolic Links:**
```bash
# Remove link (not target)
$ rm link_file

# Or use unlink
$ unlink link_file

# CAUTION: Don't use trailing /
$ rm link_file/    # ERROR if link points to directory
$ rm link_file    # Correct
```

### **Practical Use Cases:**

#### **1. Configuration Management:**
```bash
# Link to configuration file
$ ln -s /etc/nginx/nginx.conf ~/nginx.conf

# Edit through link (affects original)
$ nano ~/nginx.conf

# Verify changes in original
$ cat /etc/nginx/nginx.conf
```

#### **2. Application Versioning:**
```bash
# Install new version
$ sudo tar -xzf app-2.0.tar.gz -C /opt/

# Create current link
$ sudo ln -sfn /opt/app-2.0 /opt/app/current

# Applications use /opt/app/current
# Update link when new version installed
$ sudo ln -sfn /opt/app-2.1 /opt/app/current
```

#### **3. Shortcuts to Deep Directories:**
```bash
# Shortcut to logs
$ ln -s /var/log/nginx ~/nginx_logs

# Access easily
$ ls ~/nginx_logs/
access.log  error.log
```

#### **4. Library Linking:**
```bash
# Create library link
$ sudo ln -s /usr/local/lib/libmylib.so.1.0 /usr/lib/libmylib.so.1

# Applications can now find library
```

#### **5. Cross-Filesystem Links:**
```bash
# Link to file on different filesystem
$ ln -s /mnt/storage/file.txt ~/storage_file.txt

# Works across partitions (hard links can't)
```

### **Relative vs Absolute Paths:**

#### **Absolute Paths (Recommended for stability):**
```bash
# Create link with absolute path
$ ln -s /var/log/app.log ~/app.log

# Works from anywhere
$ cd /tmp
$ cat ~/app.log    # Still works
```

#### **Relative Paths:**
```bash
# Create link with relative path
$ ln -s ../data/file.txt link.txt

# Depends on current directory location
# May break if link is moved
```

### **Batch Operations with Symbolic Links:**

#### **Create Multiple Links:**
```bash
# Create links for all log files
$ for log in /var/log/*.log; do
    ln -s "$log" ~/logs/
done

# Verify
$ ls -l ~/logs/
lrwxrwxrwx 1 user user 20 Jan 15 10:00 access.log -> /var/log/access.log
lrwxrwxrwx 1 user user 20 Jan 15 10:00 error.log -> /var/log/error.log
```

#### **Find and Fix Broken Links:**
```bash
# Find all broken links
$ find ~ -type l -xtype l
/broken/link

# Delete broken links
$ find ~ -type l -xtype l -delete

# Or find and report
$ find ~ -type l -exec test ! -e {} \; -print
```

#### **Backup Links:**
```bash
# Create backup before overwriting
$ ln -sb /old/target link_name

# Updates link and backs up old target
```

### **Link Permissions:**

```bash
# Link permissions (lrwxrwxrwx) don't restrict access
# Actual access depends on target file permissions

# Create link to restricted file
$ sudo ln -s /etc/shadow ~/shadow_link

# Cannot read (target file restricts access)
$ cat ~/shadow_link
cat: ~/shadow_link: Permission denied

# Verify link permissions
$ ls -l ~/shadow_link
lrwxrwxrwx 1 user user 11 Jan 15 10:00 ~/shadow_link -> /etc/shadow
            │
            └─ rwxrwxrwx (doesn't matter - target permissions apply)
```

### **Advanced Link Operations:**

#### **Follow Symlink:**
```bash
# Get real path of link
$ readlink -f ~/link
/real/path/to/target

# Use in scripts
REAL_PATH=$(readlink -f ~/link)
cd "$REAL_PATH"
```

#### **Check if Path is Symbolic Link:**
```bash
# Test if path is symlink
if [ -L path ]; then
    echo "Path is symbolic link"
fi

# Get link target
if [ -L path ]; then
    TARGET=$(readlink path)
    echo "Points to: $TARGET"
fi
```

#### **Symbolic Links in Directories:**

```bash
# Create links for all files in directory
$ for file in /source/*; do
    ln -s "$file" /dest/
done

# Verify
$ ls -l /dest/
lrwxrwxrwx 1 user user 20 Jan 15 10:00 file1 -> /source/file1
lrwxrwxrwx 1 user user 20 Jan 15 10:00 file2 -> /source/file2
```

### **Best Practices:**

```bash
# ✅ DO use symbolic links for:
# - Configuration management
# - Application versioning
# - Cross-filesystem references
# - Shortcuts to deep paths
# - Library linking

# ❌ DON'T use symbolic links for:
# - Critical system components (unless required)
# - Files that need to be copied for backup
# - Situations where hard link is more appropriate

# ✅ Use absolute paths for stability
$ ln -s /absolute/path/to/target link_name

# ✅ Verify links work after creation
$ test -e link_name && echo "OK" || echo "Broken"

# ✅ Check for broken links regularly
$ find /important/path -type l -xtype l
```

### **Troubleshooting:**

#### **Link doesn't work:**
```bash
# Check if target exists
$ readlink link_name
/path/to/target
$ test -e /path/to/target && echo "OK" || echo "Target missing"

# Check if target is accessible
$ ls -l /path/to/target
```

#### **Too many levels of symbolic links:**
```bash
# Follow links to see chain
$ readlink -f link_name

# Avoid circular links
$ ln -s /a/b /a/b/c  # Creates circular reference
```

---

## Summary - Module 2: File Management & Permissions

This module covers essential file operations and permission management:

| Topic | Key Commands | Concepts |
|--------|-------------|------------|
| **File Permissions** | `chmod`, `ls -l` | rwx, octal (755, 644), user/group/other |
| **Permission Types** | read (4), write (2), execute (1) | File vs directory effects |
| **Soft vs Hard Links** | `ln`, `ln -s` | Inodes, cross-filesystem, dangling links |
| **Ownership** | `chown`, `chgrp` | UID/GID, recursive changes |
| **Umask** | `umask` | Default permissions, subtract from max |
| **Sticky Bit** | `chmod +t` | /tmp, shared directories |
| **User/Group Files** | `/etc/passwd`, `/etc/shadow`, `/etc/group` | Account management |
| **su vs sudo** | `su`, `sudo` | Password, session, logging |
| **visudo** | `sudo visudo` | Safe sudoers editing |
| **File Finding** | `find -mtime`, `-atime`, `-ctime` | Time-based searches |
| **Hidden Files** | `ls -a`, `ls -la` | Dotfiles, config files |
| **chmod** | `chmod u+x`, `chmod 755` | Symbolic and octal modes |
| **Execute Scripts** | `chmod +x`, `./script.sh` | Shebang, PATH |
| **Symbolic Links** | `ln -s`, `readlink` | Soft links, shortcuts |

### **Essential Commands from Module 2:**

```bash
# Permissions
chmod 755 script.sh
chmod 644 config.txt
chmod +x deploy.sh
chmod ugo+rwx file.txt

# Ownership
chown user:group file.txt
chown -R user:group /path
chgrp team /shared/dir

# Links
ln -s /target link_name
ln hardlink_target hardlink
readlink link_name

# Time-based find
find /path -mtime -7
find /path -atime +30
find /path -mmin -60

# Hidden files
ls -la
ls -A

# Umask
umask 022
umask 077

# User management
useradd -m newuser
usermod -aG docker user
groups user

# Sudo
visudo
sudo -l
```

---

**Next Module:** Process & System Management (Module 3)
