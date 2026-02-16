# IP Addressing Plan

## Overview

This project simulates a small enterprise campus network with segmented VLANs, dynamic routing (OSPF), NAT at the edge, and centralized DHCP.

The IP addressing scheme is structured, scalable, and easy to expand.

---

## Addressing Summary

| VLAN | Name        | Subnet              | Gateway (SVI)      | Purpose                |
|------|------------|---------------------|---------------------|------------------------|
| 10   | Users      | 192.168.10.0/24     | 192.168.10.1        | End-user devices       |
| 20   | Servers    | 192.168.20.0/24     | 192.168.20.1        | Internal services      |
| 30   | Management | 192.168.30.0/24     | 192.168.30.1        | Network administration |
| 40   | Guest      | 192.168.40.0/24     | 192.168.40.1        | Guest access network   |

---

## Core ↔ Edge Transit Network

| Link        | Subnet         | Core IP     | Edge IP     |
|------------|---------------|-------------|-------------|
| Core-Edge  | 10.0.0.0/30    | 10.0.0.1    | 10.0.0.2    |

This /30 network is used exclusively for routing adjacency (OSPF) between the Core Layer 3 switch and the Edge router.

---

## ISP External Transit Network

| Link        | Subnet         | Edge IP     | ISP IP     |
|------------|---------------|-------------|-------------|
| Edge-ISP  | 203.0.113.0/30    | 203.0.113.1    | 203.0.113.2    |

This /30 network is used exclusively for routing adjacency (OSPF) between the Core Layer 3 switch and the Edge router.

---
## DHCP Configuration Strategy

DHCP is hosted on the Core switch.

### Excluded Addresses

The first 10 IP addresses in each VLAN are reserved for:

- Gateway (SVI)
- Servers
- Infrastructure devices
