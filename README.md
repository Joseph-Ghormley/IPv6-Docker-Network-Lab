# IPv6 Networking Lab with Docker

## Overview
This project demonstrates the implementation and analysis of three networking environments using Docker:

- IPv6-only network  
- Dual Stack network (IPv4 + IPv6)  
- IPv6 over IPv4 tunneling (Protocol 41)  

The purpose of this lab was to understand how IPv6 communication works across different network configurations and how IPv6 traffic can be transported over an IPv4-only network using tunneling.

---

## What I Did

- Deployed Docker-based network topologies using provided `.yml` files  
- Verified connectivity using `ping`, `ping6`, and `curl`  
- Analyzed routing tables using `ip route` and `ip -6 route`  
- Captured traffic using `tcpdump`  
- Observed IPv6 encapsulation inside IPv4 packets  

---

## Network Topologies

### IPv6-Only Network
![IPv6 Diagram](simple-ipv6.png)

**Observation:**
- Communication occurs only over IPv6 addresses  
- Proper IPv6 routing must be configured across routers  

---

### Dual Stack Network (IPv4 + IPv6)
![Dual Stack Diagram](dual-stack.png)

**Observation:**
- Devices support both IPv4 and IPv6 communication  
- `ping` uses IPv4 while `ping6` uses IPv6  
- Both protocols can operate simultaneously  

---

### IPv6 over IPv4 Tunnel
![Tunnel Diagram](ipv6-tunnel.png)

**Observation:**
- IPv6 packets successfully travel through an IPv4-only network  
- Tunnel endpoints on dual-stack routers enable encapsulation  

---

## Connectivity Testing

### IPv6 Ping Across Tunnel

```bash
docker exec H1N6 ping -c2 fd00:1003::5
