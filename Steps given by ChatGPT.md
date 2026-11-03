---
project: SNM Lab Exam
source: SNM
---
Step 1. Define the scope and constraints. Write what the project includes and excludes. Write the budget, the schedule, and the staff available. List the devices and links that already exist. 

Step 2. Write the requirements as targets. Write each business goal as a sentence. Change each goal into a target with a number or a test. Use these groups: performance, security, scalability, availability, manageability. Example: change "fast" to "4 Gbps (gigabits per second) to the Server Room". 

Step 3. Find who talks to whom. Make a table. Put the user groups in the rows. Put the destinations in the columns. Mark each cell permit, deny, or not stated. Write each not stated cell as an assumption. Calculate the traffic load of each flow. 

Step 4. Choose the structure. Divide the network into access, distribution, and core tiers. Combine tiers if the network is small. Put shared services (servers, edge routers) in their own place. 

Step 5. Do the logical design. Make one VLAN (virtual local area network) for each user group. Give each VLAN one subnet. Give each building one contiguous block of addresses. Leave unused addresses for growth. Write the routing and the security rules. 

Step 6. Ask the failure question. Select one device or link. Ask: "If it fails, who loses what?" If the answer breaks a requirement, add a second device or link. Do this for every device and link on each path. 

Step 7. Find conflicts between requirements. Compare the requirements in pairs. Ask: "Does the tool for one requirement break the other?" Example: a redundant link makes a loop, and STP (Spanning Tree Protocol) can block that link. Write the decision and the reason for each conflict. 

Step 8. Do the physical design. Place the devices. Count the ports and select the link speeds. Make sure each device has a reason from a previous step. 

Step 9. Write the tests. Write at least one test for each requirement. Example: "From a Guest host, ping a Faculty host. The ping must fail." Use the tests to prove the design in the lab. 

Step 10. Write the documents and plan for growth. Write the diagram, the address plan, and the rules. Write all assumptions. Calculate when the design needs more hardware.




## Gemini

1. **Requirements Gathering & Constraint Analysis**
    
      
    - **Purpose:** Translate business objectives, application profiles, and facility constraints into technical targets (scale, availability, latency, budget, and compliance).
        
          
        
    - **Decided/Produced:** A Requirements Specification Document and Service Level Objectives (SLOs) detailing endpoint counts, bandwidth per user, high-availability tiers (e.g., 99.99% uptime), PoE budgets, and physical security mandates.
        
          
        
    - **Source:** Cisco Lifecycle Services (PPDIOO — Prepare/Plan phases) and the Cisco CCDE (Certified Design Expert) framework (_Vendor Preference/Methodology_).
        
          
        
2. **Traffic Flow, Application & Failure Domain Mapping**
    
      
    - **Purpose:** Determine whether traffic is predominantly North-South (client-to-WAN/core) or East-West (leaf-to-leaf/server-to-server) and define acceptable blast radii during hardware or link failures.
        
          
        
    - **Decided/Produced:** Traffic flow matrices, oversubscription ratios (commonly 20:1 access-to-distribution in campus, or 3:1 to 1:1 in leaf-spine), and failure domain boundaries.
        
          
        
    - **Source:** Cisco Campus LAN and Wireless LAN CVD (_Vendor Preference_); Arista Cloud Network Design Guide (_Vendor Preference_).
        
          
        
3. **Physical Layer & Cabling Architecture**
    
      
    - **Purpose:** Establish the physical media, optics, PoE delivery, and patch panel topology based on distance, throughput, and environmental constraints.
        
          
        
    - **Decided/Produced:** Cable plant specifications (Cat6A vs. OS2 single-mode vs. OM4 multi-mode fiber), transceiver matrices, rack/IDF layouts, and PoE power budgets.
        
          
        
    - **Source:** **TIA-568** and **ISO/IEC 11801** (structured cabling standards); **IEEE 802.3** series (802.3bt for Type 4 PoE, 802.3bs/cd for 50G/100G/200G/400G Ethernet).
        
          
        
4. **Topological & Underlay Architecture Selection**
    
      
    - **Purpose:** Choose the foundational switching hierarchy to support the required scale, redundancy, and convergence times.
        
          
        
    - **Decided/Produced:** The physical/logical topology diagram and Layer 2/Layer 3 boundary placement. This generally resolves into a 3-Tier Hierarchical model (Access-Distribution-Core), a Routed Access model, or a Clos Leaf-Spine architecture.
        
          
        
    - **Source:** Hierarchical 3-Tier and Routed Access come from the Cisco Campus LAN CVD (_Vendor Preference_). Clos Leaf-Spine comes from the Arista L3 Leaf-Spine Design Guide and Arista Universal Cloud Network (UCN) architecture (_Vendor Preference_, derived from **RFC 7938** for large-scale data centers).
        
          
        
5. **Layer 2 Switching & First-Hop Redundancy Design**
    
      
    - **Purpose:** Ensure loop-free Layer 2 forwarding, maximize link utilization from the access layer, and provide resilient default gateways for endpoints.
        
          
        
    - **Decided/Produced:** Link aggregation design, Spanning Tree root bridge placement and edge-port protections, multi-chassis link aggregation (if L2 extends to distribution/leaf), and FHRP selection.
        
          
        
    - **Source:** **IEEE 802.1Q** (VLAN tagging), **IEEE 802.1D/w/s** (STP/RSTP/MSTP), **IEEE 802.1AX / 802.3ad** (LACP), and **RFC 5798** (VRRPv3). HSRP (**RFC 2281**, informational) and StackWise Virtual / VSS are _Vendor Preferences_ (Cisco); MLAG is a _Vendor Preference_ (Arista).
        
          
        
6. **Layer 3 Routing & Underlay Control Plane Design**
    
      
    - **Purpose:** Provide fast-converging, deterministic ECMP (Equal-Cost Multi-Path) routing across the core or spine layer.
        
          
        
    - **Decided/Produced:** IPv4/IPv6 addressing and summarization schemes, IGP or BGP protocol selection, BFD (Bidirectional Forwarding Detection) timers, and ECMP hashing strategies.
        
          
        
    - **Source:** **RFC 2328** (OSPFv2), **RFC 5340** (OSPFv3), **ISO/IEC 10589** (IS-IS), **RFC 4271** (BGP-4), and **RFC 5880** (BFD). Using eBGP as the sole underlay/overlay protocol in leaf-spine topologies is a strong Arista UCN _Vendor Preference_ (backed by **RFC 7938**), whereas Cisco Campus CVDs traditionally favor IS-IS or OSPF as the fabric underlay (_Vendor Preference_).
        
          
        
7. **Network Segmentation & Overlay Fabric Design**
    
      
    - **Purpose:** Decouple logical network policy and tenant/group isolation from the physical underlay topology.
        
          
        
    - **Decided/Produced:** VRF (Virtual Routing and Forwarding) instances, VXLAN VNI (VXLAN Network Identifier) mappings, anycast gateway definitions, and micro-segmentation policy matrices.
        
          
        
    - **Source:** **RFC 7348** (VXLAN) and **RFC 7432** / **RFC 8365** (BGP EVPN control plane for VXLAN), heavily emphasized in Arista EVPN Deployment Guides (_Vendor Preference_ for standard EVPN). Cisco Software-Defined Access (SD-Access), which pairs a LISP control plane (**RFC 6830**) with VXLAN data plane and Scalable Group Tags (SGTs/TrustSec), is a proprietary _Vendor Preference_ documented in Cisco SD-Access CVDs.
        
          
        
8. **Quality of Service (QoS) & Traffic Management**
    
      
    - **Purpose:** Protect latency- and jitter-sensitive traffic (voice, real-time video, storage, control plane) during periods of link congestion.
        
          
        
    - **Decided/Produced:** Trust boundaries (port vs. DSCP marking), classification and marking rules, policing thresholds, and hardware queuing/scheduling allocations (e.g., strict priority vs. Weighted Round Robin / CBWFQ).
        
          
        
    - **Source:** **RFC 2474** (DiffServ field), **RFC 4594** (Configuration Guidelines for DiffServ Service Classes), and **IEEE 802.1p** (L2 CoS). The specific 4-class, 8-class, or 12-class queuing models are _Vendor Preferences_ detailed in the Cisco Enterprise QoS CVD.
        
          
        
9. **Identity, Access Control & Perimeter Security Design**
    
      
    - **Purpose:** Enforce zero-trust port admission, prevent L2 spoofing attacks at the access edge, and secure the network control and management planes.
        
          
        
    - **Decided/Produced:** 802.1X/MAB authentication workflows, RADIUS/TACACS+ integration, First-Hop Security profiles, infrastructure ACLs, and Control Plane Policing (CoPP) policies.
        
          
        
    - **Source:** **IEEE 802.1X** (Port-Based Network Access Control), **IEEE 802.1AE** (MACsec), **RFC 2865** (RADIUS), and **RFC 8907** (TACACS+). First-hop security features (DHCP Snooping, Dynamic ARP Inspection, IP Source Guard) and CoPP are industry-standard practices documented in Cisco Campus Security CVDs and **RFC 6192** (Protecting the Router Control Plane).
        
          
        
10. **Design Validation, Lab Staging & Pre-Deployment Testing**
    
      
    - **Purpose:** Verify that the Low-Level Design (LLD) and generated configurations meet functional, scale, and failover requirements before touching production.
        
          
        
    - **Decided/Produced:** A Test Plan, digital twin/virtual lab simulation (e.g., Containerlab, cEOS, CML), failure-injection test results (link pulls, supervisor failovers), and golden configuration templates.
        
          
        
    - **Source:** **RFC 2544** (Benchmarking Methodology for Network Interconnect Devices). Automated CI/CD network validation (using frameworks like Arista AVD — Architect, Validate, Deploy — with ANTA) is an Arista _Vendor Preference_; Cisco CVD validation methodology is a Cisco _Vendor Preference_.
        
          
        
11. **Telemetry, Day-2 Operations & Lifecycle Management**
    
      
    - **Purpose:** Maintain visibility, automate configuration compliance, and verify ongoing adherence to the original design SLOs.
        
          
        
    - **Decided/Produced:** Streaming telemetry pipelines, syslog/SNMP traps, out-of-band (OOB) management network architecture, configuration backup workflows, and OS upgrade procedures (ISSU/hitless upgrades).
        
          
        
    - **Source:** **RFC 6241** (NETCONF), **RFC 8040** (RESTCONF), **RFC 7950** (YANG data modeling), **RFC 7011** (IPFIX), **IEEE 802.1AB** (LLDP), and **gNMI/OpenConfig** (industry consortium specification, not a formal IETF RFC). Implementing these via Arista CloudVision or Cisco Catalyst Center are respective _Vendor Preferences_.