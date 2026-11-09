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

### 2026-11-04 — VLSM VNet lab (Week 1 Sat lab, done late)
- Sized 3 subnets for 50 / 20 / 8 VMs as /26, /27, /28 and built them in a 10.0.0.0/16 VNet. Portal showed 59 / 27 / 11 available IPs, matching my calculations.
- Mistakes on the way: picked sizes too small (/28 for 20 VMs, /29 for 8) and started every subnet at 10.0.0.0. Fix: Azure usable must be ≥ VMs needed, and each subnet starts after the previous one ends.
- Gap of ~10 days before this lab — needed a full re-walkthrough. Lesson: keep the daily habit going.

### 2026-11-04 (cont.) — VLSM across octets + Azure's 5 reserved IPs (Week 2, Mon–Tue)
- Carved 10.0.0.0/16 into a /24, two /26s and a /28: 10.0.0.0/24, 10.0.1.0/26, 10.0.1.64/26, 10.0.1.128/28. A /24 fills a whole third octet, so the next subnet rolls over to 10.0.1.0.
- Azure reserves 5 IPs in every subnet: the first 4 (network, default gateway, 2 × Azure DNS) and the last (broadcast). The first VM always gets range start + 4.
- The block size comes from the /N (256 − mask, or 2^(32 − N)) — the question never gives it directly.
- Mistakes: wrote the CIDR with the wrong /N, wrote a range as end–start, used the next subnet's first address as this one's last, started counting reserved IPs at .1 instead of .0. All fixed by the final attempt.

| Subnet | Reserved | First VM | Azure usable |
|---|---|---|---|
| 10.0.0.0/24 | .0–.3, .255 | .4 | 251 |
| 10.0.1.0/26 | .0–.3, .63 | .4 | 59 |
| 10.0.1.64/26 | .64–.67, .127 | .68 | 59 |
| 10.0.1.128/28 | .128–.131, .143 | .132 | 11 |

### 2026-11-04 (cont.) — DNS resolution and record types (Week 2, Wed)
1. DNS is the internet's phone book: it turns names into IPs.
2. When I type google.com, the resolver (router/ISP) checks its cache first. If it's not cached, the root server says "ask .com", the .com server says "ask Google's DNS server", and Google's authoritative server gives the IP. The resolver caches it and sends it back to my Mac.
3. The record I'll use most is the A record, because every website needs one to map its name to its server's IP — including every Repumax client site.
- Record types: A (name → IPv4), AAAA (name → IPv6), CNAME (alias), MX (mail server), TXT (verification), PTR (IP → name). Quiz: 5/5.

### 2026-11-04 (cont.) — Recursive vs authoritative, TTL, dig (Week 2, Thu)
1. A recursive resolver finds the records by asking other servers and caches them; an authoritative server owns the records for a domain.
2. TTL is the number of seconds a resolver may cache a DNS answer. It matters when moving a client's website, because the old IP stays cached until the TTL runs out.
- dig findings: TTL counted down live (294 → 210) as the cached answer aged. My router (192.168.1.1, port 53) answered every query — it's my recursive resolver. Gmail's primary MX is gmail-smtp-in.l.google.com (lower priority number = tried first). www.microsoft.com is a CNAME to an Akamai CDN host (edgekey.net). dig -x 8.8.8.8 → dns.google (PTR).
- Learned to read a dig answer line: name · TTL · class · type · (priority) · value.

### 2026-11-05 — 20 timed drills (Week 2, Fri)
- Subnet Sprint, including /16–/23: 16/20 fully correct, 18/20 under 60 seconds.
- All /24–/30 questions correct (one typo). Every real mistake was a 3rd-octet question:
  - /17: didn't apply block 128 in the 3rd octet (kept .220 instead of .128).
  - /19: used block 16 instead of 32 (picked .240 instead of .224).
  - /19 broadcast: swapped octets (.255.223 instead of .223.255).
- Rule to remember: for /17–/23, do the 4 steps in the 3rd octet, then the 4th octet is .0 for network and .255 for broadcast.

### 2026-11-05 (cont.) — Private DNS lab (Week 2, Sat)
- Built 2 VMs in snet-web /26 and snet-app /27 with a Private DNS zone (cne.internal) linked with auto-registration. vm-web pinged vm-app by IP and by name.
- Predicted both VM IPs (10.0.0.4 and 10.0.0.68) from the reserved-IP rule — both exactly right. (First predicted the network address; corrected to range start + 4.)
- Hit 3 real-world problems: no VM capacity in North Europe (moved to UK South), tag policy vs the DNS link form (used Azure CLI with --tags), and a VNet created in the wrong resource group (fixed with az resource move).

### 2026-11-09 — OSI and TCP/IP models (Week 3, Mon)
1. The OSI model is a map that splits networking into 7 layers, so you can say where something happens or where a problem is (the protocols — HTTP, TCP, IP — are the actual rules).
2. Layer 2 handles MAC addresses (the postman walking your street); layer 3 handles IP addresses (the street address, routed between towns by the post office); layer 4 handles ports (which flat in the building); layer 7 handles the HTTP request or DNS query (the question being asked).
3. "Transport" doesn't mean routing — it means which app on the device gets the data. Routing is layer 3, Network.
- TCP/IP model: Application (OSI 5–7), Transport (4), Internet (3), Network Access (1–2).
- Quiz: 3/6 first try (put MAC at 3, routing at 4, HTTP at 6), then 3/3 after the letter analogy.

### 2026-11-09 (cont.) — TCP vs UDP and key ports (Week 3, Tue)
1. TCP sets up a connection first with a 3-way handshake (SYN, SYN-ACK, ACK) and checks every piece arrives, so it's reliable but slower — used for web pages and SSH. UDP has no connection and just sends; it's faster, but lost pieces stay lost — used for video calls and DNS lookups.
2. Ports: 22 SSH/TCP (Linux terminal), 53 DNS/UDP (name lookups), 80 HTTP/TCP (web page), 443 HTTPS/TCP (secure web page), 3389 RDP/TCP (Windows remote desktop).
3. "Any" means anyone on the internet can reach the login and try passwords all day, which isn't safe. Fix: restrict the source to my IP, or use Azure Bastion / no public IP.
- Quiz: swapped 22 and 3389 at first; mixed up "protocol" (TCP/UDP) with a service name. Memory hook: SSH double S = 22 double 2; RDP = Remote Desktop = Windows.

### 2026-11-09 (cont.) — Routing (Week 3, Wed)
1. If the destination is outside its subnet, a device sends the packet to its default gateway. In Azure the default gateway is the subnet's first address + 1 (10.0.0.1 for 10.0.0.0/26, 10.0.0.65 for 10.0.0.64/27).
2. A routing table is a list of destination → next hop. 0.0.0.0/0 is the default route: it means "everything else" and is used when nothing more specific matches.
3. Longest prefix match: first check each route's range to see which ones match, then if more than one matches, the most specific route (biggest /N) wins.
- Azure creates system routes automatically (VNet range → stays in the VNet), which is why my two subnets could ping each other. UDRs override them later (AZ-700).
- Quiz: 4/6 first try — stopped at a /24 when a /25 also matched; thought 10.5.0.1 matched 10.0.0.0/16. Then 2/2 on the hard follow-ups.

## Open questions

- 
