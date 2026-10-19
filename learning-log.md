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

## Open questions

- 
