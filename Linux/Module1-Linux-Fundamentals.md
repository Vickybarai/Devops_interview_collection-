# 🟩 MODULE 1: Linux Fundamentals

> Simplified interview questions and answers for Linux basics

---

## Q1. What is Linux? Difference between Linux, UNIX, and Windows.

### **Definition**
- **Linux**: Open-source operating system kernel
- **UNIX**: Proprietary operating system
- **Windows**: Proprietary OS by Microsoft

### **Explain**
| Feature | Linux | UNIX | Windows |
|---------|-------|------|---------|
| **Source** | Open Source | Mostly Proprietary | Closed Source |
| **Cost** | Free | Paid | Paid |
| **Kernel** | Linux | Various | NT Kernel |
| **File System** | ext4, xfs | UFS, ZFS | NTFS |

### **Use**
- **Linux**: Servers, cloud, embedded systems
- **UNIX**: Enterprise systems, legacy applications
- **Windows**: Desktop, enterprise, some servers

### **Command**
```bash
# Check if running Linux
uname -a

# Check Linux version
cat /etc/os-release

# View kernel version
uname -r
```

---

## Q2. Explain Linux Architecture (Kernel, Shell, User Space)

### **Definition**
Linux has three main layers: Hardware, Kernel, User Space

### **Explain**
```
User Space:   Applications, Shell, System Libraries
Kernel Space: Process Management, Memory, Network, File System
Hardware:     CPU, RAM, Disk, Network
```

- **Kernel**: Core of the OS, manages hardware
- **Shell**: Command interpreter between user and kernel
- **User Space**: Where applications run

### **Use**
- Understand how Linux works
- Troubleshoot system issues
- Optimize performance

### **Command**
```bash
# View running processes (user space)
ps aux

# View kernel messages
dmesg

# Check shell
echo $SHELL

# View kernel modules
lsmod
```

---

## Q3. What Happens When You Type a Command and Press Enter?

### **Definition**
Shell parses, locates, and executes the command

### **Explain**
```
1. Shell reads and parses command
2. Searches PATH for command location
3. Forks a child process
4. Child process executes the command
5. Output returned to shell
6. Shell displays prompt again
```

### **Use**
- Understand command execution
- Troubleshoot command failures
- Debug script issues

### **Command**
```bash
# View PATH
echo $PATH

# Check command location
which ls

# Trace command execution
bash -x script.sh

# View command history
history
```

---

## Q4. What are Environment Variables? How to View/Set Them?

### **Definition**
Variables that affect process behavior

### **Explain**
- Stored in process memory
- Inherited by child processes
- Examples: PATH, HOME, USER, SHELL

### **Use**
- Configure system behavior
- Set application settings
- Control execution environment

### **Command**
```bash
# View all environment variables
env
printenv

# View specific variable
echo $PATH
echo $HOME

# Set variable (current session)
export MY_VAR="value"

# Set permanent (add to ~/.bashrc)
echo 'export MY_VAR="value"' >> ~/.bashrc

# Remove variable
unset MY_VAR
```

---

## Q5. What are Absolute vs Relative Paths?

### **Definition**
- **Absolute Path**: Complete path from root (`/`)
- **Relative Path**: Path from current directory

### **Explain**
```bash
# Absolute path
/etc/nginx/nginx.conf        # Starts from /

# Relative path
nginx/nginx.conf              # From current directory
../config/file.conf          # Parent directory
~/documents/file.txt         # From home directory
```

### **Use**
- **Absolute**: Scripts, configuration files
- **Relative**: Interactive use, portable scripts

### **Command**
```bash
# Current directory (relative)
.

# Parent directory (relative)
..

# Home directory (relative)
~

# Absolute path
/var/log/nginx/

# Show current directory (absolute)
pwd

# Go to absolute path
cd /etc/nginx

# Go to relative path
cd ../logs
```

---

## Q6. Explain Linux Directory Structure (/, /bin, /etc, /home, /var)

### **Definition**
Organized structure following Filesystem Hierarchy Standard (FHS)

### **Explain**
| Directory | Purpose | Examples |
|-----------|---------|----------|
| **/** | Root directory | All other directories |
| **/bin** | User binaries | ls, cat, grep |
| **/etc** | Configuration | nginx.conf, sshd_config |
| **/home** | User home directories | /home/user |
| **/var** | Variable data | logs, cache, spool |

### **Use**
- Know where files are located
- Configure system correctly
- Troubleshoot file locations

### **Command**
```bash
# View root directory
ls -la /

# View important directories
ls /bin /etc /home /var

# View configuration files
ls /etc

# View logs
ls /var/log

# View user home directories
ls /home
```

---

## Q7. What are the Types of Shells in Linux?

### **Definition**
Command-line interpreters: bash, sh, zsh, fish, etc.

### **Explain**
| Shell | Description | Use Case |
|-------|-------------|----------|
| **bash** | Default on most systems | General use |
| **sh** | POSIX shell | Portable scripts |
| **zsh** | Advanced features | Power users |
| **fish** | User-friendly | Interactive use |

### **Use**
- Interactive command line
- Scripting
- Automation

### **Command**
```bash
# View current shell
echo $SHELL

# Check installed shells
cat /etc/shells

# View shell version
bash --version

# Switch to different shell
zsh

# Change default shell
chsh -s /bin/zsh
```

---

## Q8. What is the Difference Between Shell and Kernel?

### **Definition**
- **Shell**: User interface, interprets commands
- **Kernel**: Core OS, manages hardware

### **Explain**
```
User → Shell → System Calls → Kernel → Hardware
```

| Aspect | Shell | Kernel |
|--------|-------|--------|
| **Function** | Interface | Core management |
| **Space** | User space | Kernel space |
| **Access** | Direct user interaction | No direct access |

### **Use**
- **Shell**: Daily commands, scripting
- **Kernel**: System management, hardware control

### **Command**
```bash
# Check shell
echo $SHELL

# Check kernel version
uname -r

# View kernel parameters
sysctl -a

# View shell processes
ps aux | grep bash
```

---

## Q9. Explain File System Hierarchy - Where are Configs, Binaries, and Logs Kept?

### **Definition**
Linux organizes files by purpose in specific directories

### **Explain**
| File Type | Location | Examples |
|-----------|----------|----------|
| **Configs** | `/etc/` | nginx.conf, sshd_config |
| **Binaries** | `/bin/`, `/usr/bin/` | ls, cat, python3 |
| **Logs** | `/var/log/` | syslog, nginx/access.log |
| **Libraries** | `/lib/`, `/usr/lib/` | libc.so, openssl |

### **Use**
- Find configuration files
- Locate programs
- Check logs for troubleshooting

### **Command**
```bash
# Find config files
ls /etc/nginx/
ls /etc/ssh/

# Find binaries
ls /bin/
ls /usr/bin/

# Find logs
ls /var/log/
ls /var/log/nginx/

# Find libraries
ls /lib/
ls /usr/lib/
```

---

## Q10. What is the Difference Between /root and /home Directories?

### **Definition**
- **/root**: Root user's home directory
- **/home**: Regular users' home directories

### **Explain**
| Feature | /root | /home |
|---------|-------|-------|
| **User** | root (UID 0) | Regular users (UID 1000+) |
| **Location** | Directly under `/` | Contains user directories |
| **Access** | Root only | Users access their own |
| **Example** | `/root/.bashrc` | `/home/user/.bashrc` |

### **Use**
- **/root**: System admin files
- **/home**: User data and configs

### **Command**
```bash
# View root home directory
ls -la /root

# View user home directories
ls -la /home/

# View root user home
echo ~root

# View current user home
echo ~
echo $HOME

# Switch to root home
sudo -i
pwd  # Shows /root
```

---

## Q11. What is a Linux Kernel? Why is it Important?

### **Definition**
Core component of Linux operating system

### **Explain**
Kernel manages:
- **Process Management**: Scheduling, execution
- **Memory Management**: Allocation, paging
- **File System**: Files, directories
- **Network**: TCP/IP stack
- **Device Drivers**: Hardware communication

### **Use**
- Manages all hardware
- Provides system calls to applications
- Ensures system stability and security

### **Command**
```bash
# View kernel version
uname -r

# View detailed kernel info
uname -a

# View kernel modules
lsmod

# View kernel parameters
sysctl -a

# View kernel messages
dmesg | head
```

---

## Q12. What is a Shell in Linux, and How is it Different from Bash?

### **Definition**
- **Shell**: Command-line interpreter (generic term)
- **Bash**: Specific shell implementation

### **Explain**
```
Shell (Generic Concept)
├── bash (Bourne Again Shell)
├── sh (POSIX Shell)
├── zsh (Z Shell)
└── fish (Friendly Shell)
```

Bash is one type of shell.

### **Use**
- **Shell**: General concept of CLI interpreter
- **Bash**: Default shell on most Linux systems

### **Command**
```bash
# Check current shell
echo $SHELL

# Check if running bash
if [ -n "$BASH_VERSION" ]; then
    echo "Running in bash"
fi

# Switch to bash
bash

# View bash version
bash --version
```

---

## Q13. Basic Components of Linux OS

### **Definition**
Linux OS consists of Hardware, Kernel, Libraries, Shell, Applications

### **Explain**
```
Hardware    → CPU, RAM, Disk, Network
Kernel      → Process, Memory, File System, Network
Libraries   → System libraries (glibc, openssl)
Shell       → Command interpreter (bash)
Applications → Web servers, databases, tools
```

### **Use**
- Understand system architecture
- Troubleshoot issues at different layers
- Optimize performance

### **Command**
```bash
# View hardware
lscpu          # CPU
free -h        # Memory
lsblk          # Disk
ip addr show   # Network

# View kernel
uname -a

# View running applications
ps aux

# View libraries
ldd /bin/ls
```

---

## Q14. What is the init Process in Linux?

### **Definition**
First process started by kernel (always PID 1)

### **Explain**
- **PID**: Always 1
- **Parent**: Kernel (no parent process)
- **Ancestor**: All other processes
- **Function**: System initialization, starts services

### **Use**
- Starts system services
- Adopts orphan processes
- Manages system shutdown

### **Command**
```bash
# View init process
ps -p 1 -f

# Check if using systemd
ps -p 1 -o comm=

# View process tree
pstree -p

# View systemd services
systemctl list-units --type=service
```

---

## Q15. What is Root?

### **Definition**
Two meanings:
1. **Root User**: Superuser account (UID 0)
2. **Root Directory**: Top-level directory (`/`)

### **Explain**
**Root User:**
- Username: `root`
- UID: 0
- Home: `/root`
- Full system access

**Root Directory:**
- Path: `/`
- Contains all other directories
- Mount point for filesystem

### **Use**
- **Root User**: System administration
- **Root Directory**: File system root

### **Command**
```bash
# Root directory
cd /
ls /

# Root user
sudo -i            # Become root
whoami             # Shows root
id                 # UID 0

# Root user home
ls -la /root/

# Check if running as root
[ $(id -u) -eq 0 ] && echo "root" || echo "not root"
```

---

## Q16. How to Check Hostname?

### **Definition**
Hostname is the name assigned to a device on network

### **Explain**
- **hostname**: Simple command
- **hostnamectl**: Detailed system info
- Stored in `/etc/hostname`

### **Use**
- Identify system on network
- Configure networking
- Troubleshooting

### **Command**
```bash
# View hostname
hostname

# View detailed info
hostnamectl

# View short hostname
hostname -s

# View FQDN (Full Qualified Domain Name)
hostname -f

# View hostname file
cat /etc/hostname

# Set hostname (temporary)
sudo hostname new-hostname

# Set hostname (permanent)
sudo hostnamectl set-hostname new-hostname
```

---

## Q17. How to Check Current User?

### **Definition**
Commands to see who is currently logged in

### **Explain**
- **whoami**: Current username
- **id**: User ID and groups
- **who**: All logged-in users
- **w**: Users and what they're doing

### **Use**
- Verify current user
- Check user permissions
- Monitor system access

### **Command**
```bash
# Current username
whoami

# User ID and groups
id

# Detailed user info
id username

# All logged-in users
who

# Users with activity
w

# User's groups
groups username
```

---

## Q18. How to Check Current Working Directory?

### **Definition**
Directory where you are currently located

### **Explain**
- **pwd**: Print Working Directory
- Shows absolute path
- Current location in filesystem

### **Use**
- Know where you are
- Navigate filesystem
- Use in scripts for relative paths

### **Command**
```bash
# Show current directory
pwd

# Show physical path (resolve symlinks)
pwd -P

# Show logical path (don't resolve symlinks)
pwd -L

# Current directory in variable
echo $PWD
echo $PWD

# Change directory
cd /etc
pwd    # Shows /etc

# Go to previous directory
cd -
pwd
```

---

## 📚 Quick Reference - Module 1 Commands

### System Information
```bash
uname -a              # System info
hostname              # Hostname
whoami                # Current user
id                    # User details
pwd                   # Current directory
```

### Environment
```bash
env                   # All variables
echo $PATH            # PATH variable
export VAR=value      # Set variable
```

### File System
```bash
ls -la /              # List root directory
cd /path              # Change directory
pwd                   # Show current path
```

### Processes
```bash
ps aux                # Show processes
pstree                # Process tree
```

---

**Next:** Module 2 - File Management & Permissions
