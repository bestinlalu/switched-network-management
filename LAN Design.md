
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

## ACL list

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

