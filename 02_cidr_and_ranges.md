# 🧮 Chapter 02 – CIDR and Ranges

## 🎯 Goal

Learn how CIDR notation works, how to calculate subnet sizes, and how to quickly find network, broadcast, and usable ranges using simple formulas.

## 🧠 1. What Is CIDR?

**CIDR (Classless Inter-Domain Routing)** is a compact way to represent a subnet mask.

Example:
`255.255.255.0` → `/24`
`255.255.255.192` → `/26`

It tells you **how many bits** of the 32-bit IP address are used for the **network part**.

## 🧮 2. Understanding the Relationship

Each IPv4 address has **32 bits**.
If `/N` bits are for the network, then **(32 – N)** bits are for hosts.

### Formula

```
Total IPs  = 2^(32 - N)
Usable IPs = Total - 2   (network + broadcast)
```

Example:
`/24` → 2⁸ = 256 total → 254 usable.

## 📐 3. CIDR to Subnet Mask Table

| CIDR | Subnet Mask     | Block Size | Usable Hosts | Step in 4th Octet |
| ---- | --------------- | ---------- | ------------ | ----------------- |
| /24  | 255.255.255.0   | 256        | 254          | 256               |
| /25  | 255.255.255.128 | 128        | 126          | 128               |
| /26  | 255.255.255.192 | 64         | 62           | 64                |
| /27  | 255.255.255.224 | 32         | 30           | 32                |
| /28  | 255.255.255.240 | 16         | 14           | 16                |
| /29  | 255.255.255.248 | 8          | 6            | 8                 |
| /30  | 255.255.255.252 | 4          | 2            | 4                 |

💡 *Each time the CIDR increases by 1, the block size halves.*

## ⚙️ 4. The Divide–Multiply Trick (Fast Method)

This is the **shortcut** for finding a subnet instantly — no tables, no guessing.

### Formula

```
Block Size = 256 - (last_octet_of_mask)
Network = floor(IP_last_octet / BlockSize) × BlockSize
Broadcast = Network + BlockSize - 1
Usable Range = Network + 1 → Broadcast - 1
Usable Hosts = BlockSize - 2
```

or equivalently
`BlockSize = 2^(32 - CIDR)` (when subnetting inside a single octet)

## 💡 5. Example 1 — /26 Subnet

```
IP: 192.168.10.77/26
```

1️⃣ Mask → 255.255.255.192
2️⃣ BlockSize = 256 - 192 = 64
3️⃣ 77 ÷ 64 = 1 remainder 13
4️⃣ 1 × 64 = **64 → network**
5️⃣ Broadcast = 64 + 63 = **127**
6️⃣ Usable = **65 – 126**

✅ **Result**

| Type      | Address                        | Description  |
| --------- | ------------------------------ | ------------ |
| Network   | 192.168.10.64                  | Subnet start |
| Broadcast | 192.168.10.127                 | Subnet end   |
| Usable    | 192.168.10.65 – 192.168.10.126 | 62 hosts     |

## 💡 6. Example 2 — /27 Subnet

```
IP: 10.0.9.202/27
```

1️⃣ Mask → 255.255.255.224
2️⃣ BlockSize = 256 - 224 = 32
3️⃣ 202 ÷ 32 = 6 remainder 10
4️⃣ 6 × 32 = **192 → network**
5️⃣ Broadcast = 192 + 31 = **223**
6️⃣ Usable = **193 – 222**

✅ **Result**

| Type      | Address                 | Description    |
| --------- | ----------------------- | -------------- |
| Network   | 10.0.9.192              | Start of range |
| Broadcast | 10.0.9.223              | End of range   |
| Usable    | 10.0.9.193 – 10.0.9.222 | 30 hosts       |

## 💡 7. Example 3 — /28 Subnet

```
IP: 172.16.8.34/28
```

1️⃣ Mask → 255.255.255.240
2️⃣ BlockSize = 256 - 240 = 16
3️⃣ 34 ÷ 16 = 2 remainder 2
4️⃣ 2 × 16 = **32 → network**
5️⃣ Broadcast = 32 + 15 = **47**
6️⃣ Usable = **33 – 46**

✅ **Result**

| Type      | Address                   | Description  |
| --------- | ------------------------- | ------------ |
| Network   | 172.16.8.32               | Subnet start |
| Broadcast | 172.16.8.47               | Subnet end   |
| Usable    | 172.16.8.33 – 172.16.8.46 | 14 hosts     |

## 🧮 8. Edge Cases: /31 and /32

| CIDR | Total IPs | Usable Hosts | Typical Use                            |
| ---- | --------- | ------------ | -------------------------------------- |
| /31  | 2         | 0            | Point-to-point link (router ↔ router)  |
| /32  | 1         | 0            | Single host (loopback, specific route) |

Example:

```
10.0.0.4/31 → 10.0.0.4 and 10.0.0.5 (router link)
192.168.10.55/32 → single host identifier
```

## 🧠 9. Quick Reference & Rules

* `Block Size = 256 – last_octet_of_mask`
* `Network = floor(IP / BlockSize) × BlockSize`
* `Broadcast = Network + BlockSize – 1`
* `Usable = Network + 1 → Broadcast – 1`
* `Hosts = BlockSize – 2`
* CIDR ↑ 1 → Block Size ÷ 2
* Last subnet in a /24 always ends with `.255`
* `/31` = 2 IPs (point-to-point), `/32` = single device

## ✅ Summary

CIDR notation defines how many bits are used for the **network**.
The **block size** tells you how large each subnet is.
Using the **divide–multiply trick**, you can instantly find any network and broadcast address.

## 🧩 Practice

1️⃣ What is the network and broadcast for `192.168.2.146/28`?
2️⃣ How many usable hosts in `172.16.5.33/27`?
3️⃣ Which subnet does `10.0.8.73/29` belong to?
4️⃣ Why is `/31` used for router links?

---
