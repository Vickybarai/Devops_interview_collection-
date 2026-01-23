

# 🧩 Linux + AWS DevOps Interview — Common Port Numbers (with Explanation)

| 🔢 Port | 🧠 Service / Protocol | 📚 Description (What It Does) | 🎯 Common Use Case | 💬 Ask Frequency |
|:--:|:--|:--|:--|:--:|
| 20 / 21 | FTP (File Transfer Protocol) | 20 = Data, 21 = Control | File transfer between systems | ⭐⭐⭐ 90% |
| 22 | SSH (Secure Shell) | Secure login and remote command execution | Linux server admin, Git access | ⭐⭐⭐⭐ 100% |
| 23 | Telnet | Legacy unencrypted remote login | Rarely used (insecure) | ⭐⭐ 60% |
| 25 | SMTP (Mail Transfer) | Outgoing mail server port | Used by mail servers (Postfix, SES) | ⭐⭐ 70% |
| 53 | DNS | Domain Name System queries (UDP/TCP) | Hostname → IP resolution | ⭐⭐⭐ 85% |
| 67 / 68 | DHCP | 67 = Server, 68 = Client | Dynamic IP assignment | ⭐⭐ 60% |
| 69 | TFTP | Trivial File Transfer (no auth) | Used in network boot (PXE) | ⭐ 50% |
| 80 | HTTP (Web Traffic) | Unencrypted web server traffic | Websites, APIs, ALB health checks | ⭐⭐⭐⭐ 100% |
| 110 | POP3 | Incoming mail (retrieval) | Legacy email retrieval | ⭐ 55% |
| 123 | NTP (Network Time Protocol) | Time sync between systems | Keeps system clocks aligned | ⭐⭐ 65% |
| 143 | IMAP | Mail access (modern retrieval) | Email clients | ⭐ 50% |
| 161 / 162 | SNMP | Monitoring & management protocol | Used in system/network monitoring | ⭐⭐ 65% |
| 389 | LDAP | Directory services (auth) | Used by AD, central auth | ⭐⭐ 70% |
| 443 | HTTPS (Secure HTTP) | Encrypted web traffic (TLS/SSL) | Secure websites, APIs | ⭐⭐⭐⭐ 100% |
| 465 / 587 | SMTPS / Submission | Encrypted outgoing email | Used for secure mail relay | ⭐⭐ 60% |
| 514 | Syslog | Log collection protocol | Remote logging (UDP/TCP) | ⭐⭐ 70% |
| 636 | LDAPS | Secure LDAP over TLS | Auth service | ⭐⭐ 55% |
| 873 | rsync | File synchronization service | Backups, code sync | ⭐⭐ 65% |
| 989 / 990 | FTPS (Secure FTP) | FTP over SSL/TLS | Secure file transfer | ⭐ 45% |
| 1433 | Microsoft SQL Server | Database service port | RDS SQL Server | ⭐⭐ 55% |
| 1521 | Oracle DB Listener | Oracle database connections | Enterprise databases | ⭐⭐ 60% |
| 2049 | NFS (Network File System) | Remote file sharing | EFS mounts, shared volumes | ⭐⭐⭐ 85% |
| 2181 | Zookeeper | Coordination service | Kafka clusters | ⭐⭐ 65% |
| 2379 / 2380 | etcd | Cluster coordination | Kubernetes control plane | ⭐⭐⭐ 80% |
| 3306 | MySQL / MariaDB | Database port | RDS MySQL, WordPress DB | ⭐⭐⭐⭐ 100% |
| 3389 | RDP (Remote Desktop) | Windows remote access | Windows EC2 management | ⭐⭐⭐ 85% |
| 3690 | Subversion (SVN) | Version control system | Legacy VCS | ⭐ 40% |
| 4000–4999 | Custom App Ports | Often used by in-house apps | Internal microservices | ⭐⭐ 65% |
| 5432 | PostgreSQL | Database port | RDS PostgreSQL | ⭐⭐⭐⭐ 100% |
| 5601 | Kibana | Elasticsearch web UI | ELK stack | ⭐⭐⭐ 80% |
| 5672 / 15672 | RabbitMQ (AMQP) | Messaging broker | Queueing, async jobs | ⭐⭐ 70% |
| 5900 | VNC | Remote graphical access | GUI on Linux | ⭐ 50% |
| 5985 / 5986 | WinRM / HTTPS WinRM | Windows Remote Management | Ansible on Windows | ⭐⭐ 60% |
| 6379 | Redis | In-memory key-value store | Caching, queues | ⭐⭐⭐ 85% |
| 6443 | Kubernetes API Server | K8s cluster control plane | kubectl communication | ⭐⭐⭐⭐ 100% |
| 6666 / 6667 | IRC | Legacy chat | Not common now | ⭐ 40% |
| 8000 / 8080 | Alt HTTP / Web App Ports | Used for dev/testing | App servers, proxies | ⭐⭐⭐ 90% |
| 8443 | Alt HTTPS / Secure Web App | Secure admin consoles | Jenkins, Grafana | ⭐⭐⭐ 80% |
| 8888 | Jupyter / Proxy Apps | Dev tools & dashboards | Analytics notebooks | ⭐⭐ 70% |
| 9090 | Prometheus | Metrics endpoint | Monitoring system | ⭐⭐⭐ 85% |
| 9200 / 9300 | Elasticsearch | Search & analytics engine | ELK stack | ⭐⭐⭐ 80% |
| 11211 | Memcached | In-memory cache | Caching layers | ⭐⭐ 65% |
| 27017 | MongoDB | NoSQL database | Dev/test apps | ⭐⭐⭐ 75% |
| 3000 | Grafana / Node.js Apps | Dashboards or services | DevOps monitoring | ⭐⭐⭐ 80% |
| 5000 / 5001 | Flask / Docker registry / API | Custom APIs | Dev/testing | ⭐⭐ 65% |
| 9092 | Kafka Broker | Messaging system | Data pipelines | ⭐⭐⭐ 80% |
| 9200 | ElasticSearch REST API | Cluster communication | Logging systems | ⭐⭐⭐ 85% |
| 33060 | MySQL X Protocol | Extended API for MySQL | Modern DB operations | ⭐ 45% |

---

### 🧠 Quick Reference Summary

**Most Common (100% Frequency):**
`22 (SSH), 80 (HTTP), 443 (HTTPS), 3306 (MySQL), 5432 (PostgreSQL), 3389 (RDP), 6443 (Kubernetes API)`

**AWS-Specific Security Group Ports:**
- 22 → SSH for Linux EC2  
- 3389 → RDP for Windows EC2  
- 80 / 443 → Web apps, ALB/ELB  
- 2049 → EFS Mount Targets  
- 5432 / 3306 → RDS Databases  
- 9090 / 5601 / 3000 → Monitoring (Prometheus, Kibana, Grafana)  
- 6443 → EKS/Kubernetes API  

---

✅ **Tip for Interviews:**  
Memorize ports by *category* rather than number. For example:  
- **Web:** 80, 443, 8080, 8443  
- **DB:** 3306, 5432, 1433, 1521, 27017  
- **Monitoring:** 9090, 5601, 3000  
- **Messaging:** 5672, 9092, 2181  
- **Storage:** 2049 (NFS), 873 (rsync)  
- **System Admin:** 22 (SSH), 3389 (RDP), 5985/5986 (WinRM)
___
---

_-_-_-
3. Trick Questions (frequent in real interviews):

“If port 22 is blocked, how can you still SSH?” → Use SSM Session Manager.

“HTTP works but HTTPS doesn’t — which port must be opened?” → Port 443.

“App works internally but not from outside — which ports to check?” → Security Group ingress + NACL + Load Balancer listener ports.



