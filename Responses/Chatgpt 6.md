
### Phase 1 — Requirements and discovery

1. Define business objectives and scope

- Purpose: Understand why the network is needed and what it must support.
    
- Output: Functional requirements, users, device categories, applications, geographic scope, budget, and project constraints.
    
- Basis: Cisco Campus LAN and Wireless LAN Solution Design Guide, Design section. Engineering practice, not a formal IEEE/IETF standard.
    
    Cisco
    

2. Assess the existing environment

- Purpose: Identify infrastructure, limitations, dependencies, and reusable resources.
    
- Output: Existing topology, device inventory, cabling assessment, available fiber, power, rack space, and current operational problems.
    
- Basis: Cisco Cloud Campus LAN Design Guide, Planning section; Arista's assessment/design methodology. Vendor-guided engineering practice.
    
    Cisco+1
    

3. Determine capacity and performance requirements

- Purpose: Ensure the network supports current demand and future growth.
    
- Output: Port counts, port speeds, traffic estimates, oversubscription targets, PoE budgets, latency/loss targets, growth projections, and availability objectives.
    
- Basis: Cisco Campus LAN and Wireless LAN Solution Design Guide, Planning for the Future and Oversubscription Ratios. Vendor guidance; no universal oversubscription ratio is mandated by IEEE.
    
    Cisco
    

4. Identify communication flows and security requirements

- Purpose: Determine which endpoints and services must communicate, and which communications must be restricted.
    
- Output: Traffic-flow matrix, trust zones, application dependencies, permitted/denied traffic, and access-control requirements.
    
- Basis: Arista Campus Network and Security Design Guide, Security Services; Cisco LAN Security Best Practices. Vendor design practice.
    
    Design Guide+1
    

### Phase 2 — Architecture and logical design

5. Select the overall network architecture

- Purpose: Choose a topology that satisfies scale, performance, resilience, and operational constraints.
    
- Output: High-level topology and functional placement of access, aggregation, core, or fabric nodes.
    
- Basis: Cisco's hierarchical two-tier/three-tier campus models; Arista's leaf-spine EVPN-VXLAN campus model. Both are vendor-recommended architectures, not IEEE requirements.
    
    Cisco+1
    

6. Design the physical network

- Purpose: Determine how equipment will be physically connected.
    
- Output: Switch locations, cable paths, copper/fiber media, uplink speeds, redundancy paths, optics, port allocation, and hardware specifications.
    
- Basis: Cisco CVD access/distribution/core platform guidance; IEEE 802.3 for Ethernet physical/link specifications. Hardware selection is a design decision, not prescribed by IEEE.
    

7. Design Layer 2 connectivity and fault domains

- Purpose: Control broadcast domains, prevent forwarding loops, and limit Layer 2 failures.
    
- Output: VLAN plan, trunk/access links, VLAN scope, L2/L3 boundaries, STP/MSTP approach where applicable, and link aggregation.
    
- Basis: IEEE 802.1Q-2022 (bridging/VLANs), IEEE 802.1AX-2020 (link aggregation); Cisco CVD guidance on minimizing Layer 2 fault domains.
    

8. Develop the IP addressing plan

- Purpose: Allocate scalable, non-overlapping address space.
    
- Output: IPv4/IPv6 addressing scheme, subnets, gateway addresses, infrastructure loopbacks, point-to-point addresses, and summarization boundaries.
    
- Basis: RFC 1918 (private IPv4 space), RFC 4632 (CIDR/aggregation), RFC 4291 (IPv6 addressing). Exact subnet sizes and allocation strategies remain engineering choices.
    

9. Design Layer 3 routing and gateway placement

- Purpose: Provide predictable connectivity between subnets and toward external networks.
    
- Output: Routing protocol selection, routing adjacencies, gateway locations, default-route strategy, ECMP, route summarization, and route redistribution policies.
    
- Basis: RFC 2328 (OSPFv2), RFC 4271 (BGP), RFC 9568 (VRRPv3); Cisco routed-campus guidance and Arista routed-underlay design. Protocol selection is not mandated by these standards.
    

10. Design segmentation and inter-segment policy

- Purpose: Separate traffic by security, operational, or tenancy requirements.
    
- Output: VLAN/VRF assignments, segmentation boundaries, inter-VRF communication design, firewall/ACL enforcement points, and optional overlay architecture.
    
- Basis: IEEE 802.1Q (VLANs), RFC 8365 (EVPN overlays), RFC 7348 (VXLAN, Informational RFC); Arista campus segmentation guidance. VRF/overlay selection and policy placement are design choices.
    

11. Design security controls

- Purpose: Prevent unauthorized access and reduce lateral movement and network attacks.
    
- Output: Network access authentication, authorization policies, management access restrictions, port security, DHCP/ARP protections, and security monitoring requirements.
    
- Basis: IEEE 802.1X (port-based network access control); Cisco LAN Security Best Practices; Arista Campus Network and Security Design Guide. Cisco ISE, Arista AGNI, TrustSec, and MSS are vendor solutions, not mandatory standards.
    

12. Engineer redundancy and failure recovery

- Purpose: Meet availability objectives and prevent unacceptable single points of failure.
    
- Output: Redundant links and equipment, first-hop redundancy, active/active or active/standby choices, expected convergence behavior, and failure-domain boundaries.
    
- Basis: Cisco CVD LAN High Availability; Arista leaf-spine/ECMP guidance; IEEE 802.1AX and RFC 9568. Cisco StackWise/HSRP and Arista MLAG are vendor-specific implementations or features.
    

13. Design infrastructure and access services

- Purpose: Ensure hosts can join the network and reach required services.
    
- Output: DHCP/DNS integration, DHCP relay, time synchronization, multicast requirements, wireless access where needed, and WAN/Internet edge integration.
    
- Basis: RFC 2131 (DHCPv4); Cisco Campus LAN and Wireless LAN Solution Design Guide; Arista Campus Network and Security Design Guide.
    

14. Design Quality of Service

- Purpose: Protect delay-sensitive and business-critical traffic during congestion.
    
- Output: Traffic classes, marking/trust boundaries, queue allocation, scheduling, policing, and congestion-management policies.
    
- Basis: RFC 2475 (DiffServ architecture, Informational RFC); Cisco CVD Campus QoS recommendations. Queue counts and scheduler choices are implementation-specific.
    

### Phase 3 — Detailed engineering and validation

15. Design management and operational visibility

- Purpose: Make the network observable, configurable, secure, and maintainable.
    
- Output: Management-plane design, monitoring/telemetry, logging, configuration backups, access controls, automation methods, and alert thresholds.
    
- Basis: IEEE 802.1AB (LLDP), RFC 6241 (NETCONF), RFC 8040 (RESTCONF); Cisco CVD operations guidance and Arista CloudVision practices. Management platform choice is a vendor/operational preference.
    

16. Produce and review the detailed design

- Purpose: Convert architectural decisions into an implementable specification.
    
- Output: Approved High-Level Design (HLD), Low-Level Design (LLD), addressing tables, VLAN/VRF matrices, interface maps, configuration templates, bill of materials, and documented tradeoffs.
    
- Basis: Cisco CVD design/deployment structure and Arista architecture/design deliverables. HLD/LLD documentation is an engineering practice, not an IEEE-defined procedure.
    

17. Validate the design before production

- Purpose: Verify requirements and identify design defects before deployment.
    
- Output: Test plan, lab or pilot results, connectivity/security test results, performance measurements, failover tests, and corrected configurations.
    
- Basis: Cisco Validated Design methodology and Arista predeployment validation practices. There is no single IEEE/IETF standard that prescribes this entire acceptance-test sequence.
    

### Phase 4 — Deployment and operations

18. Plan and execute the production deployment

- Purpose: Implement the design with controlled risk and minimal service disruption.
    
- Output: Migration plan, staging configurations, maintenance windows, change approvals, deployment records, checkpoints, and rollback procedures.
    
- Basis: Cisco CVD deployment guidance and Arista controlled deployment methodology. Operational practice, not a protocol standard.
    

19. Perform acceptance testing and handover

- Purpose: Confirm that the installed network satisfies the agreed requirements.
    
- Output: As-built diagrams, final configurations, test evidence, documented exceptions, operational runbooks, and formal acceptance.
    
- Basis: Cisco CVD design/deploy/operate lifecycle and Arista deployment validation/knowledge-transfer guidance. Engineering governance practice.


20. Operate, monitor, and continuously improve

- Purpose: Maintain reliability, security, performance, and scalability throughout the network's life.
    
- Output: Monitoring baselines, incident response procedures, capacity reviews, configuration compliance, software lifecycle plans, backup verification, and periodic redesign recommendations.
    
- Basis: Cisco CVD Operate guidance; Arista CloudVision Portal for Day 2 Network Operations Design Guide; RFC 6241/8040 for applicable configuration-management interfaces.