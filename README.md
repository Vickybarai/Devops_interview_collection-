# 📚 DevOps Interview Preparation Guide

> Comprehensive interview questions and answers for DevOps, Linux, AWS, Docker, Kubernetes, CI/CD, and more.

---

## 🎯 About This Repository

This repository contains **technical interview questions and detailed answers** designed for DevOps engineers, system administrators, and IT professionals. Each module focuses on practical knowledge that is frequently asked in interviews.

### ✨ Features

- ✅ **Comprehensive Answers** - Detailed technical explanations
- ✅ **Command Examples** - Real-world bash commands
- ✅ **Code Blocks** - Formatted for easy reading
- ✅ **Visual Diagrams** - ASCII art for complex concepts
- ✅ **Best Practices** - Production-ready recommendations
- ✅ **No Fluff** - Purely technical content

---

## 📖 Quick Start

Choose your learning path below or browse by category:

### 🔥 Most Popular Topics

| Topic | Status | Difficulty |
|--------|---------|------------|
| **[Linux Fundamentals](#linux)** | 🟢 Active | Beginner |
| **[File Management](#linux)** | 🟢 Active | Beginner-Intermediate |
| **[AWS Core](#aws)** | 🟡 Planned | Intermediate |
| **[Docker](#docker)** | 🟡 Planned | Intermediate |
| **[Kubernetes](#kubernetes)** | 🟡 Planned | Advanced |

---

## 🐧 Linux

> Essential foundation for any DevOps engineer

### Module 1: Linux Fundamentals ⭐
> Core concepts every DevOps professional must know

**📄 [Module 1: Linux Fundamentals](Linux/Module1-Linux-Fundamentals.md)**

**Questions:** 18 | **Commands Covered:** 50+

**📋 Question Index:**
1. [What is Linux? Difference between Linux, UNIX, and Windows.](Linux/Module1-Linux-Fundamentals.md#q1-what-is-linux-difference-between-linux-unix-and-windows) - OS comparison
2. [Explain Linux Architecture (Kernel, Shell, User Space)](Linux/Module1-Linux-Fundamentals.md#q2-explain-linux-architecture-kernel-shell-user-space) - System architecture
3. [What Happens When You Type a Command and Press Enter?](Linux/Module1-Linux-Fundamentals.md#q3-what-happens-when-you-type-a-command-and-press-enter) - Command flow
4. [What are Environment Variables? How to View/Set Them?](Linux/Module1-Linux-Fundamentals.md#q4-what-are-environment-variables-how-to-viewset-them) - Environment setup
5. [What are Absolute vs Relative Paths?](Linux/Module1-Linux-Fundamentals.md#q5-what-are-absolute-vs-relative-paths) - Path types
6. [Explain Linux Directory Structure (/, /bin, /etc, /home, /var)](Linux/Module1-Linux-Fundamentals.md#q6-explain-linux-directory-structure--bin-etchomevar) - File system
7. [What are the Types of Shells in Linux?](Linux/Module1-Linux-Fundamentals.md#q7-what-are-the-types-of-shells-in-linux) - Shell types
8. [What is the Difference Between Shell and Kernel?](Linux/Module1-Linux-Fundamentals.md#q8-what-is-the-difference-between-shell-and-kernel) - Shell vs Kernel
9. [Explain File System Hierarchy — Where are Configs, Binaries, and Logs Kept?](Linux/Module1-Linux-Fundamentals.md#q9-explain-file-system-hierarchy---where-are-configs-binaries-and-logs-kept) - Hierarchy
10. [What is the Difference Between /root and /home Directories?](Linux/Module1-Linux-Fundamentals.md#q10-what-is-the-difference-between-root-and-home-directories) - Root vs Home
11. [What is a Linux Kernel? Why is it Important?](Linux/Module1-Linux-Fundamentals.md#q11-what-is-a-linux-kernel-why-is-it-important) - Kernel concepts
12. [What is a Shell in Linux, and How is It Different from bash?](Linux/Module1-Linux-Fundamentals.md#q12-what-is-a-shell-in-linux-and-how-is-it-different-from-bash) - Shell types
13. [Basic Components of Linux OS](Linux/Module1-Linux-Fundamentals.md#q13-basic-components-of-linux-os) - OS components
14. [What is the init Process in Linux?](Linux/Module1-Linux-Fundamentals.md#q14-what-is-the-init-process-in-linux) - Init process
15. [What is Root?](Linux/Module1-Linux-Fundamentals.md#q15-what-is-root) - Root user and directory
16. [Check Hostname](Linux/Module1-Linux-Fundamentals.md#q16-check-hostname) - Hostname command
17. [Check Current User](Linux/Module1-Linux-Fundamentals.md#q17-check-current-user) - User commands
18. [Check Current Working Directory](Linux/Module1-Linux-Fundamentals.md#q18-check-current-working-directory) - pwd command

---

### Module 2: File Management & Permissions ⭐
> Master file operations and access control

**📄 [Module 2: File Management & Permissions](Linux/Module2-File-Management-Permissions.md)**

**Questions:** 14 | **Commands Covered:** 70+

**📋 Question Index:**
1. [What are File Permissions (rwx)?](Linux/Module2-File-Management-Permissions.md#q1-what-are-file-permissions-rwx-explain-chmod-755-vs-777) - chmod 755 vs 777
2. [What are the Types of Permissions (Read, Write, Execute)?](Linux/Module2-File-Management-Permissions.md#q2-types-of-permissions-read-write-execute) - rwx explained
3. [What is the Difference Between a Soft Link and a Hard Link?](Linux/Module2-File-Management-Permissions.md#q3-what-is-the-difference-between-a-soft-link-and-a-hard-link) - ln commands
4. [How Do You Change File Ownership (chown, chgrp)?](Linux/Module2-File-Management-Permissions.md#q4-how-do-you-change-file-ownership-chown-chgrp) - User/group management
5. [What is Umask? What Does It Control?](Linux/Module2-File-Management-Permissions.md#q5-what-is-umask-what-does-it-control) - Default permissions
6. [What is the Sticky Bit and Where is It Used?](Linux/Module2-File-Management-Permissions.md#q6-what-is-the-sticky-bit-and-where-is-it-used-tmp-example) - /tmp example
7. [What are /etc/passwd, /etc/shadow, and /etc/group Used For?](Linux/Module2-File-Management-Permissions.md#q7-what-are-etcpasswd-etcshadow-and-etcgroup-used-for) - User management files
8. [What is the Difference Between su and sudo?](Linux/Module2-File-Management-Permissions.md#q8-what-is-the-difference-between-su-and-sudo) - Privilege escalation
9. [How Do You Safely Edit the sudoers File (visudo)?](Linux/Module2-File-Management-Permissions.md#q9-how-do-you-safely-edit-the-sudoers-file-visudo) - sudo configuration
10. [How Do You Find Recently Modified Files (find -mtime)?](Linux/Module2-File-Management-Permissions.md#q10-how-do-you-find-recently-modified-files-find--mtime) - Time-based search
11. [What Are Hidden Files and How to View Them (ls -a)?](Linux/Module2-File-Management-Permissions.md#q11-what-are-hidden-files-and-how-to-view-them-ls--a) - Dotfiles
12. [How to Change File Permissions (chmod)?](Linux/Module2-File-Management-Permissions.md#q12-how-to-change-file-permissions-chmod) - Permission management
13. [How to Give Execute Permissions to a Script?](Linux/Module2-File-Management-Permissions.md#q13-how-to-give-execute-permissions-to-a-script) - Script execution
14. [How to Create and Manage Symbolic Links?](Linux/Module2-File-Management-Permissions.md#q14-how-to-create-and-manage-symbolic-links) - Soft links

---

### Module 3: Process & System Management ⭐
> Monitor and manage system processes

**📄 [Module 3: Process & System Management](Linux/Module3-Process-System-Management.md)**

**Questions:** 12 | **Commands Covered:** 60+

**📋 Question Index:**
1. [What is a Process?](Linux/Module3-Process-System-Management.md#q1-what-is-a-process) - Process basics
2. [Difference Between Process and Thread.](Linux/Module3-Process-System-Management.md#q2-difference-between-process-and-thread) - Multi-threading
3. [Explain ps, top, and htop Commands.](Linux/Module3-Process-System-Management.md#q3-explain-ps-top-and-htop-commands) - Process monitoring
4. [What is a Zombie Process and How to Handle It?](Linux/Module3-Process-System-Management.md#q4-what-is-a-zombie-process-and-how-to-handle-it) - Zombie processes
5. [Difference Between kill and kill -9.](Linux/Module3-Process-System-Management.md#q5-difference-between-kill-and-kill--9) - Process termination
6. [How to Run Process in Background (&, nohup, screen)?](Linux/Module3-Process-System-Management.md#q6-how-to-run-process-in-background--nohup-screen) - Background jobs
7. [What is Purpose of nice and renice?](Linux/Module3-Process-System-Management.md#q7-what-is-purpose-of-nice-and-renice) - Process priority
8. [How to Check Which Process Consumes Most CPU/Memory?](Linux/Module3-Process-System-Management.md#q8-how-to-check-which-process-consumes-most-cpumemory) - Resource monitoring
9. [What is Load Average? Interpret uptime Output.](Linux/Module3-Process-System-Management.md#q9-what-is-load-average-interpret-uptime-output) - System load
10. [Explain Swap Memory and When It's Used.](Linux/Module3-Process-System-Management.md#q10-explain-swap-memory-and-when-its-used) - Virtual memory
11. [Check Running Processes.](Linux/Module3-Process-System-Management.md#q11-check-running-processes) - Process listing
12. [Terminate Process.](Linux/Module3-Process-System-Management.md#q12-terminate-process) - Kill processes

---

### Module 4: Disk, Filesystem & Storage ⭐
> Manage disk space, partitions, and filesystems

**📄 [Module 4: Disk, Filesystem & Storage](Linux/Module4-Disk-Filesystem-Storage.md)**

**Questions:** 8 | **Commands Covered:** 80+

**📋 Question Index:**
1. [Difference Between df and du Commands.](Linux/Module4-Disk-Filesystem-Storage.md#q1-difference-between-df-and-du-commands) - Disk usage commands
2. [How to Check if Disk is Full or Inode is Full?](Linux/Module4-Disk-Filesystem-Storage.md#q2-how-to-check-if-disk-is-full-or-inode-is-full) - Space troubleshooting
3. [How to Check Disk Partitions (lsblk, fdisk)?](Linux/Module4-Disk-Filesystem-Storage.md#q3-how-to-check-disk-partitions-lsblk-fdisk) - Disk partitions
4. [What is /etc/fstab and How is It Used?](Linux/Module4-Disk-Filesystem-Storage.md#q4-what-is-etcfstab-and-how-is-it-used) - Mount configuration
5. [How to Mount and Unmount Filesystems?](Linux/Module4-Disk-Filesystem-Storage.md#q5-how-to-mount-and-unmount-filesystems) - Mount management
6. [What is LVM (Logical Volume Manager)?](Linux/Module4-Disk-Filesystem-Storage.md#q6-what-is-lvm-logical-volume-manager) - LVM concepts
7. [Different Types of Filesystems (ext4, xfs, btrfs).](Linux/Module4-Disk-Filesystem-Storage.md#q7-different-types-of-filesystems-ext4-xfs-btrfs) - Filesystem types
8. [How to Find Large Files and Directories?](Linux/Module4-Disk-Filesystem-Storage.md#q8-how-to-find-large-files-and-directories) - Space cleanup

---

### Module 5: Networking & Connectivity ⭐
> Configure, troubleshoot, and monitor network connections

**📄 [Module 5: Networking & Connectivity](Linux/Module5-Networking-Connectivity.md)**

**Questions:** 14 | **Commands Covered:** 90+

**📋 Question Index:**
1. [How to Configure Network Interface (ip, ifconfig, nmcli)?](Linux/Module5-Networking-Connectivity.md#q1-how-to-configure-network-interface-ip-ifconfig-nmcli) - Network config
2. [How to Check Open Ports and List Services (netstat, ss)?](Linux/Module5-Networking-Connectivity.md#q2-how-to-check-open-ports-and-list-services-netstat-ss) - Port monitoring
3. [How Does DNS Resolution Work (/etc/hosts, /etc/resolv.conf)?](Linux/Module5-Networking-Connectivity.md#q3-how-does-dns-resolution-work-etchosts-etcresolvconf) - DNS setup
4. [SSH Keys and Authentication - How Does It Work?](Linux/Module5-Networking-Connectivity.md#q4-ssh-keys-and-authentication---how-does-it-work) - SSH access
5. [How to Configure Firewall (iptables, ufw, firewalld)?](Linux/Module5-Networking-Connectivity.md#q5-how-to-configure-firewall-iptables-ufw-firewalld) - Firewall config
6. [Network Troubleshooting Commands (ping, traceroute, mtr)?](Linux/Module5-Networking-Connectivity.md#q6-network-troubleshooting-commands-ping-traceroute-mtr) - Network debug
7. [How to Find Process Using Specific Port?](Linux/Module5-Networking-Connectivity.md#q7-how-to-find-process-using-specific-port) - Port process
8. [How to Check Network Connections and Traffic?](Linux/Module5-Networking-Connectivity.md#q8-how-to-check-network-connections-and-traffic) - Traffic monitoring
9. [What is ARP Table and How to View It?](Linux/Module5-Networking-Connectivity.md#q9-what-is-arp-table-and-how-to-view-it) - ARP table
10. [How to Check and Modify Routing Table?](Linux/Module5-Networking-Connectivity.md#q10-how-to-check-and-modify-routing-table) - Routing
11. [How to Monitor Bandwidth and Network Performance?](Linux/Module5-Networking-Connectivity.md#q11-how-to-monitor-bandwidth-and-network-performance) - Bandwidth
12. [DNS Troubleshooting (dig, nslookup, host)?](Linux/Module5-Networking-Connectivity.md#q12-dns-troubleshooting-dig-nslookup-host) - DNS debug
13. [How to Capture and Analyze Network Traffic (tcpdump, Wireshark)?](Linux/Module5-Networking-Connectivity.md#q13-how-to-capture-and-analyze-network-traffic-tcpdump-wireshark) - Packet capture
14. [How to Check Network Interface Details and Configuration?](Linux/Module5-Networking-Connectivity.md#q14-how-to-check-network-interface-details-and-configuration) - Interface details

---

### Upcoming Linux Modules 🚧

- **Module 6:** Service, Boot & Systemctl
- **Module 7:** Security, Users & Access
- **Module 8:** Scheduling & Automation
- **Module 9:** Shell Scripting
- **Module 10:** Troubleshooting & Scenarios

---

## ☁️ AWS

> Amazon Web Services - Cloud fundamentals

### Planned Modules 🚧

- **AWS Fundamentals** - Regions, AZs, EC2 basics
- **EC2 Deep Dive** - Instance types, Auto Scaling
- **VPC & Networking** - Subnets, Security Groups, Route 53
- **IAM & Security** - Users, Roles, Policies
- **S3 & Storage** - Buckets, Lifecycle policies
- **RDS & Databases** - Multi-AZ, Read Replicas
- **Lambda & Serverless** - Functions, EventBridge
- **ECS & EKS** - Container orchestration
- **CloudWatch & Monitoring** - Metrics, Alarms
- **High Availability** - Multi-region, Disaster Recovery

---

## 🐳 Docker

> Containerization and application packaging

### Planned Topics 🚧

- Docker images vs containers
- Dockerfile best practices
- Docker networking
- Docker volumes (bind mounts, volumes)
- Multi-stage builds
- Docker Compose
- Registry management
- Security best practices
- Troubleshooting containers
- Production deployment

---

## ☸️ Kubernetes

> Container orchestration at scale

### Planned Topics 🚧

- Kubernetes architecture
- Pods, Deployments, Services
- ConfigMaps and Secrets
- Namespaces and resource limits
- Ingress controllers
- Horizontal Pod Autoscaling (HPA)
- Helm package manager
- Storage classes and PV/PVC
- RBAC and security
- Troubleshooting scenarios

---

## 🔄 CI/CD

> Continuous Integration and Continuous Deployment

### Planned Topics 🚧

- CI/CD concepts and workflows
- Jenkins pipelines
- GitHub Actions
- GitLab CI
- Build, test, deploy stages
- Artifact management
- Quality gates (SonarQube)
- Blue-green and canary deployments
- Rollback strategies
- Security in pipelines

---

## 🛠️ Other Tools

### Planned Topics 🚧

#### Git
- Git fundamentals
- Branching strategies
- Merge conflicts
- GitFlow vs Trunk-based
- Best practices

#### Terraform
- Infrastructure as Code
- State management
- Modules and reusability
- Terraform Cloud

#### Ansible
- Playbooks and roles
- Inventory management
- Configuration management

#### Monitoring
- Prometheus
- Grafana
- ELK Stack (Elasticsearch, Logstash, Kibana)

---

## 📊 Learning Paths

### 🌱 Beginner Path (0-6 months)
> Start here if you're new to DevOps

**Week 1-2:** Linux Fundamentals
- [Module 1: Linux Fundamentals](Linux/Module1-Linux-Fundamentals.md)
- Focus: Basic commands, file system, processes

**Week 3-4:** File Management
- [Module 2: File Management & Permissions](Linux/Module2-File-Management-Permissions.md)
- Focus: Permissions, ownership, links

**Week 5-6:** AWS Basics
- EC2 fundamentals
- Basic networking
- IAM basics

### 🎯 Intermediate Path (6-18 months)
> For those with basic DevOps knowledge

**Month 1-2:** Advanced Linux
- Process management
- Networking
- Security

**Month 3-4:** AWS Deep Dive
- VPC and networking
- RDS and S3
- Lambda and serverless

**Month 5-6:** Containers
- Docker essentials
- Kubernetes basics

### 🚀 Advanced Path (18+ months)
> For experienced DevOps engineers

**Quarter 1:** Kubernetes Production
- Advanced K8s topics
- Helm and operators
- Troubleshooting

**Quarter 2:** CI/CD & Automation
- Advanced Jenkins/GitHub Actions
- Infrastructure as Code with Terraform

**Quarter 3:** High Availability
- Multi-region AWS
- Disaster recovery
- Monitoring and observability

---

## 💡 Study Tips

### 📚 Effective Learning

1. **Practice Commands**
   - Don't just read - execute every command
   - Create a test environment (VM or cloud)
   - Build muscle memory

2. **Understand the "Why"**
   - Learn concepts, not memorize answers
   - Understand the underlying principles
   - Connect topics together

3. **Build Scenarios**
   - Create real-world problems
   - Solve them step-by-step
   - Document your approach

4. **Teach Others**
   - Explain concepts to peers
   - Write blog posts about what you learn
   - Create your own examples

### 🎓 Interview Preparation

1. **STAR Method**
   - **S**ituation: Describe the context
   - **T**ask: Explain what you needed to do
   - **A**ction: Detail your approach
   - **R**esult: Share the outcome

2. **Be Specific**
   - Use real examples from experience
   - Mention actual tools and versions
   - Discuss challenges and solutions

3. **Show Problem-Solving**
   - Walk through troubleshooting steps
   - Explain your thought process
   - Discuss what you learned

4. **Ask Clarifying Questions**
   - Understand the interviewer's concerns
   - Discuss trade-offs and alternatives
   - Show strategic thinking

---

## 🔧 Practice Environment Setup

### Local Setup

```bash
# Install VirtualBox or VMware
# Download Linux distribution (Ubuntu, CentOS)

# Create VM with:
# - 2 CPU cores
# - 4 GB RAM
# - 20 GB disk

# Install and practice all commands
```

### Cloud Setup

```bash
# AWS Free Tier
# - 1 EC2 instance (t2.micro)
# - 1 S3 bucket
# - Basic VPC

# Practice:
# - SSH access
# - File permissions
# - Service management
# - Basic troubleshooting
```

---

## 📈 Progress Tracking

### Linux Modules
- [x] Module 1: Linux Fundamentals (18 questions)
- [x] Module 2: File Management & Permissions (14 questions)
- [x] Module 3: Process & System Management (12 questions)
- [x] Module 4: Disk, Filesystem & Storage (8 questions)
- [x] Module 5: Networking & Connectivity (14 questions)
- [ ] Module 6: Service, Boot & Systemctl
- [ ] Module 7: Security, Users & Access
- [ ] Module 8: Scheduling & Automation
- [ ] Module 9: Shell Scripting
- [ ] Module 10: Troubleshooting & Scenarios

**Total Linux Questions Completed:** 66

### Other Topics
- [ ] AWS Fundamentals
- [ ] Docker
- [ ] Kubernetes
- [ ] CI/CD
- [ ] Git
- [ ] Terraform
- [ ] Monitoring

---

## 🤝 Contributing

This repository is maintained as a learning resource. Suggestions and improvements are welcome!

### How to Contribute

1. Fork the repository
2. Create a new branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit your changes (`git commit -m 'Add amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

---

## 📝 License

This repository is created for educational purposes. Feel free to use it for your interview preparation.

---

## 📞 Questions?

For questions or clarifications:
- Open an issue on GitHub
- Check existing issues for similar questions
- Refer to official documentation

---

## 🌟 Acknowledgments

- Questions compiled from real interview experiences
- Answers based on production environments
- Best practices from industry standards

---

**Last Updated:** January 15, 2024

**📊 Repository Stats:**
- **Total Modules:** 5 (Linux)
- **Total Questions:** 66
- **Total Commands:** 350+
- **Quick Links:** 66 direct links to questions

**Happy Learning! 🚀**

---

## 📚 Quick Reference

### Essential Linux Commands
```bash
# System Info
hostname              # Show hostname
whoami                # Show current user
id                    # Show user ID and groups
pwd                   # Print working directory
uname -a              # Show system information

# File Operations
ls -la                # List all files with details
cd /path              # Change directory
cp file1 file2        # Copy files
mv old new             # Move/rename
rm file.txt            # Delete file
mkdir dir              # Create directory

# Permissions
chmod 755 script.sh   # Set permissions
chown user:group file  # Change ownership

# Search
find /path -name "*.txt"      # Find files
grep "pattern" file.txt         # Search in file
grep -r "pattern" /path/       # Search recursively

# Processes
ps aux                # Show all processes
top                   # Interactive process viewer
htop                  # Enhanced process viewer
kill -9 PID            # Force kill process

# Services
systemctl status nginx   # Check service status
systemctl restart nginx  # Restart service
systemctl enable nginx   # Enable at boot

# Networking
ping host            # Check connectivity
ip addr show         # Show network interfaces
ss -tlnp            # Show listening ports
curl URL             # HTTP request
```

### Essential AWS Commands
```bash
# EC2
aws ec2 describe-instances
aws ec2 run-instances
aws ec2 stop-instances

# S3
aws s3 ls
aws s3 cp file.txt s3://bucket/
aws s3 sync ./dir/ s3://bucket/dir/

# Lambda
aws lambda list-functions
aws lambda invoke function.out --function-name my-function
```

---

**🎯 Remember:** Practice makes perfect. Good luck with your interviews!
