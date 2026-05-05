# IPv6 over IPv4 Tunnel Lab

## Overview

This project demonstrates IPv6 networking, dual stack communication, and IPv6-over-IPv4 tunneling using Docker-based network topologies.

The lab focuses on verifying connectivity, testing routing behavior, observing hop-limit behavior, and confirming that IPv6 traffic can be encapsulated inside IPv4 using Protocol 41.

---

## Network Topology

The IPv6-over-IPv4 tunnel topology includes:

- H1N6: IPv6 host on Site A
- R1N64: Dual-stack router
- R2N4: IPv4-only router
- R3N64: Dual-stack router
- H2N6: IPv6 host on Site B

IPv6 traffic travels between H1N6 and H2N6 while passing through an IPv4-only core network.

## IPv6 Network Diagram

![IPv6 Network Diagram](simple-ipv6.png)


## Dual Stack Network Diagram

![Dual Stack Network Diagram](dual-stack.png)


## IPv6 over IPv4 Tunnel Diagram

![IPv6 over IPv4 Tunnel Diagram](ipv6-tunnel.png)
---

## IPv6 Ping Success

This test verifies that H1N6 can successfully reach H2N6 using IPv6.

```bash
docker exec H1N6 ping -c2 fd00:1003::5
```

Result:

- H1N6 successfully reached H2N6
- 2 packets transmitted
- 2 packets received
- 0% packet loss

![IPv6 Ping Success](IPv6%20Tunnel%20over%20IPv4%20%E2%80%93%20Successful%20Ping%20from%20H1N6%20to%20H2N6.png)

---

## Reverse Connectivity

This test verifies that H2N6 can also reach H1N6 using IPv6.

```bash
docker exec H2N6 ping -c2 fd00:1001::5
```

Result:

- H2N6 successfully reached H1N6
- Reverse communication worked correctly
- This confirms bidirectional IPv6 connectivity

![Reverse Ping](IPv6%20Tunnel%20over%20IPv4%20%E2%80%93%20Successful%20Reverse%20Ping%20from%20H2N6%20to%20H1N6.png)

---

## HTTP Connection Test

This test verifies that application-layer traffic works over IPv6.

```bash
docker exec -it H1N6 curl http://[fd00:1003::5]/welcome.html
```

Result:

- The HTTP request succeeded
- The web page was retrieved over IPv6
- This confirms that services can communicate across the IPv6 tunnel

![HTTP Test](IPv6%20Tunnel%20Reachability%20%E2%80%93%20H1N6%20to%20H2N6%20Ping%20and%20HTTP%20Access%20over%20Tunnel.png)

---

## Hop Limit / TTL Limit Test

This test uses a low hop limit to show what happens when a packet cannot reach the destination before its hop limit expires.

```bash
docker exec H1N6 ping -c2 -t2 fd00:1003::5
```

Result:

- The packet failed before reaching the destination
- The output showed `Time exceeded: Hop limit`
- This demonstrates that routers drop packets when the IPv6 hop limit reaches zero

![TTL Failure](IPv6%20Network%20Limitation%20%E2%80%93%20Ping%20Failure%20with%20TTL%20Constraint%20(-t2).png))
---

## Tunnel Traffic Analysis

This test uses `tcpdump` on R2N4 to observe traffic passing through the IPv4-only router.

```bash
docker exec -it R2N4 tcpdump -i any proto 41
```

Observation:

- tcpdump showed IPv6 traffic encapsulated inside IPv4 packets
- Protocol 41 confirmed IPv6-over-IPv4 tunneling
- This proves that the IPv6 packets were carried through the IPv4 core network

![Tcpdump](IPv6%20Tunnel%20Traffic%20Analysis%20%E2%80%93%20R2N4%20tcpdump%20Showing%20IPv6%20Encapsulated%20in%20IPv4%20(Protocol%2041).png)


---

## Routing Table Verification

Routing tables were checked to confirm that both IPv4 and IPv6 routes were configured correctly.

```bash
docker exec R1N64 ip route
docker exec R1N64 ip -6 route
docker exec R2N64 ip route
docker exec R2N64 ip -6 route
```

Result:

- IPv4 routes were present on dual-stack routers
- IPv6 routes were present for IPv6 networks
- Default routes were configured correctly
- Routing tables confirmed that traffic had a valid path across the topology

![Routing Tables](Dual%20Stack%20Routing%20Tables%20%E2%80%93%20IPv4%20and%20IPv6%20Routes%20on%20R1N64%20and%20R2N64.png)

---

## Dual Stack Connectivity

The dual stack topology was tested using both IPv4 and IPv6.

```bash
docker exec H1N64 ping -c2 172.21.3.5
docker exec H1N64 ping -c2 fd00:1003::5
```

Result:

- IPv4 communication succeeded
- IPv6 communication succeeded
- This confirms that the dual stack network supports both protocols at the same time

![Dual Stack Ping](Dual%20Stack%20Connectivity%20%E2%80%93%20IPv4%20and%20IPv6%20Ping%20Success%20from%20H1N64.png)

---

## Key Findings

- IPv6 communication works across properly configured Docker networks
- Dual stack allows IPv4 and IPv6 to operate at the same time
- IPv6-over-IPv4 tunneling allows IPv6 traffic to cross an IPv4-only network
- Protocol 41 is used to encapsulate IPv6 packets inside IPv4 packets
- Hop limit behavior prevents packets from routing forever
- tcpdump is useful for proving what is actually happening inside the network

---

## Conclusion

This lab successfully demonstrated IPv6 connectivity, dual stack communication, and IPv6-over-IPv4 tunneling using Docker. The tests confirmed that IPv6 hosts can communicate across an IPv4-only core network when tunnel endpoints are configured correctly. Packet captures verified that the tunnel used Protocol 41, and routing table checks confirmed that the network paths were properly configured.
