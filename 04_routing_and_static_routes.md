# 📦 Chapter 04 – Routing & Static Routes

## 🎯 Goal
Understand how routers decide where to send packets, how static routes work, how default routes are used, and how the *longest prefix match* algorithm selects the best route.

This chapter includes a **programmer‑friendly bitwise AND visualization** to make routing intuitive.



# 🟥 1. What Does a Router Actually Do?

Routers forward packets between different **subnets**.

Every router follows this core logic:

1. Look at the **destination IP**  
2. Compare it with entries in the **routing table**  
3. Select the route with the **longest prefix match**  
4. Forward packet to the correct **interface** or **next-hop router**

Routers **never** use the source IP to forward packets.



# 🟧 2. Routing Table Basics

A routing table contains entries like:

```
Destination Network    Mask                Next Hop / Interface

192.168.1.0           255.255.255.0       eth0
10.0.0.0              255.255.255.0       eth1
172.16.0.0            255.255.255.252     eth2
0.0.0.0               0.0.0.0             172.16.0.2   <-- default route
```

Every entry has:

- **Network address**
- **Mask**
- **Next-hop** router *or* local interface



# 🟦 3. Programmer Visualization: Routing Is Bitwise AND

If you're a programmer, this will feel natural.

A route is valid for a destination IP if:

```
(dest_ip & route.mask) == (route.network & route.mask)
```

This is exactly how routers decide if a destination belongs to a network.

### Example:
Destination: `10.0.10.140`  
Route: `10.0.10.128/25`

```
dest & mask       → 10.0.10.128
network & mask    → 10.0.10.128
MATCH
```

The route is a valid candidate.

Routers collect **all matching candidates**, then choose the one with the **most mask bits**.



# 🟩 4. Longest Prefix Match (LPM)

After collecting valid routes, the router chooses the **most specific** one.

Example routing table:

```
10.0.0.0/8        via R1
10.0.0.0/16       via R2
10.0.10.0/24      via R3
10.0.10.128/25    via R4   <-- winner
```

For destination `10.0.10.140`, all these match, but:

- `/25` has **25 fixed bits**
- `/24` has **24 bits**
- `/16` has **16 bits**
- `/8` has **8 bits**

More bits = more specific → `/25` wins.



# 🟥 5. Static Routes

A static route is a **manual instruction** telling a router how to reach a network that is *NOT* directly connected.

### Format (IMPORTANT):

```
add <destination_network> <destination_mask> <next_hop_ip>
```

Examples:

```
add 10.0.0.0 255.255.255.0 172.16.0.2
add 192.168.50.0 255.255.255.128 10.0.0.1
add 172.20.0.0 255.255.0.0 192.168.1.254
```

### Rules:
- **Left side must ALWAYS be a network address**, NOT a host.
- Next-hop **must be reachable** (in the same subnet as the router's interface).
- Static routes are **one-directional** → both routers need routes for full communication.



# 🟧 6. Default Route (0.0.0.0/0)

A default route catches all packets that do not match any more specific route.

Format:
```
add 0.0.0.0 0.0.0.0 <next_hop>
```

Also written as:
```
0.0.0.0/0 → next hop
```

Used for:
- Sending unknown traffic to "the internet"
- Simplifying routing
- NetPractice puzzles

Default route = **last resort** route.



# 🟨 7. Default Gateway vs Default Route

### ✔ Default Gateway (PC)
The address the **PC** sends packets to when the destination is not in its subnet.

### ✔ Default Route (router)
A routing table entry that the **router** uses when no specific route matches.

PC → default gateway  
Router → default route

Same concept, different place.



# 🟪 8. Return Path Logic

Communication works **only if both directions are routable**.

Example:

```
PC1 → Router A → Router B → PC2
```

For full communication:

- Router A must know how to reach PC2’s subnet  
- Router B must know how to reach PC1’s subnet  
- OR Router B must have a default route pointing to A

If return route is missing → one-way ping.



# 🟫 9. Subnet Example (Bitwise Interpretation)

IP: `10.0.37.129/26`

Block size:
```
2^(32 - 26) = 64
```

Compute block:
```
129 / 64 = 2
2 * 64 = 128
```

Network: `10.0.37.128`  
Broadcast: `10.0.37.191`  
Usable: `10.0.37.129 – 10.0.37.190`  
Network bits: 26  
Host bits: 6 (**host bits can change**)

Binary visualization:

```
IP:   00001010.00000000.00100101.10|000001
Mask: 11111111.11111111.11111111.11|000000
                           network | host
```

Host bits = the part after the split.



# 🟩 10. Summary

- Routing uses **bitwise AND** to filter valid routes.
- Router selects route with **longest prefix match**.
- Static route format:

```
add <network> <mask> <next-hop>
```

- Next hop must be **in same subnet** as router interface.
- Default route (0.0.0.0/0) is used when no specific route matches.
- PCs use **default gateway**; routers use **default route**.
- Full communication requires **return path routes**.



# 🧩 Practice Questions

1. Write a static route for Router A to reach `10.0.20.0/24` through next-hop `172.16.5.2`.
2. Why must the next-hop be in the same subnet as the router interface?
3. Which route wins for destination `192.168.100.77`?
   - 192.168.0.0/12  
   - 192.168.0.0/16  
   - 192.168.100.0/24  
   - 192.168.100.64/26  
4. What happens if the next-hop is unreachable?
5. Describe the full bitwise AND logic routers use.


