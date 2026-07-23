# Week 2 — Azure Architecture & Services

## Lab: VNet, Subnets, NSG, Linux VM with nginx

### What I built
- Resource Group: rg-week2-test (North Europe)
- VNet vnet-week2 (10.0.0.0/16)
  - Subnet snet-web (10.0.1.0/24) — public-facing, NSG attached
  - Subnet snet-app (10.0.2.0/24) — private, no workload deployed this week
- NSG nsg-web with rules:
  - allow-http-inbound (port 80, source Any) — priority 100
  - allow-ssh-my-ip (port 22, source my public IP /32) — priority 110
- VM vm-web-week2 (Ubuntu 24.04) in snet-web with public IP
- nginx installed via apt, serving default page on port 80

### What I learned
- VNet address space (/16) must be larger than any subnet inside it (/24).
- Azure auto-routes traffic between subnets in the same VNet without extra config.
- NSGs attach at subnet OR NIC level. Subnet-level applies to all resources in the subnet.
- NSG rules are stateful — allowing inbound HTTP automatically permits response traffic out.
- Default rules DENY all inbound from the Internet. You explicitly ALLOW what you need.
- Rule priority: lower number = higher precedence. First match wins.
- HTTP (port 80) should be open to Any — that's the purpose of a web server.
- SSH (port 22) should be restricted to a single trusted IP. Never leave it open to Any.
- Public IPs on residential ISPs change. NSG rules need to be updated when your IP shifts.
- SSH keys must have 400 permissions on macOS or SSH refuses to use them.

### Verification tests
- curl http://localhost from inside VM → returned nginx welcome HTML
- Browser to http://<public-ip> → returned nginx welcome page

### Cost
Approx €0.08 for the full lab (VM ran ~45 minutes).

### Time to complete
~50 minutes.

### Screenshots
See `./screenshots/` folder.