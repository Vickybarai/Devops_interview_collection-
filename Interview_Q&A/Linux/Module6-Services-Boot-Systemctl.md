# 🟧 MODULE 6: Services, Boot & Systemctl

> Basic service management for beginners

---

## Q1. What is a Service in Linux?

### **Definition**
A service is a program that runs in the background and provides functionality to the system or users.

### **Explain (Detail for Beginners)**

**What is a Service?**
```
Think of a service like a worker that keeps working in the background
while you do other things.

Examples of services:
- Web server (nginx, apache) - Serves websites
- SSH server (sshd) - Allows remote login
- Database (mysql, postgresql) - Stores and retrieves data
- Print server (cups) - Handles printing

Services are different from regular programs:
Regular programs:
- You run them, they do their job, then they stop
- Example: ls, cat, grep commands

Services:
- Start when system boots
- Keep running in the background
- Wait for requests and respond
- Don't stop unless you stop them
```

**Why Do We Need Services?**
```
Services provide continuous functionality:

Web Server Service:
- Starts when your computer boots
- Keeps running 24/7
- Ready to serve web pages at any time
- You don't need to manually start it

Without services:
- You'd have to manually start web server every day
- If you close the terminal, server stops
- Not practical for production
```

**How Services Work:**
```
1. System boots
2. Services start automatically
3. Services run in background
4. Services listen for requests
5. When request comes, service responds
6. Service keeps running, waiting for next request

Example: Web Server
- Starts at boot
- Listens on port 80
- When you visit website, web server responds
- Continues running, waiting for more requests
```

**Common Services on Linux:**
```
sshd    - SSH server (remote login)
nginx   - Web server
apache2 - Another web server
mysql   - Database server
cron    - Scheduled tasks
cups    - Printing
systemd - System and service manager (init system)
```

### **Use**
- **Provide Continuous Functionality**: Services run 24/7
- **Background Tasks**: Handle jobs without user interaction
- **System Operations**: Core system functions
- **Network Services**: Web, email, database, etc.
- **Automation**: Scheduled tasks, monitoring

### **Command**
```bash
# List all active services
systemctl list-units --type=service --state=running

# List all services (active and inactive)
systemctl list-units --type=service

# View all installed services
systemctl list-unit-files --type=service

# Check if a service is installed
systemctl status nginx
systemctl status apache2

# View service details
systemctl show nginx
```

---

## Q2. What is systemctl and How to Use It?

### **Definition**
`systemctl` is the command-line tool used to control the systemd system and service manager.

### **Explain (Detail for Beginners)**

**What is systemctl?**
```
systemctl is like a remote control for services on your Linux system.

Think of it this way:
- Your system = A house with many rooms
- Services = Workers in each room
- systemctl = Walkie-talkie to talk to workers

With systemctl, you can:
- Start a service (tell worker to start working)
- Stop a service (tell worker to stop)
- Restart a service (tell worker to restart)
- Check if service is running (ask worker if they're working)
```

**Basic systemctl Syntax:**
```bash
systemctl [command] [service-name]

Examples:
systemctl start nginx      - Start nginx service
systemctl stop nginx       - Stop nginx service
systemctl restart nginx    - Restart nginx service
systemctl status nginx     - Check nginx status
```

**Common systemctl Commands:**
```bash
# Check service status
systemctl status nginx

# Output:
# ● nginx.service - A high performance web server
#      Loaded: loaded (/lib/systemd/system/nginx.service)
#      Active: active (running) since Mon 2024-01-15 10:00:00
#        Docs: man:nginx(8)
#    Process: 1234 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
#   Main PID: 1235 (nginx)
#      Tasks: 2 (limit: 4915)
#     Memory: 5.2M
#        CPU: 15ms
#     CGroup: /system.slice/nginx.service
#              ├─1235 "nginx: master process"
#              └─1236 "nginx: worker process"
#            │     │
#            │     └─ Worker processes (doing the work)
#            └───────── Main process (parent)

# Important parts:
# Active: active (running)  → Service is running
# Active: inactive          → Service is stopped
# Active: failed            → Service has error
# Loaded: loaded            → Service is installed
```

**Service States Explained:**
```
active (running)  → Service is working normally
active (exited)    → Service finished (one-time task like backup)
inactive          → Service is stopped
failed            → Service has an error
activating        → Service is starting up
deactivating      → Service is stopping
```

### **Use**
- **Manage Services**: Start, stop, restart services
- **Check Status**: See if service is running
- **Enable/Disable**: Control if service starts at boot
- **View Logs**: See service messages
- **Troubleshoot**: Diagnose service issues

### **Command**
```bash
# Check service status
systemctl status nginx
systemctl status apache2

# Start a service
sudo systemctl start nginx

# Stop a service
sudo systemctl stop nginx

# Restart a service
sudo systemctl restart nginx

# Check if service is enabled
systemctl is-enabled nginx
systemctl is-enabled apache2

# View service logs
journalctl -u nginx
journalctl -u nginx -f       # Follow logs in real-time
```

---

## Q3. How to Start, Stop, and Restart Services?

### **Definition**
Basic commands to control service lifecycle - start, stop, and restart services.

### **Explain (Detail for Beginners)**

**Starting a Service:**
```
What happens when you start a service:
1. System reads service configuration
2. Service process is created
3. Service loads into memory
4. Service begins its work
5. Service is now "active (running)"

Command:
sudo systemctl start nginx

When to use:
- Service is stopped and you need it
- After making configuration changes
- After installing a new service
```

**Stopping a Service:**
```
What happens when you stop a service:
1. System tells service to stop
2. Service finishes current work (graceful)
3. Service cleans up resources
4. Service process ends
5. Service is now "inactive"

Command:
sudo systemctl stop nginx

When to use:
- Service has a problem and needs to be stopped
- Maintenance or update required
- Service is not needed

⚠️  Important:
- Stopping web server means website goes down
- Stopping database means applications can't access data
- Be careful when stopping critical services
```

**Restarting a Service:**
```
What happens when you restart:
1. System tells service to stop
2. Service stops gracefully
3. System starts service again
4. Service loads fresh configuration
5. Service is now running again

Command:
sudo systemctl restart nginx

When to use:
- After changing configuration files
- Service is not working properly
- Service needs to reload settings

Better alternative (if supported):
sudo systemctl reload nginx
# This reloads configuration without stopping
# Downtime is minimal or zero
```

**Real-World Examples:**

**Example 1: Start web server after installation**
```bash
# Install nginx
sudo apt install nginx

# Service doesn't start automatically
# Check status
systemctl status nginx
# Shows: inactive (dead)

# Start the service
sudo systemctl start nginx

# Verify it's running
systemctl status nginx
# Shows: active (running)
```

**Example 2: Restart after configuration change**
```bash
# Edit nginx configuration
sudo nano /etc/nginx/nginx.conf

# Changes won't take effect until restart
sudo systemctl restart nginx

# Service restarted with new config
```

**Example 3: Stop service for maintenance**
```bash
# Stop web server
sudo systemctl stop nginx
# Website now down

# Do maintenance work
# ...

# Start again
sudo systemctl start nginx
# Website back up
```

### **Use**
- **Start**: Activate a stopped service
- **Stop**: Deactivate a running service
- **Restart**: Stop and start service (reload configuration)
- **Reload**: Reload configuration without stopping (if supported)

### **Command**
```bash
# Start service
sudo systemctl start nginx
sudo systemctl start apache2
sudo systemctl start mysql

# Stop service
sudo systemctl stop nginx
sudo systemctl stop apache2

# Restart service
sudo systemctl restart nginx
sudo systemctl restart apache2

# Reload (better than restart for some services)
sudo systemctl reload nginx

# Verify status after command
systemctl status nginx
```

---

## Q4. How to Check Service Status?

### **Definition**
Command to see if a service is running, stopped, or has errors.

### **Explain (Detail for Beginners)**

**Checking Service Status:**
```
Why check status?
- Know if service is running
- See if service has errors
- Check how long service has been running
- See resource usage (memory, CPU)
- Troubleshoot service problems

Command:
systemctl status nginx

This tells you everything about the service
```

**Understanding Status Output:**
```bash
$ systemctl status nginx
● nginx.service - A high performance web server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2024-01-15 10:00:00 IST; 5h ago
       Docs: man:nginx(8)
   Main PID: 1234 (nginx)
      Tasks: 2 (limit: 4915)
     Memory: 5.2M
        CPU: 150ms
     CGroup: /system.slice/nginx.service
             ├─1234 "nginx: master process"
             └─1235 "nginx: worker process"

Key information:
●        → Green dot = service is running
         → Red dot = service is stopped or failed
Active: active (running)  → Service is working
Active: inactive          → Service is stopped
Active: failed            → Service has error
Loaded: enabled          → Service starts at boot
Loaded: disabled         → Service doesn't start at boot
since ... 5h ago        → Service has been running for 5 hours
Main PID: 1234          → Process ID
Memory: 5.2M            → Memory usage (5.2 megabytes)
CPU: 150ms             → Total CPU time used
```

**Common Status States:**
```
active (running)  → Service is working fine ✓
active (exited)    → Service completed successfully (one-time tasks)
inactive          → Service is stopped
failed            → Service has an error ✗
activating        → Service is starting up
deactivating      → Service is stopping
```

**Checking Multiple Services:**
```bash
# Check if service is running
systemctl is-active nginx
# Output: active

systemctl is-active apache2
# Output: inactive

# Check if service is enabled
systemctl is-enabled nginx
# Output: enabled

systemctl is-enabled mysql
# Output: disabled
```

**Quick Status Check:**
```bash
# Simple yes/no check
systemctl is-active nginx && echo "Running" || echo "Not running"

# Check multiple services
for service in nginx mysql apache2; do
    echo "$service: $(systemctl is-active $service)"
done
```

### **Use**
- **Verify Service**: Check if service is running
- **Troubleshoot**: Identify why service isn't working
- **Monitor**: Check resource usage
- **Debug**: See error messages
- **Health Check**: Part of system monitoring

### **Command**
```bash
# Check service status (detailed)
systemctl status nginx
systemctl status apache2

# Quick check (active or not)
systemctl is-active nginx
systemctl is-active apache2

# Check if enabled at boot
systemctl is-enabled nginx

# Check multiple services
systemctl status nginx mysql apache2

# Show failed services
systemctl --failed

# Show all running services
systemctl list-units --type=service --state=running
```

---

## Q5. How to Enable/Disable Services at Boot?

### **Definition**
Commands to control whether a service automatically starts when the system boots.

### **Explain (Detail for Beginners)**

**What Does "Enable at Boot" Mean?**
```
When you enable a service:
- Service starts automatically when system boots
- Service starts even before you login
- Service keeps running until you stop it
- Good for servers that need to be always available

When you disable a service:
- Service does NOT start when system boots
- You must manually start it after boot
- Good for services you only use sometimes

Example: Web Server
Enabled:  Web server starts automatically when server boots
Disabled: You must manually start web server after boot
```

**Enabling a Service:**
```bash
Command:
sudo systemctl enable nginx

What happens:
1. System creates a symlink
2. Links service to boot process
3. On next boot, service will start
4. Service remains stopped now (doesn't start immediately)
5. Service will start on next reboot

If you want it to start now AND at boot:
sudo systemctl enable --now nginx
# enable = start at boot
# --now = start immediately too
```

**Disabling a Service:**
```bash
Command:
sudo systemctl disable nginx

What happens:
1. System removes symlink
2. Service won't start at boot
3. Service continues running now (doesn't stop immediately)
4. Service won't start on next reboot

If you want to disable AND stop now:
sudo systemctl disable --now nginx
# disable = don't start at boot
# --now = stop immediately too
```

**Checking if Service is Enabled:**
```bash
Command:
systemctl is-enabled nginx

Output:
enabled   → Service starts at boot
disabled  → Service does NOT start at boot
masked    → Service is blocked (cannot be started)
static    → Service cannot be enabled
```

**Real-World Examples:**

**Example 1: Web server should always run**
```bash
# Enable nginx to start at boot
sudo systemctl enable nginx

# Verify it's enabled
systemctl is-enabled nginx
# Output: enabled

# On next reboot, nginx will start automatically
```

**Example 2: Database only used sometimes**
```bash
# Disable mysql from starting at boot
sudo systemctl disable mysql

# Verify it's disabled
systemctl is-enabled mysql
# Output: disabled

# Now mysql won't start at boot
# You can start it manually when needed
sudo systemctl start mysql
```

**Example 3: SSH should always be available**
```bash
# SSH is critical for remote access
# Make sure it's enabled
sudo systemctl enable ssh

# Verify
systemctl is-enabled ssh
systemctl status ssh
```

### **Use**
- **Always-On Services**: Web servers, databases, SSH
- **On-Demand Services**: Services used occasionally
- **Server Setup**: Configure which services start at boot
- **Performance**: Disable unnecessary services
- **Security**: Only enable services you need

### **Command**
```bash
# Enable service (start at boot)
sudo systemctl enable nginx
sudo systemctl enable apache2
sudo systemctl enable ssh

# Disable service (don't start at boot)
sudo systemctl disable mysql
sudo systemctl disable cups

# Enable and start now
sudo systemctl enable --now nginx

# Disable and stop now
sudo systemctl disable --now mysql

# Check if enabled
systemctl is-enabled nginx
systemctl is-enabled mysql

# View all enabled services
systemctl list-unit-files --type=service --state=enabled

# View all disabled services
systemctl list-unit-files --type=service --state=disabled
```

---

## Q6. How to View Service Logs?

### **Definition**
Commands to see what a service is doing, any errors, and important messages.

### **Explain (Detail for Beginners)**

**What are Service Logs?**
```
Service logs are like a diary of what the service did:
- When it started
- When it stopped
- Any errors that happened
- Important events
- Requests it handled

Why check logs?
- Service not working - see why
- Debugging problems
- See what requests service received
- Find errors and warnings
- Audit and security
```

**Viewing Service Logs with journalctl:**
```bash
# Basic log view
sudo journalctl -u nginx

# Shows recent logs for nginx service
# Each line is a log entry with timestamp
```

**Understanding Log Output:**
```bash
$ sudo journalctl -u nginx
Jan 15 10:00:00 server systemd[1]: Starting A high performance web server...
Jan 15 10:00:00 server nginx[1234]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Jan 15 10:00:00 server systemd[1]: Started A high performance web server.
Jan 15 10:00:01 server nginx[1235]: 192.168.1.100 - - [15/Jan/2024:10:00:01 +0000] "GET / HTTP/1.1" 200
        │        │      │        │
        │        │      │        └─ Log message
        │        │      └─────────── Process (nginx with PID 1235)
        │        └─────────────────── Hostname (server name)
        └─────────────────────────── Timestamp

Key parts:
Timestamp  → When the log entry was created
Hostname   → Which server generated the log
Process    → Which service/process
Message    → What happened
```

**Common journalctl Options:**
```bash
# View service logs
sudo journalctl -u nginx           # nginx service logs
sudo journalctl -u sshd            # SSH service logs
sudo journalctl -u mysql           # MySQL service logs

# View recent logs (last 50 entries)
sudo journalctl -u nginx -n 50

# Follow logs in real-time (like tail -f)
sudo journalctl -u nginx -f
# Keeps showing new logs as they appear
# Press Ctrl+C to stop

# View logs from current boot only
sudo journalctl -u nginx -b

# View logs from last boot
sudo journalctl -u nginx -b -1

# View logs with timestamps in human-readable format
sudo journalctl -u nginx --since today
sudo journalctl -u nginx --since "1 hour ago"
sudo journalctl -u nginx --since yesterday
```

**Finding Errors in Logs:**
```bash
# Show only error messages
sudo journalctl -u nginx -p err
sudo journalctl -u nginx -p err -b   # Errors from current boot

# Show warnings and errors
sudo journalctl -u nginx -p warning
sudo journalctl -u nginx -p warning..err

# Search for specific text in logs
sudo journalctl -u nginx | grep error
sudo journalctl -u nginx | grep "failed"
```

### **Use**
- **Troubleshooting**: Find why service isn't working
- **Debugging**: See detailed error messages
- **Monitoring**: Watch service activity
- **Audit**: Track service usage
- **Security**: Detect suspicious activity

### **Command**
```bash
# View service logs
sudo journalctl -u nginx
sudo journalctl -u apache2
sudo journalctl -u sshd

# Follow logs in real-time
sudo journalctl -u nginx -f

# View recent logs
sudo journalctl -u nginx -n 100

# View logs from current boot
sudo journalctl -u nginx -b

# View logs from specific time
sudo journalctl -u nginx --since today
sudo journalctl -u nginx --since "1 hour ago"

# Find errors
sudo journalctl -u nginx -p err
sudo journalctl -u nginx | grep error

# Search logs
sudo journalctl -u nginx | grep "connection"
sudo journalctl -u nginx | grep "192.168.1.100"

# Continuous monitoring
sudo journalctl -u nginx -f
```

---

## Q7. What is systemd (Basic)?

### **Definition**
systemd is the init system and service manager for most modern Linux distributions.

### **Explain (Detail for Beginners)**

**What is systemd?**
```
systemd is the first process that starts when Linux boots.

Think of systemd as the manager of your Linux system:
- Starts all other services
- Manages services while system is running
- Stops services when system shuts down
- Controls the boot process

Without systemd:
- Services wouldn't start automatically
- System wouldn't boot properly
- No central service management
```

**What systemd Does:**
```
1. Boot Process:
   - systemd starts first (PID 1)
   - It reads configuration files
   - It starts services in the right order
   - System becomes ready for use

2. Service Management:
   - Starts services
   - Stops services
   - Restarts services if they crash
   - Manages service dependencies

3. System Control:
   - Shuts down the system
   - Reboots the system
   - Manages system resources
```

**systemd Components:**
```
systemctl    → Command to control systemd and services
journalctl   → Command to view logs
systemd      → The init system itself
unit files   → Configuration files for services

Unit file types:
.service  → Services (nginx, mysql, sshd)
.socket   → Network sockets
.timer    → Scheduled tasks
.mount    → Filesystem mounting
```

**How systemd Starts Services:**
```
Boot sequence:
1. Computer turns on
2. Linux kernel starts
3. systemd starts (becomes PID 1)
4. systemd reads /etc/systemd/system/
5. systemd finds enabled services
6. systemd starts services in order
7. System is ready

Example order:
1. systemd (PID 1)
2. Network service
3. SSH server
4. Web server
5. Database server
```

**Checking systemd Status:**
```bash
# Check systemd version
systemctl --version

# Check if systemd is running (it's always running as PID 1)
ps aux | grep systemd
# You'll see systemd as PID 1

# View all systemd units
systemctl list-units

# View failed units
systemctl --failed
```

### **Use**
- **System Boot**: Starts system in correct order
- **Service Management**: Controls all services
- **Log Management**: Collects system logs
- **System Control**: Reboots, shuts down system

### **Command**
```bash
# Check systemd version
systemctl --version

# View system state
systemctl

# View all running services
systemctl list-units --type=service --state=running

# View all failed services
systemctl --failed

# Reboot system
sudo systemctl reboot

# Shutdown system
sudo systemctl poweroff

# Check system boot time
systemd-analyze
systemd-analyze time
```

---

## Q8. What is the Linux Boot Process (Basic)?

### **Definition**
The sequence of events that happen from pressing power button to login screen.

### **Explain (Detail for Beginners)**

**Simple Boot Process:**
```
1. Power On
   └─ Computer gets electricity

2. BIOS/UEFI
   └─ Hardware check (RAM, CPU, disk)
   └─ Finds boot device (disk with Linux)

3. Bootloader (GRUB)
   └─ Loads Linux kernel into memory
   └─ Starts kernel

4. Linux Kernel
   └─ Initializes hardware
   └─ Mounts root filesystem
   └─ Starts systemd

5. systemd
   └─ Becomes PID 1 (first process)
   └─ Starts services
   └─ System is ready for use

6. Login Screen
   └─ You can now login
```

**Each Step Explained:**
```
Step 1: BIOS/UEFI (Basic Input/Output System)
- First thing that runs when you press power
- Checks if hardware is working
- RAM, CPU, disk, keyboard, mouse
- If everything OK → continues
- If something wrong → shows error

Step 2: Bootloader (GRUB)
- Small program that loads Linux
- Shows menu if multiple OS installed
- You can select which OS to boot
- Loads Linux kernel into memory

Step 3: Linux Kernel
- Core of Linux operating system
- Detects and initializes hardware
- Sets up memory
- Mounts root filesystem
- Starts systemd

Step 4: systemd
- First process (PID 1)
- Parent of all other processes
- Starts all services
- Manages system while running

Step 5: System Ready
- All services running
- Login screen appears
- You can now use the system
```

**Understanding PID 1:**
```
PID (Process ID) 1 is very special:
- It's the first process started by kernel
- It's systemd on most modern Linux
- It's parent of all other processes
- If PID 1 stops, system crashes or reboots

View PID 1:
ps aux | grep systemd
# You'll see systemd with PID 1
```

**Boot Time Analysis:**
```bash
# See how long boot took
systemd-analyze

# See time for each service
systemd-analyze blame

# See boot timeline
systemd-analyze critical-chain
```

### **Use**
- **Understanding System**: Know how Linux starts
- **Troubleshooting**: Fix boot problems
- **Performance**: Identify slow boot services
- **Configuration**: Customize boot process

### **Command**
```bash
# View boot time
systemd-analyze

# See which services took longest to start
systemd-analyze blame

# View critical boot path
systemd-analyze critical-chain

# View boot messages
sudo dmesg              # Kernel messages
sudo journalctl -b       # Current boot logs

# View failed services during boot
systemctl --failed
sudo journalctl -b -p err
```

---

## Q9. How to Troubleshoot a Service That Won't Start?

### **Definition**
Basic steps to identify and fix common service startup problems.

### **Explain (Detail for Beginners)**

**Service Won't Start - Common Issues:**
```
Common reasons why service won't start:
1. Port already in use (another service using the port)
2. Configuration file has error
3. Permission problem
4. Required dependency not installed
5. Service already running
```

**Troubleshooting Steps:**
```
Step 1: Check service status
$ sudo systemctl status nginx
# Look at "Active" line
# If it says "failed", there's a problem
# Look for error messages in output

Step 2: Check service logs
$ sudo journalctl -u nginx -n 50
# Look for error messages
# Look for "failed", "error", "cannot"
# This tells you what went wrong

Step 3: Check configuration file
$ sudo nginx -t
# This tests nginx configuration
# If it says "syntax is OK", config is fine
# If it shows error, fix the config file

Step 4: Check if port is in use
$ sudo ss -tlnp | grep :80
# If another process is using port 80
# nginx can't start
# Stop the other process or use different port

Step 5: Check file permissions
$ ls -l /var/log/nginx
# nginx must be able to write to log directory
# If permissions are wrong, fix them
```

**Common Errors and Fixes:**

**Error 1: Port already in use**
```bash
# Check what's using the port
sudo ss -tlnp | grep :80

# If another process is using it
# Option 1: Stop the other process
sudo kill <PID>

# Option 2: Change nginx to use different port
# Edit /etc/nginx/nginx.conf and change port
# Then restart nginx
```

**Error 2: Configuration error**
```bash
# Test configuration
sudo nginx -t

# If error shown, edit config
sudo nano /etc/nginx/nginx.conf

# Fix the error
# Then test again
sudo nginx -t

# If "syntax is OK", restart service
sudo systemctl restart nginx
```

**Error 3: Permission denied**
```bash
# Check log directory permissions
ls -ld /var/log/nginx

# If nginx can't write, fix permissions
sudo chown www-data:www-data /var/log/nginx
sudo chmod 755 /var/log/nginx

# Restart service
sudo systemctl restart nginx
```

**Error 4: Service already running**
```bash
# Check if service is running
systemctl status nginx

# If it says "active (running)", it's already running
# Don't try to start again
# Use restart instead
sudo systemctl restart nginx
```

### **Use**
- **Diagnose Problems**: Find why service won't start
- **Fix Errors**: Resolve common service issues
- **Prevent Issues**: Understand common mistakes
- **System Administration**: Keep services running smoothly

### **Command**
```bash
# Troubleshooting workflow
# Step 1: Check status
sudo systemctl status nginx

# Step 2: Check logs
sudo journalctl -u nginx -n 50

# Step 3: Test configuration
sudo nginx -t
sudo apache2ctl configtest

# Step 4: Check port usage
sudo ss -tlnp | grep :80

# Step 5: Check permissions
ls -l /var/log/nginx
ls -l /var/www/html

# Step 6: Restart service
sudo systemctl restart nginx

# Step 7: Verify it's working
systemctl status nginx
curl http://localhost
```

---

## Q10. Common Services You Should Know (nginx, apache, ssh, mysql)

### **Definition**
Basic information about commonly used Linux services and what they do.

### **Explain (Detail for Beginners)**

**nginx (Web Server)**
```
What is it?
- Web server software
- Serves web pages and files
- Handles HTTP/HTTPS requests
- Very fast and efficient

What it does:
- Serves websites
- Handles web traffic
- Can work as reverse proxy
- Can load balance

Port: 80 (HTTP), 443 (HTTPS)
Service name: nginx

Basic commands:
sudo systemctl start nginx      # Start web server
sudo systemctl stop nginx       # Stop web server
sudo systemctl restart nginx    # Restart
sudo systemctl status nginx     # Check status

Use case:
- Hosting websites
- Running web applications
- API server
```

**Apache (Web Server)**
```
What is it?
- Another popular web server
- Older than nginx but still widely used
- Very flexible and customizable

What it does:
- Serves websites
- Handles web traffic
- Many modules for extra features

Port: 80 (HTTP), 443 (HTTPS)
Service name: apache2 (Debian/Ubuntu) or httpd (RHEL/CentOS)

Basic commands:
sudo systemctl start apache2     # Start web server
sudo systemctl stop apache2      # Stop web server
sudo systemctl restart apache2   # Restart
sudo systemctl status apache2    # Check status

Use case:
- Hosting websites
- Running web applications
- Shared hosting environments
```

**SSH Server (sshd)**
```
What is it?
- Secure Shell server
- Allows remote login to system
- Encrypted connection (secure)

What it does:
- Accepts remote connections
- Authenticates users
- Provides command-line access
- Secure file transfer

Port: 22
Service name: sshd

Basic commands:
sudo systemctl start sshd        # Start SSH server
sudo systemctl stop sshd         # Stop SSH server
sudo systemctl restart sshd      # Restart
sudo systemctl status sshd       # Check status

Important:
- SSH is critical for remote access
- Usually enabled by default
- Keep SSH running on servers

Use case:
- Remote server management
- Secure remote access
- Remote command execution
```

**MySQL (Database)**
```
What is it?
- Database management system
- Stores and retrieves data
- Very popular for web applications

What it does:
- Stores data in tables
- Runs SQL queries
- Manages databases
- Handles multiple users

Port: 3306
Service name: mysql

Basic commands:
sudo systemctl start mysql       # Start database
sudo systemctl stop mysql        # Stop database
sudo systemctl restart mysql     # Restart
sudo systemctl status mysql      # Check status

Use case:
- Storing application data
- Website backend database
- User data management
```

**PostgreSQL (Database)**
```
What is it?
- Another popular database
- More advanced features than MySQL
- Very reliable

What it does:
- Stores and retrieves data
- Runs SQL queries
- Manages databases

Port: 5432
Service name: postgresql

Basic commands:
sudo systemctl start postgresql  # Start database
sudo systemctl stop postgresql   # Stop database
sudo systemctl restart postgresql # Restart
sudo systemctl status postgresql  # Check status

Use case:
- Storing application data
- Complex database needs
- Enterprise applications
```

**Cron (Scheduled Tasks)**
```
What is it?
- Job scheduler
- Runs tasks at scheduled times
- Like an alarm clock for tasks

What it does:
- Runs commands at specific times
- Executes scripts periodically
- Automates recurring tasks

Service name: cron

Basic commands:
sudo systemctl start cron        # Start cron
sudo systemctl stop cron         # Stop cron
sudo systemctl restart cron      # Restart
sudo systemctl status cron       # Check status

Use case:
- Daily backups
- Periodic cleanup
- Scheduled reports
- Automated maintenance
```

### **Use**
- **Web Services**: nginx, Apache for hosting websites
- **Remote Access**: SSH for server management
- **Data Storage**: MySQL, PostgreSQL for applications
- **Automation**: Cron for scheduled tasks
- **System Administration**: Understanding common services

### **Command**
```bash
# Web servers
sudo systemctl start nginx
sudo systemctl start apache2
sudo systemctl status nginx
sudo systemctl status apache2

# SSH (usually enabled by default)
sudo systemctl status sshd
sudo systemctl restart sshd

# Databases
sudo systemctl start mysql
sudo systemctl start postgresql
sudo systemctl status mysql
sudo systemctl status postgresql

# Cron
sudo systemctl status cron
sudo systemctl restart cron

# View all running services
systemctl list-units --type=service --state=running
```

---

## 📚 Quick Reference - Module 6 Commands

### Service Management
```bash
systemctl start service        # Start service
systemctl stop service         # Stop service
systemctl restart service      # Restart service
systemctl reload service       # Reload config
systemctl status service       # Check status
```

### Boot Control
```bash
systemctl enable service       # Start at boot
systemctl disable service      # Don't start at boot
systemctl is-enabled service   # Check if enabled
systemctl is-active service    # Check if running
```

### Viewing Services
```bash
systemctl list-units --type=service               # All services
systemctl list-units --type=service --state=running  # Running only
systemctl list-unit-files --type=service           # Installed services
systemctl --failed                                 # Failed services
```

### Logs
```bash
journalctl -u service        # Service logs
journalctl -u service -f     # Follow logs
journalctl -u service -n 50  # Recent 50 lines
journalctl -u service -p err # Errors only
```

### System
```bash
systemctl                   # System state
systemd-analyze             # Boot time
systemctl reboot            # Reboot
systemctl poweroff          # Shutdown
```

---

**Next:** Module 7 - Security, Users & Access
