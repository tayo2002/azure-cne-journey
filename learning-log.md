# Learning Log — Cloud Network Engineer Path

**Goal:** Move from IT Support to Cloud Network Engineer (Azure).

## Roadmap

| Cert | Status | Target | Funded by work? |
|------|--------|--------|-----------------|
| AZ-900 Azure Fundamentals | Waiting for work course | Nov 2026 | Yes (course + voucher) |
| AZ-104 Azure Administrator | Not started | TBC | Asked manager |
| AZ-700 Azure Network Engineer | Not started | TBC | Asked manager |

## Key dates

- AZ-900 course start: _TBC (asked manager 2026-10-15)_
- AZ-900 voucher expiry: _TBC_

## Log

Format: date — what I studied/built — time spent — what's still unclear

### 2026-10-15
- Emailed manager re: AZ-900 course date, voucher expiry, and funding for AZ-104/AZ-700.
- Created this learning log.

### 2026-10-16 — IPv4 addressing (Professor Messer N10-009 1.7)
1. An IPv4 address is 32 bits, split into 4 octets of 8 bits each.
2. An octet stops at 255 because there is no 9th bit: all 8 bits on (11111111) = 255.
3. The address has a network part and a host part. With a /24, the first 24 bits are the network and the last 8 are the host; the subnet mask decides where the split is.
- Still unclear: how the split changes with other masks (/16, /26) — Week 1.

### 2026-10-17 — Repo and Azure cleanup
- Repo: README rewritten with roadmap and Weeks 1–4 lab index; removed broken nested repo link and .DS_Store files.
- Azure: deleted rg-rbac-test and an empty ResourceMoverRG. No resources left running. Kept cloud-shell-storage-northeurope for Cloud Shell.
- Spend Jul–Sep 2026: €0.91 total, almost all from the Week 2 nginx VM.
- Created budget-labs: €10/month, email alert at 80%.

### 2026-10-19 — Binary ↔ decimal (Week 1, Mon)
1. To convert binary to decimal, line the bits up under the values 128, 64, 32, 16, 8, 4, 2, 1 and add up the values that sit over a 1.
2. To convert decimal to binary, go left to right from 128: if the value fits into what's left, write 1 and subtract it; if not, write 0.
3. An IPv4 address is 4 octets of 8 bits (32 bits in total); we show each octet in decimal because it's easier for people to read. The subnet mask, not the octets, decides which part is network and which is host.
- Practice: 8/8 conversions correct, including 10.0.0.4 → 00001010.00000000.00000000.00000100.

### 2026-10-20 — Private, public, loopback, APIPA (Week 1, Tue)
1. A public IP is unique across the whole internet; a private IP only needs to be unique inside its own network, so the same private ranges are reused everywhere. NAT swaps the private IP for the router's public IP on the way out.
2. The 3 private ranges are 10.0.0.0/8, 172.16.0.0/12 and 192.168.0.0/16. Traps: only 172.16–172.31 is private, and only 192.168 (not 192.169).
3. Loopback (127.0.0.1) means "this machine" — if you can ping it, the device's network software works. APIPA (169.254.x.x) is the address a device gives itself when DHCP doesn't reply, so it points to a DHCP or connection problem.
- Quiz: 8/9 — missed 192.169.1.1 (public).
- Checked my Mac: private IP is in 192.168.x.x; public IP is different because of NAT.

### 2026-10-21 — Subnet masks and CIDR (Week 1, Wed)
1. A subnet mask is 32 bits like an IP address: 1s are network bits and 0s are host bits. /N means the first N bits are 1s.
2. Host bits = 32 − N. Addresses = 2^(host bits). Usable = addresses − 2 (network + broadcast). Azure usable = addresses − 5, because Azure reserves 5 per subnet.
3. A VNet is the container and its subnets must fit inside it, so the VNet needs a bigger range — e.g. my Week 2 VNet 10.0.0.0/16 holds two /24 subnets with room for 254 more.

| CIDR | Mask | Host bits | Addresses | Usable | Azure usable |
|---|---|---|---|---|---|
| /16 | 255.255.0.0 | 16 | 65,536 | 65,534 | 65,531 |
| /24 | 255.255.255.0 | 8 | 256 | 254 | 251 |
| /26 | 255.255.255.192 | 6 | 64 | 62 | 59 |
| /28 | 255.255.255.240 | 4 | 16 | 14 | 11 |

- Table: all numbers correct. Lesson: write every octet as exactly 8 bits.

### 2026-10-22 — Block size method (Week 1, Thu)
1. Block size method: /N means N ones from the left, so convert the interesting octet to decimal to get the mask number. Block size = 256 − that number. List the blocks (0, block, 2×block…) and find where the IP falls: network = start of the block, broadcast = next block − 1, usable hosts = block − 2.
2. A class C has 256 addresses, and the block size is how big each piece is, so number of subnets = 256 ÷ block size (e.g. /27 → 8 subnets, /29 → 32).
3. A /30 has block 4, so 4 addresses − network − broadcast = 2 usable — exactly one for each router on a point-to-point link, with none wasted.
- Drills on subnettingpractice.com: completed all, with some help. Also learned: each router interface is its own subnet; switches don't create subnets.
- Hardest part: random question types. Tomorrow: 20 drills alone.

### 2026-10-24 — 20 drills, /24 to /30 (Week 1, Fri — done Sat)
1. For any /N from /24 to /30: take N − 24 bits and add those bit values from 128 down to get the last mask octet; block size = 256 − mask; usable hosts = block − 2; subnets in a class C = 256 ÷ block; binary = N − 24 ones followed by zeros (8 bits).
- 20 drills on Basic subnetting practice, all done with no issues.
- Saturday lab (rebuild Week 2 VNet with 3 self-sized subnets) moved to next Saturday and merged with the Week 2 DNS lab.
- Next: finding network and broadcast of a specific IP (Week 2).

## Open questions

- 
