# 🟨 MODULE 2: File Management & Permissions

> Simplified interview questions and answers for File Management & Permissions in Linux

---

## Q1. What are File Permissions (rwx)? Explain chmod 755 vs 777.

### **Definition**
File permissions control who can read, write, and execute files in Linux.

### **Explain**
- **r (Read)** = View file contents
- **w (Write)** = Modify file
- **x (Execute)** = Run as program
- Permissions are set for: **Owner**, **Group**, and **Others**

### **Use**
- Protect sensitive files
- Control who can run programs
- Secure system configuration

### **Command**
```bash
# View permissions
ls -l file.txt

# chmod 755 (Owner: rwx, Group: rx, Others: rx)
chmod 755 script.sh

# chmod 777 (Everyone: rwx) - NOT RECOMMENDED
chmod 777 file.txt

# Symbolic mode
chmod +x script.sh          # Add execute
chmod u+rwx,g+rx,o+rx file  # Set permissions manually
```

---

## Q2. What are the Types of Permissions (Read, Write, Execute)?

### **Definition**
Three basic permission types: Read, Write, Execute

### **Explain**
| Permission | Symbol | On Files | On Directories |
|------------|--------|----------|---------------|
| **Read** | r (4) | View content | List files |
| **Write** | w (2) | Modify content | Add/remove files |
| **Execute** | x (1) | Run as program | Enter directory |

### **Use**
- **Read** - View documents, config files
- **Write** - Edit files, create new ones
- **Execute** - Run scripts, programs

### **Command**
```bash
# Add read permission
chmod +r file.txt

# Add write permission
chmod +w file.txt

# Add execute permission
chmod +x script.sh

# Set all permissions (rwx)
chmod 777 file.txt

# Set specific permissions
chmod 644 file.txt    # rw-r--r--
chmod 755 script.sh   # rwxr-xr-x
```

---

## Q3. What is the Difference Between a Soft Link and a Hard Link?

### **Definition**
- **Soft Link (Symbolic Link)**: Points to file path (like shortcut)
- **Hard Link**: Points to same data (another name for file)

### **Explain**
| Feature | Soft Link | Hard Link |
|---------|-----------|-----------|
| **Different File** | ✅ Yes | ❌ No |
| **Can Link Directories** | ✅ Yes | ❌ No |
| **Cross Filesystem** | ✅ Yes | ❌ No |
| **If Original Deleted** | ❌ Broken | ✅ Still works |

### **Use**
- **Soft Link**: Quick access, cross-filesystem, link directories
- **Hard Link**: Save space, multiple names for same file

### **Command**
```bash
# Create soft link (symbolic link)
ln -s /path/to/file link_name

# Create hard link
ln /path/to/file hardlink_name

# View link target
readlink link_name

# Check if it's a link
ls -l link_name
```

---

## Q4. How Do You Change File Ownership (chown, chgrp)?

### **Definition**
- **chown**: Change file owner
- **chgrp**: Change file group

### **Explain**
- Only **root** can change ownership
- Owner can change group ownership (if member)
- Changes affect who can access files

### **Use**
- Transfer files between users
- Set correct ownership for applications
- Manage team access

### **Command**
```bash
# Change owner
sudo chown newuser file.txt

# Change owner and group
sudo chown newuser:newgroup file.txt

# Change group only
sudo chown :newgroup file.txt

# Change recursively (directory and contents)
sudo chown -R user:group /path/to/directory

# Change group
sudo chgrp newgroup file.txt

# View current owner
ls -l file.txt
```

---

## Q5. What is Umask? What Does It Control?

### **Definition**
**umask** sets default permissions for new files and directories

### **Explain**
- Subtracts from maximum permissions (666 for files, 777 for directories)
- Common values: 022 (files: 644, dirs: 755)
- Controls default security level

### **Use**
- Set system-wide default permissions
- Ensure new files have correct security
- Prevent overly permissive files

### **Command**
```bash
# View current umask
umask

# Set umask for current session
umask 022

# Common umask values
umask 022    # Files: 644, Dirs: 755 (standard)
umask 002    # Files: 664, Dirs: 775 (shared)
umask 077    # Files: 600, Dirs: 700 (private)

# Set permanent umask
echo "umask 022" >> ~/.bashrc
```

---

## Q6. What is the Sticky Bit and Where is It Used?

### **Definition**
Special permission on directories that prevents users from deleting others' files

### **Explain**
- Set on shared directories like `/tmp`
- Users can only delete **their own** files
- Prevents accidental/malicious deletion

### **Use**
- Protect shared directories
- `/tmp` directory
- Upload directories
- Team collaboration spaces

### **Command**
```bash
# Set sticky bit
chmod +t /shared/directory
# or
chmod 1777 /shared/directory

# Remove sticky bit
chmod -t /shared/directory

# View sticky bit (look for 't')
ls -ld /tmp
drwxrwxrwt

# Find directories with sticky bit
find / -type d -perm -1000
```

---

## Q7. What are /etc/passwd, /etc/shadow, and /etc/group Used For?

### **Definition**
- **/etc/passwd**: User account information
- **/etc/shadow**: Encrypted passwords
- **/etc/group**: Group information

### **Explain**
- `/etc/passwd`: Username, UID, GID, home, shell
- `/etc/shadow`: Password hash, expiry (root only)
- `/etc/group`: Group name, GID, members

### **Use**
- User management and authentication
- Group management
- User account information

### **Command**
```bash
# View user accounts
cat /etc/passwd

# View specific user
grep username /etc/passwd

# View encrypted passwords (root only)
sudo cat /etc/shadow

# View groups
cat /etc/group

# View user's groups
groups username

# View detailed user info
id username
```

---

## Q8. What is the Difference Between su and sudo?

### **Definition**
- **su**: Switch User
- **sudo**: Superuser Do

### **Explain**
| Feature | su | sudo |
|---------|-----|------|
| **Password Required** | Target user's password | Your password |
| **Session** | New shell | Single command |
| **Logging** | Limited | Full logging |
| **Access Control** | All-or-nothing | Fine-grained |

### **Use**
- **su**: Become another user, administrative tasks
- **sudo**: Run specific commands with elevated privileges

### **Command**
```bash
# Switch to root (needs root password)
su -
# or
su - root

# Switch to another user
su - username

# Run single command as root
sudo apt update

# Run as root shell
sudo -i

# Run as another user
sudo -u www-data command

# View sudo permissions
sudo -l
```

---

## Q9. How Do You Safely Edit the sudoers File (visudo)?

### **Definition**
`visudo` is the safe command to edit sudo configuration

### **Explain**
- Validates syntax before saving
- Prevents locking yourself out
- Locks file during editing
- Only one edit at a time

### **Use**
- Add users to sudo
- Grant specific command access
- Modify sudo configuration

### **Command**
```bash
# Edit sudoers file safely
sudo visudo

# Check syntax without editing
sudo visudo -c

# Edit with specific editor
sudo EDITOR=nano visudo

# Add user to sudoers (add this line)
username ALL=(ALL) ALL

# Grant specific commands only
user ALL=(ALL) /usr/bin/systemctl restart nginx
```

---

## Q10. How Do You Find Recently Modified Files (find -mtime)?

### **Definition**
`find` command with `-mtime` locates files based on modification time

### **Explain**
- `-mtime -n`: Modified **less than** n days ago
- `-mtime +n`: Modified **more than** n days ago
- `-mtime n`: Modified **exactly** n days ago

### **Use**
- Find recently created/modified files
- Clean up old files
- Troubleshooting recent changes

### **Command**
```bash
# Files modified in last 7 days
find /path -mtime -7

# Files modified more than 30 days ago
find /path -mtime +30

# Files modified today
find /path -mtime -1

# Files modified between 7 and 14 days ago
find /path -mtime +7 -mtime -14

# With details
find /path -mtime -7 -ls

# Modified in last 60 minutes
find /path -mmin -60
```

---

## Q11. What Are Hidden Files and How to View Them (ls -a)?

### **Definition**
Files starting with dot (.) are hidden in Linux

### **Explain**
- Hidden files start with `.` (dot)
- Used for configuration files (`.bashrc`, `.gitconfig`)
- Don't appear in normal `ls` output
- Configuration files, user settings, SSH keys

### **Use**
- Store configuration files
- Hide system files from users
- User-specific settings

### **Command**
```bash
# List all files including hidden
ls -a

# List with details
ls -la

# List hidden only (excluding . and ..)
ls -A

# View specific hidden file
cat .bashrc

# View hidden directory
ls -la ~/.ssh/

# Find hidden files
find ~ -name '.*'
```

---

## Q12. How to Change File Permissions (chmod)?

### **Definition**
`chmod` command changes file and directory permissions

### **Explain**
Two modes:
- **Symbolic**: `u+x` (add execute for user)
- **Octal**: `755` (rwxr-xr-x)

Values: r=4, w=2, x=1

### **Use**
- Make scripts executable
- Secure sensitive files
- Control file access

### **Command**
```bash
# Symbolic mode
chmod u+x script.sh       # Add execute for owner
chmod g+w file.txt        # Add write for group
chmod o-r file.txt        # Remove read for others
chmod a+rw file.txt       # Add read/write for all

# Octal mode
chmod 644 file.txt        # rw-r--r--
chmod 755 script.sh       # rwxr-xr-x
chmod 600 private.key     # rw-------
chmod 777 public.txt      # rwxrwxrwx (not recommended)

# Recursive
chmod -R 755 /var/www/html

# View permissions
ls -l file.txt
```

---

## Q13. How to Give Execute Permissions to a Script?

### **Definition**
Add execute permission so script can be run directly

### **Explain**
- Script needs `+x` permission
- Must have **shebang** line (`#!/bin/bash`)
- Can run with `./script.sh` after permission

### **Use**
- Run shell scripts
- Execute programs
- Make automation scripts runnable

### **Command**
```bash
# Create script with shebang
echo '#!/bin/bash' > script.sh
echo 'echo "Hello"' >> script.sh

# Add execute permission
chmod +x script.sh

# Verify
ls -l script.sh

# Execute
./script.sh

# Alternative: specify interpreter (no execute needed)
bash script.sh
sh script.sh

# Make all .sh files executable
chmod +x *.sh
```

---

## Q14. How to Create and Manage Symbolic Links?

### **Definition**
Symbolic links are references to other files/directories

### **Explain**
- Create link: `ln -s target link_name`
- View target: `readlink link_name`
- Remove link: `unlink link_name` or `rm link_name`

### **Use**
- Quick access to files
- Cross-filesystem references
- Application versioning

### **Command**
```bash
# Create symbolic link
ln -s /path/to/target link_name

# Link to directory
ln -s /var/www/html ~/www

# View link
ls -l link_name

# View link target
readlink link_name

# Get absolute path
readlink -f link_name

# Find broken links
find /path -type l -xtype l

# Remove link
rm link_name
# or
unlink link_name

# Update link (overwrite)
ln -sf /new/target link_name

# Check if file is a link
test -L link_name && echo "It's a link"
```

---

## 📚 Quick Reference - Module 2 Commands

### Permissions
```bash
chmod 755 file.sh      # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod +x script.sh     # Add execute
chmod -R 755 dir/      # Recursive
```

### Ownership
```bash
chown user file.txt                # Change owner
chown user:group file.txt          # Change both
chown -R user:group dir/           # Recursive
chgrp group file.txt               # Change group
```

### Links
```bash
ln -s target link                  # Soft link
ln target hardlink                 # Hard link
readlink link                      # View target
```

### User/Group Info
```bash
cat /etc/passwd                    # View users
groups user                        # View user's groups
id user                            # User details
```

### Find Files
```bash
find /path -mtime -7               # Modified last 7 days
find /path -name "*.log"           # Find .log files
ls -la                             # List all (hidden too)
```

---

**Next:** Module 3 - Process & System Management
