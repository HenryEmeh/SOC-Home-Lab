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