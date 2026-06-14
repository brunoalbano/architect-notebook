# IP Subnetting Cheat Sheet (CIDR math)

A reference for fast subnet/IP-range calculation. The whole subject is **powers of 2** — memorize the
tables, learn the "magic number" method, then drill until it's reflex. Verify edge cases against a real
calculator while learning.

## Core facts
- IPv4 = **32 bits** = 4 octets × 8 bits. `/n` = **n network bits**, **(32 − n) host bits**.
- **Addresses in block** = `2^(32 − n)`.
- **Usable hosts** = addresses − 2 (network + broadcast). Exceptions: **/31 = 2** (point-to-point, RFC 3021),
  **/32 = 1** (single host).
- **Mask in the interesting octet = 256 − block size.** Block size = `2^(bits remaining in that octet)`.

## Table A — within the last octet (most common)
| CIDR | Host bits | Block size | Mask (last octet) | Addresses | Usable |
|------|-----------|-----------|-------------------|-----------|--------|
| /24 | 8 | 256 | 0   | 256 | 254 |
| /25 | 7 | 128 | 128 | 128 | 126 |
| /26 | 6 | 64  | 192 | 64  | 62 |
| /27 | 5 | 32  | 224 | 32  | 30 |
| /28 | 4 | 16  | 240 | 16  | 14 |
| /29 | 3 | 8   | 248 | 8   | 6 |
| /30 | 2 | 4   | 252 | 4   | 2 |
| /31 | 1 | 2   | 254 | 2   | 2* |
| /32 | 0 | 1   | 255 | 1   | 1* |

\* point-to-point / single host.

## Table B — larger than /24 (block size in the 3rd octet)
| CIDR | Addresses | Mask | /24s contained |
|------|-----------|------|----------------|
| /23 | 512     | 255.255.254.0 | 2 |
| /22 | 1,024   | 255.255.252.0 | 4 |
| /21 | 2,048   | 255.255.248.0 | 8 |
| /20 | 4,096   | 255.255.240.0 | 16 |
| /19 | 8,192   | 255.255.224.0 | 32 |
| /18 | 16,384  | 255.255.192.0 | 64 |
| /17 | 32,768  | 255.255.128.0 | 128 |
| /16 | 65,536  | 255.255.0.0   | 256 |

Mask octet values, memorize this sequence: **128, 192, 224, 240, 248, 252, 254, 255** (each adds one bit).

## The "magic number" method — network & broadcast of any IP
Example: `192.168.10.77 /26`
1. Interesting octet = the one where mask ≠ 0 and ≠ 255 → 4th octet here.
2. Block size = 256 − mask(192) = **64**.
3. Network = largest multiple of block size ≤ the octet value: 64 ≤ 77 → **.64** → `192.168.10.64`.
4. Broadcast = next multiple − 1 = 128 − 1 → **.127** → `192.168.10.127`.
5. Usable host range = `.65` – `.126`.

Same method for /23 etc., but the interesting octet is the 3rd: e.g. `10.5.10.0/23` → block size in 3rd
octet = 256 − 254 = 2 → networks at 10.5.**0**.0, 10.5.**2**.0, 10.5.**4**.0 … so `10.5.10.0/23` covers
`10.5.10.0`–`10.5.11.255`.

## Subnetting a block (splitting)
To split `10.0.0.0/24` into 4 subnets → need 4 = 2² → borrow 2 bits → **/26**. Block size 64:
`10.0.0.0/26`, `10.0.0.64/26`, `10.0.0.128/26`, `10.0.0.192/26`.
Rule: to get **N subnets**, borrow `ceil(log2 N)` bits. To fit **H hosts**, need host bits `≥ ceil(log2(H+2))`.

## ⚠️ Cloud reality (don't size from the textbook number)
- **Azure** reserves **5 IPs per subnet** (`.0` network, `.1` gateway, `.2`/`.3` Azure DNS/future, last =
  broadcast). So `/24` = **251** usable, not 254. Minimum subnet **/29**; can't use `/30` or smaller.
- **AWS** also reserves **5 per subnet** (first 4 + last).
- Always size subnets with headroom — resizing a live VNet/VPC subnet is painful.

## Windows tools to verify while drilling
- `Test-NetConnection <host> -Port 443` — reachability + which interface/route.
- `Resolve-DnsName <name>` — DNS records.
- `Get-NetIPAddress` / `ipconfig /all` — your address, mask (as prefix length), gateway.
- `Get-NetRoute` — routing table.
- Sanity-check answers against any online CIDR calculator until the method is reflex.

> Source: this is foundational CIDR/RFC-4632 math + Azure/AWS subnet-reservation docs. The cloud
> reservation counts are the bit most worth re-verifying against current provider docs.
