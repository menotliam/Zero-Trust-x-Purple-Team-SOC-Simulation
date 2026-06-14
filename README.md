# ENTERPRISE ZERO-TRUST ARCHITECTURE & PURPLE TEAM SOC SIMULATION

## 1. DOCUMENT CONTROL
| Attribute | Detail |
| :--- | :--- |
| **Project Title** | Enterprise Zero-Trust Network & SOC Simulation |
| **Author** | Alex Ngo |
| **Deployment Platform** | NVIDIA DSX Air (10,000 Compute Hours, 60 vCPU Quota) |
| **Framework Alignment** | NIST SP 800-207 (Zero-Trust Architecture) |
| **Document Status** | Initialized (Phase 1) |

---

## 2. EXECUTIVE SUMMARY
This project focuses on the design, deployment, and rigorous evaluation of a complex, enterprise-grade internal network infrastructure integrated with a centralized security monitoring system. The core objective is to enforce modern security principles and assess architectural resilience through practical, real-world attack simulations.

### 2.1. Core Security Framework
The infrastructure is strictly engineered around the **Zero-Trust Architecture (ZTA) as defined by NIST SP 800-207**, adhering to the following core tenets:
*   **Continuous Verification:** All data flows and traffic must be authenticated and encrypted, regardless of network location.
*   **Least Privilege Access:** Granular access controls are enforced to strictly limit lateral movement capabilities.
*   **Comprehensive Monitoring:** Continuous log aggregation and deep packet analysis are implemented to assess the real-time security posture and quickly identify anomalies.

### 2.2. Out of Scope Constraints
*   To ensure the technical analysis remains strictly focused on routing architecture, network security algorithms, and legitimate data flows, **this project will strictly omit any evaluation or discussion of out-of-bounds read vulnerabilities or similar rabbit holes.**
*   All technical reports and write-ups will center exclusively on system logic, protocol auditing, and architectural privilege escalation vectors.

---

## 3. TECHNOLOGY STACK
*   **Infrastructure & Networking:** NVIDIA Cumulus VX (Spine-Leaf Topology), OSPF/BGP for dynamic routing, ERSPAN for network traffic mirroring.
*   **Operating Systems:** Ubuntu Server 22.04 LTS / 24.04 LTS.
*   **Endpoint Detection and Response (EDR):** Wazuh Agent & Wazuh Manager (Intrusion detection, file integrity monitoring, active response).
*   **Security Information and Event Management (SIEM):** ELK Stack (Elasticsearch, Logstash, Kibana) - Long-term log retention, data pipeline engineering, and custom alerting dashboards.
*   **Offensive Security Tooling:** Nmap, Gobuster, Wireshark (for packet-level validation), and Metasploit Framework (for internal network enumeration and lateral movement emulation).

---

## 4. IMPLEMENTATION ROADMAP

### Phase 1: Infrastructure Deployment & ZTNA Enforcement
*   Deploy a highly available Spine-Leaf network topology utilizing the NVIDIA Air simulation environment.
*   Implement strict Virtual Local Area Network (VLAN) segmentation, ensuring complete physical and logical isolation between In-Band (Data) and Out-of-Band (Management) networks.
*   Configure robust Access Control Lists (ACLs) across Cumulus switches to enforce Layer 3 and Layer 4 Zero-Trust policies.
*   Establish baseline routing protocols (OSPF/BGP) to ensure seamless yet tightly controlled inter-VLAN communication.

### Phase 2: Security Data Engineering & SOC Pipeline
*   Provision and optimize a high-resource Ubuntu node to host the centralized Wazuh Manager and ELK Stack database.
*   Deploy Wazuh Agents across all application servers and routing nodes to collect syslogs, authentication logs, and endpoint metrics.
*   Engineer a robust data pipeline: Route alerts and raw data from the Wazuh Manager directly into Elasticsearch for long-term indexing and correlation.
*   Develop customized Kibana dashboards to visualize real-time traffic anomalies, alert on Zero-Trust policy violations, and provide a single pane of glass for the simulated Security Operations Center.

### Phase 3: Purple Team Emulation & Defensive Tuning
*   Execute an "Assume Breach" scenario, initiating internal network reconnaissance from a compromised internal node.
*   Simulate advanced lateral movement attempts, testing the resilience of the established ZTA segmentation and ACL firewall rules.
*   Measure the Mean Time to Detect (MTTD) by cross-referencing offensive actions with the alerts generated within the Wazuh and Kibana dashboards.
*   Iteratively tune firewall rules, detection signatures, and routing policies based on the empirical data gathered during the attack simulations.

---

## 5. INITIAL NODE INVENTORY (PHASE 1)

| Node Designation | Role / Function | OS / Platform | Specifications / Notes |
| :--- | :--- | :--- | :--- |
| `spine-01`, `spine-02` | Core Routing Fabric | Cumulus VX | BGP/OSPF EVPN Configuration |
| `leaf-01`, `leaf-02` | Access / Distribution | Cumulus VX | VLAN Tagging, ACL Enforcement |
| `oob-mgmt-srv` | OOB Management | Ubuntu Server 22.04 | Strictly isolated access only |
| `soc-elk-wazuh` | Centralized SIEM / EDR | Ubuntu Server 24.04 | Requires highest RAM/vCPU allocation |
| `app-srv-01` | Internal Application | Ubuntu Server 22.04 | Wazuh Agent deployed, primary target node |
| `kali-adversary` | Offensive Emulation | Custom ISO / Ubuntu | Origin point for Phase 3 emulation |
