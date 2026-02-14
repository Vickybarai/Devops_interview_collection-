# 🟦 MODULE 5: Networking & Connectivity

> Complete interview preparation - Simple and Easy to Understand

---

## Q1. How to Check IP Address and Network Configuration?

### **Definition**
Commands to view and configure network interface settings on Linux.

### **Explain (Detail)**

**IP Address:**
- Unique address that identifies device on network
- Format: 192.168.1.10 (IPv4) or 2001:db8::1 (IPv6)
- Assigned to each network interface

**Network Interfaces:**
- Physical: eth0, enp0s3 (ethernet cards)
- Virtual: lo (loopback), docker0, virbr0
- Wireless: wlan0, wlp3s0

**Common Commands:**

**ip command (modern):**
- Newer tool, replaces ifconfig
- More powerful
- Standard on modern Linux

**ifconfig command (old):**
- Older tool, deprecated
- Still widely used
- Simpler for basic tasks

**Key Information to Check:**
- IP address
- Netmask
- Gateway
- DNS servers
- Interface status (up/down)

**Real-World Example:**
```
Need to check server's IP:
1. Use ip addr show
2. Find interface (eth0, ens33)
3. Look for "inet" line
4. IP: 192.168.1.100/24
5. /24 = netmask (255.255.255.0)
```

### **Use**
- **Daily**: Check server IP for SSH/connections
- **Troubleshooting**: Verify network configuration
- **Setup**: Configure network on new server
- **Debug**: Check if interface is up
- **Documentation**: Record network settings

### **Command**
```bash
# ip command (modern)
ip addr show                      # Show all interfaces
ip addr show eth0                 # Show specific interface
ip a                              # Short version
ip a s eth0                       # Short with specific

# ifconfig command (old)
ifconfig                          # Show all interfaces
ifconfig eth0                     # Show specific interface
ifconfig -a                       # Show all (including down)

# View IP address only
ip addr show eth0 | grep inet
hostname -I                       # All IP addresses
hostname -I | awk '{print $1}'   # First IP only

# View network interface status
ip link show
ip link show eth0
ifconfig eth0

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# Assign IP address
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ifconfig eth0 192.168.1.100

# Delete IP address
sudo ip addr del 192.168.1.100/24 dev eth0

# Check all network info
ip addr show
ifconfig -a

# Real-world: Check server IP for SSH
ip addr show | grep "inet "
# Output:
# inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
# Now you can: ssh user@192.168.1.100
```

---

## Q2. How to Check Open Ports and Listening Services?

### **Definition**
Commands to see which ports are open and which services are listening on them.

### **Explain (Detail)**

**Port:**
- Number (1-65535) that identifies specific service
- Example: Port 22 = SSH, Port 80 = HTTP, Port 443 = HTTPS

**Listening Service:**
- Application waiting for connections
- Bound to specific port
- Examples: SSH on 22, Nginx on 80, MySQL on 3306

**Common Commands:**

**ss command (modern):**
- Newer, faster
- Shows socket statistics
- Recommended for modern Linux

**netstat command (old):**
- Older tool
- Still common
- May need net-tools package

**Port Numbers:**
```
Well-known ports: 0-1023 (need root)
Registered ports: 1024-49151
Dynamic ports: 49152-65535

Common ports:
22   - SSH
80   - HTTP
443  - HTTPS
3306 - MySQL
5432 - PostgreSQL
6379 - Redis
8080 - Alternative HTTP
```

**Why Check Ports:**
- Verify service is running
- Troubleshoot connection issues
- Check for security issues
- Debug firewall problems

**Real-World Example:**
```
SSH not working:
1. Check if SSH service running: systemctl status sshd
2. Check if port 22 open: ss -tlnp | grep 22
3. If not running, start service: systemctl start sshd
4. Check firewall: sudo ufw status
```

### **Use**
- **Daily**: Verify services are running
- **Troubleshooting**: Why can't connect to service?
- **Security**: Check for unexpected open ports
- **Setup**: Verify service listening on correct port
- **Debug**: Connection refused errors

### **Command**
```bash
# ss command (modern)
ss -tlnp                          # TCP listening with process
ss -ulnp                          # UDP listening with process
ss -tlnp | grep 22                # Check port 22
ss -tlnp | grep nginx             # Check nginx

# netstat command (old)
netstat -tlnp                     # TCP listening with process
netstat -ulnp                     # UDP listening with process
netstat -tlnp | grep 22           # Check port 22
netstat -an                       # All connections

# View specific port
ss -tlnp | grep :80
netstat -tlnp | grep :80
lsof -i :80                       # Using lsof

# Check if port is open
ss -tlnp | grep -q :22 && echo "Port 22 open" || echo "Port 22 closed"

# Count open ports
ss -tlnp | wc -l                 # Count listening ports
ss -tlnp | grep -v 127.0.0.1     # Excluding localhost

# Check specific service
ss -tlnp | grep sshd
ss -tlnp | grep nginx
ss -tlnp | grep mysql

# View all connections
ss -tan                           # All TCP connections
ss -uap                           # All UDP connections
netstat -an                       # All connections

# Show process using port
ss -tlnp                         # Shows PID/Program name
sudo lsof -i :80                 # More detailed

# Real-world: Check why can't connect to web server
# Step 1: Check if nginx running
sudo systemctl status nginx

# Step 2: Check if port 80 listening
ss -tlnp | grep :80
# If nothing, nginx not listening

# Step 3: Check nginx config for port
sudo grep -r "listen" /etc/nginx/

# Step 4: Start nginx if needed
sudo systemctl start nginx

# Step 5: Verify port open
ss -tlnp | grep :80
```

---

## Q3. Explain /etc/hosts and /etc/resolv.conf Files.

### **Definition**
Configuration files for hostname resolution and DNS settings.

### **Explain (Detail)**

**DNS (Domain Name System):**
- Translates domain names to IP addresses
- Example: google.com → 142.250.185.238
- Like phonebook for internet

**/etc/hosts File:**
- Local hostname resolution
- Checked BEFORE DNS
- Format: IP hostname aliases
- Used for local overrides

**/etc/hosts Format:**
```
127.0.0.1       localhost
192.168.1.100   server1    myserver
192.168.1.101   database
```

**/etc/resolv.conf File:**
- DNS server configuration
- Tells system which DNS servers to use
- Format: nameserver IP
- Usually managed by system/network manager

**/etc/resolv.conf Format:**
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

**Resolution Order:**
```
1. Check /etc/hosts first
2. If not found, ask DNS servers in /etc/resolv.conf
3. If DNS fails, error
```

**Common DNS Servers:**
```
8.8.8.8, 8.8.4.4        - Google DNS
1.1.1.1                 - Cloudflare DNS
192.168.1.1             - Router (common)
10.0.0.1                - Corporate DNS
```

**Real-World Scenarios:**

**Scenario 1: Test website locally**
```
Add to /etc/hosts:
127.0.0.1   myapp.local

Now http://myapp.local points to localhost
Great for testing before DNS setup
```

**Scenario 2: Block website**
```
Add to /etc/hosts:
0.0.0.0     facebook.com

Now facebook.com resolves to nowhere
Website blocked locally
```

**Scenario 3: DNS not working**
```
Check /etc/resolv.conf:
nameserver 8.8.8.8

If empty or wrong, DNS won't work
```

### **Use**
- **/etc/hosts**: Local testing, block sites, local network
- **/etc/resolv.conf**: Configure DNS servers
- **Both**: Troubleshoot DNS issues
- **Development**: Test domains locally
- **Security**: Block malicious sites

### **Command**
```bash
# View /etc/hosts
cat /etc/hosts

# View /etc/resolv.conf
cat /etc/resolv.conf

# Edit /etc/hosts
sudo nano /etc/hosts

# Add entry to /etc/hosts
echo "192.168.1.100   server1" | sudo tee -a /etc/hosts

# Edit /etc/resolv.conf
sudo nano /etc/resolv.conf

# Add DNS server
echo "nameserver 8.8.8.8" | sudo tee -a /etc/resolv.conf

# Check DNS resolution
nslookup google.com
dig google.com
host google.com

# Check hosts file first
getent hosts google.com

# Test local resolution
ping server1
# Uses /etc/hosts first

# Flush DNS cache (if needed)
sudo systemctl restart systemd-resolved
# or
sudo systemctl restart nscd

# Check which DNS servers being used
nmcli dev show | grep DNS
resolvectl status

# Real-world: Setup local test domain
# Step 1: Add to /etc/hosts
sudo nano /etc/hosts
# Add: 127.0.0.1  myapp.test

# Step 2: Test
ping myapp.test
# Now points to localhost

# Step 3: Use in browser
# http://myapp.test

# Real-world: Fix DNS issues
# Step 1: Check DNS servers
cat /etc/resolv.conf

# Step 2: Test DNS
nslookup google.com
# If fails, DNS problem

# Step 3: Use Google DNS
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

# Step 4: Test again
nslookup google.com
```

---

## Q4. How to Connect to Remote Server via SSH?

### **Definition**
SSH (Secure Shell) - Protocol for secure remote login and command execution.

### **Explain (Detail)**

**SSH Basics:**
- Encrypted connection (secure)
- Default port: 22
- Replaces Telnet (unencrypted)
- Uses username@host format

**Basic SSH Connection:**
```
ssh username@server
ssh user@192.168.1.100
ssh root@192.168.1.100
```

**SSH Authentication Methods:**

**1. Password Authentication:**
```
ssh user@server
# Prompts for password
# Simple but less secure
```

**2. SSH Key Authentication (Recommended):**
```
ssh user@server
# Uses private key
# More secure
# No password needed
```

**SSH Keys:**
- **Private key**: Keep secret (like password)
- **Public key**: Share with servers (like padlock)
- Location: ~/.ssh/id_rsa (private), ~/.ssh/id_rsa.pub (public)

**Generate SSH Keys:**
```
ssh-keygen -t rsa
# Creates: id_rsa (private), id_rsa.pub (public)
```

**Copy Key to Server:**
```
ssh-copy-id user@server
# Copies public key to server's authorized_keys
# Now can login without password
```

**SSH with Key:**
```
ssh user@server
# Automatically uses key
# No password prompt
```

**SSH with Specific Key:**
```
ssh -i /path/to/key user@server
# Use specific key file
```

**SSH with Different Port:**
```
ssh -p 2222 user@server
# Connect to port 2222 instead of 22
```

**SSH Config File (~/.ssh/config):**
```
Host myserver
    HostName 192.168.1.100
    User username
    Port 22

# Now just use: ssh myserver
```

**Common SSH Options:**
```
-p 2222          - Use port 2222
-i key.pem       - Use specific key
-L 8080:localhost:80  - Local port forwarding
-v               - Verbose (debug)
-N               - No remote command (port forwarding only)
-f               - Background
```

### **Use**
- **Remote Administration**: Manage servers remotely
- **Automation**: Run scripts on remote servers
- **File Transfer**: With scp, rsync, sftp
- **Tunneling**: Port forwarding, VPN-like connections
- **Development**: Remote debugging

### **Command**
```bash
# Basic SSH connection
ssh user@192.168.1.100
ssh root@192.168.1.100

# SSH with different port
ssh -p 2222 user@server
ssh -p 2222 user@192.168.1.100

# Generate SSH keys
ssh-keygen -t rsa
ssh-keygen -t rsa -b 4096     # 4096-bit key
ssh-keygen -t ed25519         # Modern key type

# Copy key to server
ssh-copy-id user@server
ssh-copy-id -p 2222 user@server    # With custom port

# Manual key copy (if ssh-copy-id not available)
cat ~/.ssh/id_rsa.pub | ssh user@server 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'

# SSH with specific key
ssh -i ~/.ssh/mykey.pem user@server

# Create SSH config
nano ~/.ssh/config
# Add:
# Host myserver
#     HostName 192.168.1.100
#     User username

# Now use: ssh myserver

# Verbose SSH (debug connection)
ssh -v user@server

# Test SSH connection (no login)
ssh -o BatchMode=yes user@server echo "Success"

# Exit SSH
exit
# or press Ctrl+D

# Real-world: Setup passwordless SSH
# Step 1: Generate key
ssh-keygen -t rsa
# Press Enter for defaults (no passphrase)

# Step 2: Copy to server
ssh-copy-id user@192.168.1.100
# Enter password once

# Step 3: Test (no password)
ssh user@192.168.1.100

# Step 4: Should login without password

# Real-world: SSH with config file
# Step 1: Create config
nano ~/.ssh/config

# Step 2: Add servers
Host web1
    HostName 192.168.1.100
    User admin
    Port 22

Host web2
    HostName 192.168.1.101
    User admin
    Port 2222

# Step 3: Connect easily
ssh web1
ssh web2
```

---

## Q5. How to Check Network Connectivity (ping, traceroute)?

### **Definition**
Commands to test and troubleshoot network connectivity.

### **Explain (Detail)**

**ping Command:**
- Tests if host is reachable
- Sends ICMP packets
- Measures response time (latency)
- Shows packet loss

**ping Output:**
```
PING google.com (142.250.185.238): 56 data bytes
64 bytes from 142.250.185.238: icmp_seq=0 ttl=116 time=12.3 ms
64 bytes from 142.250.185.238: icmp_seq=1 ttl=116 time=11.8 ms

ttl   = Time To Live (hops)
time  = Response time (lower = faster)
```

**traceroute Command:**
- Shows path packets take to destination
- Lists all routers (hops) between you and destination
- Helps find where connection fails

**traceroute Output:**
```
traceroute to google.com (142.250.185.238), 30 hops max
1  192.168.1.1 (192.168.1.1)  1.2 ms  1.1 ms  1.0 ms
2  10.0.0.1 (10.0.0.1)      5.2 ms  5.1 ms  5.0 ms
3  142.250.185.238 (142.250.185.238)  12.3 ms  12.1 ms  12.0 ms

Hop number  IP address          Time
```

**mtr Command (modern):**
- Combines ping and traceroute
- Shows real-time statistics
- Better for ongoing monitoring

**When to Use:**

**ping:**
- Quick connectivity test
- Check if server is up
- Measure latency
- Detect packet loss

**traceroute:**
- Find where connection fails
- See network path
- Identify slow hops
- Debug routing issues

**Common Issues:**

**ping fails but traceroute works:**
- Firewall blocking ping (ICMP)
- Server is up but not responding to ping

**traceroute stops at hop X:**
- Issue at that hop
- Firewall or routing problem

**All hops show ***:**
- All routers blocking traceroute
- Common in corporate networks

### **Use**
- **Daily**: Check server connectivity
- **Troubleshooting**: Find where network fails
- **Performance**: Measure latency
- **Debug**: Identify network issues
- **Monitoring**: Continuous network checks

### **Command**
```bash
# ping command
ping google.com                     # Continuous ping
ping -c 4 google.com               # Send 4 packets
ping -c 4 192.168.1.100            # Ping IP
ping -i 2 google.com               # 2 second interval
ping -s 1000 google.com            # 1000 byte packets

# Stop ping
# Press Ctrl+C

# traceroute command
traceroute google.com               # Trace path
traceroute -n google.com            # No DNS resolution (faster)
traceroute -m 20 google.com         # Max 20 hops

# mtr command (modern)
mtr google.com                     # Interactive
mtr -r -c 10 google.com           # Report mode, 10 cycles

# Check if host is reachable
ping -c 1 192.168.1.100
if ping -c 1 192.168.1.100 &> /dev/null; then echo "Up"; else echo "Down"; fi

# Measure latency
ping -c 10 google.com | tail -1
# Shows packet loss and average time

# Check packet loss
ping -c 100 8.8.8.8 | grep "packet loss"

# Find where connection fails
traceroute google.com
# Shows each hop, find where stops

# Check local gateway
ping -c 1 192.168.1.1

# Check internet
ping -c 1 8.8.8.8

# Check DNS
ping -c 1 google.com

# Real-world: Troubleshoot connection to server
# Step 1: Check if server is up
ping -c 4 192.168.1.100

# Step 2: If fails, check local network
ping -c 1 192.168.1.1

# Step 3: If gateway works, find where it fails
traceroute 192.168.1.100

# Step 4: Check DNS
ping -c 1 google.com

# Step 5: Check if port open (if ping blocked)
telnet 192.168.1.100 22
# or
nc -zv 192.168.1.100 22
```

---

## Q6. How to Find Which Process is Using a Port?

### **Definition**
Commands to identify which application/process is using a specific port.

### **Explain (Detail)**

**Why Find Process by Port:**
- Port already in use (can't start service)
- Unknown process using port
- Security - find suspicious processes
- Troubleshoot service conflicts

**Common Commands:**

**lsof (List Open Files):**
- Lists open files and ports
- Very powerful
- Shows process details

**ss (Socket Statistics):**
- Shows socket information
- Includes process name/PID
- Faster than lsof
- Modern Linux default

**netstat:**
- Older tool
- Still works
- With -p flag shows process

**Real-World Scenarios:**

**Scenario 1: Can't start nginx (port 80 in use)**
```
Error: Address already in use

Find what's using port 80:
ss -tlnp | grep :80
or
lsof -i :80

Output: nginx (PID 1234)
or
Output: apache2 (PID 5678)   ← Different service!

Solution: Stop conflicting service or change port
```

**Scenario 2: Unknown service on port 8080**
```
Check what's running:
lsof -i :8080
ss -tlnp | grep :8080

Find suspicious process and investigate
```

**Common Ports and Services:**
```
22   - sshd
80   - nginx, apache, httpd
443  - nginx, apache
3306 - mysqld
5432 - postgres
6379 - redis-server
8080 - java, tomcat, node.js
```

### **Use**
- **Troubleshooting**: "Port already in use" errors
- **Security**: Find unknown/suspicious processes
- **Debugging**: Identify which service is listening
- **Management**: Kill process if needed
- **Configuration**: Verify correct service on port

### **Command**
```bash
# Find process using specific port (ss)
ss -tlnp | grep :80
ss -tlnp | grep :22
ss -tlnp | grep :3306

# Find process using specific port (lsof)
sudo lsof -i :80
sudo lsof -i :22
sudo lsof -i :3306

# Find process using specific port (netstat)
sudo netstat -tlnp | grep :80

# Show PID and process name
ss -tlnp | grep :80
# Output: tcp LISTEN 0 128  [::]:80 [::]:* users:(("nginx",pid=1234,fd=6))

# Check if port is in use
ss -tlnp | grep -q :80 && echo "Port 80 in use" || echo "Port 80 free"

# Kill process using port
sudo kill $(sudo lsof -t -i:80)
# or
sudo kill $(sudo ss -tlnp | grep :80 | awk '{print $6}' | cut -d, -f2 | cut -d= -f2)

# Find what's using port 80
sudo lsof -i :80
# Shows: COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
#        nginx   1234 root  6u  IPv4  12345      0t0  TCP *:http (LISTEN)

# Real-world: Port 8080 already in use
# Step 1: Find what's using it
sudo lsof -i :8080
# Output: java (PID 4567)

# Step 2: Check process
ps aux | grep 4567
# See what application it is

# Step 3: Decide to kill or change port
# Option A: Kill process
sudo kill 4567

# Option B: Use different port
# Change application config to use 8081

# Step 4: Verify port free
ss -tlnp | grep :8080
# Should show nothing

# Real-world: Find all listening processes
ss -tlnp
# Shows all listening ports with PIDs

# Find processes by user
sudo lsof -u www-data -i -P
```

---

## Q7. How to Check Network Connections?

### **Definition**
Commands to view active network connections and network traffic.

### **Explain (Detail)**

**Network Connection Types:**

**TCP Connections:**
- Reliable, connection-oriented
- Used by most services (HTTP, SSH, etc.)
- States: ESTABLISHED, LISTEN, TIME_WAIT, etc.

**UDP:**
- Unreliable, connectionless
- Used by DNS, gaming, streaming
- No connection state

**Connection States (TCP):**
```
LISTEN      - Waiting for connections
ESTABLISHED - Connected and active
TIME_WAIT   - Waiting after close
CLOSE_WAIT  - Remote closed, waiting for local
```

**Commands to View Connections:**

**ss command:**
```
ss -t       - TCP connections
ss -u       - UDP connections
ss -a       - All (including listening)
ss -n       - Numeric (no DNS)
ss -p       - Show process/PID
```

**netstat command:**
```
netstat -t  - TCP
netstat -u  - UDP
netstat -a  - All
netstat -n  - Numeric
netstat -p  - Show process
```

**What to Look For:**
- **ESTABLISHED**: Active connections
- **LISTEN**: Services waiting for connections
- **TIME_WAIT**: Recently closed (normal)
- **Foreign IP**: Who's connected to you

**Real-World Examples:**

**View all connections:**
```
ss -tan
netstat -tan
Shows all TCP connections
```

**View established connections only:**
```
ss -tan | grep ESTABLISHED
netstat -tan | grep ESTABLISHED
Shows active connections
```

**Who's connected to my server:**
```
ss -tan | grep ESTABLISHED | awk '{print $5}'
Shows all remote IPs connected
```

### **Use**
- **Monitoring**: See active connections
- **Security**: Find suspicious connections
- **Troubleshooting**: Debug connection issues
- **Performance**: Check connection count
- **Audit**: Who's accessing server

### **Command**
```bash
# View all TCP connections
ss -tan
netstat -tan

# View all UDP connections
ss -uan
netstat -uan

# View established connections
ss -tan | grep ESTABLISHED
ss -tn state established

# View listening ports
ss -tln
netstat -tln

# View connections with process info
ss -tanp
sudo netstat -tanp

# View connections with numeric IPs (faster)
ss -tan
# Without -n, does DNS resolution (slower)

# Count established connections
ss -tan | grep ESTABLISHED | wc -l

# View connections to specific port
ss -tan | grep :22
ss -tan | grep :80

# View all connections from specific IP
ss -tan | grep 192.168.1.100

# View foreign IPs (who's connected)
ss -tan | grep ESTABLISHED | awk '{print $5}' | cut -d: -f1 | sort -u

# Show summary of connection states
ss -s
# Shows: TCP, UDP, connection states, memory

# Real-time monitoring
watch -n 1 'ss -tan | grep ESTABLISHED'

# Real-world: Who's connected to my web server
# Step 1: View connections to port 80
ss -tan | grep :80

# Step 2: Filter established connections
ss -tan | grep :80 | grep ESTABLISHED

# Step 3: See IPs connected
ss -tan | grep :80 | grep ESTABLISHED | awk '{print $5}' | cut -d: -f1 | sort -u

# Step 4: Count connections per IP
ss -tan | grep :80 | grep ESTABLISHED | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -rn

# Real-world: Check for too many TIME_WAIT
# Step 1: Count TIME_WAIT connections
ss -tan | grep TIME_WAIT | wc -l

# Step 2: If high (>1000), might be issue
# Could indicate connection storms

# Step 3: View details
ss -tan | grep TIME_WAIT | head -20

# Real-world: Monitor connections in real-time
watch -n 2 'ss -tan | grep ESTABLISHED | wc -l'
# Shows count of active connections, updates every 2 seconds
```

---

## Q8. How to View ARP Table?

### **Definition**
ARP (Address Resolution Protocol) - Maps IP addresses to MAC addresses on local network.

### **Explain (Detail)**

**What is ARP:**
- Resolves IP to MAC address
- Only works on local network (same subnet)
- Like phone directory for local network

**MAC Address:**
- Unique hardware address
- Format: 00:11:22:33:44:55
- Assigned to network card
- Never changes

**How ARP Works:**
```
1. Computer wants to talk to 192.168.1.100
2. Checks ARP table for MAC of 192.168.1.100
3. If found, use MAC directly
4. If not found, broadcast: "Who has 192.168.1.100?"
5. 192.168.1.100 replies: "I have it, my MAC is 00:11:22:33:44:55"
6. Save in ARP table for future use
```

**View ARP Table:**
```
arp -a or ip neigh show
Shows: IP address → MAC address
```

**ARP Table Entry:**
```
192.168.1.1  dev eth0  lladdr 00:11:22:33:44:55 REACHABLE
IP          Interface  MAC Address         State
```

**ARP States:**
```
REACHABLE  - Can communicate
STALE      - Old entry, needs refresh
FAILED     - Can't reach
DELAY      - Waiting for reply
```

**When to Use ARP:**
- Find MAC address of device
- Troubleshoot local network
- Identify devices by MAC
- Clear ARP table to force refresh
- Find duplicate IP issues

**Real-World Scenarios:**

**Find MAC of router:**
```
arp -a | grep 192.168.1.1
Shows router's MAC address
```

**Clear ARP table:**
```
sudo ip neigh flush all
Forces ARP to relearn all entries
```

### **Use**
- **Local Network**: Find MAC addresses
- **Troubleshooting**: Network layer 2 issues
- **Security**: Identify devices on network
- **Debugging**: ARP cache issues
- **Inventory**: Track network devices

### **Command**
```bash
# View ARP table
arp -a
arp -n                    # Numeric (no DNS)
ip neigh                  # Modern command
ip neigh show

# Show specific IP
arp -a | grep 192.168.1.1
ip neigh show | grep 192.168.1.1

# Show ARP for specific interface
arp -i eth0 -a
ip neigh show dev eth0

# Delete ARP entry
sudo ip neigh del 192.168.1.100 dev eth0

# Clear entire ARP table
sudo ip neigh flush all
sudo ip neigh flush dev eth0

# Refresh ARP entry (ping)
ping -c 1 192.168.1.100
# Forces ARP refresh

# Show ARP with MAC address
arp -a
# Output: ? (192.168.1.1) at 00:11:22:33:44:55 [ether] on eth0

# Find MAC of gateway/router
arp -a | grep default
ip route | grep default
# Then: arp -a | grep <gateway IP>

# Show detailed ARP info
ip neigh show

# Add static ARP entry (rare)
sudo ip neigh add 192.168.1.100 lladdr 00:11:22:33:44:55 dev eth0 nud permanent

# Real-world: Find MAC of a device on network
# Step 1: Ping device to populate ARP
ping -c 1 192.168.1.100

# Step 2: Check ARP table
arp -a | grep 192.168.1.100

# Step 3: Get MAC address
# Output: 00:11:22:33:44:55

# Step 4: Use MAC to find device (check switch/dhcp logs)

# Real-world: Can't connect to device on same network
# Step 1: Ping device
ping -c 3 192.168.1.100

# Step 2: Check ARP table
arp -a | grep 192.168.1.100

# Step 3: If missing or FAILED, ARP issue
# Step 4: Clear ARP and try again
sudo ip neigh flush all
ping -c 1 192.168.1.100

# Step 5: Check ARP again
arp -a | grep 192.168.1.100
```

---

## Q9. How to View Routing Table?

### **Definition**
Routing table - Rules that determine where network packets are sent.

### **Explain (Detail)**

**What is Routing:**
- Decides where to send network packets
- Based on destination IP
- Uses routes to find next hop

**Routing Table Entry:**
```
Destination    Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0        192.168.1.1     0.0.0.0         UG    100    0        0 eth0
192.168.1.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
```

**Key Columns:**
```
Destination  - Where packets going
Gateway       - Next hop (router)
Genmask       - Network mask
Flags         - Route flags (U=up, G=gateway)
Metric        - Route priority (lower = preferred)
Iface         - Which interface to use
```

**Default Route:**
```
Destination: 0.0.0.0
Gateway: 192.168.1.1
Means: Send everything not in table to 192.168.1.1 (router)
```

**Common Routes:**
```
Local network: 192.168.1.0/24
Default route: 0.0.0.0/0 (via gateway)
Loopback: 127.0.0.0/8
```

**How Routing Works:**
```
Packet to 192.168.1.100:
1. Check routing table
2. Find best matching route
3. 192.168.1.0/24 matches (192.168.1.100 is in this network)
4. Send via eth0 (direct, no gateway)

Packet to 8.8.8.8 (Google DNS):
1. Check routing table
2. No specific match for 8.8.8.8
3. Use default route 0.0.0.0
4. Send via gateway 192.168.1.1
```

**When to Check Routing:**
- Can't connect to external network
- Packet routing issues
- Multiple network interfaces
- VPN configuration
- Gateway problems

### **Use**
- **Troubleshooting**: Can't reach external network
- **Configuration**: Add routes for specific networks
- **Debugging**: Verify packets taking correct path
- **VPN**: Check VPN routing
- **Multi-homed**: Multiple network interfaces

### **Command**
```bash
# View routing table
ip route
ip route show
route -n

# View default route
ip route | grep default
route -n | grep 0.0.0.0

# View route to specific network
ip route get 8.8.8.8
ip route get 192.168.1.100

# Add route (temporary)
sudo ip route add 192.168.2.0/24 via 192.168.1.1

# Add default route
sudo ip route add default via 192.168.1.1

# Delete route
sudo ip route del 192.168.2.0/24 via 192.168.1.1

# Clear routing table (DANGER!)
sudo ip route flush table main

# Show route cache
ip route show cache

# View specific interface routes
ip route show dev eth0

# Real-world: Can't connect to internet
# Step 1: Check routing table
ip route

# Step 2: Check default route exists
ip route | grep default
# Should see: default via 192.168.1.1 dev eth0

# Step 3: If missing, add default route
sudo ip route add default via 192.168.1.1

# Step 4: Test
ping -c 1 8.8.8.8

# Real-world: Add route to specific network
# Scenario: Network 10.0.0.0/24 reachable via 192.168.1.254

# Step 1: Add route
sudo ip route add 10.0.0.0/24 via 192.168.1.254

# Step 2: Verify
ip route | grep 10.0.0.0

# Step 3: Test
ping -c 1 10.0.0.1

# Real-world: Multiple network interfaces
# Step 1: View all routes
ip route

# Step 2: See which interface packets use
ip route get 8.8.8.8
# Shows: dev eth0 src 192.168.1.100

# Step 3: Check interface IPs
ip addr show

# Step 4: Verify routing correct
```

---

## Q10. How to Download Files from Internet (wget, curl)?

### **Definition**
Commands to download files and make HTTP requests from command line.

### **Explain (Detail)**

**wget:**
- Simple file downloader
- Download files from URL
- Resumes interrupted downloads
- Recursive download (mirror websites)

**curl:**
- Multi-purpose tool
- Upload and download
- Supports many protocols
- Great for API calls

**wget Basic Usage:**
```
wget http://example.com/file.zip
wget https://example.com/file.tar.gz
```

**curl Basic Usage:**
```
curl http://example.com/file.zip -O
curl -o file.zip http://example.com/file.zip
```

**Common Options:**

**wget:**
```
-O filename    - Save as filename
-c            - Resume interrupted download
-r            - Recursive (mirror site)
-b            - Background
-q            - Quiet
```

**curl:**
```
-o filename    - Save to filename
-O             - Save as remote filename
-L             - Follow redirects
-I             - Headers only (HEAD request)
-X POST        - HTTP POST method
-d data        - Send data (POST)
-H header      - Add header
```

**When to Use:**
- **wget**: Download files, mirror websites
- **curl**: API calls, test HTTP requests, upload files

**Real-World Examples:**

**Download file:**
```
wget https://example.com/file.zip
curl -O https://example.com/file.zip
```

**Download with custom name:**
```
wget -O myfile.zip https://example.com/file.zip
curl -o myfile.zip https://example.com/file.zip
```

**Resume download:**
```
wget -c https://example.com/largefile.iso
```

**Check if URL exists:**
```
curl -I https://example.com/file.zip
# Returns HTTP headers, shows if 200 OK or 404 Not Found
```

### **Use**
- **Downloads**: Get files from internet
- **Automation**: Download scripts, updates
- **APIs**: Test REST APIs
- **Testing**: Check HTTP endpoints
- **Monitoring**: Download and check files

### **Command**
```bash
# wget - download files
wget http://example.com/file.zip
wget https://example.com/file.tar.gz

# Save with custom name
wget -O myfile.zip http://example.com/file.zip

# Resume interrupted download
wget -c http://example.com/largefile.iso

# Background download
wget -b http://example.com/largefile.iso

# Quiet download
wget -q http://example.com/file.zip

# Recursive download (mirror site)
wget -r -np -k http://example.com/

# curl - download files
curl -O http://example.com/file.zip
curl -o myfile.zip http://example.com/file.zip

# Follow redirects
curl -L http://example.com/redirected -O

# Check HTTP status
curl -I http://example.com/file.zip
curl -w "%{http_code}" http://example.com -o /dev/null

# Download with progress
curl -O http://example.com/file.zip

# Resume download (curl)
curl -C - -O http://example.com/largefile.iso

# Test if URL exists
curl -I http://example.com/file.zip
# Look for: HTTP/1.1 200 OK (exists) or 404 Not Found

# Download with authentication
wget --user=username --password=pass http://example.com/file.zip
curl -u username:password -O http://example.com/file.zip

# Download from GitHub releases
wget https://github.com/user/repo/releases/download/v1.0/file.zip
curl -L -O https://github.com/user/repo/releases/download/v1.0/file.zip

# Real-world: Download and extract application
# Step 1: Download
wget https://example.com/app.tar.gz

# Step 2: Extract
tar -xzf app.tar.gz

# Step 3: Install
cd app
sudo ./install.sh

# Real-world: Download script and run
# Step 1: Download
curl -O https://example.com/install.sh
wget https://example.com/install.sh

# Step 2: Make executable
chmod +x install.sh

# Step 3: Run
./install.sh

# Real-world: Test API endpoint
# Step 1: GET request
curl -I https://api.example.com/status

# Step 2: POST request
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' \
     https://api.example.com/data

# Step 3: Download response
curl -O https://api.example.com/data.json
```

---

## 📚 Quick Reference - Module 5 Commands

### Network Configuration
```bash
ip addr show                   # View IP addresses
ip addr show eth0              # View specific interface
ifconfig eth0                  # Old command (deprecated)
```

### Check Ports
```bash
ss -tlnp                       # Show listening ports with process
netstat -tlnp                  # Old command
lsof -i :80                    # Show what's using port 80
```

### DNS Resolution
```bash
cat /etc/hosts                 # View hosts file
cat /etc/resolv.conf           # View DNS servers
nslookup google.com            # Check DNS
ping google.com                # Test DNS
```

### SSH Connection
```bash
ssh user@server                # Connect to server
ssh -p 2222 user@server        # Different port
ssh-keygen -t rsa              # Generate keys
ssh-copy-id user@server        # Copy key to server
```

### Network Connectivity
```bash
ping -c 4 google.com           # Test connectivity
traceroute google.com          # Trace path
mtr google.com                 # Modern traceroute
```

### Process by Port
```bash
ss -tlnp | grep :80            # Find process on port 80
lsof -i :80                    # Same with lsof
netstat -tlnp | grep :80       # Old command
```

### Network Connections
```bash
ss -tan                        # All TCP connections
ss -uan                        # All UDP connections
ss -tanp                       # With process info
netstat -tanp                  # Old command
```

### ARP Table
```bash
arp -a                         # View ARP table
ip neigh                       # Modern command
ip neigh flush all             # Clear ARP table
```

### Routing
```bash
ip route                       # View routing table
route -n                       # Old command
ip route get 8.8.8.8           # Trace route to IP
```

### Download Files
```bash
wget http://example.com/file.zip
curl -O http://example.com/file.zip
curl -I http://example.com    # Check headers
```

---

**Next:** Module 6 - Service, Boot & Systemctl
