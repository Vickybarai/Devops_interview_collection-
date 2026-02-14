# 🟣 MODULE 7: Security, Users & Access

> Detailed but beginner-friendly guide for interview preparation

---

## Q1. What is a User in Linux?

### **Definition**
A user is an identity that allows someone to log in and use the Linux system.

### **Explain (Detail for Beginners)**

**What is a User?**
```
Think of a user like having a personal account on a computer:
- Each user has their own username
- Each user has their own password
- Each user has their own home directory
- Each user can have different permissions

Why do we need users?
- Multiple people can use the same computer
- Each person has their own files and settings
- Security - users can only access what they're allowed to
- Accountability - know who did what

Types of users:
1. Root user (superuser)
   - Username: root
   - Has full control over system
   - Can do anything
   - Should be used carefully

2. Regular users
   - Normal people using the system
   - Have limited permissions
   - Can't change system settings

3. System users
   - Created for services (mysql, nginx, etc.)
   - Can't login directly
   - Used to run services in background
```

**User Information Stored In:**
```
/etc/passwd - User account information
/etc/shadow - Encrypted passwords
```

**User Attributes:**
```
Every user has:
- Username (like "john", "admin")
- User ID (UID) - Number that identifies user (root = 0)
- Group ID (GID) - Primary group
- Home directory - Personal folder (like /home/john)
- Shell - Command interpreter (like /bin/bash)
- Password - For authentication
```

**User ID (UID) Explained:**
```
UID (User ID) is a number that identifies the user:
- root user always has UID 0
- Regular users typically start from UID 1000
- System users have UID less than 1000

Why use UID?
- Numbers are faster for system to process
- Can change username without changing UID
- Consistent across systems

Example:
root     = UID 0 (system administrator)
user1    = UID 1000 (first regular user)
user2    = UID 1001 (second regular user)
```

**Home Directory Explained:**
```
Home directory is the personal space for each user:
- Located at /home/username
- User's personal files go here
- User's configuration files are here

Example:
User: john
Home: /home/john

When john logs in:
- Automatically goes to /home/john
- Can store files there
- Has privacy (others can't access by default)
```

### **Use**
- **Multi-user Environment**: Multiple people can use same system
- **Security**: Each user has limited access
- **Organization**: Separate files and settings for each user
- **Accountability**: Track who did what
- **Privacy**: Users can't access each other's files

### **Command**
```bash
# View current user
whoami
id

# View all users on system
cat /etc/passwd
cut -d: -f1 /etc/passwd

# View user information
id username
finger username    # May need to install

# View user details
grep username /etc/passwd

# View all regular users (UID >= 1000)
awk -F: '$3 >= 1000 {print $1}' /etc/passwd

# Check if user exists
id username
```

---

## Q2. How to Create, Delete, and Modify Users?

### **Definition**
Commands to manage user accounts on Linux system.

### **Explain (Detail for Beginners)**

**Creating a New User:**
```bash
Command:
sudo useradd username

What happens:
1. System creates user account
2. System assigns UID (usually 1000, 1001, etc.)
3. System creates home directory (/home/username)
4. System copies default configuration files
5. User cannot login yet (no password set)

Better way to create user (with password and options):
sudo useradd -m -s /bin/bash username
sudo passwd username

Options explained:
-m  → Create home directory
-s  → Set default shell
-G  → Add to additional group
```

**Creating User Step-by-Step:**
```bash
# Step 1: Create user
sudo useradd john

# Step 2: Set password
sudo passwd john
# You'll be prompted to enter password twice

# Step 3: Verify user was created
id john
cat /etc/passwd | grep john

# All in one command (Ubuntu/Debian):
sudo adduser john
# This command:
# - Creates user
# - Creates home directory
# - Prompts for password
# - Prompts for user info
```

**Modifying User:**
```bash
# Change user's shell
sudo usermod -s /bin/zsh username

# Add user to additional group
sudo usermod -aG docker username

# Change user's home directory
sudo usermod -d /new/home username

# Change user's login name
sudo usermod -l newname oldname

# Lock user account (can't login)
sudo usermod -L username

# Unlock user account
sudo usermod -U username

# Set account expiration date
sudo usermod -e 2024-12-31 username
```

**Deleting User:**
```bash
# Delete user (keep home directory)
sudo userdel username

# Delete user and home directory
sudo userdel -r username

# What happens when deleting:
- User account removed from /etc/passwd
- Home directory may be kept (unless -r used)
- User can no longer login
- Files owned by user remain on system

⚠️  Important:
- Be careful when deleting users
- Make sure to backup important data first
- Files owned by deleted user may be orphaned
```

**Real-World Examples:**

**Example 1: Create new user for team member**
```bash
# Create user with home directory and bash shell
sudo useradd -m -s /bin/bash alice

# Set password
sudo passwd alice

# Add to developers group
sudo usermod -aG developers alice

# Verify
id alice
groups alice
```

**Example 2: User leaving company**
```bash
# Backup user's important files first
sudo cp -r /home/bob /backup/bob_backup

# Lock account (safer than deleting)
sudo usermod -L bob

# After backup confirmed, delete user
sudo userdel -r bob
```

**Example 3: Change shell for developer**
```bash
# Default shell is bash, developer wants zsh
sudo usermod -s /bin/zsh developer

# Verify
grep developer /etc/passwd
```

### **Use**
- **User Management**: Add new team members
- **Access Control**: Remove users who leave
- **Organization**: Group users by function
- **Security**: Lock compromised accounts
- **Maintenance**: Update user settings

### **Command**
```bash
# Create user
sudo useradd username
sudo adduser username    # Interactive (better for beginners)

# Create with options
sudo useradd -m -s /bin/bash -G sudo username

# Set password
sudo passwd username

# Modify user
sudo usermod -aG docker username    # Add to group
sudo usermod -s /bin/zsh username   # Change shell
sudo usermod -L username           # Lock account
sudo usermod -U username           # Unlock account

# Delete user
sudo userdel username     # Keep files
sudo userdel -r username    # Remove files

# View user info
id username
groups username
grep username /etc/passwd

# View all users
cat /etc/passwd
cut -d: -f1 /etc/passwd

# Check if user exists
id username
```

---

## Q3. What is a Group and How to Manage Groups?

### **Definition**
A group is a collection of users who share the same permissions and access rights.

### **Explain (Detail for Beginners)**

**What is a Group?**
```
Think of a group like a team in a company:
- Users with similar needs are in same group
- Group has permissions for shared resources
- Easier to manage permissions for many users

Why use groups?
- Instead of setting permissions for each user
- Set permissions for the group once
- All group members get those permissions
- Easy to add/remove users from group

Example:
Team: Developers
- alice, bob, charlie are in developers group
- They all need access to /project/source code
- Set group permission on directory once
- All developers can access it

New developer joins:
- Add to developers group
- Automatically gets access to project files
- No need to set permissions again
```

**Group Information Stored In:**
```
/etc/group - Group information
/etc/gshadow - Group passwords (rarely used)
```

**Group Types:**
```
1. Primary group
   - Each user has one primary group
   - New files belong to primary group by default
   - Usually named same as username

2. Secondary groups
   - User can belong to multiple groups
   - Additional groups for special access
   - User gets permissions from all groups

Example:
User: alice
Primary group: alice
Secondary groups: developers, docker, sudo
```

**Group ID (GID) Explained:**
```
GID (Group ID) is number that identifies group:
- root group = GID 0
- System groups = GID < 1000
- User groups = GID >= 1000

Example:
root      = GID 0
sudo      = GID 27
docker    = GID 999
developers = GID 1001
```

**Common Default Groups:**
```
sudo      - Users can run sudo commands
docker    - Users can use docker
developers - Custom group for developers
staff     - General staff
```

**Managing Groups:**
```bash
# Create new group
sudo groupadd developers

# Add user to group
sudo usermod -aG developers alice
# -a = append (don't remove other groups)
# -G = group
# developers = group name

# Remove user from group
sudo gpasswd -d alice developers

# Delete group
sudo groupdel developers

# View group members
getent group developers
```

**Real-World Examples:**

**Example 1: Create developers group**
```bash
# Create group
sudo groupadd developers

# Add team members
sudo usermod -aG developers alice
sudo usermod -aG developers bob
sudo usermod -aG developers charlie

# Verify
getent group developers
groups alice
```

**Example 2: Shared project directory**
```bash
# Create group
sudo groupadd webteam

# Create shared directory
sudo mkdir /var/www/project

# Set group ownership
sudo chown -R :webteam /var/www/project

# Set group permissions
sudo chmod -R 775 /var/www/project

# Add users to group
sudo usermod -aG webteam alice
sudo usermod -aG webteam bob

# Now alice and bob can both work on project
```

**Example 3: Remove user from group**
```bash
# Check groups
groups alice

# Remove from specific group
sudo gpasswd -d alice developers

# Verify
groups alice
```

### **Use**
- **Permission Management**: Set permissions for multiple users at once
- **Team Organization**: Group users by function
- **Access Control**: Control who can access what
- **Easy Updates**: Add/remove users without changing permissions
- **Security**: Least privilege principle

### **Command**
```bash
# Create group
sudo groupadd groupname

# Add user to group
sudo usermod -aG groupname username
sudo gpasswd -a username groupname

# Remove user from group
sudo gpasswd -d username groupname

# Delete group
sudo groupdel groupname

# View groups
getent group groupname
cat /etc/group

# View user's groups
groups username
id username

# View all groups
cat /etc/group
cut -d: -f1 /etc/group

# View group members
getent group sudo
```

---

## Q4. What is Sudo and How to Use It?

### **Definition**
sudo (superuser do) - Command that allows regular users to run commands with root privileges.

### **Explain (Detail for Beginners)**

**What is sudo?**
```
sudo is like asking for permission to do something as root:
- Regular user = limited permissions
- sudo user = can do some root tasks
- root user = can do everything

Why use sudo?
- Don't need to be root for everything
- Safer than logging in as root
- Can control what each user can do
- Logs all sudo commands (audit trail)
```

**How sudo Works:**
```
Normal command (without sudo):
$ cat /etc/shadow
cat: /etc/shadow: Permission denied
# Regular user can't read this file

With sudo:
$ sudo cat /etc/shadow
[sudo] password for user:
# (You enter YOUR password, not root password)
# Shows file contents

What happened:
1. You typed sudo before command
2. System checks if you're allowed to use sudo
3. If allowed, asks for YOUR password
4. After entering password, command runs as root
5. Result: command succeeds with root privileges
```

**sudo vs su:**
```
sudo:
- Runs one command with elevated privileges
- Uses YOUR password
- Doesn't open new shell
- Safer (audited, controlled)

su:
- Opens new shell as root
- Uses ROOT password
- Full root access
- Less safe (harder to audit)

Example:
sudo cat /etc/shadow        # Read one file as root, then back to normal user
su -                       # Become root until you exit
```

**Sudoers File:**
```
/etc/sudoers file controls who can use sudo and what they can do:
- Defines which users can use sudo
- Defines what commands they can run
- Controls whether password is required

Example sudoers entry:
username ALL=(ALL) ALL
│         │     │   │
│         │     │   └─ Can run ALL commands
│         │     └─────── As ALL users
│         └─────────────── On ALL hosts
└───────────────────────── Username
```

**Real-World Examples:**

**Example 1: Update system**
```bash
# Regular user can't install packages
$ apt update
E: Could not open lock file /var/lib/dpkg/lock-frontend

# With sudo:
$ sudo apt update
# Works because running as root
```

**Example 2: Edit system file**
```bash
# Regular user can't edit system config
$ nano /etc/hosts
Permission denied

# With sudo:
$ sudo nano /etc/hosts
# Opens file as root
```

**Example 3: Restart service**
```bash
# Regular user can't restart services
$ systemctl restart nginx
==== AUTHENTICATING FOR org.freedesktop.systemd1 ===
==== Authentication is required to restart 'nginx.service'.

# With sudo:
$ sudo systemctl restart nginx
# Works because running as root
```

**Checking Sudo Access:**
```bash
# Check if you can use sudo
sudo -v

# View your sudo permissions
sudo -l

# Output example:
User user may run the following commands on host:
    (ALL) ALL
    This means user can run ALL commands as ALL users
```

### **Use**
- **System Administration**: Perform administrative tasks
- **Security**: Don't log in as root
- **Control**: Limit what users can do
- **Audit**: Track who did what
- **Delegation**: Give users specific permissions

### **Command**
```bash
# Use sudo to run command as root
sudo command
sudo apt update
sudo systemctl restart nginx

# Run as different user
sudo -u www-data command

# Check sudo access
sudo -v              # Verify you can use sudo
sudo -l              # View sudo permissions

# Check last sudo commands
sudo -l -U username   # Check another user's sudo access

# View sudoers file
sudo visudo           # Edit sudoers safely
sudo cat /etc/sudoers # View sudoers
```

---

## Q5. How to Manage Passwords?

### **Definition**
Commands to set, change, and manage user passwords for security.

### **Explain (Detail for Beginners)**

**What is a Password?**
```
Password is the secret that proves you are who you say you are:
- Allows you to login
- Protects your account
- Should be kept secret
- Should be strong (hard to guess)

Strong password has:
- At least 8 characters
- Mix of letters, numbers, symbols
- Not a common word
- Not personal information
```

**Setting Password:**
```bash
# Set password for user (root or sudo)
sudo passwd username

# What happens:
1. System prompts for new password
2. You type new password (doesn't show on screen)
3. System prompts to confirm password
4. You type password again
5. If passwords match, password is set

Example:
$ sudo passwd alice
New password:
Retype new password:
passwd: password updated successfully
```

**Changing Your Own Password:**
```bash
# Change your own password (no sudo needed)
passwd

# What happens:
1. System asks for current password
2. System asks for new password
3. System asks to confirm new password
4. If current password is correct and new passwords match, password changed
```

**Password Requirements:**
```
Common password rules:
- Minimum length: 8 characters
- Must not be username
- Must not be in password dictionary
- Must be different from previous password
- Must contain at least: letters, numbers, symbols

System enforces these rules for security
```

**Password Policy:**
```bash
# View password policy
chage -l username

# Output example:
Last password change: Jan 15, 2024
Password expires: never
Password inactive: never
Account expires: never
Minimum number of days between password change: 0
Maximum number of days between password change: 99999
Number of days of warning before password expires: 7
```

**Setting Password Expiration:**
```bash
# Set password to expire in 90 days
sudo chage -M 90 username

# Force user to change password on next login
sudo chage -d 0 username

# Check password age
chage -l username
```

**Forcing Password Reset:**
```bash
# Lock user (can't login with password)
sudo passwd -l username

# Unlock user
sudo passwd -u username

# Expire password (forces change on next login)
sudo chage -e 0 username
```

**Real-World Examples:**

**Example 1: New user needs password**
```bash
# Create user (Ubuntu/Debian)
sudo adduser john

# The adduser command already sets password
# But if you used useradd instead:
sudo useradd john
sudo passwd john
# Then set password
```

**Example 2: User forgot password**
```bash
# Reset user's password (you need sudo)
sudo passwd john
# Set new password
# Tell user their new password (securely)
```

**Example 3: Password policy for temporary accounts**
```bash
# Create temporary account
sudo adduser tempuser

# Set password
sudo passwd tempuser

# Make password expire in 7 days
sudo chage -M 7 tempuser

# Account will be locked in 7 days
```

### **Use**
- **Security**: Set and change passwords
- **Account Management**: Reset forgotten passwords
- **Policy Enforcement**: Set password expiration
- **Compliance**: Meet security requirements
- **Access Control**: Lock accounts when needed

### **Command**
```bash
# Set password for user (requires sudo)
sudo passwd username

# Change your own password
passwd

# Check password info
chage -l username

# Set password to expire
sudo chage -M 90 username
sudo chage -e 2024-12-31 username

# Force password change on next login
sudo chage -d 0 username

# Lock user account (can't login with password)
sudo passwd -l username

# Unlock user account
sudo passwd -u username

# Check if password is locked
sudo passwd -S username
```

---

## Q6. Basic SSH Security Best Practices?

### **Definition**
SSH (Secure Shell) security practices to protect remote access to Linux servers.

### **Explain (Detail for Beginners)**

**What is SSH Security?**
```
SSH is how you remotely access servers:
- SSH allows you to login from your computer to a server
- SSH must be secured to prevent unauthorized access
- Poor SSH security = hackers can access your server

SSH security basics:
- Don't allow root login
- Use SSH keys instead of passwords
- Change default port
- Limit who can connect
```

**Disable Root SSH Login:**
```
Why disable root SSH login?
- Everyone knows root username
- Attackers try to guess root password
- If they get root, they control everything
- Better to login as regular user, then use sudo

How to disable root SSH login:
1. Edit SSH config
sudo nano /etc/ssh/sshd_config

2. Find and change this line:
PermitRootLogin yes
to:
PermitRootLogin no

3. Restart SSH
sudo systemctl restart sshd

Now:
- Root cannot login via SSH
- Must login as regular user
- Then use sudo for administrative tasks
```

**Use SSH Keys Instead of Passwords:**
```
Why use SSH keys?
- More secure than passwords
- Can't be guessed
- No need to enter password every time
- Can disable password authentication entirely

Steps:
1. Generate SSH key pair on your computer
ssh-keygen -t rsa -b 4096

2. Copy public key to server
ssh-copy-id user@server

3. Test login (no password required)
ssh user@server

4. (Optional) Disable password authentication in SSH config
```

**Change Default SSH Port:**
```
Why change default port?
- Default port 22 is well-known
- Attackers scan port 22
- Using different port makes it harder to find

How to change SSH port:
1. Edit SSH config
sudo nano /etc/ssh/sshd_config

2. Find and change:
#Port 22
Port 2222

3. Restart SSH
sudo systemctl restart sshd

4. Update firewall to allow new port
sudo ufw allow 2222/tcp

Now connect with:
ssh -p 2222 user@server
```

**Limit SSH Access:**
```
Allow only specific users:
# In /etc/ssh/sshd_config
AllowUsers alice bob
# Only alice and bob can login

Deny specific users:
DenyUsers baduser

Allow only from specific IP:
# In /etc/hosts.allow
sshd: 192.168.1.100

Or use firewall:
sudo ufw allow from 192.168.1.0/24 to any port 22
```

**Disable Password Authentication (After Setting Up Keys):**
```
Once SSH keys are set up:
1. Edit SSH config
sudo nano /etc/ssh/sshd_config

2. Change:
PasswordAuthentication yes
to:
PasswordAuthentication no

3. Restart SSH
sudo systemctl restart sshd

Now:
- Only SSH keys work
- Passwords don't work
- More secure
```

### **Use**
- **Security**: Protect server from unauthorized access
- **Access Control**: Limit who can login
- **Compliance**: Meet security standards
- **Monitoring**: Track SSH access
- **Protection**: Prevent brute force attacks

### **Command**
```bash
# SSH config location
/etc/ssh/sshd_config

# Restart SSH after changes
sudo systemctl restart sshd

# Check SSH status
sudo systemctl status sshd

# View SSH logs
sudo journalctl -u sshd
sudo tail -f /var/log/auth.log

# Check failed login attempts
sudo grep "Failed password" /var/log/auth.log

# Generate SSH keys
ssh-keygen -t rsa -b 4096

# Copy key to server
ssh-copy-id user@server

# Test SSH connection
ssh user@server

# Connect on custom port
ssh -p 2222 user@server

# View SSH config
sudo cat /etc/ssh/sshd_config
```

---

## Q7. What are File Permissions and How to Set Them?

### **Definition**
File permissions control who can read, write, and execute files and directories.

### **Explain (Detail for Beginners)**

**Permission Basics:**
```
Every file and directory has permissions for:
1. Owner (user who created it)
2. Group (owner's primary group)
3. Others (everyone else)

Permissions are:
r = read    (can view contents)
w = write   (can modify contents)
x = execute (can run as program)

Permission examples:
-rw-r--r--  (644) = owner can read/write, others can only read
-rwxr-xr-x  (755) = owner can read/write/execute, others can read/execute
```

**Understanding Permission String:**
```bash
-rw-r--r--  1 user group 1024 Jan 15 10:00 file.txt
││││ │││ │     │     │    │       │
│││ │││ │     │     │    └──────── Filename
│││ │││ │     │     └──────────── Size
│││ │││ │     └────────────────── Date/time
│││ ││└─────────────────────────── Group
│││ └────────────────────────────── Owner
│└───────────────────────────────── Permissions

Permissions breakdown:
- = regular file
r = read permission for owner
w = write permission for owner
- = no execute for owner
r = read permission for group
- = no write for group
- = no execute for group
r = read permission for others
- = no write for others
- = no execute for others
```

**Numeric Permissions:**
```
r = 4
w = 2
x = 1

So:
rw-r--r-- = (4+2)(4)(4) = 644
rwxr-xr-x = (4+2+1)(4+1)(4+1) = 755

Common permissions:
644 = rw-r--r-- (files)
755 = rwxr-xr-x (scripts, programs)
600 = rw------- (private files)
700 = rwx------ (private directories)
777 = rwxrwxrwx (everyone can do everything - avoid!)
```

**Setting Permissions:**
```bash
# Using numeric mode
chmod 644 file.txt      # rw-r--r--
chmod 755 script.sh     # rwxr-xr-x
chmod 600 secret.txt    # rw-------

# Using symbolic mode
chmod +x script.sh      # Add execute
chmod g+w file.txt      # Add write for group
chmod o-r file.txt      # Remove read from others
```

**Directory Permissions:**
```
Directory permissions are similar but with different effects:

x on directory = can enter (cd) the directory
r on directory = can list contents (ls)
w on directory = can add/remove files in directory

Common directory permissions:
755 = rwxr-xr-x (can enter, list, add files)
700 = rwx------ (private directory, only owner)
```

### **Use**
- **Security**: Control who can access files
- **Privacy**: Protect personal files
- **Collaboration**: Share files with team
- **Scripts**: Make scripts executable
- **Websites**: Set correct web file permissions

### **Command**
```bash
# View permissions
ls -l file.txt
ls -ld directory/

# Change permissions (numeric)
chmod 644 file.txt
chmod 755 script.sh
chmod 700 directory/

# Change permissions (symbolic)
chmod +x script.sh
chmod g+w file.txt
chmod o-r file.txt
chmod ugo+rwx file.txt

# Set permissions recursively
chmod -R 755 /var/www/html

# Copy permissions from another file
chmod --reference=file1.txt file2.txt

# Check permissions
stat file.txt
ls -l file.txt

# Find files with specific permissions
find /path -perm 644
find /path -perm -o+w
```

---

## Q8. How to Switch Users (su, sudo -i, sudo su)?

### **Definition**
Commands to switch between different user accounts on Linux system.

### **Explain (Detail for Beginners)**

**What is User Switching?**
```
User switching allows you to become another user:
- Switch to root to do administrative tasks
- Switch to application user to troubleshoot
- Switch between users for testing

Why switch users?
- Normal user doesn't have root privileges
- Application needs to run as specific user
- Testing with different user permissions
```

**su Command (Switch User):**
```bash
# Switch to root
su -
# Asks for root password
# Now you are root

# Switch to another user
su - username
# Asks for that user's password

# su explained:
- = switch to root's home directory
su = switch user (keep current directory)
- = Start login shell (load user's environment)

Example:
$ su -
Password: (enter root password)
# Now you are root
root@server:~# whoami
root

# Exit back to original user
root@server:~# exit
$
```

**sudo -i (Interactive):**
```bash
# Start root shell using sudo
sudo -i
# Asks for YOUR password (not root password)
# Starts root shell

Why use sudo -i instead of su?
- Uses YOUR password (don't need root password)
- Logs who switched (audit trail)
- Safer than su

Example:
$ sudo -i
[sudo] password for user:
# Enter your password
root@server:~# whoami
root

# Exit
root@server:~# exit
$
```

**sudo su (Combine Both):**
```bash
# Switch to root using sudo (your password)
sudo su -
# or
sudo su - root

Why use this?
- Uses sudo privileges
- Doesn't need root password
- Logs the action
- Combined benefits of sudo and su
```

**When to Use Each:**
```bash
su -                # Have root password, full root access
sudo -i             # Have sudo access, want root shell
sudo su -           # Have sudo access, want root shell
sudo command        # Run single command as root
```

**Real-World Examples:**

**Example 1: Administrator needs to run several commands as root**
```bash
# Option 1: Use sudo for each command
sudo apt update
sudo apt upgrade
sudo systemctl restart nginx

# Option 2: Use sudo -i (better for multiple commands)
sudo -i
apt update
apt upgrade
systemctl restart nginx
exit
```

**Example 2: Troubleshoot application as application user**
```bash
# Application runs as 'www-data' user
# Need to test as that user

# Switch to www-data
sudo -u www-data -i
# Now you are www-data

# Test something
ls /var/www/html

# Exit
exit
```

### **Use**
- **Administration**: Perform tasks as root
- **Troubleshooting**: Test as different users
- **Testing**: Verify permissions work correctly
- **Application Debugging**: Run as application user

### **Command**
```bash
# Switch to root (needs root password)
su -

# Switch to another user
su - username

# Switch to root with sudo (your password)
sudo -i
sudo su -

# Run command as root (no shell)
sudo command

# Run command as different user
sudo -u username command

# Switch back to original user
exit

# Check current user
whoami
id

# View user history
history
```

---

## Q9. What are /etc/passwd, /etc/shadow, /etc/group?

### **Definition**
Configuration files that store user and group information on Linux system.

### **Explain (Detail for Beginners)**

**/etc/passwd - User Account Information:**
```
What is it?
- Contains information about all users
- Readable by everyone
- Does NOT store actual passwords

Format:
username:x:UID:GID:GECOS:home:shell
          │  │  │  │     │     └─ Login shell
          │  │  │  │     └───── Home directory
          │  │  │  └────────── Comment/Real name
          │  │  └───────────── Group ID (GID)
          │  └──────────────── User ID (UID)
          └───────────────────── x (password placeholder)

Example:
john:x:1000:1000:John Doe:/home/john:/bin/bash
     │  │   │   │         │           │
     │  │   │   │         │           └─ Shell (bash)
     │  │   │   │         └───────────── Home directory
     │  │   │   └───────────────────── Real name
     │  │   └───────────────────────── GID
     │  └────────────────────────────── UID
     └───────────────────────────────── Username
```

**/etc/shadow - Encrypted Passwords:**
```
What is it?
- Contains encrypted passwords
- Only readable by root
- More secure than /etc/passwd

Format:
username:hash:last_change:min_age:max_age:warn:inactive:expire
          │    │       │          │      │     │         └─ Account expiration
          │    │       │          │      │     └─────────── Days inactive before lock
          │    │       │          │      └──────────────── Days to warn before expiry
          │    │       │          └─────────────────── Maximum password age
          │    │       └───────────────────────────── Minimum password age
          │    └───────────────────────────────────── Days since last password change
          └─────────────────────────────────────────── Encrypted password

Important: This file should only be readable by root!
```

**/etc/group - Group Information:**
```
What is it?
- Contains information about all groups
- Readable by everyone

Format:
groupname:x:GID:members
             │   │    └─ Comma-separated list of users
             │   └─────── Group ID (GID)
             └─────────── Group name

Example:
sudo:x:27:user1,user2,user3
     │   │  └───────────────── Members
     │   └───────────────── GID
     └───────────────────── Group name
```

**Real-World Usage:**

**Example 1: Check if user exists**
```bash
# Look in /etc/passwd
grep username /etc/passwd

# Or use id command
id username
```

**Example 2: View all users**
```bash
# All users
cat /etc/passwd

# Just usernames
cut -d: -f1 /etc/passwd
```

**Example 3: View user's groups**
```bash
# Check /etc/group
grep username /etc/group

# Or use groups command
groups username
```

**Example 4: Add user to group in /etc/group**
```bash
# Edit /etc/group
sudo nano /etc/group

# Add user to group line:
developers:x:1001:alice,bob,charlie

# Save file
# Changes take effect immediately
```

### **Use**
- **User Management**: View and modify user information
- **Group Management**: View and modify group information
- **Troubleshooting**: Check user/group configuration
- **Security**: Ensure proper file permissions

### **Command**
```bash
# View /etc/passwd
cat /etc/passwd
grep username /etc/passwd
cut -d: -f1 /etc/passwd

# View /etc/shadow (requires root)
sudo cat /etc/shadow
sudo grep username /etc/shadow

# View /etc/group
cat /etc/group
grep groupname /etc/group
cut -d: -f1 /etc/group

# Get user info from /etc/passwd
getent passwd username

# Get group info from /etc/group
getent group groupname

# View user details
id username
grep username /etc/passwd | cut -d: -f7  # Shell
grep username /etc/passwd | cut -d: -f6  # Home directory
```

---

## Q10. Basic Security Best Practices for Beginners?

### **Definition**
Essential security practices to protect Linux systems from unauthorized access.

### **Explain (Detail for Beginners)**

**Security Mindset:**
```
Why is security important?
- Protects your data
- Prevents unauthorized access
- Keeps system stable
- Meets compliance requirements
- Prevents downtime and data loss

Security basics:
- Keep system updated
- Use strong passwords
- Limit access to what's needed
- Monitor system activity
- Backup regularly
```

**1. Keep System Updated:**
```bash
Why update?
- Security fixes for vulnerabilities
- Bug fixes
- New features
- Better performance

How to update (Ubuntu/Debian):
sudo apt update          # Update package list
sudo apt upgrade         # Upgrade packages
sudo apt full-upgrade    # Full upgrade including dependencies

How to update (RHEL/CentOS):
sudo yum update
# or
sudo dnf upgrade

Set up automatic updates:
sudo apt install unattended-upgrades
# Now system updates automatically
```

**2. Use Strong Passwords:**
```
What makes a strong password?
- At least 8 characters (longer is better)
- Mix of uppercase and lowercase
- Mix of letters and numbers
- Special characters (!@#$%)
- Not a common word
- Not personal information

Example weak passwords:
- password
- 123456
- john123
- qwerty

Example strong passwords:
- Tr0ub4dor!23# (random characters, symbols, numbers)
- MyH0use$2024! (but not this exact one)
- Blue$Sky#99@Red

Never share your password
Never reuse passwords across systems
```

**3. Limit Root Access:**
```bash
✅ DO:
- Use sudo instead of logging in as root
- Give sudo access only to trusted users
- Limit what sudo can do

❌ DON'T:
- Share root password
- Login as root for regular tasks
- Give sudo to everyone

Disable root SSH login:
sudo nano /etc/ssh/sshd_config
# Change: PermitRootLogin no
sudo systemctl restart sshd
```

**4. Secure SSH:**
```
✅ DO:
- Use SSH keys instead of passwords
- Change default port (2222 instead of 22)
- Disable password authentication
- Limit who can login
- Keep SSH updated

❌ DON'T:
- Use default port 22
- Allow password authentication
- Allow root login via SSH
- Share SSH keys insecurely
```

**5. Use Firewall:**
```bash
Why use firewall?
- Blocks unwanted traffic
- Only allows necessary ports
- Protects from network attacks

Enable firewall (Ubuntu):
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw status

Check firewall status:
sudo ufw status verbose
```

**6. Monitor System:**
```bash
What to monitor?
- Failed login attempts
- Suspicious activity
- Disk space (prevent full disk)
- System performance
- Running services

Check failed logins:
sudo grep "Failed password" /var/log/auth.log

Check disk space:
df -h

Check running services:
systemctl status
```

**7. Regular Backups:**
```
Why backup?
- Data loss can happen anytime
- Hardware failure
- Accidental deletion
- Ransomware attack
- Human error

What to backup?
- Important configuration files
- User data
- Application data
- Website files

Where to backup?
- External drive
- Cloud storage
- Different server
- Offsite backup

How often?
- Critical data: Daily
- Important files: Weekly
- Configuration: After changes
```

**8. Remove Unnecessary Software:**
```bash
Why remove unused software?
- Reduces attack surface
- Fewer security vulnerabilities
- Saves disk space
- Faster boot time

List installed packages:
dpkg --list
rpm -qa

Remove unused packages:
sudo apt remove package_name
sudo apt autoremove    # Remove dependencies
```

### **Use**
- **Protection**: Keep system secure
- **Prevention**: Stop attacks before they happen
- **Detection**: Find security issues
- **Recovery**: Backups for disaster recovery
- **Compliance**: Meet security requirements

### **Command**
```bash
# Update system
sudo apt update && sudo apt upgrade

# Check for security updates
sudo apt list --upgradable

# Check firewall status
sudo ufw status

# Check failed logins
sudo grep "Failed" /var/log/auth.log

# Check disk space
df -h

# View running services
systemctl list-units --type=service --state=running

# View last logins
last
lastlog

# View installed packages
dpkg --list

# Remove package
sudo apt remove package
sudo apt autoremove
```

---

## 📚 Quick Reference - Module 7 Commands

### User Management
```bash
useradd username              # Create user
adduser username              # Create user (interactive)
userdel username             # Delete user
userdel -r username          # Delete with home
usermod -aG group user      # Add to group
passwd username               # Set password
```

### Group Management
```bash
groupadd groupname            # Create group
gpasswd -a user group        # Add user to group
gpasswd -d user group        # Remove user
groupdel groupname           # Delete group
```

### Permissions
```bash
chmod 644 file.txt            # Set permissions
chmod +x script.sh            # Add execute
chown user:group file        # Change owner
chgrp group file              # Change group
```

### Sudo & Root
```bash
sudo command                  # Run as root
su -                          # Become root
sudo -i                       # Root shell with sudo
sudo su -                     # Combine sudo and su
```

### Security
```bash
sudo ufw enable               # Enable firewall
sudo ufw status               # Check firewall
systemctl status sshd         # Check SSH
sudo journalctl -u sshd        # SSH logs
```

### User Info
```bash
whoami                        # Current user
id                            # User ID and groups
groups user                   # User's groups
last                          # Login history
```

---

**Next:** Module 8 - Scheduling & Automation
