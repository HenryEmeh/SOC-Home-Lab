# Project Daily Logs
This file tracks my daily progress, technical challenges, and solutions for the SOC Home Lab project.

## Log Entry Template
For consistency, please use this template for all future entries:
```
## MMM DD, YYYY: [Brief Title]
**Status:** ✅ Complete / 🚧 In Progress / ⏳ Pending

### Task
[What was accomplished or worked on]

### Technical Challenges
- Challenge 1: [Description]
  - Solution: [How it was resolved]

### Decisions Made
- Decision 1: [What was decided and why]

### Next Steps
1. [Next immediate action]
2. [Follow-up items]

### Time Spent
- Estimated: [X hours]
- Actual: [X hours]

### Notes
[Any additional context, findings, or important information]
```

---

## Feb 7, 2026: Environment Initialization
**Status:** ✅ Complete

### Task
- Initialized GitHub repository for SOC Home Lab project
- Finalized project architecture and directory structure
- Documented host hardware specifications

### Technical Challenges
- Hardware constraint: 8GB RAM limiting traditional multi-VM approach
  - Solution: Pivoted to cloud-native architecture using GCP to leverage cloud resources while maintaining local control

### Decisions Made
- **Cloud Platform Selection:** Chose GCP (Google Cloud Platform) over AWS/Azure because:
  - Free tier provides 300 credits for testing
  - Better integration with containerization (Cloud Run, GKE)
  - Optimized for learning SOC concepts without significant infrastructure costs
  - Excellent documentation for security-focused deployments

### Next Steps
1. Provision VPC network in Google Cloud
2. Configure firewall rules and security groups
3. Set up initial cloud infrastructure
4. Plan container deployments for SIEM and log aggregation

### Time Spent
- Estimated: 3 hours
- Actual: 2.5 hours

### Notes
- Hardware specs documented in `../assets/mac-specs.png`
- Architecture decision aligns with cost-efficiency goals
- Ready to begin infrastructure provisioning phase

---

## Feb 7, 2026: Cloud Infrastructure Provisioning & Network Security
**Status:** ✅ Complete

### Task
- Provisioned "Victim" Server (`victim-server`) using Ubuntu 24.04 LTS on Google Cloud Platform (GCP).
- Configured custom VPC (`soc-lab-vpc`) and Subnet (`10.0.0.0/24`) for network isolation.
- Established "Least Privilege" firewall rules to restrict SSH access.
- Verified connectivity between local MacOS terminal and Cloud VM.

### Technical Challenges
- **Challenge 1:** SSH Connection Failure (Permission denied: publickey).
  - **Solution:** Identified mismatch between local SSH keys and GCP metadata. Utilized GCP's browser-based SSH to bypass local key management initially, then authenticated local machine via `gcloud` CLI.
- **Challenge 2:** IAP Connection Error (Code 4003) when using browser SSH.
  - **Solution:** Discovered that GCP's Identity-Aware Proxy (IAP) requires a specific firewall allow-list. Created a new ingress rule allowing traffic from the IAP IP range (`35.235.240.0/20`) on TCP port 22.

### Decisions Made
- **Decision 1:** Selected `e2-micro` instance in `us-central1` region.
  - **Why:** To remain strictly within the GCP "Always Free" tier while ensuring sufficient compute power for a Linux-based victim machine.
- **Decision 2:** Implemented explicit Firewall Rules instead of "Allow All."
  - **Why:** To practice security-first principles (Defense in Depth) immediately, rather than relying on insecure defaults.

### Next Steps
1. Harden the victim server using UFW (Uncomplicated Firewall).
2. Install monitoring tools (`htop`) to baseline system performance.
3. Deploy Wazuh Agent for telemetry collection.

### Time Spent
- Estimated: 2 hours
- Actual: 3 hours (Due to IAP troubleshooting)

### Notes
- Critical learning: Cloud firewalls block internal management tools (like IAP) by default. Explicit rules are required for *any* traffic ingress.

---

## Feb 12, 2026: SIEM Deployment & Infrastructure Scaling
**Status:** ✅ Complete

### Task
- Deployed a single-node Wazuh SIEM stack (Indexer, Manager, Dashboard) using Docker Compose.
- Resized GCP infrastructure from `e2-micro` to `e2-medium`.
- Hardened SSH access by transitioning from Port 22 to Port 2222.
- Initialized Phase 3: Cowrie Honeypot installation and user isolation.

### Technical Challenges
- **Challenge 1: Resource Exhaustion (OOM Kills).**
  - **Description:** The `e2-micro` (1GB RAM) was insufficient for the Java-based Wazuh Indexer, causing the container to crash repeatedly.
  - **Solution:** Performed a vertical scale-up to `e2-medium` (4GB RAM) and verified stability using `docker stats`.
- **Challenge 2: Ingress Connectivity Failure (403/Connection Refused).**
  - **Description:** Successfully reached the indexer "Green" status, but the dashboard was unreachable via browser.
  - **Solution:** Identified two configuration errors:
    1. A typo in the firewall rule (Port 433 vs 443).
    2. A network mismatch (Rule was applied to the 'default' VPC instead of the 'soc-lab-vpc').
    Resolved by updating the rule to Port 443 on the correct VPC.

### Decisions Made
- **Decision 1: Static IP Reservation.**
  - **Why:** To maintain consistent access to the Wazuh Dashboard and SSH during frequent VM shutdowns (cost-saving measure).
- **Decision 2: SSH Port Hardening (22 → 2222).**
  - **Why:** To mitigate automated brute-force attempts on the standard SSH port and clear the way for the Cowrie Honeypot to occupy Port 22.

### Next Steps
1. Launch Cowrie Honeypot service and verify log generation.
2. Deploy Wazuh Agent on the honeypot to pipe logs into the Dashboard.
3. Configure custom decoders for honeypot telemetry.

### Time Spent
- Estimated: 4 hours
- Actual: 5.5 hours (Extensive troubleshooting of firewall and RAM constraints)

### Notes
- Critical Proof: Wazuh Indexer reached `Cluster health status: [GREEN]` at 16:20:00 WAT.
- Current Credit Status: Actively managing a $5 personal budget by scheduling VM shutdowns.
*Note: Successfully transitioned to local Git CLI workflow on Feb 12.*

## Feb 13, 2026: Reflection: Evaluated infrastructure costs and adjusted GCP budgets. Prepared local environment for VS Code integration. Focus today is on documentation integrity and local workflow optimization.
*Identity verified via local git config.*

---

## Feb 17, 2026: Hybrid Cloud Expansion & Azure Policy Troubleshooting
**Status:** 🚧 In Progress

### Task
- Initiated "Phase 2" of the SOC Lab: Expanding into a Hybrid Cloud architecture.
- Attempted to provision a Windows Server 2022 "Victim" machine on Microsoft Azure to simulate a cross-cloud attack surface (GCP Manager ↔ Azure Victim).
- Registered Azure Resource Providers (`Microsoft.Compute`, `Microsoft.Network`) to enable student subscription capabilities.

### Technical Challenges
- **Challenge 1: Regional Policy Restrictions (`RequestDisallowedByAzure`)**
  - **Description:** Deployment in `North Europe` failed due to strict "Student Subscription" policies that limit resources to specific geographic regions.
  - **Solution:** Analyzed the "Allowed Resource Deployment" policy assignment and identified `East US` and `West US 2` as permitted regions.
- **Challenge 2: Compute Quota Limits (Empty SKU List)**
  - **Description:** Even in allowed regions, the specific `Standard_B2s` (Burstable) VM size required for the budget was hidden or unavailable due to high demand on student tiers.
  - **Status:** Paused deployment to avoid provisioning expensive "Pay-As-You-Go" instances. Will retry during off-peak hours.

### Decisions Made
- **Decision 1: Hybrid Cloud Architecture**
  - **Why:** Instead of keeping everything in GCP, I chose to place the Windows victim in Azure. This forces the lab to handle real-world complexity: monitoring latency, public IP routing, and firewall rules between two different cloud providers.
- **Decision 2: Infrastructure Cost Controls**
  - **Why:** Pre-configured the "Auto-shutdown" policy (19:00 WAT) and selected `Standard SSD` storage to ensure the $100 Azure credit lasts for 12 months.

### Next Steps
1. Retry provisioning of Windows Server 2022 in `East US` region.
2. Configure Azure Network Security Group (NSG) to allow Wazuh Agent traffic (Port 1514/1515).
3. Connect the Azure Victim to the GCP Wazuh Manager via Public IP.

### Time Spent
- Estimated: 1 hour
- Actual: 3 hours (Due to policy errors and regional capacity troubleshooting)

### Notes
- **Critical Lesson:** Cloud capacity is not infinite for free/student tiers. Flexibility in region selection (switching from Europe to US) is a required skill for cost-effective cloud engineering.

![Azure Validation Error](./assets/azure-validation-error.jpg)
![Azure Policy Deny Log](./assets/azure-policy-deny-log.jpg)