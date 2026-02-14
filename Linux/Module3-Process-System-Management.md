# 🟧 MODULE 3: Process & System Management

> Simplified interview questions and answers for Process & System Management

---

## Q1. What is a Process?

### **Definition**
A running instance of a program.

### **Explain**
- Each program can have multiple processes
- Has unique PID (Process ID)
- Has own memory space
- Managed by kernel

### **Use**
- Run applications
- System services
- Background jobs

### **Command**
```bash
# View all processes
ps aux

# View process tree
pstree

# View specific process
ps -p PID
```

---

## Q2. Difference Between Process and Thread.

### **Definition**
- **Process**: Independent program with own memory
- **Thread**: Lightweight execution unit within a process

### **Explain**
| Aspect | Process | Thread |
|--------|---------|---------|
| Memory | Own memory space | Shares parent memory |
| PID | Has own PID | Shares parent PID |
| Communication | Slower (IPC) | Faster (shared memory) |
| Creation | Heavier | Lighter |

### **Use**
- **Process**: Separate applications
- **Thread**: Parallel tasks in same application

### **Command**
```bash
# View threads (ps with -L)
ps -eLf | grep process_name

# View thread count
ps -o nlwp pid
```

---

## Q3. Explain ps, top, and htop Commands.

### **Definition**
Commands to monitor running processes.

### **Explain**
- **ps**: Snapshot of processes (static)
- **top**: Real-time process viewer (interactive)
- **htop**: Enhanced top with colors and mouse

### **Use**
- **ps**: Quick process check
- **top**: Monitor system resources
- **htop**: Better visual process monitoring

### **Command**
```bash
# ps - view all processes
ps aux

# ps - filter by name
ps aux | grep nginx

# ps - view process tree
ps -eLf

# top - real-time monitoring
top

# top - sort by CPU (press P)
top

# top - sort by memory (press M)
top

# htop - interactive viewer
htop

# htop - kill process (press F9)
htop
```

---

## Q4. What is a Zombie Process and How to Handle It?

### **Definition**
Process that completed but still shows in process table.

### **Explain**
- Child finished, parent didn't read exit status
- Can't be killed (already dead)
- Takes minimal resources
- Accumulates if many

### **Use**
- Identify zombie processes
- Clean up zombie processes
- Troubleshooting hung applications

### **Command**
```bash
# Find zombie processes
ps aux | grep Z

# View zombie count
ps aux | awk '{print $8}' | sort | uniq -c

# Kill parent (removes zombie)
kill -9 PARENT_PID

# Restart parent service
sudo systemctl restart service_name
```

---

## Q5. Difference Between kill and kill -9.

### **Definition**
- **kill**: Gracefully terminate process
- **kill -9**: Forcefully terminate process

### **Explain**
| Signal | kill | kill -9 |
|--------|------|---------|
| Type | SIGTERM (15) | SIGKILL (9) |
| Graceful | Yes - allows cleanup | No - immediate |
| Cleanup | Yes | No |
| Use | Normal shutdown | Force kill |

### **Use**
- **kill**: Clean shutdown, allows save data
- **kill -9**: Hung process, won't respond

### **Command**
```bash
# Gracefully kill process
kill PID

# Forcefully kill process
kill -9 PID

# Kill by name
pkill process_name

# Kill by name forcefully
pkill -9 process_name

# Verify process is killed
ps -p PID
```

---

## Q6. How to Run Process in Background (&, nohup, screen)?

### **Definition**
Commands to run processes without blocking terminal.

### **Explain**
- **&**: Run in background
- **nohup**: Keep running after logout
- **screen**: Persistent terminal sessions

### **Use**
- **&**: Quick background command
- **nohup**: Long-running jobs
- **screen**: Interactive background sessions

### **Command**
```bash
# Run in background
command &

# Run with nohup (keeps after logout)
nohup command &

# Start screen session
screen -S session_name

# Detach from screen (Ctrl+A, then D)
# screen -d

# List screen sessions
screen -ls

# Reattach to screen
screen -r session_name

# Kill screen session
screen -X -S session_name quit
```

---

## Q7. What is Purpose of nice and renice?

### **Definition**
Commands to change process priority.

### **Explain**
- **nice**: Start process with priority
- **renice**: Change running process priority
- Range: -20 (highest) to 19 (lowest)
- Default: 0

### **Use**
- **nice**: Start low-priority tasks
- **renice**: Adjust running task priority
- High CPU tasks get higher priority

### **Command**
```bash
# Start with low priority
nice -n 10 command

# Start with high priority (requires root)
sudo nice -n -5 command

# View process priority
ps -o pid,ni,comm

# Change running process priority
renice priority PID

# Make process lower priority
renice +5 PID

# Make process higher priority (requires root)
sudo renice -5 PID
```

---

## Q8. How to Check Which Process Consumes Most CPU/Memory?

### **Definition**
Commands to find resource-intensive processes.

### **Explain**
- **top/htop**: Interactive monitoring
- **ps**: Snapshot with sorting
- **pidstat**: Detailed process stats

### **Use**
- Identify performance bottlenecks
- Kill hung processes
- Optimize system resources

### **Command**
```bash
# View sorted by CPU (top)
top -o %CPU

# View sorted by memory (top)
top -o %MEM

# htop - interactive view
htop

# ps - top CPU consumers
ps aux --sort=-%CPU | head -10

# ps - top memory consumers
ps aux --sort=-%MEM | head -10

# View specific process usage
ps -p PID -o %CPU,%MEM,cmd
```

---

## Q9. What is Load Average? Interpret uptime Output.

### **Definition**
Average number of processes waiting for CPU over time periods.

### **Explain**
```
load average: 1.25, 0.98, 0.85
            │     │     └─ 15 min
            │     └───────── 5 min
            └──────────────── 1 min
```
- **1 min**: Recent load
- **5 min**: Medium load
- **15 min**: Long-term load
- Rule of thumb: Load < CPU cores = good

### **Use**
- Monitor system performance
- Identify overloading
- Plan capacity

### **Command**
```bash
# View load average
uptime

# View with users
uptime -p

# View running time
uptime -s

# View number of CPU cores
nproc

# Check if load is high
uptime
# If load > CPU cores, investigate
```

---

## Q10. Explain Swap Memory and When It's Used.

### **Definition**
Disk space used as extension of RAM.

### **Explain**
- Used when RAM is full
- Slower than physical RAM
- Prevents system crash from OOM
- Can be disabled (not recommended)

### **Use**
- Handle memory pressure
- Allow more applications than RAM size
- Emergency memory overflow

### **Command**
```bash
# View swap usage
free -h

# View swap details
swapon -s

# View swap in megabytes
free -m

# Disable swap (temporary)
sudo swapoff -a

# Enable swap
sudo swapon -a

# View swap file location
cat /proc/swaps
```

---

## Q11. Check Running Processes.

### **Definition**
View all currently executing processes.

### **Explain**
- **ps**: Basic process list
- **ps aux**: All processes with details
- **ps -ef**: Full process listing
- Can filter by user, name, PID

### **Use**
- Monitor system activity
- Troubleshooting
- Kill specific processes

### **Command**
```bash
# All processes
ps aux

# All processes (full format)
ps -ef

# View user's processes
ps -u username

# View process by name
ps aux | grep nginx

# View process tree
pstree

# View specific process
ps -p PID

# All processes with PID only
ps -e
```

---

## Q12. Terminate Process.

### **Definition**
Stop a running process.

### **Explain**
- **kill**: Graceful termination
- **kill -9**: Force termination
- **pkill**: Kill by name
- **killall**: Kill all instances

### **Use**
- Stop hung applications
- Restart services
- Free system resources

### **Command**
```bash
# Graceful kill
kill PID

# Force kill
kill -9 PID

# Kill by name
pkill process_name

# Kill all instances
killall process_name

# Force kill by name
pkill -9 process_name

# Verify process killed
ps -p PID
```

---

## 📚 Quick Reference - Module 3 Commands

### Process Monitoring
```bash
ps aux                    # View all processes
ps -p PID                # View specific process
top                       # Interactive monitoring
htop                      # Enhanced viewer
pstree                    # Process tree
```

### Process Management
```bash
kill PID                  # Graceful kill
kill -9 PID               # Force kill
pkill name               # Kill by name
killall name             # Kill all instances
```

### Background Processes
```bash
command &                 # Run in background
nohup command &          # Keep after logout
screen -S name            # Start screen
screen -ls                 # List screens
screen -r name             # Reattach screen
```

### Process Priority
```bash
nice -n 10 command       # Low priority
renice +5 PID           # Lower priority
renice -5 PID           # Higher priority (root)
```

### System Resources
```bash
uptime                   # Load average
free -h                  # Memory/swap
top -o %CPU              # Sort by CPU
top -o %MEM              # Sort by memory
```

---

**Next:** Module 4 - Disk, Filesystem & Storage
