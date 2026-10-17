# Azure Cloud Network Engineer Journey

IT Engineer → Cloud Network Engineer, documented as I go.

**Background:** TU716 (Networking Technologies), TU819 (Networking Applications & Services).
**Currently:** IT Support at Accenture Ireland.

## Certification roadmap

| Certification | Target | Status |
|---|---|---|
| AZ-900 Azure Fundamentals | Nov 2026 | In progress |
| AZ-104 Azure Administrator | Apr 2027 | Not started |
| AZ-700 Azure Network Engineer | Jul 2027 | Not started |
| HashiCorp Terraform Associate | After AZ-700 | Not started |

## Lab index

| Week | Topic | What I built | Key skills |
|---|---|---|---|
| [01](./week-01-cloud-concepts/README.md) | Cloud concepts | Windows Server VM, default VNet, public IP, NSG | Resource groups, regions, VM deallocation, NSG basics |
| [02](./week-02-azure-architecture/README.md) | Azure architecture | VNet `10.0.0.0/16` with web + app subnets, NSG rules, Ubuntu VM running nginx | Subnetting, NSG priority and statefulness, SSH lockdown to /32 |
| [03](./week-03-management-governance/README.md) | Management & governance | Hub-and-spoke cost estimate, tag-enforcement policy, custom RBAC role | Pricing Calculator, Azure Policy, RBAC scoping |
| [04](./week-04-security-identity/README.md) | Security & identity | Defender for Cloud (Foundational CSPM) | Security posture, Secure Score |

## Daily learning log

[learning-log.md](./learning-log.md): what I study each day, in my own words.

## Tech stack

Azure | Terraform | Python | PowerShell | Git

## Lab rules

- Every lab gets a README: what I built, what I learned, what I'd do differently, cost.
- Delete the resource group after every lab.
