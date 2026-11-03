Performance:
1. [ ] Server room has at least 4 Gbps of capacity.
	- <mark style="background:#fff88f">We will connect at least 4 links from the access switch of the server room. Each link has maximum capacity of 1 Gbps. All the links will be aggregated as one virtual port channel (Etherchannel).</mark>
2. [ ] Redundant links should share the load.
	- <mark style="background:#fff88f">Using Etherchannel (LACP) and vPC, we will distribute the load between links. This is used between Buildings, Server Room and core switches.</mark>
3. [ ] Reduce or eliminate unnecessary traffic detours.
	1. <mark style="background:#fff88f">For this, unauthorized transmission of packets from different user groups are stopped at the closest switches using ACLS.</mark>

Security:
1. [ ] Guest and IoT can reach only the DHCP, DNS servers and the Internet.
	1. <mark style="background:#fff88f"><font color="#000000">We will use ACLs to drop packets from Guest and IoT devices based on the destination IP address. If the destination IP address is in the Student, Faculty, or Server VLAN/subnet, the access switch will drop the traffic. The only exceptions are DHCP (UDP 67) and DNS (UDP/TCP 53), which Guest and IoT devices are allowed to reach.</font></mark>
2. [ ] Students and Faculty can reach the web app on TCP 80 and 443, and Internet.
	1. <mark style="background:#fff88f">This is done by permitting transmission of packets using ACLs which checks if the source is either Students and Faculty and destination is the Server subnet with destination port as 80 or 443 and checking if the protocol is TCP.</mark>
3. [ ] Students and faculty cannot reach each others' VLANs.
	1. This is done by denying any inter-VLAN

Scalability:
1. [ ] Growth in student + faculty counts.
	1. We leave enough addresses while assigning ip's to incorporate new students and faculty. And also have extra ports on the access switches. We are also giving more than 5Mbps bandwidth per user so that we can easily fit in more users in future. further we also have unused ports in core so we can scale in future.
2. [ ] Route summarization - No more than one static route to each building.
	1. 

Reliability/ High availability:
1. [ ] Single point of failure should not cut any user group off from the Server Room or the Internet.
	1. 
2. [ ] Recovery from failures should be fast.
	1. 

Bandwidth:
1. [ ] Students - About 5 Mbps average per user.
2. [ ] Faculty and staff - About 5 Mbps average per user.
3. [ ] Server - Student portal must serve up to 200 concurrent users at about 10 Mbps each.

DHCP and DNS:
1. [ ] User devices must get their addresses automatically.
2. [ ] The portal must be reachable by name.
3. [ ] The portal is portal.ywd.com.
