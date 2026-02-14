# Module 9: Cron Jobs & Scheduling

## Table of Contents
1. [What is Cron?](#1-what-is-cron)
2. [What is Crontab and How to Use It?](#2-what-is-crontab-and-how-to-use-it)
3. [Understanding Cron Syntax](#3-understanding-cron-syntax)
4. [Cron Special Characters (*, /, -, ,)](#4-cron-special-characters--)
5. [How to Schedule Jobs with Cron](#5-how-to-schedule-jobs-with-cron)
6. [How to View and List Cron Jobs](#6-how-to-view-and-list-cron-jobs)
7. [How to Edit Crontab](#7-how-to-edit-crontab)
8. [Cron Job Examples](#8-cron-job-examples)
9. [How to Debug Cron Jobs](#9-how-to-debug-cron-jobs)
10. [What are Cron Directories (/etc/cron.*)](#10-what-are-cron-directories-etccron)

---

## 1. What is Cron?

### Definition
Cron is a time-based job scheduler in Unix-like operating systems that enables users to schedule jobs (commands or scripts) to run at specific times or intervals.

### Explanation (For Beginners)
- Think of **Cron** as your **automatic task manager** or **personal assistant**
- It runs tasks automatically at scheduled times, even when you're not logged in
- Similar to setting an alarm clock on your phone, but for computer tasks

**How Cron works:**
- **Cron daemon (crond)** - The background process that runs continuously
- **Crontab** - The configuration file that lists all scheduled jobs
- **Cron jobs** - The individual tasks scheduled to run

**Why use Cron?**
- **Automation** - Run backups automatically at specific times
- **Maintenance** - Clean up temporary files regularly
- **Monitoring** - Check system health periodically
- **Reports** - Generate daily/weekly reports
- **Updates** - Schedule system updates during off-peak hours

**Common uses of Cron:**
- Daily database backups at midnight
- Weekly system cleanup on Sundays
- Hourly log rotation
- Monthly report generation
- Checking disk space every hour
- Sending automated emails

**Cron vs Other Schedulers:**
- **Cron** - Built-in, time-based (minute/hour/day/month/weekday)
- **Systemd timers** - More modern, event-triggered
- **At** - One-time job scheduling
- **Anacron** - For systems that aren't always running

### Use
- Schedule automated backups
- Run maintenance scripts automatically
- Generate periodic reports
- Monitor system resources
- Clean up log files
- Update software automatically
- Send automated notifications

### Command
```bash
# Check if cron service is running
systemctl status cron

# Start cron service (if not running)
sudo systemctl start cron

# Enable cron to start at boot
sudo systemctl enable cron

# Check cron daemon process
ps aux | grep cron

# Check cron logs
sudo tail -f /var/log/syslog | grep CRON

# View system-wide cron jobs
sudo cat /etc/crontab

# View current user's crontab
crontab -l

# Edit current user's crontab
crontab -e

# Remove current user's crontab
crontab -r
```

---

## 2. What is Crontab and How to Use It?

### Definition
Crontab (cron table) is a configuration file that contains cron jobs - a list of commands with their execution schedules. Each user can have their own crontab, and there's also a system-wide crontab.

### Explanation (For Beginners)
- **Crontab** = **Cron** (scheduler) + **Table** (list of jobs)
- It's like a **schedule book** where you write down all your tasks with their times
- Each line in the crontab file represents one scheduled job

**Types of Crontabs:**
1. **User crontab** - Personal cron jobs for each user
   - Location: `/var/spool/cron/crontabs/username`
   - Commands run as that specific user
   - Managed with `crontab` command

2. **System crontab** - System-wide cron jobs
   - Location: `/etc/crontab`
   - Has an extra field for username
   - Usually run as root

**How to access crontab:**
- `crontab -l` - List (view) current crontab
- `crontab -e` - Edit crontab (opens in default editor)
- `crontab -r` - Remove (delete) crontab
- `crontab -u username -l` - View another user's crontab (needs root)

**Editor for crontab:**
- Uses your default editor (usually vi/vim or nano)
- Change editor with: `export EDITOR=nano`
- Or select when first editing crontab

**Safety features:**
- Validates syntax before saving
- Backs up old crontab automatically
- Shows syntax errors if any

### Use
- Schedule personal tasks and scripts
- Automate user-specific backups
- Set up recurring maintenance jobs
- Schedule reports and notifications
- Manage personal cron jobs independently

### Command
```bash
# View your crontab
crontab -l

# Edit your crontab (creates if doesn't exist)
crontab -e

# Remove all cron jobs (use with caution!)
crontab -r

# Create crontab from a file
crontab myjobs.txt

# View another user's crontab (requires sudo)
sudo crontab -u john -l

# Edit another user's crontab (requires sudo)
sudo crontab -u john -e

# Set default editor to nano
export EDITOR=nano
crontab -e

# Set default editor to vim
export EDITOR=vim
crontab -e

# Backup current crontab to a file
crontab -l > crontab_backup.txt

# Restore crontab from backup file
crontab crontab_backup.txt

# Check which editor will be used
echo $EDITOR
```

---

## 3. Understanding Cron Syntax

### Definition
Cron syntax defines when and how often a cron job should run using five time fields followed by the command to execute.

### Explanation (For Beginners)
**Cron syntax format:**
```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, 0 or 7 = Sunday)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

**Each field explained:**
1. **Minute (0-59)** - At which minute of the hour to run
2. **Hour (0-23)** - At which hour of the day to run (24-hour format)
3. **Day of month (1-31)** - On which day of the month to run
4. **Month (1-12)** - In which month to run
5. **Day of week (0-7)** - On which day of the week to run

**Common examples:**
```
0 * * * * command        # Every hour at minute 0
0 0 * * * command        # Every day at midnight
0 2 * * * command        # Every day at 2 AM
*/5 * * * * command      # Every 5 minutes
0 9 * * 1 command        # Every Monday at 9 AM
0 0 1 * * command        # First day of every month at midnight
```

**Time format notes:**
- 24-hour format: 0-23 (0 = midnight, 12 = noon, 23 = 11 PM)
- Day of week: 0 or 7 = Sunday, 1 = Monday, 6 = Saturday
- All fields use local server time

### Use
- Schedule tasks at precise times
- Set recurring intervals
- Create complex schedules
- Combine multiple conditions

### Command
```bash
# Cron syntax examples (add these to crontab with crontab -e)

# Run every minute
* * * * * echo "Every minute" >> /tmp/cron.log

# Run every 5 minutes
*/5 * * * * echo "Every 5 minutes" >> /tmp/cron.log

# Run every hour at minute 0
0 * * * * echo "Every hour" >> /tmp/cron.log

# Run every day at 2:30 AM
30 2 * * * echo "Daily at 2:30 AM" >> /tmp/cron.log

# Run every Monday at 9 AM
0 9 * * 1 echo "Monday 9 AM" >> /tmp/cron.log

# Run first day of every month at midnight
0 0 1 * * echo "First of month" >> /tmp/cron.log

# Run every January 1st at midnight
0 0 1 1 * echo "New Year" >> /tmp/cron.log

# Run weekdays at 6 PM
0 18 * * 1-5 echo "Weekday 6 PM" >> /tmp/cron.log

# Run every Sunday at midnight
0 0 * * 0 echo "Sunday midnight" >> /tmp/cron.log

# Run at 3:15 PM on Fridays
15 15 * * 5 echo "Friday 3:15 PM" >> /tmp/cron.log
```

---

## 4. Cron Special Characters (*, /, -, ,)

### Definition
Cron uses special characters to create flexible and complex scheduling patterns: asterisk (*), forward slash (/), hyphen (-), and comma (,).

### Explanation (For Beginners)
**1. Asterisk (*) - Wildcard (All/Every)**
- Means "every possible value" in that field
- Most commonly used character

```
* * * * * command     # Every minute of every hour of every day
0 * * * * command     # Every hour (at minute 0)
* 0 * * * command     # Every minute of hour 0 (midnight to 1 AM)
```

**2. Forward Slash (/) - Step/Interval**
- Specifies how often to repeat
- Format: `*/n` = "every n"

```
*/5 * * * * command   # Every 5 minutes
*/10 * * * * command  # Every 10 minutes
*/15 * * * * command  # Every 15 minutes
0 */2 * * * command   # Every 2 hours
0 */6 * * * command   # Every 6 hours
```

**3. Hyphen (-) - Range**
- Specifies a range of values

```
0 9-17 * * * command  # Every hour from 9 AM to 5 PM
* * 1-15 * * command  # Days 1-15 of every month
0 * * * 1-5 command   # Monday to Friday
```

**4. Comma (,) - List**
- Lists specific values

```
0 9,18 * * * command  # At 9 AM and 6 PM every day
0 0 1,15 * * command  # 1st and 15th of every month
0 * * * 1,3,5 command # Monday, Wednesday, Friday
```

**Combining special characters:**

```
# Every 5 minutes during business hours (9 AM - 5 PM)
*/5 9-17 * * * command

# Every 2 hours from 8 AM to 8 PM
0 8-20/2 * * * command

# First 10 days of month, every hour
0 * 1-10 * * command

# Weekdays at 9 AM, 12 PM, and 5 PM
0 9,12,17 * * 1-5 command
```

### Use
- Create precise time schedules
- Combine multiple conditions
- Set recurring intervals
- Define business hours schedules
- Create complex timing patterns

### Command
```bash
# Add these examples to crontab (crontab -e)

# Every minute (using *)
* * * * * date >> /tmp/every_minute.log

# Every 10 minutes (using /)
*/10 * * * * date >> /tmp/every_10min.log

# Every 30 minutes
*/30 * * * * date >> /tmp/every_30min.log

# Business hours: 9 AM to 5 PM every hour
0 9-17 * * * date >> /tmp/business_hours.log

# Every 2 hours
0 */2 * * * date >> /tmp/every_2hours.log

# Every 6 hours
0 */6 * * * date >> /tmp/every_6hours.log

# Monday to Friday at 8 AM
0 8 * * 1-5 date >> /tmp/weekdays.log

# At 9 AM, 1 PM, and 5 PM on weekdays
0 9,13,17 * * 1-5 date >> /tmp/meeting_times.log

# First and 15th of every month at midnight
0 0 1,15 * * date >> /tmp/payday.log

# Every 5 minutes during work hours (9-5)
*/5 9-17 * * 1-5 date >> /tmp/work_frequent.log

# January and July at noon
0 12 1 1,7 * date >> /tmp/special_dates.log

# Every Sunday at 3 AM for maintenance
0 3 * * 0 date >> /tmp/sunday_maintenance.log
```

---

## 5. How to Schedule Jobs with Cron

### Definition
Scheduling jobs with cron involves adding entries to the crontab file with the proper time syntax and command to execute.

### Explanation (For Beginners)
**Basic process:**
1. Open crontab: `crontab -e`
2. Add your job on a new line
3. Save and exit
4. Cron automatically picks up the changes

**Crontab entry format:**
```
min hour day month weekday command
```

**Important rules:**
- Each job on a separate line
- Lines starting with `#` are comments (ignored)
- Blank lines are allowed
- Must use absolute paths for commands and files

**Best practices for cron jobs:**
1. **Use absolute paths**
   - Instead of: `python script.py`
   - Use: `/usr/bin/python /home/user/script.py`

2. **Redirect output**
   - Log output to a file
   - Capture both stdout and stderr

3. **Set environment variables**
   - Cron runs with minimal environment
   - Set PATH and other variables in crontab

4. **Test manually first**
   - Run command manually before scheduling
   - Make sure it works as expected

5. **Add comments**
   - Document what each job does
   - Include date when job was created

**Types of jobs:**
- Simple commands: `0 * * * * /usr/bin/python /path/to/script.py`
- Shell scripts: `0 2 * * * /home/user/backup.sh`
- With output redirection: `0 * * * * /path/to/script >> /tmp/output.log 2>&1`
- With environment: `PATH=/usr/bin:/bin; 0 * * * * script.py`

### Use
- Schedule backups to run automatically
- Automate system maintenance
- Run periodic reports
- Monitor system health
- Clean up temporary files
- Send scheduled emails
- Update software

### Command
```bash
# Open crontab to add jobs
crontab -e

# Add these example jobs (one per line):

# ===== BACKUP JOBS =====
# Daily backup at 2 AM
0 2 * * * /home/user/scripts/backup.sh >> /var/log/backup.log 2>&1

# Weekly backup on Sunday at 3 AM
0 3 * * 0 /home/user/scripts/weekly_backup.sh >> /var/log/weekly_backup.log 2>&1

# ===== CLEANUP JOBS =====
# Clean temporary files daily at midnight
0 0 * * * rm -rf /tmp/* >> /var/log/cleanup.log 2>&1

# Clean logs older than 7 days every Sunday
0 4 * * 0 find /var/log -name "*.log" -mtime +7 -delete >> /var/log/log_cleanup.log 2>&1

# ===== MONITORING JOBS =====
# Check disk space every hour and send email if > 90%
0 * * * * /home/user/scripts/check_disk_space.sh >> /var/log/disk_monitor.log 2>&1

# Check if service is running every 5 minutes
*/5 * * * * /home/user/scripts/check_service.sh >> /var/log/service_monitor.log 2>&1

# ===== REPORT JOBS =====
# Generate daily report at 8 AM
0 8 * * * /home/user/scripts/daily_report.sh >> /var/log/daily_report.log 2>&1

# Weekly report every Monday at 9 AM
0 9 * * 1 /home/user/scripts/weekly_report.sh >> /var/log/weekly_report.log 2>&1

# ===== MAINTENANCE JOBS =====
# Update system packages every Sunday at 2 AM
0 2 * * 0 apt-get update && apt-get upgrade -y >> /var/log/system_update.log 2>&1

# Restart service every Monday at 5 AM
0 5 * * 1 systemctl restart myservice >> /var/log/service_restart.log 2>&1

# ===== CUSTOM JOBS =====
# Run script every 15 minutes
*/15 * * * * /home/user/scripts/myscript.sh >> /tmp/myscript.log 2>&1

# Business hours job (9 AM - 5 PM weekdays)
0 9-17 * * 1-5 /home/user/scripts/business_task.sh >> /var/log/business_task.log 2>&1

# With environment variables set
PATH=/usr/bin:/bin:/usr/local/bin
HOME=/home/user
0 * * * * python3 $HOME/scripts/task.py >> $HOME/logs/task.log 2>&1

# ===== COMMENTS =====
# This is a comment - cron will ignore this line
# Daily database backup - created by admin on 2024-01-15
```

---

## 6. How to View and List Cron Jobs

### Definition
Viewing cron jobs helps you see what tasks are scheduled, when they run, and verify that your cron entries are correct.

### Explanation (For Beginners)
**Why view cron jobs?**
- Check what jobs are scheduled
- Verify your cron entries are correct
- Debug why jobs aren't running
- Review existing schedules before adding new ones
- Remove unwanted jobs

**Viewing your own crontab:**
```
crontab -l    # List your cron jobs
```

**Viewing other users' crontabs:**
```
sudo crontab -u username -l    # View another user's cron jobs
```

**Viewing system crontab:**
```
sudo cat /etc/crontab    # System-wide cron jobs
```

**Viewing cron directories:**
```
ls /etc/cron.hourly/     # Hourly system jobs
ls /etc/cron.daily/      # Daily system jobs
ls /etc/cron.weekly/     # Weekly system jobs
ls /etc/cron.monthly/    # Monthly system jobs
```

**Viewing cron logs:**
```
sudo tail -f /var/log/syslog | grep CRON    # Real-time cron logs
sudo grep CRON /var/log/syslog               # All cron log entries
```

**Understanding crontab output:**
```
# Example crontab -l output:
# m h  dom mon dow   command
0 2 * * * /home/user/backup.sh
*/5 * * * * /home/user/check.sh
```
- First line is the header showing field meanings
- Each line after is a scheduled job

### Use
- Verify cron job configuration
- Debug scheduling issues
- Document current scheduled tasks
- Review before making changes
- Audit scheduled jobs
- Identify conflicting schedules

### Command
```bash
# View your own crontab
crontab -l

# View your crontab with line numbers
cat -n <(crontab -l)

# Save your crontab to a file
crontab -l > my_crontab.txt

# View saved crontab file
cat my_crontab.txt

# View another user's crontab (requires sudo)
sudo crontab -u john -l
sudo crontab -u root -l

# View system-wide crontab
sudo cat /etc/crontab

# View cron directories (system jobs)
ls -la /etc/cron.hourly/
ls -la /etc/cron.daily/
ls -la /etc/cron.weekly/
ls -la /etc/cron.monthly/

# View contents of cron directories
sudo cat /etc/cron.hourly/*
sudo cat /etc/cron.daily/*

# View cron logs (Ubuntu/Debian)
sudo tail -f /var/log/syslog | grep CRON

# View recent cron entries
sudo grep CRON /var/log/syslog | tail -20

# View all cron logs for a specific date
sudo grep "CRON" /var/log/syslog | grep "Jan 15"

# View cron logs for a specific user
sudo grep CRON /var/log/syslog | grep "username"

# Check if cron is running
ps aux | grep cron
systemctl status cron

# List all users with crontabs (Ubuntu/Debian)
sudo ls /var/spool/cron/crontabs/

# Count how many jobs in your crontab
crontab -l | grep -v "^#" | grep -v "^$" | wc -l

# Search for specific jobs in crontab
crontab -l | grep backup
crontab -l | grep "python"

# Check if a specific job exists
crontab -l | grep "0 2 \* \* \*"
```

---

## 7. How to Edit Crontab

### Definition
Editing crontab allows you to add, modify, or remove scheduled cron jobs using a text editor.

### Explanation (For Beginners)
**Basic editing:**
```
crontab -e    # Edit your crontab
```

**What happens when you edit:**
1. Opens your default text editor (nano, vim, etc.)
2. Shows your current crontab entries
3. You can add, modify, or remove jobs
4. Save and exit
5. Cron validates and installs the new crontab

**First time editing:**
- Cron asks which editor to use
- Common options: nano (easy), vim (powerful)
- Selection becomes your default

**Choosing your editor:**
```bash
export EDITOR=nano    # Set nano as default
crontab -e

export EDITOR=vim     # Set vim as default
crontab -e
```

**Adding a new job:**
1. Open crontab: `crontab -e`
2. Go to the bottom of the file
3. Add your job on a new line:
   ```
   0 2 * * * /path/to/script.sh >> /var/log/script.log 2>&1
   ```
4. Save and exit

**Modifying an existing job:**
1. Open crontab: `crontab -l -e` or just `crontab -e`
2. Find the job you want to change
3. Edit the time or command
4. Save and exit

**Deleting a job:**
1. Open crontab: `crontab -e`
2. Find the job you want to remove
3. Delete the entire line
4. Save and exit

**Removing all jobs:**
```bash
crontab -r    # Remove entire crontab (use with caution!)
```

**Commenting out a job (disable temporarily):**
```
# 0 2 * * * /path/to/script.sh    # Disabled by adding #
```

### Use
- Add new scheduled jobs
- Modify existing schedules
- Remove unwanted jobs
- Disable jobs temporarily
- Fix broken cron entries
- Organize cron jobs with comments

### Command
```bash
# Edit your crontab
crontab -e

# Set nano as editor before editing
export EDITOR=nano
crontab -e

# Set vim as editor before editing
export EDITOR=vim
crontab -e

# Edit crontab and view current entries in one command
crontab -e && crontab -l

# Backup before editing
crontab -l > ~/crontab_backup_$(date +%Y%m%d).txt
crontab -e

# Restore from backup if something goes wrong
crontab ~/crontab_backup_20240115.txt

# Remove all cron jobs (caution!)
crontab -r

# Remove cron jobs with confirmation (safer)
crontab -i

# Create a new crontab from scratch
echo "0 2 * * * /home/user/backup.sh" | crontab -

# Add a job to existing crontab (append)
(crontab -l; echo "0 3 * * * /home/user/cleanup.sh") | crontab -

# Remove a specific job by pattern (use with caution)
crontab -l | grep -v "backup.sh" | crontab -

# Edit another user's crontab (requires sudo)
sudo crontab -u john -e

# Check which editor will be used
echo $EDITOR

# Test crontab syntax without installing
# Create a test file
cat > test_crontab << 'EOF'
0 2 * * * echo "Test" >> /tmp/test.log
EOF
# Check syntax
crontab test_crontab
# Remove test
rm test_crontab
```

---

## 8. Cron Job Examples

### Definition
Real-world cron job examples showing common scheduling scenarios for system administration, backups, monitoring, and maintenance tasks.

### Explanation (For Beginners)
**Example 1: Daily Database Backup**
```bash
# Run at 2 AM every day
0 2 * * * /home/user/scripts/db_backup.sh >> /var/log/db_backup.log 2>&1
```

**Example 2: Weekly Full Backup**
```bash
# Run at 3 AM every Sunday
0 3 * * 0 /home/user/scripts/full_backup.sh >> /var/log/full_backup.log 2>&1
```

**Example 3: Hourly Monitoring**
```bash
# Check disk space every hour on the hour
0 * * * * /home/user/scripts/check_disk.sh >> /var/log/disk_check.log 2>&1
```

**Example 4: Log Cleanup**
```bash
# Delete logs older than 7 days every Sunday at 4 AM
0 4 * * 0 find /var