# 🟦 MODULE 5: Networking & Connectivity

> Complete interview preparation for Networking & Connectivity - Freshers Perspective

---

## Q1. What is IP Address, Subnet Mask, and Gateway?

### **Definition**
- **IP Address**: Unique identifier for a device on network
- **Subnet Mask**: Defines which part of IP is network and which is host
- **Gateway**: Device that connects your network to outside world (router)

### **Explain (Detail - Interview Answer)**

**IP Address:**
```
What is it?
- Like a phone number for your computer
- Every device on network has unique IP
- Two types: IPv4 (192.168.1.1) and IPv6 (2001:db8::1)

IPv4 Format: 192.168.1.100
           │    │   │    └─ Host (unique on network)
           │    │   └────── Host part
           │    └────────── Network part
           └─────────────── Address

Example: 192.168.1.100
- 192.168.1 = Network (same for all devices on same network)
- 100 = Host (unique for each device)
```

**Subnet Mask:**
```
What is it?
- Tells computer which part of IP is network vs host
- Used to determine if another IP is on same network
- Format: 255.255.255.0 or /24 (CIDR notation)

Example: 192.168.1.100 with 255.255.255.0
          192.168.1.100
          255.255.255.0  (subnet mask)
          -------------
          192.168.1.  = Network part
                  100 = Host part

Why do we need it?
Computer A: 192.168.1.100
Computer B: 192.168.1.200
Subnet mask: 255.255.255.0

Both have same network part (192.168.1)
So they are on same network → Communicate directly

Computer C: 192.168.2.100
Different network part (192.168.2)
So they are on different network → Send via gateway
```

**Gateway (Default Gateway):**
```
What is it?
- Router that connects your network to internet
- Like exit door for your network
- All traffic for outside goes through gateway

Example:
Your IP: 192.168.1.100
Gateway:  192.168.1.1

When you want to access google.com (not on your network):
Your computer → Gateway (192.168.1.1) → Internet → Google.com

Without gateway, you can only communicate on your local network
```

**Real-World Analogy:**
```
IP Address    = Your phone number
Subnet Mask   = Area code (helps determine local vs long distance)
Gateway       = Telephone exchange (connects you to outside world)

Your phone: 011-1234-5678
           │   │     │
           │   │     └─ Unique number (IP host part)
           │   └────── Local exchange (IP network part)
           └────────── Country/Area code (Identifies your network)
```

**Interview Answer Example:**
```
Interviewer: Explain IP address, subnet mask, and gateway.

Your Answer:
"IP address is like a unique identifier for a device on a network,
similar to a phone number. For example, 192.168.1.100 is an IPv4
address where 192.168.1 is the network and 100 is the host.

Subnet mask, like 255.255.255.0, tells the computer which part of
the IP is the network and which is the host. It helps determine
if another device is on the same network or not.

Gateway is like the router or exit point to the internet. If I
want to access something outside my local network, like a website,
my computer sends that traffic through the gateway."
```

### **Use**
- **IP Address**: Identify and communicate with devices on network
- **Subnet Mask**: Determine if devices are on same network
- **Gateway**: Connect to internet and other networks
- **Network Administration**: Configure network settings
- **Troubleshooting**: Check connectivity issues

### **Command**
```bash
# View IP address and network configuration
ip addr show            # Modern command (preferred)
ip a                    # Short form

# View specific interface
ip addr show eth0
ip a show eth0

# Old method (ifconfig)
ifconfig                # May need to install net-tools
ifconfig eth0

# View subnet mask
ip addr show | grep netmask
ifconfig | grep netmask

# View gateway (default route)
ip route show
ip route show | grep default
ip r                    # Short form

# View all network info
ip -br addr             # Brief format

# Real-world example
# Check your IP
ip addr show | grep "inet "

# Check your gateway
ip route | grep default

# Check network configuration
ip addr show
ip route show
```

---

## Q2. How to Check IP Address (ip, ifconfig)?

### **Definition**
Commands to view and configure network interface IP addresses on Linux system.

### **Explain (Detail - Interview Answer)**

**ip Command (Modern - Recommended):**
```
What is it?
- Modern Linux network configuration tool
- Replaces older ifconfig, route commands
- More powerful and flexible
- Default on most modern Linux systems

Basic syntax:
ip [options] object [command]

Common objects:
- addr: IP addresses
- link: Network interfaces
- route: Routing table
```

**ip addr Examples:**
```bash
# View all network interfaces with IPs
ip addr show
# Output:
# 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
#     inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
#     │       │        │    │
#     │       │        │    └─ Broadcast address
#     │       │        └─────── Interface name
#     │       └───────────────── IP address / Subnet mask
#     └───────────────────────── Network interface
```

**ifconfig Command (Older):**
```
What is it?
- Old network configuration tool
- Deprecated but still widely used
- Simple and easy to read
- May need to install: sudo apt install net-tools

Basic usage:
ifconfig [interface]
```

**ifconfig Output Explained:**
```bash
$ ifconfig eth0
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
        │         │            │                     │
        │         │            └───────────────────── Broadcast IP
        │         └───────────────────────────────── Subnet mask
        └─────────────────────────────────────────── Your IP address
        inet6 fe80::1%eth0  prefixlen 64  scopeid 0x20<link>
        ether 00:11:22:33:44:55  txqueuelen 1000  (Ethernet)
        │                           │
        │                           └─ MAC address (hardware address)
        └───────────────────────────── Physical address
```

**Key Information in Output:**
```
inet: IPv4 address
inet6: IPv6 address
netmask: Subnet mask
broadcast: Broadcast address
ether: MAC address (physical hardware address)
flags=UP: Interface is active
mtu: Maximum Transmission Unit (packet size)
```

**Difference Between ip and ifconfig:**
```
ip command:
✅ Modern, actively maintained
✅ More features
✅ Better error messages
✅ Supports IPv6 better
✅ Default on modern systems

ifconfig:
✅ Simple, easy to read
✅ Familiar to many admins
❌ Deprecated
❌ Less features
❌ May not be installed by default
```

**Interview Answer Example:**
```
Interviewer: How do you check IP address in Linux?

Your Answer:
"I use the 'ip addr show' command to check IP addresses. This is
the modern and recommended method. For example, 'ip addr show eth0'
will display the IP address of the eth0 interface.

I can also use 'ifconfig' which is the older method, but it's now
deprecated. The ip command is more powerful and is the preferred
tool for network configuration on modern Linux systems.

The output shows the IP address, subnet mask, broadcast address,
and other details like the MAC address and interface status."
```

**Real-World Scenarios:**

**Scenario 1: Check your IP**
```bash
ip addr show | grep "inet " | grep -v "127.0.0.1"
# Shows your IP (excluding localhost)
```

**Scenario 2: Check if interface is up**
```bash
ip link show eth0
# Look for "UP" and "LOWER_UP" in flags
```

**Scenario 3: View all network info at once**
```bash
ip -br addr
# Brief output, easy to read
```

### **Use**
- **Check IP Address**: Know your system's network configuration
- **Troubleshooting**: Verify network is configured correctly
- **Network Setup**: Configure IP addresses on servers
- **Connectivity Issues**: Check if interface is up
- **Documentation**: Record network configuration

### **Command**
```bash
# Modern method (recommended)
ip addr show                    # View all interfaces
ip a show                       # Short form
ip addr show eth0              # View specific interface
ip -br addr                     # Brief format

# View only IP addresses
ip addr show | grep "inet "

# View specific information
ip addr show eth0 | grep inet   # Only IP
ip addr show eth0 | grep ether  # Only MAC address

# Old method (ifconfig)
ifconfig                       # View all interfaces
ifconfig eth0                  # View specific interface
ifconfig eth0 | grep inet      # Only IP
ifconfig eth0 | grep ether     # Only MAC

# Check interface status
ip link show                    # All interfaces
ip link show eth0              # Specific interface
ip link set eth0 up            # Bring interface up
ip link set eth0 down          # Bring interface down

# Real-world troubleshooting
# Step 1: Check IP
ip addr show

# Step 2: Check interface is up
ip link show eth0

# Step 3: Check gateway
ip route show | grep default

# Step 4: Test connectivity
ping 8.8.8.8

# View network configuration summary
ip -br addr
ip route show
```

---

## Q3. How to Check Open Ports and Listening Services (netstat, ss)?

### **Definition**
Commands to view network connections, open ports, and listening services on Linux system.

### **Explain (Detail - Interview Answer)**

**What Are Ports?**
```
What is a port?
- Like doors on a house (house = IP address, doors = ports)
- Applications listen on specific ports to receive connections
- Port numbers range from 0-65535

Common ports:
22  → SSH (remote login)
80  → HTTP (web server)
443 → HTTPS (secure web server)
3306 → MySQL database
5432 → PostgreSQL database
8080 → Alternative HTTP port
```

**ss Command (Modern - Recommended):**
```
What is it?
- Socket Statistics - modern replacement for netstat
- Faster, more feature-rich
- Default on modern Linux systems
- Better filtering options

Basic syntax:
ss [options] [filter]
```

**Common ss Commands:**
```bash
# Show all listening TCP ports
ss -tlnp

# Output explanation:
# State   Recv-Q Send-Q Local Address:Port    Peer Address:Port
# LISTEN  0      128    0.0.0.0:22            0.0.0.0:*
#         │      │      │       │            │
#         │      │      │       │            └─ Accept from any
#         │      │      │       └──────────────── Port 22 (SSH)
#         │      │      └───────────────────────── Your IP (0.0.0.0 = all)
#         │      └───────────────────────────── Send/Receive queue
#         └───────────────────────────────────── State (LISTEN = waiting for connections)

# Process info (p flag)
# Shows which process is using the port
```

**netstat Command (Older):**
```
What is it?
- Network Statistics - older tool
- Widely known and used
- Deprecated but still common
- May need to install: sudo apt install net-tools

Basic syntax:
netstat [options]
```

**Common netstat Commands:**
```bash
# Show all listening TCP ports with process
netstat -tlnp

# Output similar to ss but older format
```

**Command Options Explained:**
```
ss/netstat options:

-t  → TCP connections
-u  → UDP connections
-l  → Listening sockets only
-n  → Show numbers (don't resolve names, faster)
-p  → Show process name/PID (requires root)
-a  → Show all sockets (listening and established)
```

**Real-World Examples:**

**Example 1: Check what's listening on port 80**
```bash
ss -tlnp | grep :80
# Shows web server (Apache/Nginx) if running
```

**Example 2: Check SSH is running**
```bash
ss -tlnp | grep :22
# Should show sshd process
```

**Example 3: Check all listening services**
```bash
ss -tlnp
# Shows all services listening on ports
```

**Example 4: Find which process is using a port**
```bash
sudo ss -tlnp | grep :8080
# Shows which app is using port 8080
```

**Interview Answer Example:**
```
Interviewer: How do you check open ports on a Linux server?

Your Answer:
"I use the 'ss' command which is the modern tool for checking
network sockets. For example, 'ss -tlnp' shows all listening TCP
ports along with the process name and PID.

The options are: -t for TCP, -l for listening, -n for numeric
port numbers, and -p to show the process. This helps me identify
which services are running and which ports they're listening on.

I can also use 'netstat -tlnp' which is the older method, but
ss is preferred as it's faster and has more features."
```

**Troubleshooting Scenarios:**

**Scenario 1: Service not starting (port already in use)**
```bash
# Find what's using the port
sudo ss -tlnp | grep :8080

# Output shows process using port 8080
# Kill that process or use different port
```

**Scenario 2: Verify web server is running**
```bash
ss -tlnp | grep ':80 '
# Should show nginx or apache process
```

**Scenario 3: Check for suspicious connections**
```bash
ss -tunp
# Shows all TCP and UDP connections with processes
```

### **Use**
- **Check Services**: Verify which services are running
- **Troubleshooting**: Find why service won't start (port conflict)
- **Security**: Check for unexpected open ports
- **Network Debugging**: See active connections
- **Port Conflicts**: Find which process is using a port

### **Command**
```bash
# ss command (modern, recommended)
ss -tlnp                        # All listening TCP ports
ss -ulnp                        # All listening UDP ports
ss -tunp                        # All TCP and UDP ports
ss -tlnp | grep :22            # Check SSH (port 22)
ss -tlnp | grep :80            # Check web server
ss -s                          # Summary statistics

# netstat command (older)
netstat -tlnp                   # All listening TCP ports
netstat -tlnp | grep :80       # Check specific port
netstat -tunp                   # All TCP and UDP
netstat -an                     # All sockets (no process info)

# Find what's using a port
sudo ss -tlnp | grep :8080
sudo netstat -tlnp | grep :8080

# Count connections by state
ss -s
# Shows: established, time-wait, etc.

# View established connections
ss -tn
ss -tn state established

# Real-world troubleshooting
# Step 1: Check web server listening
ss -tlnp | grep ':80 '

# Step 2: Check if port is in use
sudo ss -tlnp | grep ':8080 '

# Step 3: Find all listening services
sudo ss -tlnp

# Step 4: Check connection count
ss -s

# Step 5: Monitor connections in real-time
watch -n 1 'ss -tunp'
```

---

## Q4. What is DNS and How to Troubleshoot DNS Issues?

### **Definition**
DNS (Domain Name System) - Converts domain names to IP addresses.

### **Explain (Detail - Interview Answer)**

**What is DNS?**
```
Simple explanation:
DNS is like phonebook of the internet

Domain name: google.com
IP address:  142.250.185.238

Without DNS, you'd need to remember IP addresses
With DNS, you just remember domain names

How it works:
1. You type: google.com
2. Computer asks DNS server: "What's IP for google.com?"
3. DNS server replies: "142.250.185.238"
4. Your browser connects to that IP address
```

**DNS Hierarchy:**
```
Your Computer
    ↓
ISP DNS Server (or your configured DNS)
    ↓
Root DNS Servers
    ↓
TLD DNS Servers (.com, .org, .net)
    ↓
Authoritative DNS Server (google.com's DNS)
```

**DNS Resolution Example:**
```
1. Browser: "Where is google.com?"
2. Check cache (already know IP?)
3. If not, ask DNS server
4. DNS server finds IP and returns it
5. Browser connects to IP
6. Connection successful!
```

**Important Files:**
```
/etc/hosts:
- Local DNS resolution
- Checked before DNS server
- Format: IP    hostname
- Example: 127.0.0.1    localhost
          192.168.1.100  myserver

/etc/resolv.conf:
- DNS server configuration
- Tells system which DNS servers to use
- Example:
  nameserver 8.8.8.8      # Google DNS
  nameserver 1.1.1.1      # Cloudflare DNS
```

**DNS Commands:**
```bash
# Query DNS
nslookup google.com          # Look up DNS record
dig google.com               # More detailed DNS lookup
host google.com              # Simple lookup

# Check DNS server
cat /etc/resolv.conf         # View configured DNS servers

# Test DNS resolution
ping google.com              # Tests if DNS works
nslookup google.com          # Shows which DNS server responded
```

**Common DNS Issues:**
```
Issue 1: Can't resolve domain names
Symptom: ping google.com fails with "name or service not known"
Cause: DNS server down or wrong configuration
Solution: Check /etc/resolv.conf, try 8.8.8.8

Issue 2: Slow website loading
Symptom: Websites take long to load
Cause: Slow DNS server
Solution: Change to faster DNS (8.8.8.8, 1.1.1.1)

Issue 3: Wrong website opens
Symptom: Opens different site than expected
Cause: DNS cache or wrong DNS server
Solution: Clear cache, check DNS settings
```

**Interview Answer Example:**
```
Interviewer: What is DNS and how do you troubleshoot DNS issues?

Your Answer:
"DNS is like the phonebook of the internet. It converts human-readable
domain names like google.com into IP addresses that computers use to
communicate.

For troubleshooting, I first check /etc/resolv.conf to see which DNS
servers are configured. Then I use 'nslookup' or 'dig' to test if DNS
resolution is working. I can also use 'ping' to test connectivity.

If DNS isn't working, I try using a public DNS like 8.8.8.8 to see if
the issue is with my configured DNS server. I also check /etc/hosts
for any local overrides that might be causing problems."
```

**Troubleshooting Steps:**
```bash
# Step 1: Check DNS configuration
cat /etc/resolv.conf

# Step 2: Test DNS resolution
nslookup google.com
dig google.com

# Step 3: Test with different DNS
nslookup google.com 8.8.8.8

# Step 4: Check if it's a DNS issue
ping 8.8.8.8                    # Ping IP (should work)
ping google.com                 # Ping domain (tests DNS)

# Step 5: Check /etc/hosts
cat /etc/hosts

# Step 6: Clear DNS cache (systemd-resolved)
sudo systemd-resolve --flush-caches
```

### **Use**
- **Domain Resolution**: Convert domain names to IPs
- **Troubleshooting**: Diagnose DNS issues
- **Configuration**: Set DNS servers
- **Local Overrides**: Use /etc/hosts for testing
- **Network Setup**: Configure DNS for servers

### **Command**
```bash
# DNS lookup commands
nslookup google.com             # Look up domain
nslookup google.com 8.8.8.8     # Use specific DNS server
dig google.com                  # Detailed lookup
dig google.com @8.8.8.8         # Query specific DNS
host google.com                 # Simple lookup

# Check DNS configuration
cat /etc/resolv.conf            # View DNS servers
systemd-resolve --status       # View DNS status (Ubuntu)

# Test DNS resolution
ping google.com                 # Tests DNS
curl -I https://google.com     # Tests HTTP + DNS

# Troubleshooting
# Check if DNS is the issue
ping 8.8.8.8                    # Should work (IP)
ping google.com                 # Tests DNS

# Try different DNS
nslookup google.com 8.8.8.8     # Google DNS
nslookup google.com 1.1.1.1     # Cloudflare DNS

# View DNS servers
cat /etc/resolv.conf

# Check /etc/hosts
cat /etc/hosts

# Clear DNS cache (Ubuntu)
sudo systemd-resolve --flush-caches

# Real-world troubleshooting
# Step 1: Check DNS config
cat /etc/resolv.conf

# Step 2: Test DNS
nslookup google.com

# Step 3: If fails, try public DNS
nslookup google.com 8.8.8.8

# Step 4: Check if it's network or DNS
ping 8.8.8.8
ping google.com

# Step 5: Check hosts file
cat /etc/hosts
```

---

## Q5. How to Check Network Connectivity (ping, traceroute, mtr)?

### **Definition**
Commands to test network connectivity and trace the path to destination.

### **Explain (Detail - Interview Answer)**

**ping Command:**
```
What is it?
- Tests if a host is reachable
- Sends ICMP packets to destination
- Measures response time (latency)
- Shows packet loss

How it works:
1. Your computer sends "ping" to destination
2. Destination replies with "pong"
3. If reply received → Host is reachable
4. If no reply → Host is down or network issue
```

**ping Examples:**
```bash
# Basic ping
ping google.com

# Output:
# PING google.com (142.250.185.238): 56 data bytes
# 64 bytes from 142.250.185.238: icmp_seq=0 ttl=117 time=12.3 ms
# 64 bytes from 142.250.185.238: icmp_seq=1 ttl=117 time=11.8 ms
# │       │        │               │     │
# │       │        │               │     └─ Response time
# │       │        │               └────────── Sequence number
# │       │        └────────────────────────── Source (google.com)
# │       └─────────────────────────────────── Reply received
# └─────────────────────────────────────────── Ping reply

# Count specific number of pings
ping -c 4 google.com           # Send 4 pings then stop

# Continuous ping (Ctrl+C to stop)
ping google.com

# Ping with interval
ping -i 2 google.com            # Ping every 2 seconds
```

**traceroute Command:**
```
What is it?
- Traces path packets take to destination
- Shows all routers/hops between you and destination
- Identifies where network issues occur

How it works:
1. Sends packets with increasing TTL (Time To Live)
2. Each router decrements TTL
3. When TTL = 0, router returns error
4. Shows IP of each router in path
```

**traceroute Examples:**
```bash
# Trace route to google.com
traceroute google.com

# Output:
# traceroute to google.com (142.250.185.238), 30 hops max
#  1  192.168.1.1  1.2 ms  1.1 ms  1.0 ms
#  2  10.0.0.1     5.4 ms  5.2 ms  5.3 ms
#  3  72.14.200.1  10.2 ms  10.1 ms  10.3 ms
#     │            │     │     │
#     │            │     │     └─ 3rd attempt time
#     │            │     └───────── 2nd attempt time
#     │            └─────────────── 1st attempt time
#     └─────────────────────────── Hop number & IP

# Uses UDP by default, can use ICMP
traceroute -I google.com       # Use ICMP
```

**mtr Command:**
```
What is it?
- Combines ping and traceroute
- Shows real-time statistics
- Continuously pings each hop
- Better for diagnosing network issues

Why use mtr?
- Shows packet loss at each hop
- Real-time updates
- Better than traceroute for troubleshooting
```

**mtr Examples:**
```bash
# Run mtr (requires root for certain features)
sudo mtr google.com

# Output shows each hop with:
# - Response time
# - Packet loss
# - Number of packets sent

# Report mode (generate report)
sudo mtr -r -c 10 google.com
# -r: Report mode
# -c 10: Send 10 packets
```

**Interview Answer Example:**
```
Interviewer: How do you troubleshoot network connectivity?

Your Answer:
"I start with 'ping' to check if the destination is reachable.
For example, 'ping -c 4 google.com' sends 4 packets and shows if
the host responds and the response time.

If ping works but there's an issue, I use 'traceroute' to see the
path packets take. This helps identify which hop is causing the problem.

For more detailed troubleshooting, I use 'mtr' which combines ping
and traceroute. It shows packet loss and latency at each hop, which
helps pinpoint network issues."
```

**Troubleshooting Scenarios:**

**Scenario 1: Can't reach website**
```bash
# Step 1: Check if internet works
ping 8.8.8.8                    # Ping IP (tests connectivity)
ping google.com                 # Ping domain (tests DNS)

# Step 2: If IP works but domain doesn't
# → DNS issue (see Q4)

# Step 3: If both fail
ping 192.168.1.1                # Ping gateway
# If gateway fails → Local network issue
```

**Scenario 2: Slow connection**
```bash
# Check latency
ping -c 10 google.com
# Look at time values (lower is better)

# Trace path to find slow hop
traceroute google.com
# Look for hop with high response time
```

**Scenario 3: Intermittent connection**
```bash
# Continuous ping
ping google.com
# Watch for dropped packets (request timeout)

# Use mtr for better analysis
sudo mtr google.com
# Shows packet loss at each hop
```

### **Use**
- **Test Connectivity**: Check if host is reachable
- **Diagnose Issues**: Find where network fails
- **Measure Latency**: Check response time
- **Packet Loss**: Identify network problems
- **Path Analysis**: See route to destination

### **Command**
```bash
# ping - test connectivity
ping google.com                 # Continuous ping
ping -c 4 google.com            # Send 4 pings
ping -i 2 google.com            # Ping every 2 seconds
ping 8.8.8.8                    # Ping IP (bypasses DNS)

# traceroute - trace path
traceroute google.com           # Trace route
traceroute -I google.com        # Use ICMP
traceroute -n google.com        # Don't resolve names (faster)

# mtr - continuous traceroute
sudo mtr google.com             # Interactive mode
sudo mtr -r -c 10 google.com    # Report mode
sudo mtr -n google.com          # Don't resolve names

# Real-world troubleshooting
# Step 1: Check connectivity
ping -c 4 8.8.8.8

# Step 2: Check DNS
ping -c 4 google.com

# Step 3: Check gateway
ping -c 4 192.168.1.1

# Step 4: Trace route
traceroute google.com

# Step 5: Continuous monitoring
ping google.com
# Press Ctrl+C to stop

# Check latency
ping -c 10 google.com | grep "time="
# Shows response times

# Find problematic hop
sudo mtr -r -c 20 google.com
# Shows which hop has packet loss/latency
```

---

## Q6. How to Find Which Process is Using a Port?

### **Definition**
Command to identify which application/process is listening on or using a specific port.

### **Explain (Detail - Interview Answer)**

**Why Find Process Using Port?**
```
Common scenarios:
1. Port conflict - service won't start because port already in use
2. Security check - unknown process listening on port
3. Troubleshooting - which service is using which port
4. Cleanup - kill process using old/unused port
```

**Methods to Find Process:**

**Method 1: Using ss (Recommended)**
```bash
# Find process using port 80
sudo ss -tlnp | grep :80

# Output:
# LISTEN 0  128  0.0.0.0:80  0.0.0.0:*  users:(("nginx",pid=1234,fd=6))
#                                    │      │    │    │
#                                    │      │    │    └─ File descriptor
#                                    │      │    └────── Process ID
#                                    │      └─────────── Process name
#                                    └────────────────── Port being used
```

**Method 2: Using netstat**
```bash
# Find process using port 8080
sudo netstat -tlnp | grep :8080

# Similar output to ss
```

**Method 3: Using lsof (List Open Files)**
```bash
# List files opened by processes on port 22
sudo lsof -i :22

# Output:
# COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
# sshd     1234 root    3u  IPv4  12345      0t0  TCP *:22 (LISTEN)
#          │    │     │    │     │
#          │    │     │    │     └─ Network socket info
#          │    │     │    └─────────── Type (IPv4/TCP)
#          │    │     └──────────────── File descriptor
#          │    └───────────────────── User running process
#          └───────────────────────── Command name
```

**Method 4: Using fuser**
```bash
# Find process using port
sudo fuser 80/tcp

# Output shows PIDs using that port
```

**Interview Answer Example:**
```
Interviewer: How do you find which process is using a port?

Your Answer:
"I use 'ss -tlnp' command with grep to find the process. For example,
'sudo ss -tlnp | grep :8080' will show which process is listening on
port 8080, along with the PID and process name.

The -p flag shows the process information, -l shows listening sockets,
-t shows TCP, and -n shows numeric ports. I can also use 'lsof -i :8080'
which provides similar information about which process is using the port."
```

**Real-World Scenarios:**

**Scenario 1: Port conflict**
```bash
# Application fails to start: "Port 8080 already in use"

# Find what's using port 8080
sudo ss -tlnp | grep :8080

# Output: java (PID 5432) using port 8080

# Options:
# 1. Kill the process: sudo kill 5432
# 2. Use different port for new app
# 3. Restart the old service properly
```

**Scenario 2: Unknown process on port**
```bash
# Suspicious port found
sudo ss -tlnp | grep :3333

# If unknown process, investigate:
ps aux | grep 1234              # Check process details
lsof -p 1234                   # Check files opened
systemctl status service_name   # Check if it's a service
```

**Scenario 3: Multiple processes on same port?**
```bash
# Usually only one process can listen on a port
# But multiple connections (from different clients) to same port

# Show all connections to port 80
sudo ss -tnp | grep :80

# Shows established connections, not just listening
```

### **Use**
- **Port Conflicts**: Find why service won't start
- **Security**: Identify unknown processes
- **Troubleshooting**: Know which service uses which port
- **Cleanup**: Kill unused processes
- **Documentation**: Record process-port mappings

### **Command**
```bash
# Find process using port
sudo ss -tlnp | grep :8080       # Using ss (recommended)
sudo netstat -tlnp | grep :8080  # Using netstat
sudo lsof -i :8080               # Using lsof
sudo fuser 8080/tcp              # Using fuser

# Find all listening ports
sudo ss -tlnp                    # All listening ports with processes

# Find connections to specific port
sudo ss -tnp | grep :80          # All connections to port 80

# Find process by port name
sudo ss -tlnp | grep nginx       # Find all nginx ports

# Check multiple ports
sudo ss -tlnp | grep -E ':(80|443|8080)'

# Kill process using port
sudo kill $(sudo lsof -t -i:8080)  # Kill process on port 8080

# Real-world troubleshooting
# Step 1: Application fails to start
# Error: "Address already in use"

# Step 2: Find what's using the port
sudo ss -tlnp | grep :8080

# Step 3: Identify process
ps aux | grep PID

# Step 4: Decide action
sudo kill PID                   # Or use different port

# Step 5: Verify port is free
sudo ss -tlnp | grep :8080
```

---

## Q7. What is SSH and How to Connect to Remote Server?

### **Definition**
SSH (Secure Shell) - Secure protocol for remote login and command execution.

### **Explain (Detail - Interview Answer)**

**What is SSH?**
```
SSH (Secure Shell):
- Secure way to access remote servers
- Encrypted connection (all data is encrypted)
- Replaces insecure protocols like Telnet
- Uses port 22 by default
- Provides authentication (password or keys)

How it works:
1. Client connects to server on port 22
2. Server presents its public key
3. Client verifies server identity
4. Client authenticates (password or key)
5. Secure encrypted connection established
```

**Basic SSH Connection:**
```bash
# Connect to remote server
ssh username@remote_server

# Example:
ssh user@192.168.1.100        # Connect by IP
ssh user@server.example.com    # Connect by domain
ssh root@192.168.1.100         # Connect as root

# Connection process:
# 1. First time: "The authenticity of host... can't be established"
# 2. Type "yes" to accept server's fingerprint
# 3. Enter password when prompted
# 4. Connected! You're now on remote server
```

**SSH Key Authentication:**
```
Why use SSH keys?
- More secure than passwords
- No need to enter password every time
- Better for automation/scripts
- Can revoke access by removing key

How SSH keys work:
1. Generate key pair (private + public)
2. Public key goes on server
3. Private key stays on your computer
4. Server checks if your private key matches its public key
5. If match → Access granted
```

**Generate SSH Keys:**
```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Options:
# -t rsa       → Key type (RSA recommended)
# -b 4096      → Key size (4096 bits = very secure)
# -C comment   → Label/comment for key

# Prompts:
# 1. Save location: press Enter (default ~/.ssh/id_rsa)
# 2. Passphrase: press Enter (no passphrase) or type password
# 3. Confirm passphrase

# Keys generated:
# ~/.ssh/id_rsa      → Private key (NEVER share!)
# ~/.ssh/id_rsa.pub  → Public key (share this one)
```

**Copy Public Key to Server:**
```bash
# Method 1: ssh-copy-id (easiest)
ssh-copy-id username@remote_server

# Method 2: Manual copy
# Step 1: View your public key
cat ~/.ssh/id_rsa.pub

# Step 2: On remote server, add to authorized_keys
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "your_public_key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

**Connect with SSH Key:**
```bash
# After copying key, connect without password
ssh username@remote_server

# If you have multiple keys, specify which to use
ssh -i ~/.ssh/my_key username@remote_server

# Or use SSH config file (see below)
```

**SSH Config File:**
```bash
# Create ~/.ssh/config file
nano ~/.ssh/config

# Add hosts:
Host myserver
    HostName 192.168.1.100
    User myuser
    IdentityFile ~/.ssh/id_rsa

Host webserver
    HostName server.example.com
    User root
    Port 2222

# Now connect easily:
ssh myserver                   # Uses config above
ssh webserver                  # Uses config above
```

**Common SSH Operations:**
```bash
# Connect with specific port
ssh -p 2222 user@server

# Run command on remote server
ssh user@server "ls -la"

# Copy file to remote server
scp local_file.txt user@server:/remote/path/

# Copy file from remote server
scp user@server:/remote/file.txt ./

# Copy directory (recursive)
scp -r local_dir/ user@server:/remote/path/
```

**Interview Answer Example:**
```
Interviewer: What is SSH and how do you use it?

Your Answer:
"SSH (Secure Shell) is a protocol for securely accessing remote servers
over an encrypted connection. It's the standard way to manage Linux servers
remotely.

To connect, I use 'ssh username@server' and enter my password. For
better security, I use SSH key authentication. I generate keys with
'ssh-keygen', copy the public key to the server with 'ssh-copy-id',
and then I can connect without entering a password.

I can also run commands remotely, copy files with SCP, and configure
common hosts in the SSH config file for easier access."
```

**Security Best Practices:**
```
✅ DO:
- Use SSH keys instead of passwords
- Disable root SSH login
- Use non-standard SSH port
- Use key passphrase for extra security
- Keep private key secure (never share!)

❌ DON'T:
- Use weak passwords
- Allow root SSH login
- Share your private key
- Use default port 22 (change it)
- Leave unused SSH keys
```

### **Use**
- **Remote Access**: Login to servers remotely
- **File Transfer**: Copy files with SCP/SFTP
- **Remote Commands**: Execute commands on remote servers
- **Automation**: Script remote operations
- **Tunneling**: Secure port forwarding
- **System Administration**: Manage servers remotely

### **Command**
```bash
# SSH connection
ssh user@server                 # Basic connection
ssh -p 2222 user@server        # With custom port
ssh user@192.168.1.100         # With IP address

# Generate SSH keys
ssh-keygen -t rsa -b 4096      # Generate key pair
ssh-keygen -t ed25519         # Modern key type

# Copy public key to server
ssh-copy-id user@server        # Easiest method

# Connect with key (no password)
ssh user@server                # Works if key copied

# Run remote command
ssh user@server "ls -la"
ssh user@server "df -h"

# File transfer (SCP)
scp file.txt user@server:/path/          # Upload
scp user@server:/path/file.txt .          # Download
scp -r dir/ user@server:/path/           # Upload directory

# SSH config
nano ~/.ssh/config                       # Edit config
ssh myserver                             # Connect using config

# Check SSH connection
ssh -v user@server                      # Verbose (debug)

# Real-world setup
# Step 1: Generate keys
ssh-keygen -t rsa -b 4096

# Step 2: Copy to server
ssh-copy-id user@server

# Step 3: Test connection (no password)
ssh user@server

# Step 4: Configure SSH config
cat >> ~/.ssh/config << EOF
Host myserver
    HostName server.example.com
    User myuser
EOF

# Step 5: Easy connect
ssh myserver
```

---

## Q8. How to View and Manage Network Connections?

### **Definition**
Commands to view established, listening, and network connection states.

### **Explain (Detail - Interview Answer)**

**Connection States:**
```
TCP Connection States:

LISTEN:
- Waiting for incoming connections
- Server listening on port
- Example: Web server listening on port 80

ESTABLISHED:
- Active connection between client and server
- Data can be transferred
- Example: Connected to google.com

TIME_WAIT:
- Connection closed, waiting for final packets
- Normal part of closing connection
- Usually waits 2-4 minutes

CLOSE_WAIT:
- Remote side closed connection
- Waiting for local side to close

FIN_WAIT:
- Connection closing
- Waiting for remote acknowledgment
```

**View All Connections:**
```bash
# Show all TCP connections
ss -tn

# Show all TCP and UDP
ss -tun

# Show with process info
ss -tunp

# Show listening and established
ss -tan
```

**Connection Output Explained:**
```bash
$ ss -tn
State      Recv-Q Send-Q Local Address:Port   Peer Address:Port
ESTAB      0      0      192.168.1.100:54321  142.250.185.238:443
│          │      │      │            │       │              │
│          │      │      │            │       │              └─ Remote port
│          │      │      │            │       └───────────────── Remote IP
│          │      │      │            └────────────────────────── Local port
│          │      │      └─────────────────────────────────────── Local IP
│          │      └────────────────────────────────────────────── Send queue
│          └────────────────────────────────────────────────────── Receive queue
└───────────────────────────────────────────────────────────────── State
```

**Count Connections:**
```bash
# Count all connections
ss -tun | wc -l

# Count by state
ss -tan | awk 'NR>1 {print $1}' | sort | uniq -c
# Output:
#      5 ESTABLISHED
#     10 TIME_WAIT
#      3 LISTEN

# Count connections to specific port
ss -tn | grep :80 | wc -l
```

**Filter Connections:**
```bash
# Show established connections only
ss -tn state established

# Show connections to specific IP
ss -tn dst 192.168.1.100

# Show connections from specific IP
ss -tn src 192.168.1.100

# Show connections to specific port
ss -tn dst :443
ss -tn '( dport = :443 )'        # Same as above

# Show connections from specific port
ss -tn sport :54321
```

**Monitor Connections in Real-time:**
```bash
# Watch connections
watch -n 1 'ss -tn'

# Show summary
ss -s
# Total: 150
# TCP: 100 (estab 80, closed 20)
# UDP: 50
```

**Interview Answer Example:**
```
Interviewer: How do you view network connections?

Your Answer:
"I use 'ss' command to view network connections. 'ss -tnp' shows all
TCP connections with process information, including the state of each
connection.

Common states I look for are ESTABLISHED (active connections), LISTEN
(ports waiting for connections), and TIME_WAIT (recently closed
connections). I can also filter by specific ports or IPs using grep
or ss filters.

For monitoring, I use 'ss -s' to get a summary of all connections,
or 'watch ss -tn' to see connections updating in real-time."
```

**Real-World Scenarios:**

**Scenario 1: Too many TIME_WAIT connections**
```bash
# Check connection states
ss -tan | awk 'NR>1 {print $1}' | sort | uniq -c

# If many TIME_WAIT:
# - Normal for busy web server
# - Can be adjusted with kernel parameters
# - Usually not a problem unless port exhaustion
```

**Scenario 2: Find connections to specific service**
```bash
# All connections to web server (port 80)
ss -tn dst :80

# Show details
ss -tnp dst :80
```

**Scenario 3: Monitor connection spikes**
```bash
# Real-time monitoring
watch -n 5 'ss -s'

# Or log to file
while true; do ss -s >> connections.log; sleep 60; done
```

### **Use**
- **Monitor Traffic**: See active connections
- **Troubleshoot**: Identify connection issues
- **Security**: Detect unusual connections
- **Performance**: Check connection counts
- **Debugging**: Track connection states

### **Command**
```bash
# View connections
ss -tn                          # All TCP connections
ss -tun                         # TCP + UDP
ss -tunp                        # With process info
ss -tan                         # All TCP with state

# Count connections
ss -tun | wc -l                 # Total connections
ss -s                           # Summary statistics

# Filter by state
ss -tn state established        # Established only
ss -tn state time-wait          # TIME_WAIT only

# Filter by port
ss -tn dst :80                  # Connections to port 80
ss -tn src :54321               # From local port

# Filter by IP
ss -tn dst 192.168.1.100        # To specific IP
ss -tn src 192.168.1.100        # From specific IP

# Count by state
ss -tan | awk 'NR>1 {print $1}' | sort | uniq -c

# Real-time monitoring
watch -n 1 'ss -tn'            # Update every second
watch -n 5 'ss -s'             # Update summary

# Find specific connections
ss -tnp | grep :443            # HTTPS connections
ss -tnp | grep ESTAB           # Established connections

# Real-world monitoring
# Step 1: Check connection summary
ss -s

# Step 2: Check connection states
ss -tan | awk 'NR>1 {print $1}' | sort | uniq -c

# Step 3: Find connections to specific service
ss -tnp | grep nginx

# Step 4: Monitor in real-time
watch -n 5 'ss -s'
```

---

## Q9. What is Firewall and How to Configure (iptables, ufw)?

### **Definition**
Firewall - Security system that controls incoming and outgoing network traffic.

### **Explain (Detail - Interview Answer)**

**What is Firewall?**
```
Firewall:
- Network security system
- Controls traffic based on rules
- Allows or blocks connections
- Protects server from unauthorized access

How it works:
1. Network packet arrives
2. Firewall checks rules
3. If rule says allow → packet passes
4. If rule says block → packet dropped

Like a security guard at building entrance:
- Checks everyone entering/exiting
- Allows authorized people
- Blocks unauthorized people
```

**iptables (Low-level):**
```
What is it?
- Linux kernel firewall
- Very powerful and flexible
- Complex syntax
- Used on most Linux systems

Concepts:
- Tables: Collection of chains
- Chains: List of rules (INPUT, OUTPUT, FORWARD)
- Rules: Conditions and actions
- Targets: What to do (ACCEPT, DROP, REJECT)
```

**Basic iptables Commands:**
```bash
# View current rules
sudo iptables -L -n -v
# -L: List rules
# -n: Numeric (don't resolve names)
# -v: Verbose (show packet counts)

# View specific chain
sudo iptables -L INPUT -n -v

# Common chains:
# INPUT    → Incoming traffic to your server
# OUTPUT   → Outgoing traffic from your server
# FORWARD  → Traffic passing through (router/gateway)
```

**iptables Rules:**
```bash
# Allow SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# -A INPUT  → Append to INPUT chain
# -p tcp    → Protocol is TCP
# --dport 22 → Destination port 22
# -j ACCEPT → Jump to ACCEPT (allow)

# Allow HTTP (port 80)
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Allow HTTPS (port 443)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Drop all other incoming traffic
sudo iptables -A INPUT -j DROP

# Delete rule (by line number)
sudo iptables -D INPUT 3           # Delete rule 3

# Flush all rules (clear all)
sudo iptables -F

# Save rules (persist after reboot)
sudo iptables-save > /etc/iptables/rules.v4
```

**ufw (Uncomplicated Firewall - Easy):**
```
What is it?
- Simplified firewall for Ubuntu/Debian
- Easy to use
- Good for beginners
- Frontend for iptables

Why use ufw?
- Simple commands
- Easy to understand
- Good for most use cases
```

**ufw Commands:**
```bash
# Enable/disable firewall
sudo ufw enable                   # Enable firewall
sudo ufw disable                  # Disable firewall
sudo ufw status                   # Check status

# Allow services
sudo ufw allow ssh                # Allow SSH (port 22)
sudo ufw allow http               # Allow HTTP (port 80)
sudo ufw allow https              # Allow HTTPS (port 443)

# Allow specific port
sudo ufw allow 8080/tcp          # Allow port 8080 TCP
sudo ufw allow 3306               # Allow MySQL

# Deny port
sudo ufw deny 23                  # Block Telnet

# Allow from specific IP
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24 to any port 22

# Delete rule
sudo ufw delete allow 8080

# Reset to defaults
sudo ufw reset

# Show numbered rules
sudo ufw status numbered
```

**Interview Answer Example:**
```
Interviewer: How do you configure firewall on Linux?

Your Answer:
"On Ubuntu systems, I use 'ufw' which is simpler. I start with 'sudo
ufw enable' to turn on the firewall, then allow specific ports like
SSH with 'sudo ufw allow ssh', HTTP with 'sudo ufw allow http', and
HTTPS with 'sudo ufw allow https'.

On other systems, I use 'iptables' which is more powerful but
complex. For example, to allow SSH I use 'sudo iptables -A INPUT -p
tcp --dport 22 -j ACCEPT'. I always make sure to allow SSH before
enabling the firewall so I don't lock myself out."
```

**Important: Don't Lock Yourself Out!**
```
✅ ALWAYS do this before enabling firewall:
1. Allow SSH (port 22) FIRST
2. Then enable firewall
3. Test SSH connection
4. If fails, you need console access to fix

Wrong way:
ufw enable           # Blocks everything!
ufw allow ssh        # Too late if you're locked out

Right way:
ufw allow ssh        # Allow SSH first
ufw enable           # Then enable
# Test SSH connection
# If still can connect → Good!
```

**Common Firewall Rules:**
```bash
# Basic server firewall
sudo ufw default deny incoming      # Block all incoming
sudo ufw default allow outgoing       # Allow all outgoing
sudo ufw allow ssh                   # Allow SSH
sudo ufw allow http                  # Allow web server
sudo ufw allow https                 # Allow secure web
sudo ufw enable                      # Enable firewall

# Web server only
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw deny 22                     # Block SSH if not needed

# Database server
sudo ufw allow from 192.168.1.0/24 to any port 3306
```

### **Use**
- **Security**: Protect server from unauthorized access
- **Access Control**: Allow only necessary ports
- **Network Security**: Block malicious traffic
- **Compliance**: Meet security requirements
- **Monitoring**: Track allowed/blocked traffic

### **Command**
```bash
# ufw (Ubuntu/Debian - recommended)
sudo ufw enable                   # Enable firewall
sudo ufw disable                  # Disable firewall
sudo ufw status                   # Check status
sudo ufw status numbered          # Show numbered rules

# Allow services/ports
sudo ufw allow ssh                # Allow SSH
sudo ufw allow 22                 # Allow port 22
sudo ufw allow http               # Allow HTTP (80)
sudo ufw allow https              # Allow HTTPS (443)
sudo ufw allow 8080/tcp          # Allow specific port

# Deny
sudo ufw deny 23                  # Block Telnet
sudo ufw deny from 192.168.1.50  # Block specific IP

# Allow from specific network
sudo ufw allow from 192.168.1.0/24
sudo ufw allow from 192.168.1.0/24 to any port 22

# Delete rules
sudo ufw delete allow 8080
sudo ufw delete 1                 # Delete rule number 1

# Reset
sudo ufw reset                    # Reset to defaults

# iptables (all systems)
sudo iptables -L -n -v            # List rules
sudo iptables -F                  # Flush (clear) all
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -j DROP
sudo iptables-save > /etc/iptables/rules.v4

# Real-world setup
# Step 1: Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Step 2: Allow necessary ports
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

# Step 3: Enable firewall
sudo ufw enable

# Step 4: Verify
sudo ufw status

# Step 5: Test SSH connection
ssh user@server
```

---

## Q10. What is ARP and How to View ARP Table?

### **Definition**
ARP (Address Resolution Protocol) - Maps IP addresses to MAC addresses on local network.

### **Explain (Detail - Interview Answer)**

**What is ARP?**
```
ARP (Address Resolution Protocol):
- Maps IP addresses to MAC addresses
- Works on local network only
- Essential for Ethernet communication

Why do we need it?
- Devices communicate using MAC addresses on Ethernet
- But we use IP addresses to identify devices
- ARP converts IP → MAC

Example:
You want to communicate with 192.168.1.100
Your computer needs the MAC address of 192.168.1.100
ARP finds it: "Who has 192.168.1.100?"
192.168.1.100 replies: "I have it, my MAC is 00:11:22:33:44:55"
Now your computer can send data to that MAC address
```

**How ARP Works:**
```
Step 1: Computer A wants to send to IP 192.168.1.100
Step 2: Check ARP cache (already know MAC?)
Step 3: If not, broadcast ARP request
Step 4: All devices receive: "Who has 192.168.1.100?"
Step 5: 192.168.1.100 replies: "I have it, MAC is 00:11:22:33:44:55"
Step 6: Computer A updates ARP cache
Step 7: Computer A sends data to MAC 00:11:22:33:44:55
```

**View ARP Table:**
```bash
# View ARP table
arp -a

# Output:
# ? (192.168.1.1) at 00:11:22:33:44:55 [ether] on eth0
# ? (192.168.1.100) at aa:bb:cc:dd:ee:ff [ether] on eth0
#  │      │               │                  │        │
#  │      │               │                  │        └─ Interface
#  │      │               │                  └─ Connection type
#  │      │               └───────────────────── MAC address
#  │      └─────────────────────────────────── IP address
#  └─────────────────────────────────────────── Host type (? = dynamic)
```

**ip neigh (Modern Command):**
```bash
# View neighbor cache (ARP table)
ip neigh show
ip n                           # Short form

# Output:
# 192.168.1.1 dev eth0 lladdr 00:11:22:33:44:55 REACHABLE
# 192.168.1.100 dev eth0 lladdr aa:bb:cc:dd:ee:ff STALE
#              │      │                    │
#              │      │                    └─ State
#              │      └─────────────────────── MAC address
#              └────────────────────────────── Interface

# States:
# REACHABLE   → Active, recent communication
# STALE       → Old entry, needs verification
# FAILED      → Can't reach device
```

**ARP Cache Management:**
```bash
# Add static ARP entry
sudo arp -s 192.168.1.100 00:11:22:33:44:55

# Delete ARP entry
sudo arp -d 192.168.1.100

# Clear ARP cache
sudo ip neigh flush all

# View detailed ARP
arp -n
arp -vn
```

**Interview Answer Example:**
```
Interviewer: What is ARP and how do you check the ARP table?

Your Answer:
"ARP stands for Address Resolution Protocol. It maps IP addresses to
MAC addresses on a local network. When my computer wants to communicate
with another device on the same network, it needs the MAC address, not
just the IP address. ARP finds this mapping.

To view the ARP table, I use 'arp -a' or the modern 'ip neigh show'
command. This shows all the IP to MAC mappings my computer has learned,
along with the state of each entry (REACHABLE, STALE, etc.).

I can also clear the ARP cache with 'ip neigh flush all' if needed,
which forces the system to relearn all ARP entries."
```

**Real-World Scenarios:**

**Scenario 1: Device not responding**
```bash
# Check if MAC is correct
arp -a | grep 192.168.1.100

# Clear ARP and try again
sudo ip neigh flush all
ping 192.168.1.100

# Check new ARP entry
ip neigh show | grep 192.168.1.100
```

**Scenario 2: Duplicate IP detected**
```bash
# Check ARP table for duplicate MACs
arp -a

# If same MAC has multiple IPs → duplicate IP problem
```

**Scenario 3: Static ARP for security**
```bash
# Add static entry for important server
sudo arp -s 192.168.1.10 00:11:22:33:44:55

# Now ARP spoofing won't work for this IP
```

### **Use**
- **Network Debugging**: Check IP-MAC mappings
- **Troubleshooting**: Identify ARP issues
- **Security**: Detect ARP spoofing
- **Cache Management**: Clear stale entries
- **Network Discovery**: See devices on local network

### **Command**
```bash
# View ARP table
arp -a                          # All ARP entries
arp -n                          # Numeric (no DNS)
ip neigh show                   # Modern method
ip n                            # Short form

# View specific IP
arp -a | grep 192.168.1.1
ip neigh show | grep 192.168.1.1

# Manage ARP cache
sudo arp -d 192.168.1.100        # Delete entry
sudo ip neigh flush all          # Clear all ARP
sudo ip neigh flush dev eth0     # Clear specific interface

# Add static entry
sudo arp -s 192.168.1.100 00:11:22:33:44:55

# Detailed output
arp -vn

# Monitor ARP
watch -n 2 'arp -a'

# Real-world troubleshooting
# Step 1: Check ARP table
arp -a

# Step 2: Find specific device
arp -a | grep 192.168.1.100

# Step 3: If wrong MAC, clear cache
sudo ip neigh flush all

# Step 4: Ping device to relearn ARP
ping -c 1 192.168.1.100

# Step 5: Verify new entry
ip neigh show | grep 192.168.1.100
```

---

## 📚 Quick Reference - Module 5 Commands

### Network Configuration
```bash
ip addr show                    # View IP addresses
ifconfig                        # Old method
ip route show                   # View routes/gateway
```

### DNS
```bash
nslookup domain.com             # DNS lookup
dig domain.com                  # Detailed DNS
cat /etc/resolv.conf            # DNS servers
ping domain.com                 # Test DNS
```

### Connectivity
```bash
ping host                       # Test connectivity
traceroute host                 # Trace path
mtr host                       # Continuous trace
```

### Ports & Connections
```bash
ss -tlnp                        # Listening ports
ss -tunp                        # All connections
netstat -tlnp                   # Old method
lsof -i :port                   # Find process on port
```

### SSH
```bash
ssh user@server                 # Connect
ssh-keygen                      # Generate keys
ssh-copy-id user@server        # Copy key
scp file user@server:/path/    # Copy file
```

### Firewall
```bash
sudo ufw status                 # Check firewall
sudo ufw allow 22               # Allow port
sudo ufw enable                 # Enable
iptables -L -n -v               # View iptables
```

### ARP
```bash
arp -a                          # View ARP table
ip neigh show                   # Modern ARP
ip neigh flush all              # Clear ARP
```

---

**Next:** Module 6 - Services, Boot & Systemctl
