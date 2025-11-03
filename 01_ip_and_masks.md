# 🧩 Chapter 01 – IP and Masks

## 🎯 Goal

Understand what an IP address is, how IPv4 works, what a subnet mask does, and how devices decide whether they can communicate directly or need a router.


## 🧠 1. What Is an IP Address?

An **IP address (Internet Protocol address)** is a unique identifier assigned to every device in a network.
It tells other devices *where* to send data — like a digital home address.

Each device can have:

* **Local (private) IP** → used inside your local network (e.g. home Wi-Fi).
* **Global (public) IP** → used on the Internet to communicate with the outside world.

When your computer connects to the internet, the **router** uses a process called **NAT (Network Address Translation)** to convert your private IP to a public one.

## 🌍 2. IPv4 and IPv6

| Version | Bits     | Bytes    | Example      | Approx. # of Addresses |
| ------- | -------- | -------- | ------------ | ---------------------- |
| IPv4    | 32 bits  | 4 bytes  | 192.168.1.25 | ~4.3 billion           |
| IPv6    | 128 bits | 16 bytes | 2001:0db8::1 | practically unlimited  |

* Each **byte (octet)** = 8 bits → range **0–255**
* IPv4 written as `x.x.x.x` (e.g. `192.168.1.25`)
* IPv6 written in hexadecimal, separated by colons.

💡 The world hasn’t completely switched to IPv6 yet — both IPv4 and IPv6 run side by side (**dual-stack mode**).
IPv4 is still dominant, supported by **NAT** and **address reuse**.

## 🧱 3. Private vs Public IP Addresses

**Private IP ranges (used in LANs):**

* `10.0.0.0` → `10.255.255.255`
* `172.16.0.0` → `172.31.255.255`
* `192.168.0.0` → `192.168.255.255`

Everything else is **public** and used on the Internet.

## 📐 4. Subnet Mask Basics

A **subnet mask** defines which part of the IP address identifies the **network**, and which part identifies the **host** (the specific device).

Example:

```
IP Address:   192.168.1.25
Subnet Mask:  255.255.255.0
CIDR:         /24
```

* `/24` means the first 24 bits belong to the **network**.
* The last 8 bits (1 byte) belong to **hosts** inside that network.

🧮 Formula:

```
Usable hosts = 2^(32 - prefix) - 2
```

(We subtract 2 for the **network** and **broadcast** addresses.)

Example:
`/24` → `2^(32-24) - 2 = 254` usable IPs
(from `.1` to `.254`)

## 🌐 5. Network, Host, and Broadcast

In every subnet:

* **Network address** → first address (all host bits = 0)
* **Broadcast address** → last address (all host bits = 1)
* **Usable host range** → everything in between

Example for `192.168.1.0/24`:

| Type       | Address                     | Description                             |
| ---------- | --------------------------- | --------------------------------------- |
| Network    | 192.168.1.0                 | identifies the subnet itself            |
| Host Range | 192.168.1.1 – 192.168.1.254 | usable for devices                      |
| Broadcast  | 192.168.1.255               | sends data to all devices on the subnet |

## 🧭 6. Communication Between Devices

Two devices can communicate **directly** only if:

1. They are in the **same subnet**, meaning their **network part** (determined by mask) is the same.
2. Their IPs are **unique** within that subnet.

Otherwise, they need a **router** to transfer data between networks.

Example:

```
PC1: 192.168.1.10/24
PC2: 192.168.2.10/24
```

* PC1 is in network **192.168.1.0/24**
* PC2 is in network **192.168.2.0/24**
  ➡️ They **cannot** talk directly, because the network parts differ.
  They need a **router** with interfaces in both subnets to connect them.

## 🧩 7. Quick Reference Table

| CIDR | Subnet Mask     | Hosts | Typical Use           |
| ---- | --------------- | ----- | --------------------- |
| /24  | 255.255.255.0   | 254   | Standard LAN          |
| /25  | 255.255.255.128 | 126   | Medium subnet         |
| /30  | 255.255.255.252 | 2     | Router-to-Router link |

## 🧠 8. Key Takeaways

* IPv4 = 32 bits → 4 octets (0–255 each).
* Subnet mask splits **network** and **host** parts.
* `/24` = 255.255.255.0 = 254 usable IPs.
* First address → **network**, last → **broadcast**.
* Devices in **different networks** need a **router** to communicate.
* NAT allows many local devices to share one public IP.

## 🧩 Practice Questions

1. What’s the network, broadcast, and usable range of `192.168.10.25/24`?
2. How many usable hosts exist in `/30`?
3. Why can’t `192.168.1.10/24` and `192.168.2.10/24` communicate directly?
4. What does NAT do in a home network?
5. What’s the difference between a private and public IP?

