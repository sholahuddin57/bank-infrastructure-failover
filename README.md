# 🏦 High-Availability Bank Infrastructure: Zero-Downtime Load Balancing
# bank-infrastructure-failover
Enterprise-grade high availability architecture using Docker and Nginx.

### 📌 Project Objective
Simulating an enterprise-grade IT infrastructure for a banking application that requires **99.9% uptime**. The goal is to build a robust architecture capable of handling high traffic and automatically surviving unexpected server crashes without disrupting the user experience (Zero-Downtime).

### 🏗️ Architecture Topology
This project utilizes a containerized environment to ensure isolated, scalable, and reproducible server deployments.
- **Environment:** Ubuntu Server running on VirtualBox (Windows 11 Host).
- **Containerization:** Docker.
- **Load Balancer (Reverse Proxy):** Nginx deployed on Port 80.
- **Backend Nodes (Tellers):** Two independent Nginx web servers running on Port 9090 (Node 1) and Port 9091 (Node 2).

### 🚀 Implementation & Disaster Recovery Simulation
To validate the architecture's resilience, I implemented a strict **Problem-Action-Result (PAR)** testing phase:

1. **The Setup:** Configured an Nginx Load Balancer using the `upstream` directive to distribute incoming traffic equally (Round Robin) between Node 1 and Node 2.
   <img width="1311" height="690" alt="1-normal-state" src="https://github.com/user-attachments/assets/73e42d4b-e6e9-405a-9004-410b9d6365a4" />
   
2. **The Disaster Simulation (Problem):** Intentionally forced a critical failure by aggressively stopping Node 1 (`docker stop web-promosi-v2`) while the system was receiving traffic.
   <img width="1320" height="350" alt="2-disaster-simulation" src="https://github.com/user-attachments/assets/8a73d862-c104-4cee-b7d9-88b9fa746f1b" />
   
3. **The Failover (Action & Result):** Initially caught a `502 Bad Gateway` error, proving the Load Balancer was actively monitoring backend health. Within milliseconds, the Load Balancer recognized the dead node and automatically routed all subsequent traffic to the healthy Node 2. **Result:** The banking application remained accessible. User operations were uninterrupted.
   <img width="1316" height="661" alt="3-failover-success" src="https://github.com/user-attachments/assets/3f9312e5-440f-4420-8110-4b4cb1f6960f" />

   <img width="1327" height="307" alt="4-nginx-config" src="https://github.com/user-attachments/assets/5a0e1957-af1c-4e41-913f-340508edc278" />

### 🧠 Key Engineering Takeaways
- Overcame Linux Kernel I/O bottlenecks (`watchdog: BUG: soft lockup`) by optimizing VirtualBox CPU/RAM allocations and disabling Windows Hyper-V conflicts.
- Mastered Docker container lifecycle management, volume mapping (`-v`), and port forwarding.
- Demonstrated practical understanding of Reverse Proxy configuration and backend health monitoring.

---
*Note: Check the `loadbalancer.conf` file in this repository for the exact Nginx routing configuration.*
