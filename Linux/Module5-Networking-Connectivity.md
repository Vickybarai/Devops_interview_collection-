# 🟦 MODULE 5: Networking & Connectivity

> Complete interview preparation - Simple and Easy to Understand

---

## Q1. How to Configure Network Interface (ip, ifconfig, nmcli)?

### **Definition**
Commands to configure and manage network interfaces in Linux.

### **Explain (Detail)**

**Network Interface Basics:**
```
Network Interface = Connection point to network
Examples: eth0, ens33, wlan0, lo

Each interface:
- Has IP address
- Has MAC address
- Has network mask
- Can be up or down
- Connected to network
```

**ip Command (Modern):**
```
Recommended for modern Linux systems
Part of iproute2 package
Replaces many old commands (ifconfig, route, netstat)

Key commands:
- ip addr         → Show/manage IP addresses
- ip link         → Show/manage interfaces
- ip route        → Show/manage routing
- ip neigh        → Show/manage ARP table
```

**ifconfig Command (Deprecated):**
```
Older tool, mostly deprecated
Still available on many systems
Simple to use but limited

Shows:
- IP address
- Netmask
- MAC address
- Interface status (UP/DOWN)
- RX/TX statistics
```

**nmcli Command (NetworkManager):**
```
NetworkManager CLI tool
Used on Ubuntu, RHEL, Fedora desktops
Manages network connections
Can create, modify, delete connections
```

**ip addr Examples:**
```
Show all interfaces:
ip addr show
ip a

Show specific interface:
ip addr show eth0

Add IP address:
sudo ip addr add 192.168.1.10/24 dev eth0

Remove IP address:
sudo ip addr del 192.168.1.10/24 dev eth0

Bring interface up:
sudo ip link set eth0 up

Bring interface down:
sudo ip link set eth0 down

Set MAC address:
sudo ip link set dev eth0 address 00:11:22:33:44:55
```

**ifconfig Examples:**
```
Show all interfaces:
ifconfig

Show specific interface:
ifconfig eth0

Set IP address:
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0

Bring interface up:
sudo ifconfig eth0 up

Bring interface down:
sudo ifconfig eth0 down

View interface details:
ifconfig -a
```

**nmcli Examples:**
```
List connections:
nmcli connection show

List devices:
nmcli device show

Show device status:
nmcli device status

Connect to network:
nmcli connection up "Connection Name"

Disconnect:
nmcli connection down "Connection Name"

Add new connection:
nmcli connection add type ethernet ifname eth0 con-name "My Connection" ip4 192.168.1.10/24
```

**Network Files:**
```
/etc/network/interfaces    → Debian/Ubuntu network config
/etc/sysconfig/network-scripts/ → RHEL/CentOS config
/etc/netplan/*.yaml          → Ubuntu 18.04+
/etc/NetworkManager/system-connections/ → Connection profiles
```

**Interface States:**
```
UP    → Interface is active and connected
DOWN  → Interface is disabled
UNKNOWN → Can't determine status
```

### **Use**
- **Configure IP**: Set network IP addresses
- **Manage Interfaces**: Bring up/down network cards
- **Troubleshooting**: Check network configuration
- **View Status**: Monitor interface state
- **NetworkManager**: GUI systems with dynamic networks

### **Command**
```bash
# ip command (modern)
ip addr show                 # Show all interfaces and IPs
ip link show                 # Show interface status
ip addr show eth0            # Show specific interface
sudo ip addr add 192.168.1.10/24 dev eth0    # Add IP
sudo ip addr del 192.168.1.10/24 dev eth0    # Remove IP
sudo ip link set eth0 up                 # Bring up
sudo ip link set eth0 down               # Bring down

# ifconfig command (older)
ifconfig                     # Show all interfaces
ifconfig eth0               # Show specific
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# nmcli command (NetworkManager)
nmcli connection show          # List connections
nmcli device show            # List devices
nmcli device status           # Device status
nmcli connection up "Wired connection 1"
nmcli connection down "Wired connection 1"

# View interface details
ip -br addr show            # Brief format
ip -details link show eth0  # Detailed info

# Network configuration files
cat /etc/network/interfaces    # Debian/Ubuntu
cat /etc/sysconfig/network-scripts/ifcfg-eth0  # RHEL/CentOS
cat /etc/netplan/01-netcfg.yaml  # Ubuntu 18.04+

# Check interface status
ip link show eth0 | grep state
cat /sys/class/net/eth0/operstate

# View MAC address
ip link show eth0 | grep link/ether
cat /sys/class/net/eth0/address

# View IP address only
ip -4 addr show eth0 | grep inet
ip -6 addr show eth0 | grep inet6
```

---

## Q2. How to Check Open Ports and List Services (netstat, ss)?

### **Definition**
Commands to display open ports, network connections, and listening services.

### **Explain (Detail)**

**Port Basics:**
```
Port = Communication endpoint for network
0-65535 possible ports
Types:
- TCP (Transmission Control Protocol) - Reliable
- UDP (User Datagram Protocol) - Fast, no guarantee

Common Ports:
22  → SSH
80  → HTTP
443 → HTTPS
3306 → MySQL
5432 → PostgreSQL
```

**netstat Command (Deprecated but still common):**
```
Shows:
- Network connections
- Routing tables
- Interface statistics
- Masquerade connections
- Multicast memberships

Options:
-t     → Show TCP connections
-u     → Show UDP connections
-l     → Show listening sockets
-p     → Show PID and program name
-n     → Show numeric (don't resolve)
-a     → Show all (listening and non-listening)
```

**ss Command (Modern Replacement):**
```
Newer, faster, more detailed
Recommended over netstat
Shows same information as netstat but better

Options similar to netstat:
-t, -u, -l, -p, -n, -a
Additional:
-s → Summary statistics
-m → Memory information
-o → Timer information
```

**Connection States:**
```
LISTEN     → Waiting for connection
ESTABLISHED → Connection established
TIME_WAIT  → Waiting to close
CLOSE_WAIT  → Waiting for close request
FIN_WAIT   → Closing connection
```

**netstat Examples:**
```
Show all listening TCP ports:
netstat -tlnp

Show all listening UDP ports:
netstat -ulnp

Show all TCP connections:
netstat -tanp

Show all UDP connections:
netstat -aunp

Show numeric (no DNS):
netstat -an

Show process name:
netstat -p

Show interface statistics:
netstat -i

Show routing table:
netstat -r

Summary:
netstat -s
```

**ss Examples:**
```
Show all listening TCP ports:
ss -tlnp

Show all listening UDP ports:
ss -ulnp

Show all TCP connections:
ss -tanp

Show all UDP connections:
ss -aunp

Show listening on specific port:
ss -tlnp | grep :22
ss -tlnp 'sport = :22'

Show process using port:
ss -tulpn | grep :8080

Show connections for specific IP:
ss -tn | grep 192.168.1.10

Summary statistics:
ss -s

Show established connections:
ss -state established

Show time wait connections:
ss -state time-wait

Memory information:
ss -m
```

**Finding Services Using Ports:**
```
Find what's using port 80:
sudo ss -tulpn | grep :80
sudo netstat -tlnp | grep :80

Find all listening ports:
sudo ss -tulnp
sudo netstat -tlnp

Find HTTP/HTTPS:
ss -tlnp | grep -E ':(80|443)'

Find SSH:
ss -tlnp | grep :22
```

**Filtering Results:**
```
Find process name:
netstat -p | grep nginx
ss -p | grep nginx

Find specific port:
netstat -an | grep 8080
ss -an | grep 8080

Find IP address:
netstat -an | grep 192.168.1.10
ss -tn | grep 192.168.1.10

Count connections:
netstat -an | grep ESTABLISHED | wc -l
ss -tn state established | wc -l
```

### **Use**
- **Check Services**: See what's listening on system
- **Troubleshooting**: Find conflicts, debug issues
- **Security**: Check for unauthorized services
- **Port Usage**: Identify which service uses which port
- **Network Debugging**: Understand active connections
- **Audit**: Monitor open ports

### **Command**
```bash
# netstat commands
netstat -tlnp                    # TCP listening with process
netstat -ulnp                    # UDP listening with process
netstat -tlnp | grep :22        # Check SSH
netstat -tlnp | grep :80        # Check HTTP
netstat -an                       # All connections (numeric)
netstat -tanp                     # All TCP with process
netstat -s                        # Summary statistics
netstat -i                        # Interface statistics
netstat -r                        # Routing table

# ss commands (modern)
ss -tlnp                        # TCP listening with process
ss -ulnp                        # UDP listening with process
ss -tulpn                        # All listening
ss -tnp                          # All TCP connections
ss -unp                          # All UDP connections
ss -tlnp 'sport = :22'          # Port 22
ss -tlnp | grep :80            # Port 80
ss -s                            # Summary
ss -m                            # Memory usage

# Find process using specific port
sudo ss -tulpn | grep :8080
sudo netstat -tlnp | grep :8080

# Find all web servers (HTTP/HTTPS)
sudo ss -tlnp | grep -E ':(80|443)'

# Count established connections
netstat -an | grep ESTABLISHED | wc -l
ss -tn state established | wc -l

# Show connections by state
ss -tn state all
netstat -an | awk '{print $6}' | sort | uniq -c | sort -nr

# Find suspicious connections (foreign IPs)
netstat -an | grep ESTABLISHED | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
ss -tn state established | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

# Check port conflicts
sudo ss -tulpn | awk '{print $5}' | cut -d: -f2 | sort | uniq -c | sort -nr | awk '$1 > 1'

# Monitor connections in real-time
watch -n 2 'ss -tlnp'
watch -n 2 'netstat -tlnp'

# Find all listening services
sudo netstat -tlnp | awk '{print $7}' | sort | uniq -c | sort -nr
sudo ss -tulpn | awk '{print $7}' | sort | uniq -c | sort -nr

# Check network statistics
netstat -i
ss -s
```

---

## Q3. How Does DNS Resolution Work (/etc/hosts, /etc/resolv.conf)?

### **Definition**
DNS (Domain Name System) - Translates domain names to IP addresses.

### **Explain (Detail)**

**DNS Resolution Process:**
```
1. Application wants to connect to google.com
2. Check /etc/hosts file first
3. If not found, check /etc/resolv.conf
4. Query DNS servers listed in resolv.conf
5. Get IP address
6. Connection established

Example:
google.com → 172.217.14.46
```

**/etc/hosts File:**
```
Purpose: Local hostname resolution
Format: IP address hostname aliases

Example:
127.0.0.1   localhost localhost.localdomain
127.0.1.1   localhost4
::1           localhost localhost.localdomain
192.168.1.10   webserver.example.com webserver
192.168.1.11   dbserver.example.com

Priority: Higher than DNS (checked first)
Use Cases:
- Local development
- Override DNS entries
- Block websites
- Test without DNS
- Offline system configuration
```

**/etc/resolv.conf File:**
```
Purpose: Configure DNS servers

Format:
nameserver 8.8.8.8
nameserver 8.8.4.4
search example.com
domain example.com
options timeout:2 attempts:3 rotate

Directives:
nameserver IP  → DNS server IP (max 3)
search domain   → Search domain suffix
domain domain   → Local domain
options        → Various options

Common options:
timeout:N     → Query timeout in seconds
attempts:N    → Retry attempts
rotate        → Rotate servers
single-request → Send queries to single server
```

**DNS Query Types:**
```
A Record    → IPv4 address
AAAA Record → IPv6 address
CNAME Record → Alias to another domain
MX Record   → Mail server
TXT Record  → Text data (SPF, DKIM)
NS Record   → Name server
SOA Record → Start of authority
```

**DNS Resolution Tools:**
```
host      → Simple DNS lookup
dig       → Advanced DNS lookup (recommended)
nslookup  → Traditional DNS lookup
getent    → Check system name service switch
```

**Common DNS Servers:**
```
8.8.8.8      → Google DNS
8.8.4.4       → Google DNS (backup)
1.1.1.1       → Cloudflare
1.0.0.1       → Cloudflare (backup)
208.67.222.222 → OpenDNS
```

**host Command Examples:**
```
Simple lookup:
host google.com

Reverse lookup:
host 8.8.8.8

Query specific DNS server:
host google.com 8.8.8.8

Query MX record:
host -t mx gmail.com

Query TXT record:
host -t txt google.com
```

**dig Command Examples:**
```
Simple lookup:
dig google.com

Short output:
dig +short google.com

Query specific DNS server:
dig @8.8.8.8 google.com

Query specific record:
dig A google.com
dig AAAA google.com
dig MX gmail.com
dig TXT google.com

Reverse lookup:
dig -x 8.8.8.8

Trace DNS path:
dig +trace google.com
```

**nslookup Command:**
```
Simple lookup:
nslookup google.com

Query specific server:
nslookup google.com 8.8.8.8

Interactive mode:
nslookup
> google.com
> set type=mx
> gmail.com
> exit
```

**getent Command:**
```
Check hosts resolution:
getent hosts google.com

Check DNS resolution:
getent ahosts google.com

Check all name services:
getent hosts
getent services
```

**DNS Caching:**
```
systemd-resolved: Cached DNS (modern systems)
nscd: Name Service Cache Daemon
dnsmasq: Lightweight DNS forwarder

Check cache (systemd-resolved):
resolvectl query-cache

Flush cache:
sudo systemctl restart systemd-resolved
```

**DNS Troubleshooting:**
```
1. Check /etc/hosts:
   cat /etc/hosts

2. Check /etc/resolv.conf:
   cat /etc/resolv.conf

3. Test DNS:
   ping google.com
   dig google.com

4. Test local resolution:
   ping localhost

5. Check DNS servers:
   resolvectl status

6. Check caching:
   resolvectl query-cache

7. Restart DNS service:
   sudo systemctl restart systemd-resolved
```

### **Use**
- **Domain Resolution**: Translate names to IPs
- **Local Configuration**: Override DNS locally
- **Development**: Test without DNS
- **Troubleshooting**: Debug DNS issues
- **Network Setup**: Configure DNS servers
- **Security**: Block malicious domains

### **Command**
```bash
# DNS resolution files
cat /etc/hosts                    # Local hosts
cat /etc/resolv.conf              # DNS servers

# Edit hosts file (add local entry)
echo "192.168.1.10 myapp.local" | sudo tee -a /etc/hosts

# Edit resolv.conf (add DNS server)
echo "nameserver 8.8.8.8" | sudo tee -a /etc/resolv.conf

# DNS lookup tools
host google.com                    # Simple lookup
dig google.com                      # Advanced lookup
nslookup google.com                 # Traditional lookup

# host examples
host google.com                    # A record
host -t mx gmail.com                # MX record
host -t txt google.com              # TXT record
host 8.8.8.8                       # Reverse lookup

# dig examples
dig google.com                      # Full output
dig +short google.com              # IP only
dig @8.8.8.8 google.com           # Query specific server
dig A google.com                   # A record
dig MX gmail.com                    # MX record
dig -x 8.8.8.8                     # Reverse lookup
dig +trace google.com              # Trace DNS path

# nslookup examples
nslookup google.com
nslookup google.com 8.8.8.8

# getent - check resolution
getent hosts google.com
getent hosts localhost
getent ahosts google.com

# DNS cache management (systemd-resolved)
resolvectl status                  # Check status
resolvectl query-cache           # View cache
sudo systemctl restart systemd-resolved   # Flush cache

# DNS troubleshooting
ping google.com                    # Test connectivity
dig google.com                      # Test DNS
dig @8.8.8.8 google.com           # Test specific server
host google.com                    # Alternative test

# Check DNS configuration
cat /etc/systemd/resolved.conf
cat /etc/nsswitch.conf

# Flush DNS cache (different methods)
sudo systemctl restart systemd-resolved  # systemd
sudo systemctl restart dnsmasq         # dnsmasq
sudo systemctl restart nscd             # nscd

# Block website (in /etc/hosts)
echo "127.0.0.1 facebook.com" | sudo tee -a /etc/hosts

# Local development setup
echo "127.0.0.1 dev.local" | sudo tee -a /etc/hosts
echo "127.0.0.1 test.local" | sudo tee -a /etc/hosts
```

---

## Q4. SSH Keys and Authentication - How Does It Work?

### **Definition**
SSH (Secure Shell) keys provide passwordless, secure authentication for remote access.

### **Explain (Detail)**

**SSH Authentication Methods:**
```
1. Password Authentication
   - Traditional method
   - Less secure (can be brute-forced)
   - User must enter password each time

2. SSH Key Authentication
   - More secure
   - Passwordless (after initial setup)
   - Uses public/private key pair
```

**SSH Key Pair:**
```
Public Key:
- Shared with servers
- Placed in ~/.ssh/authorized_keys
- Safe to share
- Used for encryption

Private Key:
- Kept secret on client
- Stored in ~/.ssh/id_rsa
- NEVER share with anyone
- Used for decryption
```

**SSH Key Generation:**
```
Key Types:
- RSA (older, larger keys)
- ED25519 (modern, smaller, more secure)

Key Generation:
ssh-keygen -t rsa -b 4096 -C "email@example.com"
ssh-keygen -t ed25519 -C "email@example.com"

Files Created:
~/.ssh/id_rsa          → Private key
~/.ssh/id_rsa.pub      → Public key
~/.ssh/id_ed25519      → Private key
~/.ssh/id_ed25519.pub  → Public key
```

**Key Generation Process:**
```
1. Generate key pair:
   ssh-keygen -t ed25519

2. (Optional) Set passphrase:
   Enter passphrase (recommended)
   Adds security if private key is stolen

3. Keys saved to ~/.ssh/

4. Copy public key to server:
   ssh-copy-id user@server

5. Test connection:
   ssh user@server
   Should connect without password!
```

**ssh-copy-id Command:**
```
Copy public key to server:
ssh-copy-id user@192.168.1.10

Copy specific key:
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server

Use specific port:
ssh-copy-id -p 2222 user@server
```

**Manual Key Copy:**
```
1. View public key:
   cat ~/.ssh/id_rsa.pub

2. Add to server:
   ssh user@server
   mkdir -p ~/.ssh
   echo "PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
```

**Authorized Keys File:**
```
Location: ~/.ssh/authorized_keys on server
Format: key-type key-string comment

Example:
ssh-rsa AAAAB3NzaC1yc2EAAA... user@example.com
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... user@example.com

Permissions:
~/.ssh/              → 700 (drwx------)
~/.ssh/authorized_keys → 600 (-rw-------)
```

**SSH Agent:**
```
Purpose: Manage keys and passphrases
No need to enter passphrase repeatedly

Start agent:
eval $(ssh-agent -s)

Add key to agent:
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_ed25519

List loaded keys:
ssh-add -l

Remove all keys:
ssh-add -D
```

**SSH Config File:**
```
Location: ~/.ssh/config

Format:
Host alias
    HostName server.example.com
    User username
    Port 2222
    IdentityFile ~/.ssh/id_rsa

Example:
Host webserver
    HostName 192.168.1.10
    User admin
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Use:
ssh webserver
```

**Multiple Keys:**
```
Generate different keys for different servers:
ssh-keygen -t ed25519 -f ~/.ssh/key_work -C "work"
ssh-keygen -t ed25519 -f ~/.ssh/key_personal -C "personal"

Use specific key:
ssh -i ~/.ssh/key_work user@server

Or use SSH config:
Host work
    HostName work.example.com
    IdentityFile ~/.ssh/key_work
```

**Key Best Practices:**
```
✅ Use ED25519 (modern)
✅ Set passphrase on private key
✅ Use SSH agent for convenience
✅ Use SSH config for organization
✅ Backup keys securely
✅ Use separate keys for different environments

❌ Don't share private key
❌ Don't commit private keys to git
❌ Don't use keys without passphrase on public machines
❌ Don't disable password auth without key setup first
```

**SSH Key Permissions:**
```
Critical for security:
~/.ssh                    → 700 (drwx------)
~/.ssh/id_rsa             → 600 (-rw-------)
~/.ssh/id_rsa.pub         → 644 (-rw-r--r--)
~/.ssh/authorized_keys     → 600 (-rw-------)

Fix permissions:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys
```

### **Use**
- **Passwordless Login**: No password prompts
- **Automation**: Scripts, cron jobs
- **Security**: More secure than passwords
- **Convenience**: Quick access to multiple servers
- **Git**: Access private repositories
- **CI/CD**: Automated deployments

### **Command**
```bash
# Generate SSH keys
ssh-keygen                           # Default RSA
ssh-keygen -t rsa -b 4096             # RSA 4096-bit
ssh-keygen -t ed25519 -C "email"     # ED25519 (recommended)
ssh-keygen -t ed25519 -f ~/.ssh/custom_key -C "comment"  # Custom name

# View public key
cat ~/.ssh/id_rsa.pub
cat ~/.ssh/id_ed25519.pub

# Copy public key to server
ssh-copy-id user@192.168.1.10
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server
ssh-copy-id -p 2222 user@server

# Test SSH connection (passwordless)
ssh user@server
ssh -v user@server       # Verbose mode (debug)

# SSH agent
eval $(ssh-agent -s)              # Start agent
ssh-add ~/.ssh/id_rsa             # Add key
ssh-add -l                          # List keys
ssh-add -D                          # Remove all keys

# SSH config
cat ~/.ssh/config                   # View config
nano ~/.ssh/config                   # Edit config

# Manual key copy (if ssh-copy-id not available)
cat ~/.ssh/id_rsa.pub | ssh user@server 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'

# Check key fingerprints
ssh-keygen -l -f ~/.ssh/id_rsa.pub
ssh-keygen -lf ~/.ssh/id_ed25519.pub

# Remove old host key (if changed)
ssh-keygen -R hostname

# Generate key with passphrase
ssh-keygen -t ed25519 -C "email"
# Enter passphrase when prompted

# Use specific key
ssh -i ~/.ssh/custom_key user@server

# Check permissions
ls -la ~/.ssh/
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys

# Verify SSH connection
ssh -T user@server 'echo "Connected!"'

# Port forwarding (SSH tunnel)
ssh -L 8080:localhost:80 user@server    # Local port forwarding
ssh -R 8080:localhost:80 user@server    # Remote port forwarding
```

---

## Q5. How to Configure Firewall (iptables, ufw, firewalld)?

### **Definition**
Firewall - Network security system that controls incoming/outgoing traffic.

### **Explain (Detail)**

**Firewall Basics:**
```
Purpose:
- Filter network traffic
- Block unwanted connections
- Allow only required services
- Protect from attacks

Types:
- iptables (low-level, universal)
- ufw (Ubuntu simple frontend)
- firewalld (RHEL/CentOS modern)
- nftables (successor to iptables)
```

**iptables (Low-Level):**
```
Most common Linux firewall
Works with kernel's netfilter
Chain-based system

Chains:
INPUT    → Incoming traffic
OUTPUT   → Outgoing traffic
FORWARD  → Forwarded traffic
PREROUTING  → Before routing
POSTROUTING → After routing

Actions:
ACCEPT   → Allow packet
DROP     → Silently drop packet
REJECT   → Drop and send rejection message
LOG      → Log packet
```

**iptables Commands:**
```
List rules:
iptables -L              # List all
iptables -L -n -v         # Numeric, verbose
iptables -L INPUT        # List INPUT chain

Add rule:
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # Allow SSH
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    # Allow HTTP

Drop all other traffic:
iptables -A INPUT -j DROP

Delete rule:
iptables -D INPUT -p tcp --dport 22 -j ACCEPT

Flush all rules:
iptables -F              # Flush
iptables -X              # Delete custom chains

Save rules:
iptables-save > /etc/iptables/rules.v4   # Debian/Ubuntu
service iptables save                  # RHEL/CentOS
```

**Common iptables Rules:**
```
Allow SSH:
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

Allow HTTP/HTTPS:
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

Allow loopback:
iptables -A INPUT -i lo -j ACCEPT

Allow established connections:
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

Drop all other:
iptables -A INPUT -j DROP
```

**ufw (Uncomplicated Firewall):**
```
Ubuntu default firewall
Simple, easy to use
Frontend for iptables

Commands:
ufw enable        # Enable firewall
ufw disable       # Disable firewall
ufw status        # Show status
ufw allow 22      # Allow SSH
ufw allow 80      # Allow HTTP
ufw deny 8080     # Deny port
ufw reload        # Reload rules
```

**ufw Examples:**
```
Enable firewall:
sudo ufw enable
sudo ufw status

Allow services:
sudo ufw allow ssh              # Allow SSH (port 22)
sudo ufw allow http             # Allow HTTP (port 80)
sudo ufw allow https            # Allow HTTPS (port 443)

Allow specific ports:
sudo ufw allow 22/tcp
sudo ufw allow 8080/tcp

Allow IP address:
sudo ufw allow from 192.168.1.10

Deny specific IP:
sudo ufw deny from 10.0.0.0/8

Allow from subnet to specific port:
sudo ufw allow from 192.168.1.0/24 to any port 3306

Delete rule:
sudo ufw delete allow 80

Reset to defaults:
sudo ufw reset
```

**firewalld (RHEL/CentOS/Fedora):**
```
Dynamic firewall manager
Zone-based configuration
Default frontend on RHEL-based systems

Zones:
public      → Public network, untrusted
dmz         → DMZ (demilitarized zone)
work        → Work network
home        → Home network
trusted     → All traffic allowed
internal    → Internal network
block       → All traffic blocked
drop        → All traffic dropped

Commands:
firewall-cmd --state              # Check status
firewall-cmd --get-active-zones   # Active zones
firewall-cmd --list-all           # All settings
firewall-cmd --reload             # Reload rules
firewall-cmd --permanent          # Permanent rules
```

**firewalld Examples:**
```
Check status:
sudo firewall-cmd --state
sudo firewall-cmd --list-all

Add service to current zone:
sudo firewall-cmd --add-service=http

Add service permanently:
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload

Add port to current zone:
sudo firewall-cmd --add-port=8080/tcp

Add port permanently:
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload

Remove service:
sudo firewall-cmd --permanent --remove-service=http
sudo firewall-cmd --reload

List active rules:
sudo firewall-cmd --list-all

Change default zone:
sudo firewall-cmd --set-default-zone=public

Panic mode (drop all):
sudo firewall-cmd --panic
```

**Firewall Zones Explanation:**
```
public:
- Untrusted network
- Only allowed services
- Default for public IP

dmz:
- Demilitarized zone
- Between trusted and untrusted
- Public-facing servers

trusted:
- All traffic allowed
- No restrictions
- Internal network

block:
- All incoming blocked
- Outgoing allowed
- Maximum security
```

**Common Firewall Rules:**
```
Web Server:
- Allow 80 (HTTP)
- Allow 443 (HTTPS)
- Allow 22 (SSH) (if needed)
- Block all else

Database Server:
- Allow 22 (SSH) - from admin IP only
- Allow 3306 (MySQL) - from app server only
- Block all else

Application Server:
- Allow 22 (SSH) - from VPN/admin IP
- Allow 8080 (App) - from load balancer
- Allow 443 (HTTPS)
- Block all else
```

### **Use**
- **Security**: Protect system from attacks
- **Access Control**: Limit who can connect
- **Compliance**: Meet security requirements
- **Network Segmentation**: Create network zones
- **Intrusion Prevention**: Block malicious IPs
- **Service Exposure**: Allow only necessary ports

### **Command**
```bash
# iptables (universal)
iptables -L                          # List rules
iptables -L -n -v                     # Numeric, verbose
iptables -L INPUT                   # List INPUT chain
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # Allow SSH
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    # Allow HTTP
iptables -A INPUT -p tcp --dport 443 -j ACCEPT   # Allow HTTPS
iptables -A INPUT -i lo -j ACCEPT                     # Allow loopback
iptables -A INPUT -j DROP                             # Drop all other
iptables -F                              # Flush all rules
iptables -X                              # Delete custom chains
iptables -L -n --line-numbers                         # List with line numbers

# Save/restore iptables
iptables-save > /etc/iptables/rules.v4
iptables-restore < /etc/iptables/rules.v4
service iptables save                    # RHEL/CentOS

# ufw (Ubuntu)
sudo ufw enable                       # Enable
sudo ufw disable                      # Disable
sudo ufw status                       # Status
sudo ufw allow ssh                    # Allow SSH
sudo ufw allow 80/tcp                  # Allow HTTP
sudo ufw allow 443/tcp                # Allow HTTPS
sudo ufw allow from 192.168.1.10      # Allow IP
sudo ufw deny 8080                    # Deny port
sudo ufw delete allow 80                # Remove rule
sudo ufw reload                       # Reload

# firewalld (RHEL/CentOS/Fedora)
sudo firewall-cmd --state             # Status
sudo firewall-cmd --get-active-zones  # Active zones
sudo firewall-cmd --list-all           # All rules
sudo firewall-cmd --add-service=http   # Allow HTTP (current)
sudo firewall-cmd --add-port=8080/tcp # Allow port (current)
sudo firewall-cmd --permanent --add-service=http   # Allow HTTP (permanent)
sudo firewall-cmd --permanent --add-port=8080/tcp # Allow port (permanent)
sudo firewall-cmd --reload            # Apply changes
sudo firewall-cmd --remove-service=http   # Remove service
sudo firewall-cmd --set-default-zone=public  # Set default zone

# View firewall rules
sudo iptables -L -n -v
sudo ufw status numbered
sudo firewall-cmd --list-all

# Block specific IP
sudo iptables -A INPUT -s 10.0.0.0/8 -j DROP
sudo ufw deny from 10.0.0.0/8
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="10.0.0.0/8" reject'

# Log dropped packets
sudo iptables -A INPUT -j LOG
sudo ufw logging on
```

---

## Q6. Network Troubleshooting Commands (ping, traceroute, mtr)?

### **Definition**
Network diagnostic tools to test connectivity, trace paths, and identify network issues.

### **Explain (Detail)**

**ping Command:**
```
Purpose: Test host reachability
Sends ICMP packets
Measures round-trip time

Output shows:
- Packets sent
- Packets received
- Packet loss percentage
- Round-trip time (ms)

Use Cases:
- Test if host is up
- Check network latency
- Measure packet loss
```

**ping Options:**
```
-c count     → Send specific number of packets
-w timeout   → Timeout in seconds
-i interval  → Seconds between packets
-s size       → Packet size in bytes
-f           → Flood ping (don't use without permission)
-t ttl       → Time to live
```

**traceroute Command:**
```
Purpose: Trace packet path to destination
Shows all routers/hops between you and target

Use Cases:
- Find where connection fails
- Identify network bottlenecks
- Trace routing path
- Debug network issues
```

**mtr Command (My Traceroute):**
```
Purpose: Network diagnostic tool
Combines ping and traceroute
Real-time updates
Better than traceroute for continuous monitoring

Use Cases:
- Continuous network monitoring
- Identify packet loss at specific hop
- Real-time path analysis
```

**Common Troubleshooting Steps:**
```
1. Test connectivity:
   ping google.com

2. If ping fails, check DNS:
   ping 8.8.8.8

3. Trace path:
   traceroute google.com

4. Monitor with mtr:
   mtr google.com

5. Check local interface:
   ip addr show

6. Check routing:
   ip route show

7. Check firewall:
   iptables -L
```

**ping Examples:**
```
Basic ping:
ping google.com

Ping with packet count:
ping -c 5 google.com

Ping interval:
ping -i 2 google.com       # 2 second interval

Ping timeout:
ping -w 5 google.com       # 5 second timeout

Ping packet size:
ping -s 1000 google.com    # 1000 byte packets

Flood ping (be careful):
sudo ping -f 192.168.1.10
```

**traceroute Examples:**
```
Basic traceroute:
traceroute google.com

Limit hops:
traceroute -m 10 google.com

Don't resolve names:
traceroute -n google.com

Wait time:
traceroute -w 2 google.com       # 2 seconds

Use ICMP instead of UDP:
traceroute -I google.com
```

**mtr Examples:**
```
Basic mtr:
mtr google.com

Report mode (non-interactive):
mtr -r -c 10 google.com

Don't resolve names:
mtr -n google.com

Packet size:
mtr -s 1000 google.com

Interval:
mtr -i 1 google.com         # 1 second interval

Count mode:
mtr -c 10 google.com         # Send 10 packets
```

**Network Troubleshooting Checklist:**
```
1. Can I ping localhost?
   ping 127.0.0.1

2. Can I ping gateway?
   ping 192.168.1.1

3. Can I ping external IP?
   ping 8.8.8.8

4. Can I ping domain name?
   ping google.com

5. Where does connection fail?
   traceroute google.com

6. Is there packet loss?
   mtr -c 100 google.com

7. Check interface status:
   ip addr show

8. Check routes:
   ip route show

9. Check DNS:
   dig google.com

10. Check firewall:
    iptables -L
```

**Interpreting ping Output:**
```
64 bytes from 142.250.74.46: icmp_seq=1 ttl=114 time=15.2 ms
      │                │           │             │     │      └─ Response time (good if < 50ms)
      │                │           │             └─────────── Packet sequence
      │                │           └─────────────────────────── Destination IP
      │                └─────────────────────────────────────── Bytes per packet
      └─────────────────────────────────────────────────── Response size

--- google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss
   │                         │                   └─ Should be 0%
   │                         └────────────────────── Packets received
   └───────────────────────────────────────────── Packets sent

rtt min/avg/max/mdev = 14.8/15.2/16.1/0.4 ms
   └─ Round-trip time statistics
```

**Interpreting traceroute Output:**
```
 1  router1.example.com (192.168.1.1)  1.234 ms
 2  isp-gateway (10.0.0.1)           5.678 ms
 3  google-server (172.217.14.46)      15.234 ms *  ← Destination

*  → Destination reached

High latency at hop 2:
Network issue at ISP gateway

Timeout at hop 3:
Blocking or unreachable
```

**Interpreting mtr Output:**
```
Host                 Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. router1.local      0.0%    10    1.2   1.0   1.5   1.8   0.2
 2. gateway.isp      5.0%    10   15.3  14.0  18.2   16.5   1.5
 3. google.com        0.0%    10   20.1  18.5  23.4   21.0   1.8

Loss%  → Packet loss at this hop (high = issue)
Avg     → Average latency
Best    → Best response time
Wrst    → Worst response time
StDev   → Standard deviation (stability)
```

### **Use**
- **Connectivity Testing**: Verify hosts are reachable
- **Network Debugging**: Find where connection fails
- **Performance**: Check latency and packet loss
- **Route Analysis**: Understand network path
- **Troubleshooting**: Identify network bottlenecks
- **Monitoring**: Continuous network health

### **Command**
```bash
# ping commands
ping google.com                      # Basic ping
ping -c 5 google.com                 # 5 packets
ping -w 10 google.com                # 10 second timeout
ping -i 2 google.com                 # 2 second interval
ping -s 1000 google.com              # 1000 byte packets
ping 127.0.0.1                      # Test localhost
ping 192.168.1.1                     # Test gateway
ping 8.8.8.8                        # Test DNS server

# traceroute commands
traceroute google.com                 # Basic traceroute
traceroute -n google.com               # No name resolution
traceroute -m 10 google.com             # Max 10 hops
traceroute -I google.com               # Use ICMP
traceroute -w 2 google.com             # 2 second wait

# mtr commands
mtr google.com                        # Interactive
mtr -r -c 10 google.com              # Report mode, 10 packets
mtr -n google.com                     # No name resolution
mtr -c 50 google.com                 # 50 packets
mtr -i 1 google.com                 # 1 second interval

# Network troubleshooting workflow
ping -c 4 8.8.8.8                 # Test DNS server
ping -c 4 google.com                 # Test domain resolution
traceroute google.com                 # Trace path
mtr -r -c 10 google.com             # Continuous test

# Check network interfaces
ip addr show
ifconfig

# Check routing
ip route show
route -n

# Check DNS resolution
dig google.com
nslookup google.com

# Check ARP table
ip neigh show
arp -an
```

---

## Q7. How to Find Process Using Specific Port?

### **Definition**
Identify which process is listening or using a specific network port.

### **Explain (Detail)**

**Why Find Process by Port:**
```
Common Scenarios:
- Port conflict: Can't start service (port in use)
- Unknown process: What's using port 8080?
- Security: Unauthorized service running
- Troubleshooting: Service not responding
- Audit: Check all listening services
```

**Using lsof Command:**
```
Purpose: List open files and network sockets
Shows processes using ports

Key options:
-i     → Internet files (network sockets)
-P     → Port numbers only
-n     → No name resolution
-p     → Show PID
-u     → Show username
-t     → TCP only
-u     → UDP only
```

**lsof Examples:**
```
Find process using port 22:
sudo lsof -i :22

Find all listening processes:
sudo lsof -i -P -n | grep LISTEN

Find process using port 8080:
sudo lsof -i :8080

Show UDP listeners:
sudo lsof -i UDP -P -n | grep LISTEN

Find by protocol:
sudo lsof -i TCP:80
sudo lsof -i UDP:53

Show full details:
sudo lsof -i :22 -P -n -v
```

**Using fuser Command:**
```
Purpose: Find processes using files or sockets
Works with network sockets

Options:
-n     → Name space (tcp, udp)
-k     → Kill processes
-v     → Verbose
```

**fuser Examples:**
```
Find process using port 8080:
sudo fuser -n tcp 8080

Find process using UDP port 53:
sudo fuser -n udp 53

Kill process using port:
sudo fuser -k -n tcp 8080

Show details:
sudo fuser -v -n tcp 22
```

**Using netstat:**
```
Find process using port:
sudo netstat -tlnp | grep :22
sudo netstat -ulpn | grep :53

Show PID and program:
sudo netstat -tlnp
```

**Using ss:**
```
Find process using port:
sudo ss -tlnp | grep :22
sudo ss -ulpn | grep :53

Show listening with process:
sudo ss -tulpn
```

**Finding Port Conflicts:**
```
Check if port is in use:
sudo lsof -i :8080
sudo ss -tlnp | grep :8080

If in use, identify process:
sudo lsof -i :8080 | grep LISTEN

Kill conflicting process:
sudo kill -9 PID
```

**Finding All Web Server Processes:**
```
Apache/Nginx on port 80:
sudo lsof -i :80
sudo lsof -i :443

Find all HTTP/HTTPS:
sudo lsof -i TCP:80,TCP:443
```

**Finding Database Processes:**
```
MySQL on port 3306:
sudo lsof -i :3306

PostgreSQL on port 5432:
sudo lsof -i :5432

MongoDB on port 27017:
sudo lsof -i :27017
```

**Security Audit:**
```
Find all listening processes:
sudo lsof -i -P -n | grep LISTEN
sudo ss -tulpn

Find unknown/unexpected ports:
sudo lsof -i -P -n | grep LISTEN | grep -v ':22\|:80\|:443'
```

**Process Information:**
```
After finding PID, get more info:
ps aux | grep PID
ps -p PID -f

Kill process:
kill PID
kill -9 PID

Check what user owns process:
sudo lsof -i :22 -u
```

### **Use**
- **Port Conflicts**: Find what's blocking port
- **Security**: Identify unauthorized services
- **Troubleshooting**: Debug service issues
- **Audit**: Check all listening services
- **Process Management**: Kill specific processes
- **System Admin**: Understand port usage

### **Command**
```bash
# Find process using specific port
sudo lsof -i :22                    # Port 22 (SSH)
sudo lsof -i :80                    # Port 80 (HTTP)
sudo lsof -i :443                   # Port 443 (HTTPS)
sudo lsof -i :3306                  # Port 3306 (MySQL)
sudo lsof -i :8080                  # Port 8080

# Using ss command
sudo ss -tlnp | grep :22             # SSH
sudo ss -tlnp | grep :80             # HTTP
sudo ss -tlnp | grep :443            # HTTPS
sudo ss -tulpn                        # All UDP

# Using netstat command
sudo netstat -tlnp | grep :22
sudo netstat -tlnp | grep :80

# Using fuser command
sudo fuser -n tcp 22
sudo fuser -n tcp 8080
sudo fuser -v -n tcp 443            # Verbose

# Find all listening processes
sudo lsof -i -P -n | grep LISTEN
sudo ss -tulpn
sudo netstat -tlnp

# Check port conflicts
sudo lsof -i :8080 | grep LISTEN
sudo ss -tlnp | grep :8080

# Find all web servers
sudo lsof -i TCP:80,TCP:443

# Find all databases
sudo lsof -i :3306          # MySQL
sudo lsof -i :5432          # PostgreSQL
sudo lsof -i :27017         # MongoDB

# Kill process using port
sudo lsof -i :8080 -t | awk 'NR==2 {print $2}' | xargs kill -9
sudo fuser -k -n tcp 8080

# Get process details
PID=$(sudo lsof -i :8080 -t | awk 'NR==2 {print $2}')
ps aux | grep $PID
ps -p $PID -f

# Security audit (all listening ports)
sudo lsof -i -P -n | grep LISTEN
sudo ss -tulpn

# Find suspicious ports (non-standard)
sudo lsof -i -P -n | grep LISTEN | grep -v ':22\|:80\|:443\|:53\|:3306'

# Monitor port usage
watch -n 2 'sudo ss -tlnp | grep :8080'
```

---

## Q8. How to Check Network Connections and Traffic?

### **Definition**
Monitor active network connections, view traffic, analyze network activity.

### **Explain (Detail)**

**Connection States:**
```
ESTABLISHED → Connection established, data flowing
TIME_WAIT   → Waiting for connection to close
CLOSE_WAIT  → Waiting for close request
FIN_WAIT    → Closing connection
LISTEN      → Waiting for incoming connections
SYN_SENT    → Connection initiated
SYN_RECV    → Connection received
```

**Using netstat:**
```
View all connections:
netstat -an                 # All connections (numeric)
netstat -at                 # TCP connections
netstat -au                 # UDP connections

View with process:
netstat -tapn               # TCP with process
netstat -aupn               # UDP with process

View listening:
netstat -tlnp               # TCP listening
netstat -ulnp               # UDP listening
```

**Using ss:**
```
View all connections:
ss -an                     # All connections
ss -at                     # TCP
ss -au                     # UDP

View with process:
ss -tapn                   # TCP with process
ss -aupn                   # UDP with process

View by state:
ss -tn state established
ss -tn state time-wait
```

**Using iptraf:**
```
Network traffic analyzer
Shows:
- Interface traffic
- Protocol breakdown
- Connection statistics

Modes:
- General interface stats
- Detailed interface stats
- Statistical breakdown
- Connection logs
```

**Using nethogs:**
```
Per-process bandwidth monitor
Shows which process uses bandwidth

Options:
-t     → View TCP only
-u     → View UDP only
-p     → Show process names
-d     → Delay (seconds)
```

**Network Traffic Commands:**
```
Real-time monitoring:
sudo nethogs eth0
sudo iptraf -i eth0

View interface statistics:
ip -s link show eth0
cat /proc/net/dev
```

**Connection Information:**
```
Show connection details:
netstat -anp               # All with process
ss -tunp                   # TCP/UDP with process

Filter by state:
ss -tn state established    # Established only
ss -tn state time-wait     # Time wait only

Filter by protocol:
ss -tn                       # TCP
ss -un                       # UDP
```

**Interface Traffic:**
```
View traffic:
ip -s link show
ip -s addr show

View network stats:
cat /proc/net/dev

Continuous monitoring:
watch -n 1 'cat /proc/net/dev'
```

**Connection Count:**
```
Count established:
ss -tn state established | wc -l

Count all:
ss -tn | wc -l

Count by state:
ss -tn state all | awk '{print $1}' | sort | uniq -c | sort -nr
```

**Top Connections:**
```
Top 10 by data transfer:
ss -tnp | awk '{print $2}' | sort | uniq -c | sort -nr | head -10

Top IPs by connection:
netstat -an | awk '{print $5}' | sort | uniq -c | sort -nr | head -10
```

### **Use**
- **Network Monitoring**: Track connections and traffic
- **Troubleshooting**: Debug connection issues
- **Security**: Detect suspicious activity
- **Bandwidth**: Monitor network usage
- **Performance**: Analyze network patterns
- **Audit**: Understand network behavior

### **Command**
```bash
# Check network connections
netstat -an                    # All connections
netstat -at                    # TCP
netstat -au                    # UDP
netstat -tapn                  # TCP with process
netstat -aupn                  # UDP with process

# Using ss (modern)
ss -an                         # All connections
ss -at                         # TCP
ss -au                         # UDP
ss -tapn                       # TCP with process
ss -aupn                       # UDP with process

# View connection states
ss -tn state established        # Established
ss -tn state time-wait         # Time wait
ss -tn state all

# View interface statistics
ip -s link show
cat /proc/net/dev

# Monitor traffic (need to install)
sudo nethogs eth0              # Per-process bandwidth
sudo iptraf -i eth0           # Detailed stats
sudo iftop                    # Interface bandwidth

# Count connections
ss -tn state established | wc -l
netstat -an | grep ESTABLISHED | wc -l

# Top connections by IP
netstat -an | awk '{print $5}' | sort | uniq -c | sort -nr | head -10

# Top processes by connection
ss -tnp | awk '{print $7}' | sort | uniq -c | sort -nr | head -10

# Filter by specific IP
ss -tn | grep 192.168.1.10
netstat -an | grep 192.168.1.10

# Monitor in real-time
watch -n 2 'ss -tn state established'
watch -n 1 'cat /proc/net/dev'

# Connection summary
ss -s
netstat -s
```

---

## Q9. What is ARP Table and How to View It?

### **Definition**
ARP (Address Resolution Protocol) - Maps IP addresses to MAC addresses on local network.

### **Explain (Detail)**

**ARP Purpose:**
```
Problem: Know IP, need MAC address (layer 2)
Solution: ARP protocol

How it works:
1. Device wants to send to IP
2. Broadcasts "Who has this IP?"
3. Owner IP responds "I have it, my MAC is..."
4. Device caches MAC address for future use
```

**ARP Table:**
```
Stores: IP ↔ MAC mappings
Purpose: Avoid constant ARP requests
TTL: Entries expire (typically 30 seconds to 20 minutes)

Entry contains:
- IP address
- MAC address
- Interface
- Device type
```

**Viewing ARP Table:**
```
Using arp command:
arp -a          # Show all
arp -n          # Numeric (no DNS)
arp -v          # Verbose

Using ip command:
ip neigh        # Show neighbor table
ip neigh show  # Show all neighbors
```

**ARP Commands:**
```
View ARP table:
arp -a
ip neigh show

View specific interface:
arp -i eth0 -a
ip neigh show dev eth0

Add static entry:
arp -s 192.168.1.100 00:11:22:33:44:55
ip neigh add 192.168.1.100 lladdr 00:11:22:33:44:55 dev eth0

Delete entry:
arp -d 192.168.1.100
ip neigh del 192.168.1.100 dev eth0

Flush ARP cache:
arp -d -a
ip neigh flush all
```

**ARP Output:**
```
? (192.168.1.1) at 00:11:22:33:44:55 [ether] on eth0
  │            │                    │            │        │      └─ Interface
  │            │                    │            └─────────── Hardware type
  │            │                    └──────────────────────── MAC address
  │            └───────────────────────────────────────────── IP address
  └───────────────────────────────────────────────────────── Device type
```

**ip neigh Output:**
```
192.168.1.1 dev eth0 lladdr 00:11:22:33:44:55 REACHABLE
            │             │                     │          │
            │             │                     │          └─ State
            │             │                     └──────────── MAC address
            │             └───────────────────────────────── Interface
            └───────────────────────────────────────────────── IP address
```

**ARP States:**
```
REACHABLE → Working, reachable
STALE     → Entry expired, need refresh
DELAY      → Waiting for ARP response
FAILED     → ARP failed
PERMANENT → Static entry, never expires
```

**Static ARP Entries:**
```
Why use static ARP:
- High-availability servers
- Fixed infrastructure
- Security (prevent spoofing)
- Performance (no ARP delay)

Add static:
sudo arp -s 192.168.1.100 00:11:22:33:44:55
sudo ip neigh add 192.168.1.100 lladdr 00:11:22:33:44:55 dev eth0 nud permanent
```

**Troubleshooting with ARP:**
```
Check ARP table:
arp -a
ip neigh show

Ping host (creates ARP entry):
ping 192.168.1.10
arp -a | grep 192.168.1.10

Flush ARP cache if issues:
sudo arp -d -a
sudo ip neigh flush all

Check ARP on specific interface:
arp -i eth0 -a
ip neigh show dev eth0
```

### **Use**
- **Network Debugging**: Identify MAC conflicts
- **Connectivity Issues**: Check ARP table
- **Static Configuration**: High-availability setups
- **Security**: Detect ARP spoofing
- **Performance**: Static ARP reduces delay
- **Inventory**: Track network devices

### **Command**
```bash
# View ARP table
arp -a                          # All entries
arp -n                          # Numeric (no DNS)
arp -v                          # Verbose
ip neigh show                   # Show neighbors
ip neigh                        # Show all

# ARP by interface
arp -i eth0 -a
ip neigh show dev eth0
ip neigh show dev wlan0

# Add static entry
sudo arp -s 192.168.1.100 00:11:22:33:44:55
sudo ip neigh add 192.168.1.100 lladdr 00:11:22:33:44:55 dev eth0 nud permanent

# Delete entry
sudo arp -d 192.168.1.100
sudo ip neigh del 192.168.1.100 dev eth0

# Flush ARP cache
sudo arp -d -a
sudo ip neigh flush all

# Flush specific interface
sudo ip neigh flush dev eth0

# Show ARP table in numeric form
arp -an
ip neigh show nud all

# Monitor ARP table
watch -n 5 'arp -an'

# Find specific IP in ARP table
arp -a | grep 192.168.1.10
ip neigh show | grep 192.168.1.10

# Find specific MAC in ARP table
arp -a | grep 00:11:22:33:44:55
ip neigh show | grep 00:11:22:33:44:55
```

---

## Q10. How to Check and Modify Routing Table?

### **Definition**
Routing table - Determines where network packets are sent based on destination IP.

### **Explain (Detail)**

**Routing Purpose:**
```
Problem: Multiple network paths
Solution: Routing table

How it works:
1. Packet needs to go to destination
2. System checks routing table
3. Finds matching route
4. Sends packet via that interface/gateway
```

**Route Entry:**
```
Destination    → Target network or host
Gateway       → Next hop router
Genmask       → Network mask
Flags         → Route flags
Metric        → Cost/priority
Interface     → Network interface
```

**Routing Table Commands:**
```
View routing table:
ip route show
route -n
netstat -r

View default route:
ip route show default
route -n | grep default
```

**Common Routes:**
```
Default route (gateway):
default via 192.168.1.1 dev eth0

Local network:
192.168.1.0/24 dev eth0

Specific host:
10.0.0.10 via 192.168.1.1 dev eth0
```

**Adding Routes:**
```
Add default route:
sudo ip route add default via 192.168.1.1

Add network route:
sudo ip route add 10.0.0.0/24 via 192.168.1.1

Add host route:
sudo ip route add 10.0.0.10 via 192.168.1.1

Using route command (older):
sudo route add default gw 192.168.1.1
sudo route add -net 10.0.0.0/24 gw 192.168.1.1
```

**Deleting Routes:**
```
Delete route:
sudo ip route del default
sudo ip route del 10.0.0.0/24

Using route command:
sudo route del default
sudo route del -net 10.0.0.0/24
```

**Route Metrics:**
```
Metric determines route priority:
- Lower = higher priority
- Higher = lower priority

Add with metric:
sudo ip route add 10.0.0.0/24 via 192.168.1.1 metric 100
sudo ip route add 10.0.0.0/24 via 192.168.1.2 metric 200
```

**Multiple Gateways:**
```
Scenario: Two internet connections

Primary gateway:
ip route add default via 192.168.1.1 metric 100

Secondary gateway:
ip route add default via 192.168.2.1 metric 200

Traffic uses primary first, falls back to secondary
```

**Policy Routing:**
```
Different routes for different sources
Advanced setup for complex networks
Uses routing tables and rules

Add table:
ip route add table 100 default via 192.168.1.1

Add rule to use table:
ip rule add from 192.168.1.10 table 100
```

**Routing Table Output:**
```
default via 192.168.1.1 dev eth0 metric 100
  │         │            │              │      │
  │         │            │              │      └─ Route priority
  │         │            │              └─────────── Interface
  │         │            └─────────────────────────────── Gateway
  │         └─────────────────────────────────────────────── Default route
  └─────────────────────────────────────────────────────── Destination

192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
  │              │                │      │           │      │
  │              │                │      │           │      └─ Source IP
  │              │                │      │           └───────────── Scope
  │              │                │      └─────────────────── Protocol
  │              │                └────────────────────────── Device
  │              └─────────────────────────────────────────────── Network
  └─────────────────────────────────────────────────────────────── Destination
```

**Persistent Routes:**
```
Ubuntu (netplan):
Edit /etc/netplan/*.yaml
Add routes: section

Ubuntu (interfaces):
Edit /etc/network/interfaces
Add gateway and routes

RHEL/CentOS:
Edit /etc/sysconfig/network-scripts/ifcfg-eth0
Add GATEWAY and routes=
```

**Troubleshooting Routes:**
```
Check routing table:
ip route show

Test connectivity:
ping -I eth0 8.8.8.8
ping -I eth1 8.8.8.8

Trace route:
traceroute google.com

Check default gateway:
ip route show default
cat /proc/net/route
```

### **Use**
- **Network Configuration**: Set default gateway
- **Multi-homing**: Configure multiple internet connections
- **VPN**: Add VPN routes
- **Traffic Shaping**: Control path selection
- **Load Balancing**: Multiple gateways
- **Troubleshooting**: Debug routing issues

### **Command**
```bash
# View routing table
ip route show                      # Modern
route -n                           # Older
netstat -r                         # Alternative

# View default route
ip route show default
route -n | grep default

# Add default route
sudo ip route add default via 192.168.1.1
sudo route add default gw 192.168.1.1

# Add network route
sudo ip route add 10.0.0.0/24 via 192.168.1.1
sudo route add -net 10.0.0.0/24 gw 192.168.1.1

# Add host route
sudo ip route add 10.0.0.10 via 192.168.1.1

# Delete route
sudo ip route del default
sudo route del default

# Add route with metric
sudo ip route add 10.0.0.0/24 via 192.168.1.1 metric 100

# View route to specific destination
ip route get 8.8.8.8
route -n get 8.8.8.8

# Flush all routes (be careful!)
sudo ip route flush table main

# Add route via specific interface
sudo ip route add 10.0.0.0/24 dev eth0

# Add static ARP with route
sudo arp -s 10.0.0.1 00:11:22:33:44:55
sudo ip route add 10.0.0.1 dev eth0

# Check routing cache
ip route show cache

# Monitor routing table
watch -n 5 'ip route show'
```

---

## Q11. How to Monitor Bandwidth and Network Performance?

### **Definition**
Commands to monitor network bandwidth, analyze performance, and detect bottlenecks.

### **Explain (Detail)**

**Monitoring Tools:**
```
iftop:   Per-connection bandwidth
nethogs:  Per-process bandwidth
iptraf:  Detailed traffic analysis
bmon:     Real-time bandwidth monitor
netstat:  Interface statistics
ethtool:  Interface details and errors
```

**iftop (Interface Bandwidth):**
```
Shows:
- Total bandwidth
- Per-connection bandwidth
- Source and destination

Options:
-t     → Use text mode
-s     → Delay (seconds)
-n     → No name resolution
```

**nethogs (Per-Process Bandwidth):**
```
Shows:
- Process using bandwidth
- Upload/download speed
- Total usage

Options:
-t     → Show traffic only (no processes)
-u     → Show UDP only
-p     → Show process names
-d     → Delay (seconds)
```

**iptraf (Traffic Analysis):**
```
Shows:
- Protocol breakdown
- Interface statistics
- Connection logs

Modes:
- General interface stats
- Detailed interface stats
- Traffic by protocol
```

**bmon (Bandwidth Monitor):**
```
Shows:
- Multiple interfaces
- Graphical bandwidth
- Detailed statistics

Options:
-p     → Use curses UI
```

**Checking Interface Statistics:**
```
Using ip:
ip -s link show eth0
ip -s addr show eth0

Using /proc:
cat /proc/net/dev
```

**Network Performance Metrics:**
```
RX (Receive):    Download speed
TX (Transmit):  Upload speed
Errors:          Packet errors
Dropped:         Dropped packets
Overruns:       Buffer overruns
```

**ethtool (Interface Details):**
```
Check interface stats:
ethtool eth0
ethtool -S eth0    # Statistics

Check link:
ethtool eth0 | grep "Link detected"

Check duplex/speed:
ethtool eth0 | grep -E '(Speed|Duplex)'
```

**Real-time Monitoring:**
```
Monitor bandwidth:
sudo iftop -i eth0
sudo nethogs eth0
sudo bmon

Monitor statistics:
watch -n 1 'cat /proc/net/dev'
watch -n 2 'ip -s link show eth0'
```

**Performance Testing:**
```
Test speed:
iperf -s (server)
iperf -c server (client)

Ping test:
ping -c 100 -i 0.2 target  # 100 packets, 0.2s interval

TCP test:
tcptraceroute server
```

### **Use**
- **Performance**: Monitor network speed
- **Troubleshooting**: Identify bottlenecks
- **Capacity Planning**: Understand usage patterns
- **Audit**: Track unusual traffic
- **Process Analysis**: Find bandwidth-hungry processes
- **Debugging**: Detect network errors

### **Command**
```bash
# Bandwidth monitoring
sudo iftop -i eth0                # Per-connection
sudo nethogs eth0                   # Per-process
sudo bmon eth0                       # Multiple interfaces
sudo iptraf -i eth0                 # Detailed stats

# Interface statistics
ip -s link show eth0
cat /proc/net/dev

# Real-time monitoring
watch -n 1 'cat /proc/net/dev'
watch -n 2 'ip -s link show'

# ethtool statistics
ethtool -S eth0
ethtool eth0 | grep -E '(Link|Speed|Duplex)'

# Check errors
ip -s link show eth0 | grep -E '(error|drop)'

# Monitor specific interface
sudo iftop -i eth0 -t           # Text mode
sudo nethogs eth0 -t            # Traffic only

# Monitor multiple interfaces
sudo bmon -p                    # Curses UI

# Find process using bandwidth
sudo nethogs eth0 | sort

# Test network performance
ping -c 100 -i 0.2 8.8.8.8     # Latency test
```

---

## Q12. DNS Troubleshooting (dig, nslookup, host)?

### **Definition**
DNS diagnostic tools to troubleshoot DNS resolution issues.

### **Explain (Detail)**

**DNS Troubleshooting Steps:**
```
1. Check local DNS files
2. Test basic DNS resolution
3. Test specific DNS server
4. Check DNS records
5. Test reverse DNS
6. Verify DNS caching
```

**dig Command (Recommended):**
```
Most powerful DNS tool
Shows complete DNS information

Key options:
@server    → Query specific DNS server
+short     → Show only answer
+trace     → Trace DNS path
+short     → Brief output
+noall     → Don't show all sections
```

**dig Examples:**
```
Basic lookup:
dig google.com

Short output:
dig +short google.com

Query specific server:
dig @8.8.8.8 google.com

Query specific record:
dig A google.com              # A record
dig AAAA google.com           # IPv6
dig MX gmail.com             # MX
dig TXT google.com           # TXT

Reverse lookup:
dig -x 8.8.8.8

Trace DNS path:
dig +trace google.com
```

**nslookup Command (Traditional):**
```
Interactive mode:
nslookup
> server 8.8.8.8
> google.com
> set type=mx
> gmail.com
> exit

One-shot:
nslookup google.com 8.8.8.8
nslookup -type=mx gmail.com
```

**host Command (Simple):**
```
Simple DNS lookup:
host google.com

Query specific server:
host google.com 8.8.8.8

Query specific record:
host -t mx gmail.com

Reverse lookup:
host 8.8.8.8
```

**Common DNS Issues:**
```
1. DNS not resolving:
   Check /etc/resolv.conf
   ping 8.8.8.8

2. Slow DNS:
   Test different DNS servers
   Check caching

3. Wrong IP returned:
   Verify DNS records
   Check for spoofing

4. Partial failure:
   Some sites work, some don't
   Check DNS server logs
```

**Testing DNS Servers:**
```
Test Google DNS:
dig @8.8.8.8 google.com
dig @8.8.4.4 google.com

Test Cloudflare:
dig @1.1.1.1 google.com
dig @1.0.0.1 google.com

Test ISP DNS:
dig @ISP_DNS_IP google.com
```

**DNS Record Types:**
```
A Record:     IPv4 address
AAAA Record:  IPv6 address
CNAME:       Alias to another domain
MX Record:    Mail server
TXT Record:   Text data (SPF, DKIM, verification)
NS Record:    Name server
SOA Record:   Start of authority
```

### **Use**
- **DNS Issues**: Troubleshoot resolution problems
- **Record Verification**: Check DNS records
- **Server Testing**: Test DNS server performance
- **Migration**: Verify DNS changes
- **Debugging**: Understand DNS paths
- **Security**: Check for DNS spoofing

### **Command**
```bash
# DNS troubleshooting commands
dig google.com                      # Basic lookup
dig +short google.com              # IP only
dig @8.8.8.8 google.com           # Query specific server
dig A google.com                    # A record
dig AAAA google.com                 # IPv6 record
dig MX gmail.com                    # MX record
dig TXT google.com                  # TXT record

# Reverse DNS
dig -x 8.8.8.8                     # Reverse lookup
host 8.8.8.8                         # Reverse lookup

# DNS tracing
dig +trace google.com               # Trace DNS path
dig +trace @8.8.8.8 google.com     # Trace through specific server

# Using nslookup
nslookup google.com
nslookup google.com 8.8.8.8

# Using host
host google.com
host -t mx gmail.com
host -t txt google.com

# Test multiple DNS servers
for dns in 8.8.8.8 1.1.1.1 208.67.222.222; do
    echo "=== Testing $dns ==="
    dig @$dns google.com +short
done

# Check DNS cache
resolvectl query-cache

# Flush DNS cache
sudo systemctl restart systemd-resolved
sudo systemctl restart dnsmasq

# Check DNS configuration
cat /etc/resolv.conf
cat /etc/nsswitch.conf

# Verify DNS servers work
dig +short @8.8.8.8 google.com
dig +short @1.1.1.1 google.com
```

---

## Q13. How to Capture and Analyze Network Traffic (tcpdump, Wireshark)?

### **Definition**
Network packet capture tools for analyzing network traffic and debugging.

### **Explain (Detail)**

**tcpdump (Command Line):**
```
Powerful packet capture tool
Runs on command line
Can save to file
Real-time analysis or post-capture
```

**Wireshark (GUI):**
```
Advanced network protocol analyzer
GUI application
Deep packet inspection
File and live capture
```

**tcpdump Options:**
```
-i interface   → Capture on specific interface
-w file        → Write to file
-r file        → Read from file
-c count      → Capture N packets
-s snaplen     → Snapshot length
-v             → Verbose
-X             → Hex and ASCII
-n             → No name resolution
```

**tcpdump Capture Examples:**
```
Capture all traffic:
sudo tcpdump -i eth0

Capture specific port:
sudo tcpdump -i eth0 port 22
sudo tcpdump -i eth0 port 80

Capture specific host:
sudo tcpdump -i eth0 host 192.168.1.10

Capture specific protocol:
sudo tcpdump -i eth0 icmp
sudo tcpdump -i eth0 tcp

Capture and save to file:
sudo tcpdump -i eth0 -w capture.pcap

Read from file:
tcpdump -r capture.pcap

Capture verbose with hex:
sudo tcpdump -i eth0 -vv -X
```

**tcpdump Filters:**
```
Source IP:
sudo tcpdump src 192.168.1.10

Destination IP:
sudo tcpdump dst 192.168.1.10

Network:
sudo tcpdump net 192.168.1.0/24

Both directions:
sudo tcpdump host 192.168.1.10

Port and host:
sudo tcpdump host 192.168.1.10 and port 80

Exclude traffic:
sudo tcpdump not port 22
```

**tcpdump Capture to Wireshark:**
```
Capture for Wireshark analysis:
sudo tcpdump -i eth0 -w capture.pcap

View in Wireshark:
wireshark capture.pcap

Capture specific traffic for analysis:
sudo tcpdump -i eth0 -w http.pcap port 80
wireshark http.pcap
```

**tcpdump Real-time Analysis:**
```
View HTTP traffic:
sudo tcpdump -i eth0 -A -X port 80

View SSH traffic:
sudo tcpdump -i eth0 -A port 22

View DNS traffic:
sudo tcpdump -i eth0 -A port 53

View TCP SYN packets (new connections):
sudo tcpdump -i eth0 'tcp[tcpflags] == tcp-syn'
```

**Common tcpdump Scenarios:**
```
Debug web server:
sudo tcpdump -i eth0 port 80 -A

Debug SSH connection:
sudo tcpdump -i eth0 port 22 -A

Monitor specific IP:
sudo tcpdump -i eth0 host 192.168.1.10

Capture all for later analysis:
sudo tcpdump -i eth0 -w fullcapture.pcap

Capture HTTP/HTTPS:
sudo tcpdump -i eth0 -w web.pcap 'port 80 or port 443'
```

**Wireshark Analysis:**
```
After capture with tcpdump:
sudo tcpdump -i eth0 -w capture.pcap

Open in Wireshark:
wireshark capture.pcap

Key features:
- Filter: host 192.168.1.10
- Filter: port 80
- Follow TCP stream
- Packet details
- Statistics → Conversations
- Statistics → Endpoints
```

**Capture Filters:**
```
Host filter:
host 192.168.1.10

Port filter:
port 22

Protocol filter:
tcp
udp

Network filter:
net 192.168.1.0/24

Combined:
host 192.168.1.10 and port 80
```

**Real-world Use Cases:**
```
1. Debug slow connection:
   sudo tcpdump -i eth0 -A port 80

2. Find unauthorized access:
   sudo tcpdump -i eth0 host 192.168.1.50

3. Capture for malware analysis:
   sudo tcpdump -i eth0 -w infected.pcap

4. Debug application:
   sudo tcpdump -i eth0 -A port 8080

5. Network performance:
   sudo tcpdump -i eth0 -w traffic.pcap
   # Analyze in Wireshark
```

### **Use**
- **Troubleshooting**: Debug network issues
- **Security**: Detect attacks, unauthorized access
- **Application Debug**: Analyze application protocol
- **Performance**: Understand traffic patterns
- **Compliance**: Capture traffic logs
- **Forensics**: Analyze network incidents

### **Command**
```bash
# tcpdump capture
sudo tcpdump -i eth0                         # All traffic
sudo tcpdump -i eth0 -w capture.pcap           # Save to file
sudo tcpdump -i eth0 port 80                    # HTTP
sudo tcpdump -i eth0 port 22                    # SSH
sudo tcpdump -i eth0 host 192.168.1.10          # Specific host

# tcpdump read
tcpdump -r capture.pcap
tcpdump -r capture.pcap -A                    # ASCII
tcpdump -r capture.pcap -X                    # Hex+ASCII

# tcpdump verbose
sudo tcpdump -i eth0 -vv                     # Very verbose
sudo tcpdump -i eth0 -vv -X                 # Hex dump
sudo tcpdump -i eth0 -n                       # No name resolution

# tcpdump filters
sudo tcpdump -i eth0 src 192.168.1.10       # Source IP
sudo tcpdump -i eth0 dst 192.168.1.10       # Destination IP
sudo tcpdump -i eth0 port 80 or port 443   # HTTP/HTTPS
sudo tcpdump -i eth0 'tcp[tcpflags] == tcp-syn'  # New connections

# Capture for Wireshark
sudo tcpdump -i eth0 -w capture.pcap
sudo tcpdump -i eth0 -w http.pcap port 80
sudo tcpdump -i eth0 -w traffic.pcap 'port 80 or port 443'

# Real-time analysis
sudo tcpdump -i eth0 -A port 80            # HTTP (ASCII)
sudo tcpdump -i eth0 -A port 22            # SSH (ASCII)
sudo tcpdump -i eth0 -A port 53            # DNS (ASCII)

# Limit capture
sudo tcpdump -i eth0 -c 100                # 100 packets
sudo tcpdump -i eth0 -s 256                # 256 bytes per packet

# Capture specific protocol
sudo tcpdump -i eth0 icmp                  # ICMP
sudo tcpdump -i eth0 tcp                   # TCP
sudo tcpdump -i eth0 udp                   # UDP
```

---

## Q14. How to Check Network Interface Details and Configuration?

### **Definition**
View and configure network interface settings like speed, duplex, MAC address, and statistics.

### **Explain (Detail)**

**Network Interface Basics:**
```
Interface: Network card connection point
Examples: eth0, ens33, wlan0, lo

Each interface has:
- IP address
- MAC address
- MTU (Maximum Transmission Unit)
- Speed (10M/100M/1G)
- Duplex (half/full)
- Statistics (RX/TX)
```

**Viewing Interfaces:**
```
Using ip command:
ip addr show
ip link show

Using ifconfig (deprecated):
ifconfig -a

Using ethtool:
ethtool eth0
```

**ip addr Command:**
```
Show all interfaces:
ip addr show

Show specific interface:
ip addr show eth0

Brief format:
ip -br addr show

Add IP:
sudo ip addr add 192.168.1.10/24 dev eth0

Remove IP:
sudo ip addr del 192.168.1.10/24 dev eth0

Up/Down interface:
sudo ip link set eth0 up
sudo ip link set eth0 down
```

**ip link Command:**
```
Show interfaces:
ip link show

Show with details:
ip -details link show eth0

Change MTU:
sudo ip link set dev eth0 mtu 1500

Set interface up/down:
sudo ip link set eth0 up
sudo ip link set eth0 down
```

**ethtool Command:**
```
View settings:
ethtool eth0

View statistics:
ethtool -S eth0

View driver info:
ethtool -i eth0

View link settings:
ethtool eth0 | grep -E '(Speed|Duplex)'

Change speed:
sudo ethtool -s eth0 speed 1000 duplex full autoneg off

Pause interface:
sudo ethtool -A eth0

Unpause:
sudo ethtool -a eth0
```

**Interface Information:**
```
MAC address:
ip link show eth0 | grep link/ether
cat /sys/class/net/eth0/address

MTU:
ip addr show eth0 | grep mtu
cat /sys/class/net/eth0/mtu

IP address:
ip addr show eth0 | grep inet
ifconfig eth0
```

**Interface Statistics:**
```
Using ip:
ip -s link show eth0

Using /proc:
cat /proc/net/dev | grep eth0

Using ethtool:
ethtool -S eth0
```

**Statistics Explained:**
```
RX (Receive):   Download packets/bytes
TX (Transmit):  Upload packets/bytes
Errors:         Packet errors (bad)
Dropped:        Dropped packets (full buffer)
Overruns:       Buffer overruns
Collisions:     Ethernet collisions
```

**ifconfig Output:**
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.10  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 00:11:22:33:44:55  txqueuelen 1000  (Ethernet)
        RX packets 12345  bytes 1234567
        TX packets 67890  bytes 678901
        collisions 0 txqueuelen 1000
```

**ip addr Output:**
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff:ff
    inet 192.168.1.10/24 brd 192.168.1.255 scope global eth0
         │             │         │                    │
         │             │         │                    └─ Interface
         │             │         └───────────────────────── Broadcast address
         │             └─────────────────────────────────── Network mask
         └───────────────────────────────────────────── IP address
```

**Changing Interface Settings:**
```
Change MTU:
sudo ip link set dev eth0 mtu 9000
ifconfig eth0 mtu 9000

Enable/disable interface:
sudo ip link set eth0 up
sudo ip link set eth0 down
ifconfig eth0 up
ifconfig eth0 down

Promiscuous mode (packet capture):
sudo ip link set eth0 promisc on
sudo ip link set eth0 promisc off
```

### **Use**
- **Configuration**: Set IP, MTU, speed
- **Troubleshooting**: Check link status, errors
- **Monitoring**: Track traffic statistics
- **Diagnostics**: Identify interface issues
- **Network Setup**: Configure new interfaces
- **Performance**: Check speed/duplex

### **Command**
```bash
# View interfaces
ip addr show                         # All interfaces
ip link show                         # Link status
ifconfig -a                          # All (deprecated)

# View specific interface
ip addr show eth0
ip link show eth0
ifconfig eth0

# Brief format
ip -br addr show
ip -br link show

# Interface details
ethtool eth0                         # Detailed settings
ethtool -i eth0                       # Driver info
ethtool -S eth0                       # Statistics

# MAC address
ip link show eth0 | grep link/ether
cat /sys/class/net/eth0/address

# MTU
ip addr show eth0 | grep mtu
cat /sys/class/net/eth0/mtu

# IP address
ip addr show eth0 | grep inet
ifconfig eth0 | grep inet

# Statistics
ip -s link show eth0                 # IP statistics
cat /proc/net/dev | grep eth0        # Kernel stats
ethtool -S eth0                      # ethtool stats

# Change MTU
sudo ip link set dev eth0 mtu 9000

# Interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down

# Promiscuous mode
sudo ip link set eth0 promisc on
sudo ip link set eth0 promisc off

# Add/remove IP
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip addr del 192.168.1.10/24 dev eth0

# Monitor interface
watch -n 2 'ip -s link show eth0'
watch -n 1 'cat /proc/net/dev | grep eth0'
```

---

## 📚 Quick Reference - Module 5 Commands

### Network Configuration
```bash
ip addr show                     # View interfaces and IPs
ip link show                     # View link status
ip route show                    # View routing table
ifconfig                        # View interfaces (deprecated)
nmcli connection show             # NetworkManager
```

### Ports & Connections
```bash
sudo ss -tulpn                 # Listening ports
sudo netstat -tlnp               # Listening (netstat)
sudo lsof -i :80                # Find process by port
ss -tn state established          # Active connections
```

### DNS
```bash
dig google.com                  # DNS lookup
dig @8.8.8.8 google.com         # Query specific server
dig +short google.com           # IP only
nslookup google.com             # Alternative lookup
```

### SSH Keys
```bash
ssh-keygen -t ed25519           # Generate keys
ssh-copy-id user@server         # Copy key to server
ssh user@server                 # Connect
cat ~/.ssh/id_rsa.pub          # View public key
```

### Firewall
```bash
sudo ufw enable                 # Enable UFW (Ubuntu)
sudo ufw allow 22              # Allow SSH
sudo firewall-cmd --add-service=http   # Allow HTTP (firewalld)
sudo iptables -L                 # List rules (iptables)
```

### Network Troubleshooting
```bash
ping google.com                # Test connectivity
traceroute google.com          # Trace path
mtr google.com                # Continuous traceroute
dig google.com                # DNS test
ip neigh show                  # ARP table
ip -s link show eth0          # Interface stats
```

### Network Monitoring
```bash
sudo iftop -i eth0            # Bandwidth
sudo nethogs eth0              # Per-process
sudo tcpdump -i eth0 -w capture.pcap  # Packet capture
cat /proc/net/dev             # Statistics
```

### Process by Port
```bash
sudo lsof -i :22              # SSH
sudo ss -tlnp | grep :80       # HTTP
sudo fuser -n tcp 22           # Using port
```

---

**Next:** Module 6 - Services, Boot & Systemctl
