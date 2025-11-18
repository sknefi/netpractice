# 42 NetPractice Notes

Personal cheat sheet built while working through the NetPractice project.\
Every chapter is its own `.md` file so it is easy to jump straight to the
topic you need while debugging a map or answering theory questions.

| Chapter | File | Focus |
| --- | --- | --- |
| 01 | `01_ip_and_masks.md` | IPv4 anatomy, private/public ranges, subnet masks and binary intuition. |
| 02 | `02_cidr_and_ranges.md` | CIDR notation, block-size math, quick formulas for network/broadcast/usable ranges. |
| 03 | `03_lan_routing_basics.md` | Same-LAN checks, gateways, default routes, overlapping subnets, packet paths. |
| 04 | `04_routing_and_static_routes.md` | Router logic, longest-prefix match, static/default routes, programmer-style AND demo. |

---

## How to Use These Notes

1. **Skim Chapter 01** whenever you need a refresher on masks, binary, or why NAT
   exists. It ends with quick practice questions you can run mentally.
2. **Keep Chapter 02 open** while solving the level blocks. It has the block-size
   table and the divide–multiply shortcut so you can do `/26`, `/28`, etc. in
   seconds without external calculators.
3. **Refer to Chapter 03** when a machine cannot reach another one. It walks
   through ARP vs routing paths, default gateways, and common mistakes such as
   overlapping subnets or missing gateways.
4. **Use Chapter 04** for every static-route puzzle. It explains the exact `add
   <network> <mask> <next-hop>` syntax NetPractice expects and includes the
   longest-prefix-match mindset (bitwise AND comparison) so you can reason about
   router tables like a programmer.

---

## Quick Reference

- **Usable hosts formula**: `2^(32 - prefix) - 2`
- **Block size (single-octet subnets)**: `256 - mask_last_octet`
- **Network discovery**: `floor(last_octet / block) * block`
- **Routing decision**: collect every route where
  `(dest_ip & mask) == (network & mask)`, then pick the longest prefix.
- **Default gateway vs default route**:
  - Hosts send non-local traffic to the gateway.
  - Routers use `0.0.0.0/0` when nothing more specific matches.
- **Return traffic reminder**: both directions need a route (either explicit or a
  default) or pings will succeed only one way.

---

## Practice Flow

1. Pick any NetPractice level.
2. Identify every subnet and write down its prefix from Chapter 01–02 notes.
3. Validate direct/indirect communication with the Chapter 03 checklist.
4. Program the needed static routes using the Chapter 04 template.
5. Answer the practice questions at the end of each chapter to lock the concept
   in before moving on.

Happy subnetting! 🧠💡
