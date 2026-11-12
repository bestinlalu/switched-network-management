Performance:
1. [ ] Server room has at least 4 Gbps of capacity.
	- <mark style="background:#fff88f">We will connect at least 4 links from the access switch of the server room. Each link has maximum capacity of 1 Gbps. All the links from one server side access switch will be aggregated as one virtual port channel (Etherchannel).</mark>
2. [ ] Redundant links should share the load.
	- <mark style="background:#fff88f">Using Etherchannel (LACP) and vPC, we will distribute the load between links. This is used between Buildings, Server Room and core switches.</mark>
3. [ ] Reduce or eliminate unnecessary traffic detours.
	1. <mark style="background:#fff88f">For this, unauthorized transmission of packets from different user groups are stopped at the closest switches (access switches) using ACLS.</mark>

Security:
1. [ ] Guest and IoT can reach only the DHCP, DNS servers and the Internet.
	- <mark style="background:#fff88f"><font color="#000000">We will use ACLs to drop packets from Guest and IoT devices based on the source and destination IP address. If the destination IP address is in the Student, Faculty, or Server VLAN/subnet, the access switch will drop the traffic.</font></mark>
2. [ ] Students and Faculty can reach the web app on TCP 80 and 443, and Internet.
	- <mark style="background:#fff88f">This is done by permitting transmission of packets using ACLs which checks if the source is either Students and Faculty and destination is the Server subnet and checking if the protocol is TCP/UDP.</mark>
3. [ ] Students and faculty cannot reach each others' VLANs.
	- <mark style="background:#fff88f"> This is done by denying any inter-VLAN communication between Student and Faculty using ACLs.</mark>

Scalability:
1. [ ] Growth in student + faculty counts.
	1. <mark style="background:#fff88f">We designed the network to absorb new students and faculty without a rebuild. Each subnet is larger than today's user count, and each building has a contiguous address block with unused space. New users and subnets fit inside that block, so the single static route to each building does not change. The access switches have unused ports for new users, and we allocated more than the 5 Mbps average per user (X Mbps), so the links can carry additional users. The core switches also have unused ports, so we can add access switches without changing the existing design. </mark>
2. [ ] Route summarization - No more than one static route to each building.
	1. There is only one route per building at the edge router. The Guest and the corresponding student/faculty group present in a building have contiguous ip subnets. So route summarization is possible. More details are present in Section 4.

Reliability/ High availability:
1. [ ] Single point of failure should not cut any user group off from the Server Room or the Internet.
	1. We are providing redundant links everywhere. From the access switches in a building, there are total of two connections towards the core switches. So even if one link fails, the other link can route the traffic.
	2. The server room has total of eight different links towards the core switches. Four to C1 and four to C2. This is to provide redundancy if any link goes down, it is able to manage the connection. If an access/core switch is down or all the four links between an access or core is down, the other four links can still provide availability bundled with HSRP and also provide the bandwidth since there still are 4 links present.
	3. There are two links from each core which goes to the two different routers providing redundancy.
2. [ ] Recovery from failures should be fast.
	1. HSRP on the core switches makes on core as active and one as backup. The hello messages between them are transmitted through the ether channel separate from the links towards the access or core switches.
	2. If one of the edge router goes down, both the cores can transmit through the other one. 
	3. If the upstream links from one core to the routers goes down, the core which is disconnected still can transmit through the redundant layer 3 link between the cores and reach the router.
	4. If the upstream links from one core to the routers and the layer 3 link between the cores goes down, then the layer 2 etherchannel between the cores is used to communicate from the disconnected core to the router along with the HSRP hello messages.

Bandwidth:
1. [ ] Students - About 5 Mbps average per user.
	1. Total number of students = 120
	   Minimum capacity required = 120 x 5 Mbps = 600 Mbps
	   Total number of links from the access switches to cores = 6
	   One link has a bandwidth of 1Gbps. This condition is inherently met.
2. [ ] Faculty and staff - About 5 Mbps average per user.
	1. Total number of students = 60
	   Minimum capacity required = 60 x 5 Mbps = 300 Mbps
	   Total number of links from the access switches to cores = 6
	   One link has a bandwidth of 1Gbps. This condition is inherently met.
3. [ ] Server - Student portal must serve up to 400 concurrent users at about 10 Mbps each.
	1. 400 users at 10 Mbps = 4 Gbps
	   We have 8 total links connected from the server room towards the server-side access switches. Assuming that the server is running on more than 4 servers, this condition is met.

DHCP and DNS:
1. [ ] User devices must get their addresses automatically.
       The DHCP server is running in both of the edge routers. One router has pool of IP addresses to 
2. [ ] The portal must be reachable by name.
3. [ ] The portal is portal.ywd.com.
