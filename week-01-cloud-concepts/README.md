# Week 1 — Cloud Concepts

## Lab: First Resources via Azure Portal

### What I built
- Resource Group: rg-week1-test (North Europe)
- Windows Server 2022 VM (D2s_v3 size — B-series unavailable on free trial)
- Default VNet, public IP, NSG with RDP (3389) allowed

### What I learned
- Resource Groups are the logical container for all related resources.
  Deleting an RG deletes everything inside — clean way to tear down a lab.
- North Europe = Dublin region. Default choice for Ireland workloads.
- Free trial accounts have zero quota on B-series. D-series works as fallback.
- "Stopped" still charges for compute. Must be "Stopped (deallocated)".
- Default NSG opens RDP to 0.0.0.0/0 — not safe for prod. Restrict to source IP.
- macOS connects to Windows VMs via the Windows App from the App Store.

### What I'd do differently
- Restrict NSG source IP to my home IP only.
- Use a consistent name prefix convention (vm-, rg-, snet-, nsg-).

### Cost
Approx €0.05 for the full lab (VM ran ~30 minutes).

### Time to complete
90 minutes including write-up.