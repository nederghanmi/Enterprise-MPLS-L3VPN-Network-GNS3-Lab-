# 🚀 Enterprise MPLS L3VPN Network (GNS3 Lab)

---

## 📌 Project Description

This project demonstrates the implementation of a complete Enterprise MPLS Layer 3 VPN (L3VPN) architecture using GNS3.

The network simulates a real-world service provider backbone delivering secure connectivity between multiple customer sites (HQ and Branches) using:

- MPLS (LDP)
- MP-BGP VPNv4
- VRF (VPN_CYBER)
- OSPF (Core + Customer)
- VLANs, HSRP, EtherChannel
- DHCP & Inter-VLAN Routing

---

## 🧠 Architecture Overview

### 🔹 Backbone (Provider Network)

- OSPF Area 0 for core routing  
- MPLS enabled on all core links  
- LDP used for label distribution  

Routers:

- P1, P2 → MPLS forwarding only  
- PE1, PE2 → VRF + MP-BGP  

---

### 🔹 VPN Layer

- VRF: VPN_CYBER  
- RD: 100:3  
- RT: 100:3  
- MP-BGP AS: 100  

---

### 🔹 Customer Sites

#### 🏢 Headquarters (Siège)

- VLAN 10 → 172.16.210.0/24  
- VLAN 15 → 172.16.215.0/24  
- VLAN 20 → 172.16.220.0/24  

Features:

- Inter-VLAN Routing (Layer 3 Switch)  
- HSRP (Gateway Redundancy)  
- EtherChannel (Link Aggregation)  
- DHCP Service  

---

#### 🌍 Branch1

- VLAN 201 → 172.16.201.0/24  
- VLAN 202 → 172.16.202.0/24  

- Router-on-a-Stick  
- DHCP on router  
- OSPF with PE  

---

#### 🌍 Branch2

- VLAN 203 → 172.16.203.0/24  
- VLAN 204 → 172.16.204.0/24  

---

## 🔄 Routing Design

- OSPF Process 1 → CE ↔ PE  
- OSPF Process 300 → VRF routing  
- MP-BGP → VPNv4 route exchange  
- Redistribution between OSPF and BGP  

---

## 🧪 VERIFICATION RESULTS (REAL LAB)

### 🔹 MPLS & BGP (PE2)

```bash
PE2# show ip bgp vpnv4 all summary

Neighbor        AS     State/PfxRcd
1.1.1.1         100    4
```

✔ BGP session is UP and exchanging routes  

---

### 🔹 VRF Routing Table

```bash
PE2# show ip route vrf VPN_CYBER 172.16.204.0

Routing entry for 172.16.204.0/24
Known via "ospf 300"
via 192.168.1.14
```

✔ Branch2 network successfully learned via MPLS VPN  

---

### 🔹 OSPF Neighbors (PE2)

```bash
Neighbor ID     State
12.12.12.12     FULL
22.22.22.22     FULL
```

✔ All CE neighbors are FULL  

---

### 🔹 MPLS LDP

```bash
show mpls ldp neighbor

State: Oper
```

✔ MPLS core is operational  

---

### 🔹 HSRP (Redundancy)

```bash
show standby brief

Vl10 Active 172.16.210.1
Vl15 Active 172.16.215.1
Vl20 Active 172.16.220.1
```

✔ Gateway redundancy working  

---

### 🔹 EtherChannel

```bash
show etherchannel summary

Po1(SU)  Et0/0(P) Et0/1(P)
```

✔ Link aggregation active  

---

## 🧪 END-TO-END VALIDATION

- ✔ PC0 → Branch1 ✅  
- ✔ PC0 → Branch2 ✅  
- ✔ PC2 → HQ ✅  
- ✔ Full connectivity between all sites  

---

## ⚠️ Troubleshooting & Fixes

During the project, several real issues were identified and resolved:

- ❌ OSPF stuck in EXSTART → ✔ Fixed with `ip ospf mtu-ignore`  
- ❌ Missing routes → ✔ Enabled `ip routing` on CE routers  
- ❌ No connectivity → ✔ Fixed OSPF redistribution into BGP  
- ❌ VLAN / trunk issues → ✔ Corrected native VLAN and allowed VLANs  

---

## 🛠️ Technologies Used

- MPLS (LDP)  
- MP-BGP (VPNv4)  
- OSPF  
- VRF (L3VPN)  
- VLANs (802.1Q)  
- HSRP  
- EtherChannel  
- DHCP  
- GNS3  

---

## 🎯 Skills Demonstrated

- Enterprise Network Design  
- MPLS L3VPN Deployment  
- Routing (OSPF + BGP)  
- High Availability (HSRP)  
- Switching & VLAN segmentation  
- Advanced Troubleshooting  

---

## 👨‍💻 Author

**Neder Ghanmi**
