---
project: SNM Lab Exam
---

# **Step 1:** "who talks to whom" matrix.

|         | Server | Internet | DHCP/DNS | Student | Faculty | Guest-A    | Guest-B    |
| ------- | ------ | -------- | -------- | ------- | ------- | ---------- | ---------- |
| Student | ✅      | ✅        | ✅        | ✅       | ❌       | ❌          | ❌          |
| Faculty | ✅      | ✅        | ✅        | ❌       | ✅       | ❌          | ❌          |
| Guest-A | ❌      | ✅        | ✅        | ❌       | ❌       | ✅          | Not stated |
| Guest-B | ❌      | ✅        | ✅        | ❌       | ❌       | Not stated | ✅          |
==🟡***Assumption: Guest-A and Guest-B cannot talk to each other.***==

VLAN Groups: Student, Faculty, Guest-A, Guest-B, Server

### **Assign VLANs:**

|          | Student | Faculty | Guest-A | Guest-B | Server |
| -------- | ------- | ------- | ------- | ------- | ------ |
| VLAN No. | 10      | 20      | 11      | 21      | 30     |

## Traffic Flow list

| Source  | Destination | Source Port | Destination Port | Protocol | Permit/Deny |     |
| ------- | ----------- | ----------- | ---------------- | -------- | ----------- | --- |
| Student | Faculty     | -           | -                | -        | Deny        | ❌   |
| Faculty | Student     | -           | -                | -        | Deny        | ❌   |
| Guest-A | Student     | -           | -                | -        | Deny        | ❌   |
| Guest-A | Faculty     | -           | -                | -        | Deny        | ❌   |
| Guest-B | Student     | -           | -                | -        | Deny        | ❌   |
| Guest-B | Faculty     | -           | -                | -        | Deny        | ❌   |
| Student | Guest-A     | -           | -                | -        | Deny        | ❌   |
| Student | Guest-B     | -           | -                | -        | Deny        | ❌   |
| Faculty | Guest-A     | -           | -                | -        | Deny        | ❌   |
| Faculty | Guest-B     | -           | -                | -        | Deny        | ❌   |
| Student | Server      | -           | 80, 443          | TCP, UDP | Permit      | ✅   |
| Faculty | Server      | -           | 80, 443          | TCP, UDP | Permit      | ✅   |
| Student | Server      | -           | Other ports      | -        | Deny        | ❌   |
| Faculty | Server      | -           | Other ports      | -        | Deny        | ❌   |
| Student | Internet    | -           | -                | -        | Permit      | ✅   |
| Faculty | Internet    | -           | -                | -        | Permit      | ✅   |
| Guest-A | Internet    | -           | -                | -        | Permit      | ✅   |
| Guest-B | Internet    | -           | -                | -        | Permit      | ✅   |
| Student | DHCP/DNS    | -           | -                | -        | Permit      | ✅   |
| Faculty | DHCP/DNS    | -           | -                | -        | Permit      | ✅   |
| Guest-A | DHCP/DNS    | -           | -                | -        | Permit      | ✅   |
| Guest-B | DHCP/DNS    | -           | -                | -        | Permit      | ✅   |
***==🟡Once the ACL is applied, anything that doesn't match a permit line is dropped.==***


# **Step 2.** Put numbers on everything.

## Hosts per group

| Group   | Hosts | Bandwidth |
| ------- | ----- | --------- |
| Student | 120   | 600 Mbps  |
| Faculty | 60    | 300 Mbps  |
| Guest-A | 50    | -         |
| Guest-B | 30    | -         |
| Server  | 8     | 4 Gbps    |
- Student & Faculty requires average 5 Mbps per user.
- Guest bandwidth is insignificant.
- Server room capacity at least 4 Gbps.
- Server should support 400 concurrent users with at least 10 Mbps => 4 Gbps
## What should survive failure?

Connectivity from:

| From       | To          |
| ---------- | ----------- |
| Building A | Server      |
| Building A | Edge router |
| Building B | Server      |
| Building B | Edge router |

# **Step 3.** Ask the failure question.

![[Pasted image 20261031165018.png|rotate90|428]]

# **Step 4.** Choose the structure.

![[LAN Design-1793481305042.webp|401]]  ![[LAN Design-1793481602082.webp|408]] 

Due to internal routing, scaling and lack of ports in the router, we need a switch in between.
![[LAN Design-1793481986226.webp|449]]

Since one core switch is a single point of failure, we add two core switches.
![[LAN Design-1793482150041.webp|457]]

### Network capacity Requirements
![[LAN Design-1793482969532.webp|622]]


# Where should the ACLs be?

<mark style="background:#ff4d4f">The ACLs will be present in the access switches. This is because in a collapsed core and distribution architecture, all the user traffic moves through the core switches and keeping ACLs at the access switches prevents all unauthorized traffic to reach the core and congest it.</mark>

<mark style="background:#d4b106">We place the ACLs on the collapsed-core switches because they provide the Layer 3 gateways for all four VLANs. All inter-VLAN traffic passes through these gateways. This lets us apply the security policies at a central point. It reduces configuration work and makes maintenance easier. The disadvantage is that denied traffic still uses the uplinks to reach the core. Also, the core SVI ACLs cannot block traffic that stays within one VLAN. We use access-layer filtering only when we need this additional control.</mark>

### Tradeoffs: Access switch vs. collapsed core

| Factor             | ACL at access switch                                        | ACL at collapsed core                              |
| ------------------ | ----------------------------------------------------------- | -------------------------------------------------- |
| Traffic filtering  | Drops packets close to the source                           | Drops packets at the routing boundary              |
| Configuration      | Must apply policy to relevant access ports or VLANs         | Can use one ACL attachment per SVI                 |
| Management         | More places to maintain policy                              | Easier to maintain centrally                       |
| Uplink bandwidth   | Saves bandwidth by dropping unwanted traffic early          | Denied packets still use the access-to-core uplink |
| Same-VLAN traffic  | Can filter it with PACLs or suitable VLAN controls          | SVI ACL cannot filter locally switched traffic     |
| Hardware resources | Uses ACL resources on access switches                       | Uses ACL resources on core switches                |
| Failure impact     | A configuration error is often limited to one access switch | A configuration error can affect a whole VLAN      |
| Troubleshooting    | More distributed rules to check                             | Fewer enforcement points to inspect                |

## Case 2: primary and backup (floating static route)

Configuration on Core1:

```
ip route 0.0.0.0 0.0.0.0 172.31.0.1        ← distance 1 (default for static)
ip route 0.0.0.0 0.0.0.0 172.31.0.3 5      ← distance 5, backup
```

The core installs only the route with the lowest distance. The backup stays in the configuration but not in the routing table.

How a packet moves: every flow goes to Edge1. Edge2's link carries no traffic.

When a link or router fails:

- Edge1's link goes down. The distance-1 route is withdrawn. The core installs the distance-5 route, and all traffic moves to Edge2.
- When Edge1 returns, the distance-1 route wins again and traffic moves back.

How to test: ping the Internet target from a test host, disconnect the Edge1 link, and watch for the number of lost pings before it recovers. Reconnect and confirm the path moves back.

Trade-offs:

- Predictable and easy to demonstrate, so troubleshooting is simple.
- The Edge2 link and router sit idle. It does not meet the "share the load" idea for this path. If your design requires use of redundant links where possible, state why you accepted this.
	- The core must detect the failure. This works for a link that goes down. If Edge1 stops forwarding while the link stays up, the static route stays in the table. Add route tracking (IP SLA with `track`) or BFD, or accept the gap and document it.