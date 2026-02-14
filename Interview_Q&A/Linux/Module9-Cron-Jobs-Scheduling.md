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
0 4 * * 0 find /var/log -name "*.log" -mtime +7 -delete
```

**Example 5: Business Hours Report**
```bash
# Generate report at 9 AM weekdays
0 9 * * 1-5 /home/user/scripts/daily_report.sh >> /var/log/report.log 2>&1
```

**Example 6: Frequent Monitoring**
```bash
# Check service every 5 minutes
*/5 * * * * /home/user/scripts/check_service.sh >> /var/log/service_check.log 2>&1
```

**Example 7: Month-End Tasks**
```bash
# Run on last day of month at 11 PM
0 23 28-31 * * /home/user/scripts/month_end.sh >> /var/log/month_end.log 2>&1
```

**Example 8: System Update**
```bash
# Update system every Sunday at 2 AM
0 2 * * 0 apt-get update && apt-get upgrade -y >> /var/log/update.log 2>&1
```

### Use
- Copy and adapt examples for your needs
- Learn common scheduling patterns
- Understand real-world cron usage
- Build your own cron jobs from examples

### Command
```bash
# ===== BACKUP EXAMPLES =====

# Daily backup at midnight
0 0 * * * /home/user/backup/backup_daily.sh >> /var/log/backup.log 2>&1

# Incremental backup every 6 hours
0 */6 * * * /home/user/backup/incremental.sh >> /var/log/incremental.log 2>&1

# Weekly backup Sunday 3 AM
0 3 * * 0 /home/user/backup/weekly.sh >> /var/log/weekly.log 2>&1

# Monthly backup on 1st at 1 AM
0 1 1 * * /home/user/backup/monthly.sh >> /var/log/monthly.log 2>&1

# ===== MONITORING EXAMPLES =====

# Check disk space every hour
0 * * * * /home/user/monitor/disk_space.sh >> /var/log/disk.log 2>&1

# Check CPU every 15 minutes
*/15 * * * * /home/user/monitor/cpu_check.sh >> /var/log/cpu.log 2>&1

# Check service every 5 minutes
*/5 * * * * systemctl status nginx >> /var/log/nginx_status.log 2>&1

# Check website availability every 10 minutes
*/10 * * * * curl -s http://example.com || echo "Website down" | mail -s "Alert" admin@example.com

# ===== CLEANUP EXAMPLES =====

# Clean /tmp daily at 1 AM
0 1 * * * rm -rf /tmp/* >> /var/log/cleanup.log 2>&1

# Clean logs older than 30 days every Sunday
0 2 * * 0 find /var/log -name "*.log.*" -mtime +30 -delete >> /var/log/log_cleanup.log 2>&1

# Clean cache every day at 3 AM
0 3 * * * rm -rf /var/cache/* >> /var/log/cache_cleanup.log 2>&1

# ===== REPORTING EXAMPLES =====

# Daily report at 8 AM
0 8 * * * /home/user/reports/daily.sh >> /var/log/daily_report.log 2>&1

# Weekly report Monday 9 AM
0 9 * * 1 /home/user/reports/weekly.sh >> /var/log/weekly_report.log 2>&1

# Email report at 5 PM weekdays
0 17 * * 1-5 /home/user/reports/send_report.sh >> /var/log/email_report.log 2>&1

# ===== MAINTENANCE EXAMPLES =====

# Restart service daily at 4 AM
0 4 * * * systemctl restart myservice >> /var/log/restart.log 2>&1

# Update packages Sunday 2 AM
0 2 * * 0 apt-get update && apt-get upgrade -y >> /var/log/update.log 2>&1

# Rotate logs every Sunday at 3 AM
0 3 * * 0 /home/user/scripts/logrotate.sh >> /var/log/logrotate.log 2>&1

# ===== CUSTOM SCHEDULES =====

# Every 30 minutes
*/30 * * * * /home/user/scripts/task.sh >> /var/log/task.log 2>&1

# Business hours only (9 AM - 5 PM weekdays)
0 9-17 * * 1-5 /home/user/scripts/business.sh >> /var/log/business.log 2>&1

# Nightly maintenance (11 PM - 5 AM)
0 23-5 * * * /home/user/scripts/nightly.sh >> /var/log/nightly.log 2>&1

# Multiple times per day
0 8,12,18 * * * /home/user/scripts/thrice_daily.sh >> /var/log/thrice.log 2>&1

# ===== WITH ENVIRONMENT VARIABLES =====

# Set PATH and run script
PATH=/usr/local/bin:/usr/bin:/bin
0 * * * * python3 /home/user/scripts/myscript.py >> /var/log/myscript.log 2>&1

# Set email for notifications
MAILTO=admin@example.com
0 * * * * /home/user/scripts/alert.sh >> /var/log/alert.log 2>&1
```

---

## 9. How to Debug Cron Jobs

### Definition
Debugging cron jobs involves identifying why scheduled tasks aren't running correctly by checking logs, paths, permissions, and environment variables.

### Explanation (For Beginners)
**Common cron job problems:**

1. **Job not running at all**
   - Check if cron service is running: `systemctl status cron`
   - Check cron syntax: `crontab -l`
   - Check cron logs: `grep CRON /var/log/syslog`

2. **Job running but failing**
   - Check job output in log files
   - Check script permissions: `ls -l script.sh`
   - Test script manually first

3. **Permission issues**
   - Script not executable: `chmod +x script.sh`
   - Can't write to log file: Check file permissions
   - Running as wrong user: Use `crontab -u`

4. **Path issues**
   - Commands not found: Use absolute paths
   - Files not found: Use absolute paths
   - Script uses relative paths: Change to absolute paths

5. **Environment issues**
   - Cron runs with minimal environment
   - Missing PATH variable
   - Missing environment variables

**Debugging steps:**

**Step 1: Check cron service**
```bash
systemctl status cron
```

**Step 2: Verify cron syntax**
```bash
crontab -l
```

**Step 3: Check cron logs**
```bash
sudo grep CRON /var/log/syslog | tail -20
```

**Step 4: Test command manually**
```bash
/path/to/script.sh
```

**Step 5: Check permissions**
```bash
ls -l /path/to/script.sh
chmod +x /path/to/script.sh
```

**Step 6: Check paths**
```bash
which python3
/usr/bin/python3    # Use this absolute path in cron
```

**Best practices for debugging:**
- Always redirect output: `>> /var/log/mylog.log 2>&1`
- Use absolute paths for all commands and files
- Test commands manually before adding to cron
- Add logging to your scripts
- Start with simple schedules (every minute) to test

### Use
- Troubleshoot failing cron jobs
- Verify cron job execution
- Fix permission problems
- Resolve path issues
- Debug script errors

### Command
```bash
# Check if cron is running
systemctl status cron
ps aux | grep cron

# View cron logs for recent activity
sudo tail -f /var/log/syslog | grep CRON

# View all cron log entries
sudo grep CRON /var/log/syslog

# View cron logs for a specific date
sudo grep CRON /var/log/syslog | grep "Jan 15"

# View cron logs for specific user
sudo grep CRON /var/log/syslog | grep "username"

# Check your crontab
crontab -l

# Check syntax of a crontab file
crontab test_crontab.txt

# Test your script manually first
/path/to/script.sh

# Check script permissions
ls -l /path/to/script.sh
chmod +x /path/to/script.sh

# Check if paths are correct
which python3
which bash

# Create a test cron job that runs every minute
echo "* * * * * date >> /tmp/cron_test.log 2>&1" | crontab -

# Wait 1-2 minutes and check the output
cat /tmp/cron_test.log

# Check system cron logs for your test
sudo tail -f /var/log/syslog | grep CRON

# Remove test job
crontab -r

# Add job with full debugging
crontab -e
# Add this line:
*/5 * * * * /path/to/script.sh >> /var/log/script_debug.log 2>&1

# Monitor the debug log in real-time
tail -f /var/log/script_debug.log

# Check if script is executable
file /path/to/script.sh

# Run script as the cron user (simulate cron environment)
su - username -c "/path/to/script.sh"

# Check for common path issues
echo $PATH
crontab -l | grep PATH

# Add environment variables to crontab
crontab -e
# Add at top:
PATH=/usr/bin:/bin:/usr/local/bin
HOME=/home/username

# Check log file permissions
ls -l /var/log/
sudo touch /var/log/mylog.log
sudo chmod 666 /var/log/mylog.log

# Test cron with a simple command
echo "*/5 * * * * echo 'Cron works' >> /tmp/cron_works.log" | crontab -
sleep 300
cat /tmp/cron_works.log

# Check for mail notifications (if configured)
mail
mailq

# View cron mail logs
sudo tail /var/log/mail.log | grep cron

# Debug by capturing environment in cron
crontab -e
# Add:
* * * * * env > /tmp/cron_env.txt
# Then compare:
env > /tmp/current_env.txt
diff /tmp/current_env.txt /tmp/cron_env.txt
```

---

## 10. What are Cron Directories (/etc/cron.*)?

### Definition
Cron directories in `/etc/cron.*` contain system-wide scheduled scripts that run at specific intervals (hourly, daily, weekly, monthly) without needing to edit the main crontab file.

### Explanation (For Beginners)
**What are cron directories?**
- Pre-configured directories for common scheduling intervals
- Place scripts in these directories, and they run automatically
- Managed by the system, not individual users

**Types of cron directories:**

1. **/etc/cron.hourly/**
   - Runs scripts once every hour
   - Minute: 1 (first minute of the hour)
   - Example: Log rotation, temporary file cleanup

2. **/etc/cron.daily/**
   - Runs scripts once every day
   - Time: Usually 6:25 AM (varies by system)
   - Example: Daily backups, log archiving

3. **/etc/cron.weekly/**
   - Runs scripts once every week
   - Time: Usually Sunday morning
   - Example: Full backups, system updates

4. **/etc/cron.monthly/**
   - Runs scripts once every month
   - Time: Usually first day of the month
   - Example: Monthly reports, deep cleanup

5. **/etc/cron.d/**
   - Custom cron job files
   - Same format as /etc/crontab
   - More flexible than cron.* directories

**How they work:**
- Scripts must be executable: `chmod +x script.sh`
- Scripts are run alphabetically (01-script.sh runs before 10-script.sh)
- Run by the system (usually as root)
- No need to specify time - already defined

**Cron directories vs Crontab:**

| Feature | Cron Directories | Crontab |
|---------|------------------|---------|
| Time control | Fixed intervals | Custom schedules |
| Location | /etc/cron.* | User or /etc/crontab |
| Permission | Usually root | Per user |
| Complexity | Simple | Flexible |
| Use case | Regular intervals | Custom timing |

**Best practices:**
- Number scripts for order: `01-backup.sh`, `02-cleanup.sh`
- Make scripts executable
- Add descriptive names
- Include logging in scripts
- Test scripts before adding

### Use
- System maintenance tasks
- Regular backups at standard intervals
- Log rotation and cleanup
- Package management updates
- Standardized system jobs

### Command
```bash
# List cron directories
ls -la /etc/cron.hourly/
ls -la /etc/cron.daily/
ls -la /etc/cron.weekly/
ls -la /etc/cron.monthly/
ls -la /etc/cron.d/

# View scripts in cron directories
cat /etc/cron.daily/*
cat /etc/cron.weekly/*

# Check if scripts are executable
ls -l /etc/cron.daily/

# Add a script to cron.daily
sudo cp my_backup.sh /etc/cron.daily/my_backup.sh
sudo chmod +x /etc/cron.daily/my_backup.sh

# Add a script to cron.weekly
sudo cp weekly_cleanup.sh /etc/cron.weekly/01_weekly_cleanup.sh
sudo chmod +x /etc/cron.weekly/01_weekly_cleanup.sh

# Create a simple test script for cron.hourly
sudo cat > /etc/cron.hourly/test_hourly << 'EOF'
#!/bin/bash
echo "$(date): Hourly cron job ran" >> /tmp/hourly_test.log
EOF
sudo chmod +x /etc/cron.hourly/test_hourly

# Wait for next hour and check
tail -f /tmp/hourly_test.log

# Check what runs in cron.daily
sudo cat /etc/crontab | grep daily

# View anacron configuration (if using anacron)
cat /etc/anacrontab

# View run-parts configuration
man run-parts

# List all cron-related directories
find /etc -name "cron*" -type d

# List all cron-related files
find /etc -name "cron*" -type f

# Check which cron scheduler is being used
ls -la /etc/cron*  # crontab = cron, anacrontab = anacron

# Remove a script from cron directory
sudo rm /etc/cron.daily/my_backup.sh

# Temporarily disable a script (make non-executable)
sudo chmod -x /etc/cron.daily/script.sh

# Re-enable a script
sudo chmod +x /etc/cron.daily/script.sh

# Add custom cron job to /etc/cron.d
sudo cat > /etc/cron.d/my_custom_jobs << 'EOF'
# Custom cron jobs
*/5 * * * * root /root/scripts/monitor.sh >> /var/log/monitor.log 2>&1
0 2 * * * root /root/scripts/backup.sh >> /var/log/backup.log 2>&1
EOF

# Check syntax of cron.d files
sudo cat /etc/cron.d/my_custom_jobs

# Reload cron after adding to /etc/cron.d
sudo systemctl reload cron
```

---

## Summary

Module 9 covered **Cron Jobs & Scheduling**:

### Key Topics:
1. **What is Cron** - Time-based job scheduler
2. **Crontab** - Configuration file for cron jobs
3. **Cron Syntax** - Time field format (min hour day month weekday)
4. **Special Characters** - *, /, -, , for flexible scheduling
5. **Scheduling Jobs** - Adding cron jobs to crontab
6. **Viewing Jobs** - Listing and checking cron entries
7. **Editing Crontab** - Modifying scheduled tasks
8. **Cron Examples** - Real-world job scenarios
9. **Debugging** - Troubleshooting cron job issues
10. **Cron Directories** - System-wide /etc/cron.* directories

### Total Commands in Module 9: **80+**

### Interview Tips:
- Always use absolute paths in cron jobs
- Redirect output to log files: `>> /var/log/file.log 2>&1`
- Test scripts manually before adding to cron
- Use comments to document cron jobs
- Understand the five time fields: minute hour day month weekday
- Know special characters: *, /, -, ,
- Remember cron runs with minimal environment
- Check logs when debugging: `/var/log/syslog`

### Cron Syntax Quick Reference:
```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Next Steps:
- Practice creating simple cron jobs
- Learn systemd timers as an alternative
- Study anacron for systems that aren't always on
- Explore advanced cron features
- Build a collection of useful cron jobs
