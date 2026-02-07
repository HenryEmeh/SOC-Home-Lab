# 🛡️ SOC Home Lab: Cloud-Native Security Monitoring

## 🚀 Project Overview
A cloud-based Security Operations Center (SOC) built to simulate real-world attack and defense scenarios. This lab leverages **Google Cloud Platform (GCP)** to host a vulnerable "Victim" machine and a centralized SIEM (Wazuh) for threat detection, log aggregation, and incident response.

## 🟢 Project Status: Phase 1 (Infrastructure Provisioning)
**Current Focus:** Provisioning secure Cloud Infrastructure, configuring VPC networks, and establishing Identity-Aware Proxy (IAP) access.

---

## 🏗️ Architecture & Network Topology
This lab leverages a cloud-native approach to simulate a realistic enterprise environment.

```mermaid
graph TD
    subgraph GCP [Google Cloud Platform]
        subgraph VPC [soc-lab-vpc]
            subgraph Subnet [soc-admin-subnet 10.0.0.0/24]
                Firewall{Cloud Firewall}
                Victim[Ubuntu 24.04 Victim Server]
                
                Firewall -- Allow SSH (IAP Only) --> Victim
                Firewall -- Block All Else --> Victim
            end
        end
    end
    
    User[Admin Mac Terminal] -- IAP Tunnel (Port 22) --> Firewall
    Internet((Internet)) -- Updates (Outbound Only) --> Victim```






---
## 🛠️ Tech Stack & Configuration
| Component | Technology | Configuration | Status |
| :--- | :--- | :--- | :--- |
| **Infrastructure** | Google Cloud (GCP) | `e2-micro` (Free Tier) in `us-central1` | ✅ Active |
| **Network** | Custom VPC | Isolated Subnet (`10.0.0.0/24`) | ✅ Active |
| **Operating System** | Ubuntu 24.04 LTS | Server Minimal Install | ✅ Active |
| **Access Control** | Cloud Firewall + IAP | Least Privilege (Ingress Deny All) | ✅ Active |
| **SIEM / XDR** | Wazuh | Manager + Agent Deployment | ⏳ Pending |

---

## 📚 Learning Resources & References
* **Cloud Infrastructure:** [Google Cloud VPC Documentation](https://cloud.google.com/vpc/docs/vpc)
* **Defense Theory:** [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
* **Lab Guide:** Inspired by Open-Source SOC Analyst roadmaps.
* **Academic Context:** Integrating principles from **AltSchool Africa** (Cybersecurity track).
