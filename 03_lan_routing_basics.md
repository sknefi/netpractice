# 🧩 Chapter 03 -- Same LAN & Routing Basics

## 🎯 Goal

Understand how devices decide whether they can communicate **directly**
(inside the same subnet) or must use a **router (gateway)**. This is the
foundation of routing logic and NetPractice.



# 🧠 1. What Is a Subnet?

A **subnet (subnetwork)** is a group of IP addresses defined by a
**network address + mask**.

Example: \
192.168.1.0/24 → subnet containing 192.168.1.1 -- 192.168.1.254
192.168.1.128/25 → subnet containing 192.168.1.129 -- 192.168.1.254

✔️ Devices in the same subnet → direct communication\
✔️ Devices in different subnets → require a router



# 🧮 2. How to Check if Two Devices Are in the Same Subnet

Devices (and routers) check using:

(IP AND Mask) == (IP AND Mask)

Example: \
192.168.1.10/24 → 192.168.1.0 192.168.1.200/24 → 192.168.1.0 →
Same subnet → direct communication

Another: \
192.168.1.10/24 → 192.168.1.0 192.168.2.10/24 → 192.168.2.0 →
Different subnet → must use router



# 🌐 3. Direct vs Indirect Communication

## ✔️ Direct Communication

Happens when: - Both devices are in the **same subnet** - PC finds the
MAC via ARP - Traffic flows through the switch

Example: \
PC1: 192.168.1.10/24\
PC2: 192.168.1.20/24\
→ Direct



## ✔️ Indirect Communication

Happens when: - Devices are in **different subnets** - Traffic must go
to a **router**

Example: \
PC1: 192.168.1.10/24\
PC2: 10.0.0.10/24\
→ Indirect (via default gateway)

Packet path: \
PC1 → Default Gateway (PC1) → Router → Destination subnet → PC2\
PC2 → Default Gateway (PC2) → Router → PC1



# 🧭 4. Router Interfaces (Gateways)

A **router** connects two or more subnets.

Each router interface has: - Its own IP address - Its own subnet

Example: \
eth0 → 192.168.1.1/24\
eth1 → 10.0.0.1/24

Each interface = **gateway** for its subnet.



# 🚪 5. Default Gateway (VERY IMPORTANT)

Every host must have a **default gateway**.

Definition:\
The default gateway is the router interface IP *inside the host's
subnet*, used for all destinations outside that subnet.

If destination is not local → send to default gateway.



# 🔄 6. How Return Packets Work

Packets do **not** remember the path.

When PC1 sends a packet to PC2: - PC2 sees PC1's **source IP** - PC2
checks: "Is source in my subnet?" - If not → PC2 sends reply to **its
default gateway** - Router forwards back to PC1

✔️ Both PCs must have correct default gateways for two-way
communication.



# 🚫 7. Overlapping Subnets (Bad!)

Example: \
192.168.1.0/24\
192.168.1.0/26

They overlap → routing breaks.

Reason: - Some devices think traffic is local - Some think it requires a
router

Always use **non-overlapping** subnets.



# 🧱 8. Router Forwarding Logic

Routers have a **routing table** containing: - Directly connected
networks - Static routes - Dynamic routes (RIP, OSPF...)

Example router: \
192.168.1.1/24\
10.0.0.1/24

Router automatically knows: 192.168.1.0/24 → via interface 192.168.1.1\
10.0.0.0/24 → via interface 10.0.0.1

This is why two networks connected to the same router can communicate
immediately.



# 🔍 9. Packet Flow Example

PC1: 192.168.1.10/24, GW = 192.168.1.1\
Router: 192.168.1.1/24 --- 10.0.0.1/24\
PC2: 10.0.0.10/24, GW = 10.0.0.1

Forward path: PC1 → 192.168.1.1 → 10.0.0.1 → PC2

Return path: PC2 → 10.0.0.1 → 192.168.1.1 → PC1



# 🧩 10. Key Takeaways

-   Same subnet → direct communication\
-   Different subnet → router required\
-   Router interface = gateway\
-   PC's default gateway = router's IP inside its subnet\
-   No default gateway = cannot leave subnet\
-   Overlapping subnets break routing\
-   Router automatically routes between its interfaces\
-   Both sides must have correct default gateways



# 📝 11. Practice Questions

1.  Can `192.168.1.10/24` talk directly to `192.168.2.10/24`?\
2.  PC = `192.168.1.70/26`, GW = `192.168.1.1`. Will routing work?\
3.  Describe packet path from `192.168.1.20` to `10.0.0.10`.\
4.  For `/25`, which IPs belong together?\
5.  Can a host with no default gateway reach the Internet?\
6.  Define: gateway vs default gateway.
