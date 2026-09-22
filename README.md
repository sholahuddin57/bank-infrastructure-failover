# bank-infrastructure-failover
Enterprise-grade high availability architecture using Docker and Nginx.

# 🏦 High-Availability Bank Infrastructure: Zero-Downtime Load Balancing

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
2. **The Disaster Simulation (Problem):** Intentionally forced a critical failure by aggressively stopping Node 1 (`docker stop web-promosi-v2`) while the system was receiving traffic. 
3. **The Failover (Action & Result):** Initially caught a `502 Bad Gateway` error, proving the Load Balancer was actively monitoring backend health. Within milliseconds, the Load Balancer recognized the dead node and automatically routed all subsequent traffic to the healthy Node 2. **Result:** The banking application remained accessible. User operations were uninterrupted.

### 🧠 Key Engineering Takeaways
- Overcame Linux Kernel I/O bottlenecks (`watchdog: BUG: soft lockup`) by optimizing VirtualBox CPU/RAM allocations and disabling Windows Hyper-V conflicts.
- Mastered Docker container lifecycle management, volume mapping (`-v`), and port forwarding.
- Demonstrated practical understanding of Reverse Proxy configuration and backend health monitoring.

---
*Note: Check the `loadbalancer.conf` file in this repository for the exact Nginx routing configuration.*
