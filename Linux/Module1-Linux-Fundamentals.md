# 🟩 MODULE 1: Linux Fundamentals (Core Concepts)

## Q1. What is Linux? Difference between Linux, UNIX, and Windows?

### **Answer:**

**Linux:**
- Linux is an open-source, Unix-like operating system kernel created by Linus Torvalds in 1991
- It's distributed under the GNU General Public License (GPL)
- The complete Linux OS includes kernel, system libraries, shell, and utilities
- Examples: Ubuntu, CentOS, Debian, Fedora, Red Hat Enterprise Linux

**UNIX:**
- UNIX is a proprietary, commercial operating system developed in the 1960s at AT&T Bell Labs
- It's a complete OS with closed source code (except some variants like AIX, Solaris)
- Requires licensing fees
- Examples: AIX (IBM), Solaris (Oracle), HP-UX (Hewlett Packard)

**Windows:**
- Windows is a proprietary operating system developed by Microsoft
- Closed source with commercial licensing
- Uses NTFS file system, registry for configuration
- Graphical User Interface (GUI) centric

### **Key Differences:**

| Feature        | Linux               | UNIX                | Windows               |
|----------------|---------------------|---------------------|-----------------------|
| **Source**     | Open Source         | Mostly Proprietary  | Closed Source         |
| **Kernel**     | Monolithic          | Monolithic/Micro    | Hybrid (NT)           |
| **File System**| ext4, xfs, btrfs    | UFS, ZFS, JFS       | NTFS, FAT32           |
| **Cost**       | Free                | Licensed/Paid       | Licensed/Paid         |
| **Security**   | High (permissions)  | High                | Variable              |
| **Shell**      | bash, zsh, fish     | sh, csh, ksh        | PowerShell, CMD       |

---

## Q2. Explain Linux Architecture (Kernel, Shell, User Space)

### **Answer:**

The Linux architecture consists of three main layers:

```
┌─────────────────────────────────────────────────┐
│               User Space (Applications)         │
│  - User Applications                            │
│  - System Libraries (glibc, etc.)               │
│  - Shell (bash, zsh)                            │
└─────────────────────┬───────────────────────────┘
                      │ System Calls
┌─────────────────────▼───────────────────────────┐
│               Kernel Space                      │
│  - System Call Interface                        │
│  - Process Management                           │
│  - Memory Management                            │
│  - Virtual File System                          │
│  - Network Stack                                │
│  - Device Drivers                               │
└─────────────────────┬───────────────────────────┘
                      │ Hardware Instructions
┌─────────────────────▼───────────────────────────┐
│              Hardware Layer                     │
│  - CPU, RAM, Storage, Network Devices          │
└─────────────────────────────────────────────────┘
```

### **Components:**

**1. Hardware Layer:**
- Physical components: CPU, memory, disks, network interfaces
- Directly controlled by the kernel

**2. Kernel Space:**
- **System Call Interface:** API between user space and kernel
- **Process Management:** Creates, schedules, terminates processes
- **Memory Management:** Virtual memory, paging, memory allocation
- **Virtual File System:** Abstract layer for different file systems
- **Network Stack:** TCP/IP protocol implementation
- **Device Drivers:** Interface for hardware devices

**3. User Space:**
- **Applications:** User programs (web servers, databases, etc.)
- **System Libraries:** Provide API for system calls (glibc)
- **Shell:** Command-line interpreter for user interaction

---

## Q3. What Happens When You Type a Command and Press Enter?

### **Answer:**

When you execute a command in Linux, the following sequence occurs:

```
1. Shell Parsing
   ↓
2. Command Resolution (PATH lookup)
   ↓
3. Fork System Call (create child process)
   ↓
4. Exec System Call (replace child process with command)
   ↓
5. Command Execution
   ↓
6. Wait/Exit Handling
   ↓
7. Prompt Return
```

### **Detailed Steps:**

**Step 1: Shell Parsing**
```bash
$ ls -la /home
```
- Shell reads the input and parses it into:
  - Command name: `ls`
  - Options: `-la`
  - Arguments: `/home`

**Step 2: Command Resolution**
- Shell checks if command is:
  - **Built-in:** Shell executes it directly (e.g., `cd`, `echo`, `export`)
  - **External:** Shell searches `$PATH` directories for executable

```bash
$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
```

**Step 3: Fork System Call**
- Shell creates a child process using `fork()`
- Child process is an exact copy of the parent (shell)

**Step 4: Exec System Call**
- Child process calls `execve()` to replace itself with the command
- The command's code is loaded into memory and starts executing

**Step 5: Command Execution**
- The command runs in the child process
- It may make additional system calls (read files, write output, etc.)

**Step 6: Wait/Exit Handling**
- Parent shell waits for child to complete using `wait()`
- Child process exits with an exit status (0 = success, non-zero = error)
- Exit status stored in `$?` variable

**Step 7: Prompt Return**
- Shell displays the prompt again, ready for next command

```bash
$ ls -la
total 48
drwxr-xr-x 12 user user 4096 Jan 15 10:30 .
drwxr-xr-x  3 root root 4096 Jan 10 09:00 ..
-rw-r--r--  1 user user  220 Jan 10 09:00 .bashrc
$ echo $?
0
```

---

## Q4. What are Environment Variables? How to View/Set Them?

### **Answer:**

**Environment Variables** are dynamic values that affect process behavior without changing the program code. They are stored in the process's memory and inherited by child processes.

### **Viewing Environment Variables:**

**View all environment variables:**
```bash
$ env                    # Display all environment variables
$ printenv              # Alternative method
$ export                # Show all exported variables
```

**View specific variable:**
```bash
$ echo $PATH
/usr/local/bin:/usr/bin:/bin

$ echo $HOME
/home/user

$ echo $USER
user
```

**View all variables (including shell variables):**
```bash
$ set
```

### **Setting Environment Variables:**

**Temporary (current session only):**
```bash
$ export MY_VAR="value"
$ echo $MY_VAR
value
```

**For current command only:**
```bash
$ MY_VAR="value" command
```

**Permanent (add to shell configuration):**

**For bash:**
```bash
# Add to ~/.bashrc or ~/.bash_profile
export JAVA_HOME=/usr/lib/jvm/java-11
export PATH=$PATH:$JAVA_HOME/bin
export DB_HOST=localhost
```

**For zsh:**
```bash
# Add to ~/.zshrc
export NODE_VERSION=18
export PATH=$PATH:/usr/local/node/bin
```

### **Common Environment Variables:**

| Variable      | Purpose                              | Example                     |
|---------------|--------------------------------------|-----------------------------|
| `PATH`        | Directories to search for commands   | `/usr/bin:/bin`             |
| `HOME`        | Current user's home directory        | `/home/user`                |
| `USER`        | Current username                     | `user`                      |
| `SHELL`       | Default shell path                   | `/bin/bash`                 |
| `PWD`         | Current working directory            | `/home/user/projects`       |
| `TERM`        | Terminal type                        | `xterm-256color`            |
| `LANG`        | System language and locale           | `en_US.UTF-8`               |
| `PS1`         | Primary prompt string                | `\u@\h:\w$`                 |
| `EDITOR`      | Default text editor                  | `/usr/bin/vim`              |
| `DISPLAY`     | X11 display server                   | `:0`                        |

### **Unsetting Variables:**
```bash
$ unset MY_VAR
```

---

## Q5. What are Absolute vs Relative Paths?

### **Answer:**

**Absolute Path:**
- Starts from the root directory (`/`)
- Complete path from root to the target file/directory
- Always unique and works from any location
- Example: `/home/user/documents/file.txt`

**Relative Path:**
- Starts from the current working directory
- Relative to your current location in the file system
- Changes based on your current directory
- Example: `documents/file.txt` or `../config/app.conf`

### **Examples:**

**Absolute Path:**
```bash
$ cd /var/log/nginx
$ cat /etc/nginx/nginx.conf
$ ls -la /home/user/projects
```

**Relative Path:**
```bash
# If currently in /home/user
$ cd documents              # Goes to /home/user/documents
$ cat config/app.conf       # Reads /home/user/config/app.conf
$ ls ../downloads           # Lists /home/downloads
```

### **Special Relative Path Symbols:**

| Symbol | Meaning                            | Example                  |
|--------|------------------------------------|--------------------------|
| `.`    | Current directory                  | `./script.sh`            |
| `..`   | Parent directory (one level up)    | `../config/file.conf`    |
| `~`    | User's home directory              | `~/documents`            |
| `-`    | Previous working directory         | `cd -`                   |

### **Practical Examples:**

```bash
# Current directory: /home/user/projects

# Absolute path - works from anywhere
$ cat /etc/hosts

# Relative path - from current directory
$ cat ../.bashrc              # Reads /home/user/.bashrc

# Using ~ for home directory
$ cd ~/downloads              # Goes to /home/user/downloads

# Using . for current directory
$ ./script.sh                 # Executes script in current dir

# Using .. to go up
$ cd ../../var/log            # From /home/user/projects to /var/log
```

### **Best Practices:**

- Use **absolute paths** in:
  - Shell scripts (for reliability)
  - Configuration files
  - Cron jobs
  - System services

- Use **relative paths** in:
  - Interactive shell usage
  - Project-specific references
  - Portable scripts

---

## Q6. Explain Linux Directory Structure (/, /bin, /etc, /home, /var)

### **Answer:**

Linux follows the **Filesystem Hierarchy Standard (FHS)** which defines the directory structure:

```
/
├── bin       # Essential user binaries
├── boot      # Boot loader files
├── dev       # Device files
├── etc       # System configuration files
├── home      # User home directories
├── lib       # System libraries
├── media     # Removable media mount points
├── mnt       # Temporary mount points
├── opt       # Optional software packages
├── proc      # Process and kernel information
├── root      # Root user's home directory
├── run       # Runtime data
├── sbin      # System binaries
├── srv       # Service data
├── sys       | Kernel and hardware information
├── tmp       # Temporary files
├── usr       # User utilities and applications
└── var       | Variable data (logs, cache, etc.)
```

### **Key Directories Explained:**

#### **`/` (Root)**
- Top-level directory of the entire file system
- All other directories are under root
- Root user's home is separate at `/root`

```bash
$ ls -la /
total 24
drwxr-xr-x  24 root root 4096 Jan 15 10:00 .
drwxr-xr-x  24 root root 4096 Jan 15 10:00 ..
drwxr-xr-x   2 root root 4096 Jan 10 00:00 bin
drwxr-xr-x   4 root root 4096 Jan 10 00:00 boot
drwxr-xr-x  20 root root 3560 Jan 15 10:30 dev
drwxr-xr-x 120 root root 4096 Jan 15 10:00 etc
drwxr-xr-x   3 root root 4096 Jan 10 00:00 home
```

#### **`/bin` (Binaries)**
- Essential user command binaries
- Available to all users (including single-user mode)
- Contains commands like: `ls`, `cp`, `mv`, `cat`, `grep`

```bash
$ ls /bin | head -20
bash
cat
chmod
cp
date
df
echo
grep
kill
ls
mkdir
mv
ping
rm
sh
sleep
```

#### **`/etc` (Etcetera - Configuration)**
- System-wide configuration files
- No binary files, only text configurations
- Critical for system administration

```bash
$ ls /etc | head -20
passwd
shadow
group
hosts
resolv.conf
fstab
nginx/
ssh/
systemd/
cron.d/
```

**Important files in `/etc`:**
- `/etc/passwd` - User account information
- `/etc/shadow` - Encrypted passwords
- `/etc/hosts` - Hostname to IP mapping
- `/etc/fstab` - File system mount table
- `/etc/ssh/sshd_config` - SSH server configuration
- `/etc/nginx/nginx.conf` - Nginx web server config

#### **`/home` (Home Directories)**
- Contains personal directories for regular users
- Each user has a subdirectory: `/home/username`
- Stores user data, configurations, documents

```bash
$ ls /home
user1
user2
developer
```

**User home directory structure:**
```
/home/user/
├── .bashrc          # Bash configuration
├── .bash_profile    # Login shell config
├── .ssh/            # SSH keys
├── documents/       # User documents
├── downloads/       # Downloaded files
└── projects/        # User projects
```

#### **`/var` (Variable Data)**
- Files whose content is expected to grow
- System logs, cache, spool files
- Data that changes frequently

```bash
$ ls /var
log/        # System and application logs
cache/      # Application cache
tmp/        # Temporary files
spool/      # Print/mail queues
www/        # Web server files (sometimes)
lib/        # Variable state information
```

**Important subdirectories in `/var`:**

| Path          | Purpose                          |
|---------------|----------------------------------|
| `/var/log`    | System logs (syslog, auth.log)   |
| `/var/log/nginx` | Nginx access and error logs     |
| `/var/cache`  | Application cache data           |
| `/var/spool`  | Queued jobs (cron, mail, print)  |
| `/var/tmp`    | Temporary files preserved across reboots |

```bash
$ ls /var/log
auth.log
syslog
kern.log
nginx/
journal/
```

#### **Other Important Directories:**

| Directory | Purpose                              |
|-----------|--------------------------------------|
| `/usr`    | User utilities, applications, libraries |
| `/sbin`   | System binaries (admin commands)     |
| `/dev`    | Device files (hardware interfaces)    |
| `/proc`   | Virtual filesystem for process info   |
| `/sys`    | Kernel and hardware information       |
| `/tmp`    | Temporary files (cleared on reboot)   |
| `/opt`    | Optional software packages           |
| `/root`   | Root user's home directory           |

---

## Q7. What are the Types of Shells in Linux?

### **Answer:**

A **shell** is a command-line interpreter that provides a user interface for the Unix/Linux operating system. It reads commands from input and executes them.

### **Common Shell Types:**

#### **1. Bourne Shell (sh)**
- Original Unix shell by Stephen Bourne
- Available on all Unix systems
- Basic scripting capabilities

```bash
#!/bin/sh
echo "Bourne Shell script"
```

#### **2. Bash (Bourne Again Shell)**
- Default shell on most Linux distributions
- Compatible with sh but with many enhancements
- Features: command history, tab completion, job control

```bash
$ echo $SHELL
/bin/bash
```

#### **3. Zsh (Z Shell)**
- Powerful shell with advanced features
- Improved tab completion, theme support
- Compatible with bash scripts

```bash
$ zsh --version
zsh 5.8 (x86_64-ubuntu-linux-gnu)
```

#### **4. C Shell (csh) and TC Shell (tcsh)**
- C-like syntax for scripting
- `tcsh` is an improved version of `csh`

```bash
#!/bin/csh
setenv MY_VAR "value"
```

#### **5. Korn Shell (ksh)**
- Developed by David Korn
- Combines features of sh and csh

#### **6. Fish (Friendly Interactive Shell)**
- Modern, user-friendly shell
- Auto-suggestions, syntax highlighting
- Not POSIX-compliant

### **Shell Comparison:**

| Shell | POSIX Compatible | Features | Use Case |
|-------|------------------|----------|----------|
| `sh`  | Yes              | Basic    | Portability |
| `bash`| Yes              | Rich     | Default/General |
| `zsh` | Mostly           | Advanced | Power users |
| `csh` | No               | C-like   | Legacy |
| `ksh`  | Yes              | Enhanced | AIX/Solaris |
| `fish`| No               | Modern   | Interactive use |

### **Checking Available Shells:**

```bash
# List all installed shells
$ cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/zsh
/bin/tcsh
/bin/fish

# Check current shell
$ echo $SHELL
/bin/bash

# Check shell version
$ bash --version
GNU bash, version 5.1.16(1)-release
```

### **Changing Shells:**

```bash
# Temporary change (current session only)
$ zsh

# Permanent change
$ chsh -s /bin/zsh
Changing shell for user.
Password: ****

# Or edit /etc/passwd
user:x:1000:1000:User:/home/user:/bin/zsh
```

### **Shell Script Shebang:**

```bash
#!/bin/bash    # Use bash
#!/bin/sh      # Use POSIX sh
#!/bin/zsh     # Use zsh
#!/usr/bin/env python3  # Use Python
```

---

## Q8. What is the Difference Between Shell and Kernel?

### **Answer:**

### **Shell:**
- **Interface** between user and kernel
- **Command-line interpreter** that reads and executes commands
- Runs in **user space**
- A **program** that provides a user interface
- Examples: bash, zsh, fish

```bash
$ ls -la           # Shell interprets this command
$ cat file.txt     # Shell reads and executes
```

### **Kernel:**
- **Core** of the operating system
- **Manages hardware resources** (CPU, memory, I/O)
- Runs in **kernel space** (privileged mode)
- Provides **system calls** for user programs
- Handles process scheduling, memory management, device drivers

```
User → Shell → System Calls → Kernel → Hardware
```

### **Key Differences:**

| Aspect         | Shell                               | Kernel                             |
|----------------|-------------------------------------|------------------------------------|
| **Layer**      | User interface layer                | Core operating system layer        |
| **Space**      | User space                          | Kernel space                       |
| **Purpose**    | Interpret commands                  | Manage hardware and resources      |
| **Access**     | Direct user interaction             | No direct user access              |
| **Mode**       | User mode (restricted)              | Kernel mode (privileged)           |
| **Examples**   | bash, zsh, fish                     | Linux Kernel 5.x, 6.x              |
| **Function**   | Parses input, executes programs     | Schedules processes, manages memory|

### **How They Work Together:**

```
┌──────────────────────────────────────┐
│  User types: ls -la /home            │
└──────────────┬───────────────────────┘
               │
┌──────────────▼───────────────────────┐
│  Shell (bash)                        │
│  - Parses command: "ls -la /home"    │
│  - Resolves "ls" to /bin/ls          │
│  - Forks child process               │
└──────────────┬───────────────────────┘
               │ System Call
┌──────────────▼───────────────────────┐
│  Kernel                              │
│  - Executes fork() system call       │
│  - Executes execve() to run /bin/ls  │
│  - Handles file system operations    │
│  - Manages memory and I/O            │
└──────────────┬───────────────────────┘
               │ Hardware Instructions
┌──────────────▼───────────────────────┐
│  Hardware                            │
│  - CPU executes instructions         │
│  - Disk reads directory contents     │
│  - Memory stores process data        │
└──────────────────────────────────────┘
```

### **Example Flow:**

```bash
$ ls -la /home/user

# 1. Shell reads and parses the command
# 2. Shell finds /bin/ls in PATH
# 3. Shell makes system calls:
#    - fork() to create child process
#    - execve("/bin/ls", ["ls", "-la", "/home/user"])
# 4. Kernel executes the system calls
# 5. Kernel reads filesystem via VFS
# 6. Output returned to shell, displayed to user
```

---

## Q9. Explain File System Hierarchy - Where are Configs, Binaries, and Logs Kept?

### **Answer:**

The Linux Filesystem Hierarchy Standard (FHS) defines where different types of files should be stored.

### **Configuration Files Location:**

| Location          | Purpose                                     | Example Files                          |
|-------------------|---------------------------------------------|----------------------------------------|
| `/etc`            | System-wide configuration                   | `/etc/nginx/nginx.conf`                |
| `/etc/nginx`      | Nginx configuration                          | `/etc/nginx/conf.d/`                   |
| `/etc/ssh`        | SSH configuration                            | `/etc/ssh/sshd_config`                 |
| `/etc/systemd`    | Systemd service configurations              | `/etc/systemd/system/`                 |
| `~/.config`       | User-specific application config            | `~/.config/git/config`                 |
| `~/.ssh`          | User SSH keys and config                    | `~/.ssh/config`, `~/.ssh/id_rsa`      |

```bash
$ ls /etc
passwd          shadow          group           hosts
resolv.conf     fstab           cron.d/         nginx/
ssh/            systemd/        logrotate.d/    network/
```

### **Binaries Location:**

| Location      | Purpose                              | Example Commands            |
|---------------|--------------------------------------|-----------------------------|
| `/bin`        | Essential user binaries              | `ls`, `cat`, `cp`, `grep`   |
| `/sbin`       | System administration binaries       | `ifconfig`, `iptables`, `reboot` |
| `/usr/bin`    | Most user commands                   | `python3`, `git`, `docker`  |
| `/usr/sbin`   | Additional system admin commands     | `useradd`, `sshd`           |
| `/usr/local/bin` | Locally installed software        | Custom compiled tools       |

```bash
# Essential binaries
$ ls /bin
bash  cat  chmod  cp  echo  grep  ls  mkdir  mv  rm  sh

# System binaries
$ ls /sbin
ifconfig  iptables  reboot  shutdown  route

# User binaries
$ ls /usr/bin | grep -E "^(git|docker|python|node)"
docker  git  node  npm  python3
```

### **Logs Location:**

| Location          | Purpose                              | Example Files                       |
|-------------------|--------------------------------------|-------------------------------------|
| `/var/log`        | System and application logs          | `/var/log/syslog`, `/var/log/auth.log` |
| `/var/log/nginx`  | Nginx access and error logs          | `access.log`, `error.log`           |
| `/var/log/apache2`| Apache web server logs               | `access.log`, `error.log`           |
| `/var/log/journal`| Systemd journal logs                 | Binary log files                    |
| `/var/log/mysql`  | MySQL/MariaDB logs                   | `error.log`, `slow.log`             |

```bash
$ ls /var/log
auth.log       syslog         kern.log       nginx/
journal/       dmesg          boot.log       mysql/
```

### **Complete File System Organization:**

```
File System Hierarchy

CONFIGURATION FILES:
├── /etc                    # System-wide configurations
│   ├── nginx/              # Nginx config
│   ├── ssh/                # SSH config
│   ├── systemd/            # Service configs
│   └── passwd              # User accounts
└── ~/.config               # User application configs

BINARIES:
├── /bin                    # Essential user commands
├── /sbin                   # System admin commands
├── /usr/bin                # Standard user commands
├── /usr/sbin               # Additional admin commands
└── /usr/local/bin          # Local software

LOGS:
└── /var/log                # All logs
    ├── syslog              # System logs
    ├── auth.log            # Authentication logs
    ├── nginx/              # Nginx logs
    ├── journal/            # Systemd logs
    └── mysql/              # Database logs

LIBRARIES:
├── /lib                    # Essential system libraries
├── /lib64                  # 64-bit system libraries
├── /usr/lib                # Standard libraries
└── /usr/local/lib          # Local libraries

DATA:
├── /home                   # User home directories
├── /var/lib                # Variable state data
├── /var/www                # Web server files
└── /opt                    # Optional software packages
```

### **Finding Files:**

```bash
# Find configuration files
$ find /etc -name "*.conf" -type f
$ find /etc -name "nginx.conf"

# Find binaries
$ which nginx
/usr/sbin/nginx

$ whereis python
python: /usr/bin/python3 /usr/share/man/man1/python3.1.gz

# Find logs
$ find /var/log -name "*.log" -type f
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/syslog
```

---

## Q10. What is the Difference Between `/root` and `/home` Directories?

### **Answer:**

### **`/root` Directory:**
- **Home directory for the root user** (superuser)
- Located at absolute path `/root`
- Only accessible by root user
- Contains root user's configuration and files
- Not under `/home`

```bash
# Check root home
$ echo ~root
/root

# List root directory (need sudo)
$ sudo ls -la /root
total 24
drwx------  5 root root 4096 Jan 15 10:00 .
drwxr-xr-x 24 root root 4096 Jan 15 10:00 ..
-rw-r--r--  1 root root 3106 Jan 10 00:00 .bashrc
-rw-r--r--  1 root root  161 Jan 10 00:00 .profile
```

### **`/home` Directory:**
- **Contains home directories for regular users**
- Each user gets a subdirectory: `/home/username`
- Users have full access to their own home directory
- Default location for user data and configs

```bash
$ ls /home
user1  user2  developer

# Regular user home
$ echo ~
/home/user1

$ pwd
/home/user1
```

### **Key Differences:**

| Aspect          | `/root`                        | `/home`                           |
|-----------------|--------------------------------|-----------------------------------|
| **Purpose**     | Root user's home directory     | Regular users' home directories   |
| **User**        | Only root user                 | All regular users                 |
| **Location**    | Directly under `/`             | Contains user subdirectories      |
| **Permissions** | 700 (root only)                | 700 or 755 (user + group/others) |
| **Access**      | Requires root privileges       | Users access their own directory  |
| **Example**     | `/root/.bashrc`                | `/home/user1/.bashrc`             |

### **Permissions Comparison:**

```bash
# /root directory permissions
$ sudo ls -ld /root
drwx------ 5 root root 4096 Jan 15 10:00 /root
#           | |  |    |    |
#           | |  |    |    └─ Owner group: root
#           | |  |    └────── Owner: root
#           | |  └─────────── Permissions: 700 (rwx------)
#           | └────────────── Directory
#           └──────────────── Link count

# /home directory permissions
$ ls -ld /home
drwxr-xr-x 5 root root 4096 Jan 15 10:00 /home
#           | |  |    |    |
#           | |  |    |    └─ Owner group: root
#           | |  |    └────── Owner: root
#           | |  └─────────── Permissions: 755 (rwxr-xr-x)
#           | └────────────── Directory
#           └──────────────── Link count

# User home directory
$ ls -ld /home/user1
drwxr-xr-x 30 user1 user1 4096 Jan 15 10:00 /home/user1
#             | |    |      |
#             | |    |      └─ Owner: user1, Group: user1
#             | |    └────────── Permissions: 755
#             | └─────────────── Owner: user1
#             └───────────────── Link count
```

### **Why Separate `/root`?**

1. **Security:** Root's files are isolated from regular users
2. **System Recovery:** `/home` might be on a separate partition
3. **Boot Process:** Root needs access during single-user mode
4. **Separation of Concerns:** System admin vs regular user data

### **Practical Usage:**

```bash
# Switch to root user
$ sudo -i
# or
$ su -

# Check current directory
# pwd
/root

# As regular user
$ pwd
/home/user1

# Access root directory (fails as regular user)
$ ls /root
ls: cannot open directory '/root': Permission denied

# Access user home (works)
$ ls /home/user1
documents  downloads  projects
```

---

## Q11. What is a Linux Kernel? Why is it Important?

### **Answer:**

The **Linux Kernel** is the core component of the Linux operating system. It is a monolithic kernel that manages system resources and provides an interface between hardware and software.

### **What the Kernel Does:**

```
┌─────────────────────────────────────────────────────┐
│              User Space Applications                 │
│         (Web servers, databases, etc.)               │
└───────────────────┬─────────────────────────────────┘
                    │ System Calls
┌───────────────────▼─────────────────────────────────┐
│              Linux Kernel                            │
│                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  Process    │  │   Memory    │  │   Virtual   │ │
│  │ Management  │  │ Management  │  │ File System │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
│                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │   Network   │  │   Device    │  │  Security   │ │
│  │    Stack    │  │  Drivers    │  │  (SELinux)  │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
└───────────────────┬─────────────────────────────────┘
                    │ Hardware Instructions
┌───────────────────▼─────────────────────────────────┐
│                  Hardware                            │
│  CPU  │  RAM  │  Disk  │  Network  │  I/O Devices  │
└─────────────────────────────────────────────────────┘
```

### **Kernel Responsibilities:**

#### **1. Process Management**
- Creates, schedules, and terminates processes
- Implements process scheduling algorithms (CFS - Completely Fair Scheduler)
- Handles inter-process communication (IPC)
- Manages process priorities and CPU time

```bash
# View process tree managed by kernel
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168020 12168 ?        Ss   10:00   0:01 /sbin/init
root       456  0.0  0.2 180234 23456 ?        Ss   10:01   0:00 /usr/sbin/sshd
user      1234  0.5  1.2 345678 45678 ?        Sl   10:05   0:10 /usr/bin/python3
```

#### **2. Memory Management**
- Virtual memory management
- Paging and swapping
- Memory allocation and deallocation
- Memory protection between processes

```bash
# View memory information
$ free -h
              total        used        free      shared  buff/cache   available
Mem:           7.7G        2.1G        3.2G        256M        2.4G        5.1G
Swap:          2.0G          0B        2.0G

# View memory maps for a process
$ cat /proc/1234/maps
```

#### **3. Virtual File System (VFS)**
- Abstracts different file systems (ext4, xfs, btrfs, NFS)
- Provides unified API for file operations
- Handles file permissions, caching, and I/O

```bash
# View mounted file systems
$ mount
/dev/sda1 on / type ext4 (rw,relatime)
/dev/sda2 on /home type ext4 (rw,relatime)
```

#### **4. Network Stack**
- Implements TCP/IP protocol suite
- Handles network interfaces, routing, sockets
- Manages network connections and packet filtering

```bash
# View network interfaces
$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
```

#### **5. Device Drivers**
- Interface between kernel and hardware devices
- Handles device-specific operations
- Supports plug-and-play (udev)

```bash
# View loaded kernel modules (drivers)
$ lsmod
Module                  Size  Used by
nfs                   450000  1
tcp                   234567  15
ipv4                  123456  20
```

#### **6. Security**
- Enforces file permissions (rwx)
- Supports SELinux/AppArmor for mandatory access control
- Implements user/group permissions
- Handles capabilities and privileged operations

```bash
# Check SELinux status
$ sestatus
SELinux status:                 enabled
Current mode:                   enforcing
```

### **Why the Kernel is Important:**

| Importance          | Explanation                                |
|---------------------|--------------------------------------------|
| **Resource Manager** | Allocates CPU, memory, I/O to processes    |
| **Hardware Abstraction** | Provides unified interface to hardware    |
| **Security**        | Enforces access control and isolation      |
| **Performance**     | Optimizes resource utilization             |
| **Stability**       | Prevents system crashes through protection  |
| **Portability**     | Same kernel runs on different architectures |

### **Kernel Version Information:**

```bash
# Check kernel version
$ uname -r
5.15.0-76-generic

# Detailed kernel information
$ uname -a
Linux server 5.15.0-76-generic #83-Ubuntu SMP Thu Jun 15 19:16:32 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux

# View /proc/version
$ cat /proc/version
Linux version 5.15.0-76-generic (buildd@lcy02) (gcc version 11.3.0)
```

### **Kernel Parameters (sysctl):**

```bash
# View all kernel parameters
$ sysctl -a

# View specific parameter
$ sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0

# Modify kernel parameter (temporary)
$ sudo sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1

# Permanent modification
$ echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
```

---

## Q12. What is a Shell in Linux, and How is it Different from Bash?

### **Answer:**

### **Shell:**

A **shell** is a command-line interpreter that provides a user interface to the operating system. It:

- Reads user input from the terminal
- Parses and interprets commands
- Executes programs and scripts
- Returns results to the user

**Shell is a generic term** for any command-line interpreter.

### **Bash (Bourne Again Shell):**

**Bash** is a specific implementation of a shell. It is:

- A free software replacement for the original Bourne Shell (`sh`)
- Developed by Brian Fox for the GNU Project
- The default shell on most Linux distributions
- POSIX-compliant with many extensions

### **Key Differences:**

| Aspect          | Shell (Generic)              | Bash (Specific)                    |
|-----------------|------------------------------|------------------------------------|
| **Definition**  | Command-line interpreter      | Specific shell implementation      |
| **Type**        | Category/Concept              | Specific program                   |
| **Examples**    | bash, zsh, fish, csh, ksh    | Bash is one example of a shell     |
| **Scope**       | Broad concept                | Specific implementation            |
| **Compatibility**| Varies by shell             | POSIX-compliant with extensions    |

### **Shell Types:**

```
Shell (Generic Concept)
├── Bourne Shell Family (POSIX)
│   ├── sh (Original Bourne Shell)
│   ├── bash (Bourne Again Shell) ← Most common
│   ├── dash (Debian Almquist Shell)
│   └── ksh (Korn Shell)
├── C Shell Family
│   ├── csh (C Shell)
│   └── tcsh (TC Shell)
└── Modern Shells
    ├── zsh (Z Shell)
    └── fish (Friendly Interactive Shell)
```

### **Shell vs Bash - Practical Example:**

```bash
# Using bash specifically
#!/bin/bash
# This script requires bash features

# Using generic shell (POSIX)
#!/bin/sh
# This script works with any POSIX-compliant shell
```

### **Bash-Specific Features:**

**1. Arrays:**
```bash
#!/bin/bash
# Bash arrays (not available in pure sh)
files=("file1.txt" "file2.txt" "file3.txt")
echo ${files[0]}  # Output: file1.txt
echo ${#files[@]} # Output: 3
```

**2. Double Brackets [[ ]]:**
```bash
#!/bin/bash
# Bash extended test
if [[ $filename == *.txt ]]; then
    echo "Text file"
fi
```

**3. Process Substitution:**
```bash
#!/bin/bash
# Bash process substitution
diff <(cat file1.txt) <(cat file2.txt)
```

**4. Here Strings:**
```bash
#!/bin/bash
# Bash here string
grep "pattern" <<< "search in this string"
```

**5. Command History:**
```bash
# Bash maintains command history
$ history
  1  ls -la
  2  cd /home
  3  vim file.txt

$ !2    # Repeats command #2 (cd /home)
$ !!    # Repeats last command
```

### **POSIX Shell (sh) - More Portable:**

```bash
#!/bin/sh
# POSIX-compliant script (works with sh, dash, bash, etc.)

# No arrays - use positional parameters instead
set -- file1.txt file2.txt file3.txt
echo "$1"  # Output: file1.txt

# Single brackets [ ] for tests
if [ "$filename" = "*.txt" ]; then
    echo "Text file"
fi
```

### **Checking Shell Type:**

```bash
# Check current shell
$ echo $SHELL
/bin/bash

# Check if running in bash
$ if [ -n "$BASH_VERSION" ]; then echo "Running in bash"; fi
Running in bash

# Check bash version
$ bash --version
GNU bash, version 5.1.16(1)-release

# List all available shells
$ cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/zsh
/bin/dash
/bin/fish
```

### **When to Use Which:**

| Use Case                     | Recommended Shell |
|------------------------------|-------------------|
| System scripts (portability) | `#!/bin/sh`       |
| Interactive use              | `bash` or `zsh`   |
| Complex scripts              | `#!/bin/bash`     |
| Maximum compatibility        | `#!/bin/sh`       |
| Advanced features            | `#!/bin/bash`     |

---

## Q13. Basic Components of Linux OS

### **Answer:**

The Linux operating system consists of several key components that work together:

```
┌─────────────────────────────────────────────────────────┐
│                    USER SPACE                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │ Applications│  │   System    │  │    Shell    │    │
│  │ (nginx,     │  │  Libraries  │  │  (bash,     │    │
│  │  mysql,     │  │  (glibc,    │  │   zsh)      │    │
│  │  python)    │  │   openssl)  │  │             │    │
│  └─────────────┘  └─────────────┘  └─────────────┘    │
└───────────────────────┬─────────────────────────────────┘
                        │ System Call Interface
┌───────────────────────▼─────────────────────────────────┐
│                   KERNEL SPACE                          │
│  ┌─────────────────────────────────────────────────┐   │
│  │              LINUX KERNEL                       │   │
│  │                                                  │   │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐  │   │
│  │  │Process │ │ Memory │ │   VFS  │ │Network │  │   │
│  │  │Manager │ │Manager │ │        │ │ Stack  │  │   │
│  │  └────────┘ └────────┘ └────────┘ └────────┘  │   │
│  │                                                  │   │
│  │  ┌─────────────────────────────────────────┐   │   │
│  │  │         Device Drivers                  │   │   │
│  │  │  (Disk, Network, USB, GPU, etc.)        │   │   │
│  │  └─────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────┘   │
└───────────────────────┬─────────────────────────────────┘
                        │ Hardware Instructions
┌───────────────────────▼─────────────────────────────────┐
│                    HARDWARE                             │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐      │
│  │   CPU   │ │   RAM   │ │  Disks  │ │ Network │      │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘      │
└─────────────────────────────────────────────────────────┘
```

### **Component Breakdown:**

#### **1. Hardware Layer**
The physical components of the system:

| Component | Function | Example |
|-----------|----------|---------|
| **CPU**   | Executes instructions | Intel Core, AMD Ryzen |
| **RAM**   | Temporary memory storage | DDR4, DDR5 |
| **Storage** | Persistent data storage | SSD, HDD, NVMe |
| **Network** | Network communication | Ethernet, Wi-Fi |
| **I/O Devices** | Input/output devices | Keyboard, Mouse, Display |

```bash
# View CPU information
$ lscpu
Architecture:            x86_64
CPU(s):                  4
Model name:              Intel(R) Core(TM) i5

# View memory information
$ free -h
Mem:           7.7G

# View disk information
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk
└─sda1   8:1    0  500G  0 part /
```

#### **2. Kernel**
The core component managing hardware:

| Kernel Subsystem | Function |
|------------------|----------|
| **Process Scheduler** | Manages process execution and CPU time |
| **Memory Manager** | Handles virtual memory, paging, swapping |
| **VFS (Virtual File System)** | Provides unified file system interface |
| **Network Stack** | Implements TCP/IP protocols |
| **Device Drivers** | Interfaces with hardware devices |
| **IPC (Inter-Process Communication)** | Enables process communication |

```bash
# View kernel information
$ uname -r
5.15.0-76-generic

# View kernel modules
$ lsmod | head -10
Module                  Size  Used by
nfs                   450000  1
tcp                   234567  15
```

#### **3. System Libraries**
Shared libraries that provide common functions:

```bash
# View shared library dependencies
$ ldd /bin/ls
linux-vdso.so.1
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6
ld-linux-x86-64.so.2 => /lib64/ld-linux-x86-64.so.2

# Common libraries
/lib/x86_64-linux-gnu/
├── libc.so.6       # C standard library
├── libssl.so.1.1   # OpenSSL library
├── libcrypto.so.1.1 # Cryptography library
└── libz.so.1       # Compression library
```

#### **4. Shell**
Command-line interpreter:

```bash
# Available shells
$ cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/zsh
```

#### **5. System Utilities**
Core utilities for system administration:

```bash
# Core utilities location
$ ls /bin
ls  cp  mv  rm  cat  grep  find  chmod  chown

# System utilities
$ ls /sbin
ifconfig  iptables  reboot  shutdown  useradd
```

#### **6. Applications**
User-space programs:

| Category | Examples |
|----------|----------|
| **Web Servers** | nginx, Apache httpd |
| **Databases** | MySQL, PostgreSQL, MongoDB |
| **Programming** | Python, Node.js, Java |
| **Monitoring** | Prometheus, Grafana |
| **Containers** | Docker, Podman |

### **Component Interaction Example:**

```bash
# When you run: curl https://example.com

# 1. Shell (bash) interprets the command
# 2. Shell locates curl binary in /usr/bin/curl
# 3. Shell makes system calls:
#    - fork() to create process
#    - execve() to load curl
# 4. Kernel handles:
#    - Process scheduling
#    - Memory allocation
#    - Network stack (TCP/IP)
#    - Device driver (network card)
# 5. Hardware:
#    - CPU executes instructions
#    - RAM stores process data
#    - Network card sends/receives packets
# 6. System libraries (libcurl, openssl) provide:
#    - HTTP protocol implementation
#    - SSL/TLS encryption
# 7. Output returned to shell, displayed to user
```

---

## Q14. What is the init Process in Linux?

### **Answer:**

The **init process** is the first process started by the Linux kernel during boot. It has **PID 1** and is the ancestor of all other processes.

### **Role of init Process:**

```
Boot Sequence:
BIOS → MBR/GRUB → Kernel → init (PID 1) → System Processes → Login
```

### **Key Characteristics:**

| Characteristic | Description |
|----------------|-------------|
| **PID**        | Always 1 (first process) |
| **Parent**     | Kernel (no parent process) |
| **Role**       | System initialization and process management |
| **Ancestor**   | All other processes are descendants |
| **Cannot be killed** | Killing init crashes the system |

### **Viewing init Process:**

```bash
# View init process (PID 1)
$ ps -p 1 -f
UID          PID    PPID  C STIME TTY      TIME CMD
root           1       0  0 10:00 ?        00:00:01 /sbin/init

# Modern systems use systemd as init
$ ps -p 1 -o comm=
systemd

# View process tree starting from init
$ pstree -p
systemd(1)─┬─NetworkManager(567)
           ├─cron(678)
           ├─sshd(789)───sshd(1234)───bash(1245)
           ├─nginx(5678)─┬─nginx(5679)
           │            └─nginx(5680)
           └─systemd-journal(456)
```

### **init Process Responsibilities:**

#### **1. System Initialization**
```bash
# systemd runs targets (similar to runlevels)
# View current target
$ systemctl get-default
graphical.target

# View active targets
$ systemctl list-units --type=target
```

#### **2. Starting System Services**
```bash
# Services started by init (systemd)
$ systemctl list-units --type=service --state=running
UNIT                      LOAD   ACTIVE SUB     DESCRIPTION
cron.service              loaded active running Regular background program processing daemon
nginx.service             loaded active running A high performance web server
ssh.service               loaded active running OpenBSD Secure Shell server
```

#### **3. Process Management**
```bash
# Init is parent of all processes
$ ps -ef | head
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:00 ?        00:00:01 /sbin/init
root       567     1  0 10:00 ?        00:00:00 /usr/sbin/cron
root       789     1  0 10:00 ?        00:00:00 /usr/sbin/sshd
```

#### **4. Orphan Process Adoption**
- When a parent process dies, its children become orphans
- init (PID 1) adopts these orphan processes
- Ensures no process becomes a zombie permanently

```bash
# View orphaned processes (adopted by init)
$ ps -elf | awk '$3 == 1 {print $4, $11}'
```

#### **5. System Shutdown and Reboot**
```bash
# Init handles shutdown
$ sudo systemctl poweroff   # Controlled by init
$ sudo systemctl reboot     # Controlled by init

# Traditional init commands
$ sudo shutdown -h now      # Halt system
$ sudo reboot               # Reboot system
```

### **init Implementations:**

#### **1. systemd (Modern)**
- Default on most modern Linux distributions
- Uses `.target` and `.service` units
- Parallel service startup
- Comprehensive logging with journald

```bash
# Check if using systemd
$ ps -p 1 -o comm=
systemd

# systemd configuration
$ ls /etc/systemd/system/
multi-user.target.wants/
graphical.target.wants/
```

#### **2. SysVinit (Traditional)**
- Older initialization system
- Uses runlevels (0-6)
- Sequential service startup
- Uses init scripts in `/etc/init.d/`

```bash
# Traditional runlevels
# 0 - Halt
# 1 - Single user mode
# 2 - Multiuser (no NFS)
# 3 - Full multiuser
# 4 - Unused
# 5 - Graphical mode
# 6 - Reboot

# Check runlevel (if using SysVinit)
$ runlevel
N 3
```

### **init vs systemd:**

| Feature        | SysVinit              | systemd                  |
|----------------|-----------------------|--------------------------|
| **Service Management** | `/etc/init.d/` scripts | `.service` unit files    |
| **Runlevels**   | 0-6                   | Targets (.target)        |
| **Startup**     | Sequential            | Parallel                 |
| **Logging**     | Text files in `/var/log` | journald (binary)       |
| **Process ID**  | Still PID 1           | Still PID 1              |

### **Viewing init Configuration:**

```bash
# systemd: view default target
$ systemctl get-default
multi-user.target

# systemd: view all targets
$ systemctl list-unit-files --type=target

# systemd: view boot process
$ systemd-analyze
Startup finished in 3.456s (kernel) + 12.345s (userspace) = 15.801s

# View init logs
$ journalctl -b
```

---

## Q15. What is Root?

### **Answer:**

**Root** refers to two related concepts in Linux:

### **1. Root User (Superuser)**
The administrative account with full system access:

| Attribute        | Description                         |
|------------------|-------------------------------------|
| **Username**     | `root`                              |
| **UID**          | 0 (zero)                            |
| **GID**          | 0 (zero)                            |
| **Home Directory**| `/root`                            |
| **Privileges**   | Unrestricted access to all commands |

```bash
# Check if root user
$ whoami
user

# Switch to root
$ sudo -i
# or
$ su -

# Now root
# whoami
root

# Check root UID
# id root
uid=0(root) gid=0(root) groups=0(root)
```

### **2. Root Directory**
The top-level directory of the filesystem:

```
/ (root directory)
├── bin
├── etc
├── home
├── var
└── ...
```

```bash
# Root directory
$ cd /
$ pwd
/

# List root directory
$ ls /
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### **Root User Capabilities:**

```bash
# Root can do anything:
# - Modify any file
# - Kill any process
# - Change system configuration
# - Install/uninstall software
# - Manage users and groups
# - Modify kernel parameters
# - Access all hardware

# Examples requiring root:
$ sudo useradd newuser           # Create user
$ sudo systemctl restart nginx   # Restart service
$ sudo apt install nginx         # Install software
$ sudo iptables -L               # View firewall rules
$ sudo kill -9 1234              # Kill any process
```

### **Root vs Regular User:**

| Aspect              | Root User                     | Regular User                    |
|---------------------|-------------------------------|---------------------------------|
| **UID**             | 0                             | 1000+                           |
| **Permissions**     | Unrestricted                  | Restricted by permissions       |
| **Home Directory**  | `/root`                       | `/home/username`                |
| **System Files**    | Full read/write/execute       | Limited access                  |
| **Process Control** | Can kill any process          | Can only kill own processes     |
| **System Changes**  | Can modify anything           | Limited modifications           |

### **Best Practices for Using Root:**

```bash
# ❌ BAD: Login directly as root
# (Avoid logging in as root user directly)

# ✅ GOOD: Use sudo for specific commands
$ sudo systemctl restart nginx

# ✅ GOOD: Use sudo -i for temporary root shell
$ sudo -i
# Perform tasks, then exit
# exit

# ✅ GOOD: Run specific command as another user
$ sudo -u postgres psql
```

### **Checking Root Access:**

```bash
# Check current user
$ whoami
user

# Check if running with sudo
$ sudo -v

# Check effective user ID
$ id -u
1000

# Check if user has sudo access
$ sudo -l
User user may run the following commands on server:
    (ALL : ALL) ALL
```

### **Root Security:**

```bash
# Disable direct root login via SSH
# /etc/ssh/sshd_config
PermitRootLogin no

# Use key-based authentication
PasswordAuthentication no

# Limit sudo access in /etc/sudoers
user    ALL=(ALL) /usr/bin/systemctl
```

---

## Q16. Check Hostname

### **Answer:**

The **hostname** is the name assigned to a device on a network.

### **Methods to Check Hostname:**

#### **1. hostname command**
```bash
$ hostname
server01

$ hostname -f    # Fully qualified domain name (FQDN)
server01.example.com

$ hostname -i    # IP address
192.168.1.100

$ hostname -I    # All IP addresses
192.168.1.100 10.0.0.5
```

#### **2. hostnamectl (systemd systems)**
```bash
$ hostnamectl
   Static hostname: server01
         Icon name: computer-server
           Chassis: server
        Machine ID: abc123def456...
           Boot ID: xyz789...
  Operating System: Ubuntu 22.04.3 LTS
            Kernel: Linux 5.15.0-76-generic
      Architecture: x86-64

# View only hostname
$ hostnamectl --static
server01
```

#### **3. uname command**
```bash
$ uname -n
server01

# All system information
$ uname -a
Linux server01 5.15.0-76-generic #83-Ubuntu SMP Thu Jun 15 19:16:32 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

#### **4. /etc/hostname file**
```bash
$ cat /etc/hostname
server01
```

#### **5. /etc/hosts file**
```bash
$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 server01
192.168.1.100 server01.example.com server01
```

#### **6. Environment variables**
```bash
$ echo $HOSTNAME
server01

$ env | grep HOST
HOSTNAME=server01
```

### **Changing Hostname:**

#### **Temporary (until reboot):**
```bash
$ sudo hostname new-server01

$ hostname
new-server01
```

#### **Permanent (systemd):**
```bash
# Set static hostname
$ sudo hostnamectl set-hostname new-server01

# Set pretty hostname (with spaces)
$ sudo hostnamectl set-hostname "Production Server 01" --pretty

# Verify
$ hostnamectl status
   Static hostname: new-server01
   Pretty hostname: Production Server 01
```

#### **Permanent (traditional method):**
```bash
# Edit /etc/hostname
$ sudo nano /etc/hostname
new-server01

# Update /etc/hosts
$ sudo nano /etc/hosts
127.0.1.1 new-server01

# Reboot or restart services
$ sudo systemctl restart systemd-logind
```

### **Hostname Types:**

| Type              | Description                          | Example               |
|-------------------|--------------------------------------|-----------------------|
| **Static**        | Configured hostname                  | `server01`            |
| **Transient**     | Temporary hostname (DHCP-assigned)   | `dhcp-192-168-1-100`  |
| **Pretty**        | Human-readable name (can have spaces) | `Production Server 01`|
| **FQDN**          | Fully Qualified Domain Name          | `server01.example.com`|

### **Hostname in Shell Prompt:**

```bash
# Prompt shows hostname (user@hostname)
user@server01:~$

# Customize prompt in ~/.bashrc
export PS1="\u@\h:\w\$ "

# Result: user@server01:/home/user$
```

---

## Q17. Check Current User

### **Answer:**

Multiple commands to check the current logged-in user.

### **Methods to Check Current User:**

#### **1. whoami command**
```bash
$ whoami
user
```

#### **2. id command (detailed information)**
```bash
$ id
uid=1000(user) gid=1000(user) groups=1000(user),27(sudo),1001(docker)

# Show only username
$ id -un
user

# Show only user ID
$ id -u
1000

# Show only group ID
$ id -g
1000

# Show all groups
$ id -G
1000 27 1001

# Show all group names
$ id -Gn
user sudo docker
```

#### **3. who command**
```bash
$ who
user     tty1         2024-01-15 10:00 (:0)
user     pts/0        2024-01-15 10:05 (192.168.1.50)

# Show detailed info
$ who -a
user     tty1         2024-01-15 10:00 00:05        1234 (:0)
user     pts/0        2024-01-15 10:05 00:02        5678 (192.168.1.50)
```

#### **4. w command (who + what they're doing)**
```bash
$ w
 10:30:45 up  2:30,  2 users,  load average: 0.15, 0.10, 0.05
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
user     tty1      10:00    2:30m  0.05s  0.02s systemd
user     pts/0     10:05    0.00s  0.10s  0.00s w
```

#### **5. logname command**
```bash
$ logname
user
```

#### **6. Environment variables**
```bash
$ echo $USER
user

$ echo $LOGNAME
user

$ echo $USERNAME
user
```

#### **7. Checking if running as root/sudo**
```bash
# Check if root
$ [ $(id -u) -eq 0 ] && echo "Running as root" || echo "Not root"
Not root

# Check if using sudo
$ sudo -v
# (if no error, you have sudo access)

# Check if effective user is root
$ [ "$EUID" -eq 0 ] && echo "root" || echo "not root"
not root
```

### **User Information Files:**

```bash
# /etc/passwd - User account information
$ grep user /etc/passwd
user:x:1000:1000:User Name:/home/user:/bin/bash
#       |  |    |    |           |             |
#       |  |    |    |           |             └─ Default shell
#       |  |    |    |           └───────────────── Home directory
#       |  |    |    └────────────────────────────── GECOS/comment
#       |  |    └─────────────────────────────────── Primary GID
#       |  └──────────────────────────────────────── UID
#       └─────────────────────────────────────────── Username (x = shadow password)

# Current user details
$ getent passwd user
user:x:1000:1000:User Name:/home/user:/bin/bash
```

### **Checking All Logged-in Users:**

```bash
# All logged-in users
$ users
user user

# Detailed login information
$ last
user     tty1                          Mon Jan 15 10:00   still logged in
user     pts/0                         Mon Jan 15 10:05   still logged in

# Recent logins
$ last -n 5
```

### **Practical Examples:**

```bash
# Script: Check if running as root
#!/bin/bash
if [ "$(id -u)" -ne 0 ]; then
    echo "This script must be run as root"
    exit 1
fi
echo "Running as root"

# Script: Get current username
CURRENT_USER=$(whoami)
echo "Current user: $CURRENT_USER"

# Check user groups
groups
# Output: user sudo docker

# Check if user is in sudo group
groups | grep -q sudo && echo "Has sudo access" || echo "No sudo"
```

---

## Q18. Check Current Working Directory

### **Answer:**

The **current working directory (CWD)** is the directory where a process is currently operating.

### **Methods to Check Current Working Directory:**

#### **1. pwd command (Print Working Directory)**
```bash
$ pwd
/home/user/projects

# Show physical path (resolve symlinks)
$ pwd -P
/home/user/projects

# Show logical path (don't resolve symlinks)
$ pwd -L
/home/user/projects
```

#### **2. Shell variable $PWD**
```bash
$ echo $PWD
/home/user/projects

# Check if PWD is set
$ [ -n "$PWD" ] && echo "Current dir: $PWD"
```

#### **3. Shell built-in (works without external command)**
```bash
# In bash/ksh
$ dirs
~/projects

# In any shell
$ echo "$(pwd)"
/home/user/projects
```

### **Practical Examples:**

#### **Navigate and verify:**
```bash
# Change directory
$ cd /var/log/nginx

# Verify location
$ pwd
/var/log/nginx

# Go to parent directory
$ cd ..

# Verify
$ pwd
/var/log
```

#### **Use in scripts:**
```bash
#!/bin/bash
# Save current directory
ORIGINAL_DIR=$(pwd)

# Go to another directory
cd /tmp

# Do work
echo "Working in: $(pwd)"

# Return to original directory
cd "$ORIGINAL_DIR"
echo "Back to: $(pwd)"
```

#### **Find script location:**
```bash
#!/bin/bash
# Directory where script is located
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

echo "Script is in: $SCRIPT_DIR"
```

### **Related Commands:**

```bash
# Change to home directory
$ cd
$ pwd
/home/user

# Change to previous directory
$ cd -
$ pwd
/tmp

# Change to parent directory
$ cd ..
$ pwd
/home

# Go up two levels
$ cd ../..
$ pwd
/
```

### **Shell Prompt Shows Current Directory:**
```bash
# Default bash prompt shows path
user@server01:~/projects$ pwd
/home/user/projects

user@server01:/var/log/nginx$ pwd
/var/log/nginx

# ~ represents home directory
user@server01:~$ pwd
/home/user
```

### **PWD vs OLDPWD:**

```bash
$ cd /home/user
$ echo $PWD
/home/user

$ cd /tmp
$ echo $PWD
/tmp
$ echo $OLDPWD
/home/user

$ cd -    # Switch to OLDPWD
$ pwd
/home/user
```

---

## Summary - Module 1: Linux Fundamentals

This module covers the essential foundational concepts of Linux:

| Topic                          | Key Commands/Concepts                         |
|--------------------------------|-----------------------------------------------|
| **Linux vs UNIX vs Windows**   | Open source, kernel, licensing differences    |
| **Linux Architecture**         | Kernel, Shell, User Space, Hardware Layer     |
| **Command Execution**          | Parsing, PATH lookup, fork/exec, system calls |
| **Environment Variables**      | `env`, `export`, `$PATH`, `$HOME`             |
| **Paths**                      | Absolute (`/path/to/file`), Relative (`../`)  |
| **Directory Structure**        | `/bin`, `/etc`, `/home`, `/var`, FHS          |
| **Shells**                     | bash, sh, zsh, shebang (`#!/bin/bash`)        |
| **Shell vs Kernel**            | Interface vs Core, User space vs Kernel space |
| **Filesystem Hierarchy**       | Configs in `/etc`, logs in `/var/log`         |
| **/root vs /home**             | Root user vs regular users' directories       |
| **Linux Kernel**               | Process, memory, VFS, network, device drivers |
| **Shell vs Bash**              | Generic vs specific implementation            |
| **OS Components**              | Hardware, Kernel, Libraries, Shell, Apps      |
| **init Process**               | PID 1, system initialization, systemd         |
| **Root**                       | Superuser (UID 0), root directory (/)          |
| **Hostname**                   | `hostname`, `hostnamectl`, `/etc/hostname`     |
| **Current User**               | `whoami`, `id`, `who`, `w`                    |
| **Working Directory**          | `pwd`, `$PWD`, `cd`                           |

### **Essential Commands from Module 1:**

```bash
# System Information
hostname          # Show hostname
whoami            # Show current user
id                # Show user ID and groups
pwd               # Show current directory
uname -a          # Show system information

# Environment
env               # Show all environment variables
echo $PATH        # Show PATH variable
export VAR=value  # Set environment variable

# File System Navigation
cd /path          # Change directory
ls -la            # List files with details
pwd               # Print working directory

# Process Information
ps aux            # Show all processes
pstree            # Show process tree
```

---

**Next Module:** File Management & Permissions (Module 2)
