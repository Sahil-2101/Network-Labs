## Overview

This document outlines structured troubleshooting scenarios encountered during the development of the Enterprise Campus Network project.  
Each issue was diagnosed using OSI-based methodology, CLI diagnostics, and root cause analysis.

---

# Scenario 1 – Inter-VLAN Ping Failure
PC in VLAN 10 unable to ping default gateway (192.168.10.1).

### OSI Layer Suspected
Layer 3 (Network Layer)

### Commands Used
show ip interface brief  
show vlan brief  
show interfaces trunk  

### Findings
- SVI configured correctly
- VLAN present
- Trunk operational
- PC had no default gateway configured

### Root Cause
Default gateway missing on end device.

### Resolution
Configured gateway manually on PC.

### Validation
Successful ping to 192.168.10.1.

---

# Scenario 2 – OSPF Configuration Error

### Symptoms
Attempting to configure OSPF returned:
"IP routing not enabled"

### OSI Layer Suspected
Layer 3 configuration issue

### Commands Used
router ospf 1  
show ip route  

### Findings
Switch operating in Layer 2 mode only.

### Root Cause
Global routing not enabled on multilayer switch.

### Resolution
Enabled routing:
ip routing

### Validation
OSPF process started successfully.
Adjacency formed with Edge router.

---

# Scenario 3 – DHCP Lease Not Assigned

### Symptoms
Clients did not receive IP addresses.
show ip dhcp binding returned empty.

### OSI Layer Suspected
Layer 7 (Application Service)

### Commands Used
show running-config | section dhcp  
show ip interface brief  

### Findings
Clients configured as static.

### Root Cause
End devices not set to DHCP mode.

### Resolution
Changed client configuration to DHCP.

### Validation
IP leases successfully assigned.
show ip dhcp binding displayed active entries.

---

# Scenario 4 – NAT Not Translating

### Symptoms
Internal clients could not access ISP network.
show ip nat translations returned empty.

### OSI Layer Suspected
Layer 3/4 (Address Translation)

### Commands Used
show ip nat statistics  
show running-config  
show ip route  

### Findings
Inside/outside interfaces not designated correctly.

### Root Cause
Missing ip nat inside / ip nat outside configuration.

### Resolution
Configured:
interface g0/0
 ip nat inside

interface g0/1
 ip nat outside

Re-applied NAT overload rule.

### Validation
show ip nat translations displayed active entries after traffic generation.

---

# Scenario 5 – ACL Blocking Legitimate Traffic

### Symptoms
Guest VLAN unable to access internet.

### OSI Layer Suspected
Layer 3 filtering

### Commands Used
show access-lists  
show ip route  
ping (Edge router outside address)

### Findings
ACL applied without final permit statement.

### Root Cause
Implicit deny blocked outbound internet traffic.

### Resolution
Added:
permit ip 192.168.40.0 0.0.0.255 any

### Validation
Guest VLAN regained internet access.
ACL hit counters incremented correctly.

---
