# IPv6 Networking Lab with Docker

## Overview
This project demonstrates the implementation and analysis of three networking environments using Docker:

- IPv6-only network
- Dual Stack network (IPv4 + IPv6)
- IPv6 over IPv4 tunneling (Protocol 41)

The lab focuses on connectivity, routing behavior, and packet encapsulation using container-based environments.

---

## Network Topologies

### 1. IPv6-Only Network
![IPv6 Diagram](simple-ipv6.png)

### 2. Dual Stack (IPv4 + IPv6)
![Dual Stack Diagram](dual-stack.png)

### 3. IPv6 over IPv4 Tunnel
![Tunnel Diagram](ipv6-tunnel.png)

---

## Technologies Used

- Docker / Docker Compose
- IPv6 and IPv4 networking
- Linux networking tools
- tcpdump for packet analysis

---

## How to Run

```bash
docker compose -f docker/LAN64-3R2H-v6-over-v4.yml up -d
docker ps
