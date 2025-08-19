# ⚖️ Load Balancers – High Level Explanation (Traditional & AWS)

---

## 📌 Introduction
A **Load Balancer (LB)** distributes incoming traffic across multiple targets (servers, containers, microservices, or appliances) to ensure:
- **High Availability**
- **Scalability**
- **Performance**
- **Fault Tolerance**
- **Security**

---

## 🔹 Why Use Load Balancers?
- 🚦 Balance workload across multiple servers.  
- 🛡️ Protect applications from failures (automatic failover).  
- 🔑 Add SSL termination, authentication, and routing logic.  
- 📈 Handle high traffic with auto-scaling.  

---

# ☁️ AWS Elastic Load Balancers (ELB)

AWS provides **4 main types of Load Balancers**:  
- Application Load Balancer (ALB)  
- Network Load Balancer (NLB)  
- Gateway Load Balancer (GWLB)  
- Classic Load Balancer (CLB)  

---

## 1️⃣ Application Load Balancer (ALB) – Layer 7
The **most advanced load balancer** in AWS, designed for **HTTP/HTTPS** traffic.  
It can inspect the **application layer** and make intelligent routing decisions.  

### 🔹 Key Features
- **Path-based routing:**  
  `/api/*` → API servers, `/app/*` → frontend servers.  
- **Host-based routing:**  
  `shop.example.com` → e-commerce app, `blog.example.com` → CMS app.  
- **Header-based routing:**  
  Traffic with `X-Version: beta` → beta environment.  
- **Query string/rule-based routing:**  
  `/products?category=clothing` → fashion backend.  
- **Policy-based rules:** Combine multiple conditions (path + header + source IP).  
- **WebSocket & gRPC support** (real-time apps).  
- **SSL termination** – terminate HTTPS at LB and forward HTTP internally.  
- **Authentication offload** – integrate with Cognito/IdP before requests hit backend.  

### 🔹 Example Scenarios (5)
1. **E-commerce website** → `/cart` routes to checkout microservice, `/api` routes to API service.  
2. **Multi-tenant SaaS** → Host-based routing per client domain.  
3. **Blue/Green Deployment** → ALB policy routes 90% to v1, 10% to v2.  
4. **A/B Testing** → Header-based routing: `X-Test: true` routes to experimental backend.  
5. **Microservices** → Each service (auth, payments, products) mapped to ALB target groups.  

---

## 2️⃣ Network Load Balancer (NLB) – Layer 4
Designed for **TCP/UDP/TLS** workloads at **very high scale**.  
Focuses on **performance and low latency**.

### 🔹 Key Features
- **Ultra-low latency** (millions of requests/sec).  
- **Static IP & Elastic IP** support.  
- **TLS offloading** (terminate SSL at NLB).  
- **Preserve source IP** – backend sees client IP.  
- **Zonal isolation** – fault-tolerant per AZ.  
- **Target groups can be EC2, ECS, IP addresses, Lambda**.  

### 🔹 Example Scenarios (5)
1. **Financial trading system** – Sub-millisecond response required.  
2. **Real-time multiplayer gaming** – UDP-based low-latency connections.  
3. **IoT telemetry ingestion** – Millions of sensors sending TCP/UDP data.  
4. **VoIP infrastructure** – Reliable and low-latency UDP routing.  
5. **TLS-secured APIs** – Terminate TLS at NLB, forward decrypted traffic to backends.  

---

## 3️⃣ Gateway Load Balancer (GWLB) – Layer 3
Specialized LB to deploy and scale **network/security appliances** inline.

### 🔹 Key Features
- **Bump-in-the-wire** architecture – transparent insertion of appliances.  
- Uses **GENEVE protocol** to encapsulate traffic.  
- **Auto scaling of appliances** (firewalls, IDS/IPS).  
- Works like a **gateway** for North-South traffic.  

### 🔹 Example Scenarios (5)
1. **Centralized Firewall** – Route VPC traffic through firewall appliances.  
2. **Intrusion Detection/Prevention (IDS/IPS)** – Inspect packets before reaching servers.  
3. **DDoS Mitigation Layer** – Send traffic through scrubbing centers.  
4. **Compliance Monitoring** – PCI DSS / HIPAA deep packet inspection.  
5. **Partner Network Filtering** – Enforce security policies on external partner connections.  

---

## 4️⃣ Classic Load Balancer (CLB) – Layer 4 & 7 (Legacy)
The **first generation AWS LB**, now mostly replaced by ALB and NLB.  

### 🔹 Key Features
- Supports **both TCP (Layer 4)** and **HTTP/HTTPS (Layer 7)**.  
- Basic SSL termination.  
- Health checks.  
- Limited rule-based routing (compared to ALB).  
- Still used for **legacy apps** and EC2-Classic networks.  

### 🔹 Example Scenarios (5)
1. **Legacy monolithic web app** – Simple HTTP distribution to backend EC2s.  
2. **Basic intranet applications** – Quick setup for internal dashboards.  
3. **Startups MVP** – Fast way to balance small apps before scaling.  
4. **Test environments** – Cheap/simple LB for dev/test workloads.  
5. **Older EC2-Classic deployments** – Compatibility requirement.  

---

# 🔹 Summary: When to Use Which?

| Load Balancer | Best For | Layer | Key Capabilities |
|---------------|----------|-------|------------------|
| **ALB** | Web apps, APIs, Microservices, Containers | L7 | Path/Host/Header/Policy routing, WebSockets, SSL, Auth |
| **NLB** | High-performance TCP/UDP, Gaming, IoT | L4 | Millions req/sec, Static IP, TLS termination |
| **GWLB** | Security appliances (firewalls, IDS/IPS) | L3 | Transparent inline traffic inspection |
| **CLB** | Legacy/simple apps, EC2-Classic | L4 & L7 | Basic load balancing, legacy compatibility |

---

## 🔹 Outcomes of Load Balancers
- 🚀 **Performance** – Faster response time.  
- 🟢 **High Availability** – Redundancy and failover.  
- 🔒 **Security** – SSL/TLS offloading, integration with WAF/Shield.  
- 📈 **Scalability** – Scale horizontally with demand.  
- ⚡ **Reliability** – Seamless user experience.  

---

## 🎯 Conclusion
- **ALB** – Use for modern **HTTP/HTTPS apps** needing intelligent routing (microservices, SaaS).  
- **NLB** – Use for **ultra-low latency TCP/UDP** workloads (gaming, finance, IoT).  
- **GWLB** – Use for **security appliances** inline (firewalls, IDS/IPS).  
- **CLB** – Use for **legacy/simple** apps (backward compatibility).  

👉 **Rule of Thumb:**  
If it’s **web traffic → ALB**.  
If it’s **TCP/UDP → NLB**.  
If it’s **security appliances → GWLB**.  
If it’s **legacy → CLB**.
