
## Prompt: 
Act as a senior network design engineer who follows Cisco Validated Design (CVD) campus guidance and Arista design-guide practice (leaf-spine, segmentation), plus IEEE/IETF standards.

List the general, step-by-step process that professional designers follow to design a LAN from any set of requirements. Don't tailor it to a specific scenario.

Cover the full lifecycle, from requirements through validation and operations.

For each step, give the purpose, what gets decided or produced, and the Cisco or Arista guide or standard it comes from. Label anything that is vendor preference rather than a formal standard. If you're unsure of a source, say so instead of guessing.

Format: a numbered list with a short explanation per step.

Mapping security, QoS, wireless and management design topics to standards

Below is the general lifecycle I’d follow. The design method itself is industry practice, not a formal standard. The standards come in at specific technical decisions.

**Labels used**

- **[STD]**: formal standard (IEEE, IETF, TIA/ISO, ITU)
- **[CISCO]** / **[ARISTA]**: vendor guidance or preference, not a standard
- **[METHOD]**: industry methodology
- **[UNSURE]**: I’m not certain of the exact document title or status

I haven’t checked the current Cisco Design Zone or Arista documentation for this answer, so confirm guide titles and versions there before citing them formally.

---

1. **Gather business and technical requirements**
    - **Purpose:** Design from the top down: business goals first, technology last.
    - **Decides/produces:**
        - Requirements document: users and sites, applications, SLAs (availability, latency, convergence), security and compliance needs, growth horizon, budget, staffing and operations maturity.
        - Constraints, assumptions, and measurable success criteria.
    - **Sources:**
        - [METHOD] _Top-Down Network Design_ (Oppenheimer, Cisco Press).
        - [CISCO] PPDIOO lifecycle (Prepare/Plan). This is a legacy Cisco framework that is still widely taught; Cisco has since moved to other lifecycle branding.
2. **Assess the existing environment (baseline)**
    - **Purpose:** Know what you are replacing or integrating with.
    - **Decides/produces:**
        - Inventory of devices, versions, and end-of-life dates.
        - Current topology, traffic flows, utilization baselines, and pain points.
        - Cabling plant condition, power, cooling, and rack space.
    - **Sources:**
        - [STD] TIA-568 / ISO/IEC 11801 for cabling performance and certification.
        - [STD] TIA-606 for labeling and administration.
        - [CISCO] PPDIOO Plan-phase network assessment.
3. **Analyze traffic and size capacity**
    - **Purpose:** Turn requirements into numbers.
    - **Decides/produces:**
        - Endpoint counts per closet.
        - Flow direction (north-south vs. east-west).
        - Access port speeds and uplink sizing.
        - Oversubscription targets.
        - PoE power budgets.
        - Wireless density and AP uplink speeds.
    - **Sources:**
        - [STD] IEEE 802.3 Ethernet PHYs; 802.3bz (2.5/5 Gb/s multigig); 802.3af/at/bt (PoE).
        - [STD] IEEE 802.11ax/be.
        - [CISCO] Commonly cited campus oversubscription rules of thumb: about 20:1 access-to-distribution and 4:1 distribution-to-core. These are guidance, not requirements.
        - [ARISTA] Leaf-spine designs typically target lower ratios (for example 3:1 or better). This is vendor practice.
4. **Select the architecture and topology**
    - **Purpose:** Choose the structural model and the failure domains.
    - **Decides/produces:**
        - The structural model, chosen from:
            - three-tier (access/distribution/core)
            - two-tier collapsed core
            - campus leaf-spine
            - fabric overlay (EVPN-VXLAN, or SD-Access)
        - Physical hierarchy: MDF, IDF, building and site.
        - Interconnect points to the WAN, data center, internet, and cloud.
    - **Sources:**
        - [CISCO] _Campus LAN and Wireless LAN Solution Design Guide_ (CVD).
        - [CISCO] _Campus Network for High Availability Design Guide_.
        - [CISCO] _SD-Access Design Guide_ (CVD).
        - [ARISTA] Universal Cloud Network / leaf-spine design guides, and Arista Validated Designs (AVD) for L2LS, L3LS-EVPN and campus. [UNSURE] of the exact current title of Arista’s campus-specific design guide.
        - Hierarchical design is a design principle, not a standard.
5. **Define the Layer 2/Layer 3 boundary and redundancy model**
    - **Purpose:** Control the size of each failure domain and how fast the network converges.
    - **Decides/produces:**
        - Where routing starts: L2 access, routed access, or a fabric.
        - Spanning-tree mode and root placement.
        - Link aggregation.
        - Multichassis technology.
        - First-hop gateway redundancy.
    - **Sources:**
        - [STD] IEEE 802.1Q (VLANs, RSTP and MSTP are now folded into it).
        - [STD] IEEE 802.1AX (LACP).
        - [STD] RFC 5798 (VRRPv3).
        - [STD] RFC 7432 and RFC 8365 (EVPN and EVPN over VXLAN). RFC 7348 (VXLAN) is _Informational_, not Standards Track.
        - [CISCO] HSRP, StackWise Virtual/VSS and vPC are proprietary. Cisco campus CVDs favor routed access or multichassis designs that remove STP-blocked links.
        - [ARISTA] MLAG is proprietary. Arista favors L3 leaf-spine with EVPN anycast gateway.
6. **Plan addressing, VLANs and naming**
    - **Purpose:** Create a scalable, summarizable, operable scheme.
    - **Decides/produces:**
        - IPv4 and IPv6 allocation plan, with summarization per building or block.
        - VLAN and VNI numbering.
        - Hostname, interface description and DNS conventions.
        - DHCP and IPAM design.
    - **Sources:**
        - [STD] RFC 1918 (private IPv4).
        - [STD] RFC 4291 (IPv6 addressing).
        - RFC 7381 (enterprise IPv6 deployment). This is _Informational_.
        - [CISCO] _Deploying IPv6 in Campus Networks_ (CVD).
        - Naming conventions are [METHOD], with no standard.
7. **Design routing and the control plane**
    - **Purpose:** Achieve deterministic, fast, stable reachability.
    - **Decides/produces:**
        - IGP choice and area or level design.
        - BGP use for the underlay, overlay, or edge.
        - Summarization and filtering.
        - BFD and timer tuning.
        - Graceful restart / NSF.
    - **Sources:**
        - [STD] OSPFv2 (RFC 2328) and OSPFv3 (RFC 5340).
        - [STD] IS-IS (ISO/IEC 10589, RFC 1195).
        - [STD] BGP-4 (RFC 4271).
        - [STD] BFD (RFC 5880).
        - RFC 7938 (BGP in large-scale Clos fabrics). This is _Informational_.
        - [CISCO] EIGRP. It is published as RFC 7868, which is _Informational_, and is effectively Cisco-centric. Timer guidance comes from the Cisco HA campus guide.
        - [ARISTA] Arista generally prefers an eBGP underlay plus an EVPN overlay for L3LS. This is vendor practice.
8. **Design segmentation and security**
    - **Purpose:** Enforce least privilege and contain breaches.
    - **Decides/produces:**
        - Macro-segmentation with VRFs.
        - Micro or group-based segmentation.
        - Network access control (NAC): 802.1X, MAB fallback, and policy servers.
        - MACsec where needed.
        - First-hop security.
        - Control-plane protection and device hardening.
    - **Sources:**
        - [STD] IEEE 802.1X; RADIUS (RFC 2865, and RFC 3580 for 802.1X usage).
        - [STD] IEEE 802.1AE (MACsec).
        - [STD] RA Guard (RFC 6105).
        - [STD] LISP (RFC 9300/9301), used by SD-Access.
        - [CISCO] TrustSec/SGT and SD-Access policy are proprietary. VXLAN-GPO is, to my knowledge, an IETF draft rather than an RFC [UNSURE of its current status].
        - [CISCO] Cisco SAFE architecture.
        - [ARISTA] Multi-Domain Segmentation Service (MSS) and group-based segmentation are vendor features. [UNSURE] of current naming.
        - Supporting frameworks: NIST SP 800-207 (zero trust) and CIS benchmarks for hardening.
9. **Design QoS**
    - **Purpose:** Protect real-time and critical traffic consistently end to end.
    - **Decides/produces:**
        - Class model and DSCP marking.
        - Trust boundaries.
        - Queuing and policing per platform.
        - Wired-to-wireless mapping.
    - **Sources:**
        - [STD] DiffServ (RFC 2474/2475).
        - [STD] IEEE 802.1Q PCP.
        - RFC 4594 (service-class guidelines) and RFC 8325 (Wi-Fi mapping). Both are _Informational_.
        - [CISCO] 12-class QoS model from the Enterprise Medianet/QoS SRND and the Cisco Press _End-to-End QoS Network Design_.
        - [ARISTA] Typically simpler trust-DSCP models. This is vendor practice.
10. **Integrate wireless**
    - **Purpose:** Treat WLAN as part of the access layer rather than an add-on.
    - **Decides/produces:**
        - Controller model (centralized, distributed, or cloud).
        - SSID and policy mapping to VLANs or groups.
        - RF survey and AP placement.
        - Roaming domains.
        - AP uplink speed and PoE class.
    - **Sources:**
        - [STD] IEEE 802.11 (including 802.11r/k/v for roaming).
        - WPA3 is a _Wi-Fi Alliance_ certification, not an IEEE standard.
        - [CISCO] Catalyst 9800 deployment best-practice guides.
        - [ARISTA] CloudVision CUE / Arista WiFi documentation.
        - RF surveys are [METHOD].
11. **Engineer high availability and resiliency**
    - **Purpose:** Meet availability and convergence SLAs.
    - **Decides/produces:**
        - Failure analysis per tier.
        - Redundant links, supervisors and power.
        - Upgrade strategy (ISSU or hitless options).
        - UPS and power diversity.
        - Convergence targets in seconds or milliseconds.
    - **Sources:**
        - [CISCO] _Campus Network for High Availability Design Guide_; SSO/NSF/ISSU are vendor features.
        - [ARISTA] SSU and MLAG/EVPN upgrade procedures are vendor features.
        - [STD] Graceful restart (for example RFC 3623 for OSPF).
12. **Design management, monitoring and automation**
    - **Purpose:** Make the network observable and repeatable from day one.
    - **Decides/produces:**
        - Out-of-band management network.
        - AAA for device administration.
        - Time, logging and telemetry stack.
        - Source of truth and IPAM.
        - Configuration templating, CI pipeline, and backup and compliance checks.
    - **Sources:**
        - [STD] NTPv4 (RFC 5905), Syslog (RFC 5424), SNMPv3 (RFC 3411–3418), IPFIX (RFC 7011), NETCONF (RFC 6241), YANG (RFC 7950), RESTCONF (RFC 8040).
        - gNMI/OpenConfig is an industry or OpenConfig specification, not IETF.
        - [CISCO] Catalyst Center (formerly DNA Center).
        - [ARISTA] CloudVision and AVD (Ansible/Python design-as-code).
13. **Do the physical design and bill of materials**
    - **Purpose:** Make the logical design buildable.
    - **Decides/produces:**
        - Platform and model selection, with lifecycle and end-of-life checks.
        - Optics and media.
        - Cabling runs and distances.
        - Rack elevations; power and heat loads.
        - Licensing, spares and support contracts.
    - **Sources:**
        - [STD] IEEE 802.3 PMD specifications for optics and reach.
        - [STD] TIA-568 / ISO 11801 (cabling); TIA-569 (pathways and spaces).
        - Platform datasheets are vendor material.
14. **Document the high-level and low-level design (HLD/LLD)**
    - **Purpose:** Produce a single reviewable design of record.
    - **Decides/produces:**
        - HLD: architecture and rationale.
        - LLD: per-device configuration standards, IP/VLAN tables, port maps, diagrams.
        - Design decisions log, risks and assumptions.
        - Peer or architecture review sign-off.
    - **Sources:**
        - [METHOD]; [CISCO] PPDIOO Design-phase deliverables. There is no formal standard.
15. **Validate before deployment**
    - **Purpose:** Prove the design meets requirements before production.
    - **Decides/produces:**
        - Test plan and lab or proof-of-concept results, or a digital twin.
        - Failover and convergence tests.
        - Scale tests.
        - Security, NAC and QoS verification.
        - Go/no-go decision.
    - **Sources:**
        - [STD] RFC 2544 and RFC 2889 (benchmarking, _Informational_).
        - [STD] RFC 6349 (TCP throughput).
        - [STD] ITU-T Y.1564.
        - [CISCO] Cisco Modeling Labs. “Validated design” itself means Cisco lab-tested the CVD.
        - [ARISTA] Arista Cloud Test (ACT) and cEOS/vEOS labs.
16. **Plan migration and implementation**
    - **Purpose:** Deploy with minimal risk.
    - **Decides/produces:**
        - Phased cutover plan.
        - Methods of procedure (MOPs) with pre- and post-checks.
        - Rollback plans.
        - Change windows and communications.
        - Pilot site before broad rollout.
    - **Sources:**
        - [METHOD] ITIL change management.
        - [CISCO] PPDIOO Implement phase.
        - [ARISTA] CloudVision change control workflows.
17. **Accept, baseline and hand over**
    - **Purpose:** Confirm delivery against the success criteria from step 1.
    - **Decides/produces:**
        - Acceptance test results.
        - Performance baseline.
        - As-built documentation.
        - Runbooks and operations team training.
    - **Sources:** [METHOD]. There is no formal standard.
18. **Operate and optimize (closing the loop)**
    - **Purpose:** Keep the network healthy and aligned with changing needs.
    - **Decides/produces:**
        - Monitoring and alerting.
        - Capacity trending.
        - Patch and lifecycle management.
        - Configuration drift and compliance checks.
        - Incident and problem management.
        - Periodic design reviews that feed new requirements back into step 1.
    - **Sources:**
        - [CISCO] PPDIOO Operate/Optimize.
        - [METHOD] ITIL 4.
        - [ARISTA] CloudVision compliance and telemetry.

---

**Key takeaway:** The _process_ (steps 1–4 and 14–18) is methodology from Cisco’s lifecycle model and the top-down design discipline, with no IEEE or IETF standard behind it. Formal standards govern the _technical decisions_ in steps 5–13. Topology choice, multichassis, segmentation and design-as-code are where Cisco and Arista preferences diverge most.