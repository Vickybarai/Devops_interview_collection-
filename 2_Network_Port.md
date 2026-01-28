# 🚀 MOST ASKED PORTS IN DEVOPS (High → Low)

|      🔢 Port     | 🧠 Service     | ⚙️ What It Powers           | 🎯 Where You Use It              | 💬 Frequency |
| :--------------: | :------------- | :-------------------------- | :------------------------------- | :----------: |
|      **22**      | SSH            | Secure remote server access | Linux admin, Git, CI/CD runners  |   ⭐⭐⭐⭐ 100%  |
|      **80**      | HTTP           | Web traffic (non-TLS)       | Websites, APIs, LB health checks |   ⭐⭐⭐⭐ 100%  |
|      **443**     | HTTPS          | Secure web traffic          | Production apps, APIs, Ingress   |   ⭐⭐⭐⭐ 100%  |
| **53 (TCP/UDP)** | DNS            | Domain → IP resolution      | Entire infrastructure            |   ⭐⭐⭐⭐ 100%  |
|     **3306**     | MySQL          | Relational DB               | Web app backends                 |   ⭐⭐⭐⭐ 100%  |
|     **5432**     | PostgreSQL     | Relational DB               | Cloud-native backends            |   ⭐⭐⭐⭐ 100%  |
|     **6443**     | Kubernetes API | K8s control plane           | `kubectl`, cluster ops           |   ⭐⭐⭐⭐ 100%  |
|     **8080**     | Alt HTTP       | App servers                 | Spring Boot, Tomcat              |    ⭐⭐⭐ 95%   |
|     **6379**     | Redis          | Cache / sessions            | High-speed backend caching       |    ⭐⭐⭐ 90%   |
|     **27017**    | MongoDB        | NoSQL DB                    | Microservices stacks             |    ⭐⭐⭐ 90%   |
|     **9090**     | Prometheus     | Metrics endpoint            | Monitoring infra                 |    ⭐⭐⭐ 85%   |
|     **3000**     | Grafana / Node | Dashboards / Dev apps       | Observability, frontend dev      |    ⭐⭐⭐ 85%   |
|     **5601**     | Kibana         | Log UI                      | ELK stack                        |    ⭐⭐⭐ 85%   |
|     **9200**     | Elasticsearch  | Search & logging            | Log storage, analytics           |    ⭐⭐⭐ 85%   |
|     **9092**     | Kafka          | Event streaming             | Data pipelines                   |    ⭐⭐⭐ 80%   |
|     **2049**     | NFS            | Shared storage              | EFS, volumes                     |    ⭐⭐⭐ 80%   |
|     **3389**     | RDP            | Windows remote access       | Windows server mgmt              |    ⭐⭐⭐ 80%   |
|   **2379–2380**  | etcd           | K8s database                | Cluster coordination             |    ⭐⭐⭐ 80%   |
|     **5672**     | RabbitMQ       | Messaging queue             | Async jobs                       |    ⭐⭐ 70%    |
|   **25 / 587**   | SMTP           | Email sending               | Alerts, app mail                 |    ⭐⭐ 70%    |
|      **389**     | LDAP           | Directory auth              | AD/SSO                           |    ⭐⭐ 65%    |
|   **5985/5986**  | WinRM          | Windows automation          | Ansible + Windows                |    ⭐⭐ 65%    |
|      **123**     | NTP            | Time sync                   | Logs, clusters                   |    ⭐⭐ 65%    |
|      **514**     | Syslog         | Log forwarding              | Central logging                  |    ⭐⭐ 60%    |
|     **1433**     | MS SQL         | SQL Server DB               | Enterprise apps                  |    ⭐⭐ 60%    |
|     **1521**     | Oracle DB      | Oracle listener             | Legacy enterprise DB             |    ⭐⭐ 55%    |
---

# 🛠️ DEVOPS / TOOLING PORTS

|    🔢 Port    | Tool             | Role |
| :-----------: | :--------------- | :--- |
|    **6443**   | Kubernetes API   |      |
|   **10250**   | Kubelet          |      |
| **2375/2376** | Docker daemon    |      |
|    **5601**   | Kibana           |      |
|    **9090**   | Prometheus       |      |
|    **3000**   | Grafana          |      |
|    **8081**   | Nexus repo       |      |
|    **8082**   | Sonatype         |      |
|    **8443**   | Jenkins HTTPS UI |      |
|    **9418**   | Git (native)     |      |

---

# ✅ CORRECT DEFAULT PORTS (Fix List)

| Service         | Default Port |
| --------------- | ------------ |
| HTTP            | **80**       |
| HTTPS           | **443**      |
| SSH             | **22**       |
| FTP             | **21**       |
| MySQL           | **3306**     |
| PostgreSQL      | **5432**     |
| MongoDB         | **27017**    |
| Redis           | **6379**     |
| Kubernetes API  | **6443**     |
| Docker Registry | **5000**     |
| Elasticsearch   | **9200**     |
| Kibana          | **5601**     |
| Grafana         | **3000**     |
| Prometheus      | **9090**     |

---
---

# 🌐 FRONTEND DEV PORTS (Very Common)

|    🔢 Port   | Framework / Tool | Purpose                     |
| :----------: | :--------------- | :-------------------------- |
|   **3000**   | React / Node     | Dev server                  |
|   **5173**   | Vite             | Modern frontend tooling     |
|   **4200**   | Angular          | Angular dev server          |
|   **8080**   | Vue / Webpack    | Frontend dev builds         |
| **80 / 443** | Nginx / CDN      | Production frontend hosting |

---

# 🧠 BACKEND DEV PORTS

|    🔢 Port    | Backend Tech       | Purpose             |
| :-----------: | :----------------- | :------------------ |
|    **8000**   | Django / FastAPI   | Python backend      |
|    **5000**   | Flask              | Python API          |
|    **8080**   | Spring Boot / Java | REST services       |
| **3001–4000** | Node APIs          | Express/NestJS      |
|    **9000**   | SonarQube          | Code quality server |
| **7001/7002** | WebLogic           | Enterprise Java     |



# 🎯 Interview Reality

Most scenarios revolve around just these core ports:

**22, 80, 443, 53, 3306, 5432, 6379, 6443, 8080, 9090**
