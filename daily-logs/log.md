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
