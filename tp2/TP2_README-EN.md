# TP2 - DHCP Configuration on Cisco Router

**Author** : ALPHA Ziad Ramdan
**Field** : L1 Cybersecurity
**School** : Institut Superieur de Management (ISM) - Dakar, Senegal
**Date** : March 15, 2026
**Tool** : Cisco Packet Tracer 8.2.2 - Cisco Router 2911

---

## Description

Configuration of the DHCP service on two Cisco 2911 routers. PCs automatically obtain their IP address, subnet mask, gateway and DNS without any manual configuration. This lab is the direct continuation of TP1 on static routing.

---

## Network Topology

![Network Topology](tp2_topology.png)

```
PC0 (DHCP - LAN1)
        |
     [Gi0/1]
        R1  <-- DHCP Server for LAN1 (192.168.1.0/24)
     [Gi0/0]
        |
   10.0.0.0/30
        |
     [Gi0/0]
        R2  <-- DHCP Server for LAN2 (192.168.2.0/24)
     [Gi0/1]
        |
PC1 (DHCP - LAN2)
```

---

## IP Addressing Plan

| Device | Interface | IP Address | Mask | Gateway |
|--------|-----------|------------|------|---------|
| R1 | Gi0/1 | 192.168.1.1 | 255.255.255.0 /24 | - |
| R1 | Gi0/0 | 10.0.0.1 | 255.255.255.252 /30 | - |
| R2 | Gi0/0 | 10.0.0.2 | 255.255.255.252 /30 | - |
| R2 | Gi0/1 | 192.168.2.1 | 255.255.255.0 /24 | - |
| PC0 | Fa0 | DHCP (received : 192.168.1.11) | 255.255.255.0 /24 | 192.168.1.1 |
| PC1 | Fa0 | DHCP (received : 192.168.2.11) | 255.255.255.0 /24 | 192.168.2.1 |

---

## What is DHCP ?

Without DHCP (TP1) : each PC's IP address is configured manually.

With DHCP (TP2) : the PC sends a request and the router automatically assigns an IP address, subnet mask, gateway and DNS.

This process is called DORA :
- D - Discover : the PC searches for a DHCP server
- O - Offer : the router proposes an available IP address
- R - Request : the PC accepts the offer
- A - Acknowledge : the router confirms the assignment

---

## DHCP Configuration on R1 (for LAN1)

```
enable
configure terminal
ip dhcp pool LAN1
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
exit
ip dhcp excluded-address 192.168.1.1 192.168.1.10
end
wr
```

Command explanation :

| Command | Description |
|---------|-------------|
| ip dhcp pool LAN1 | Create a DHCP pool named LAN1 |
| network | Define the address range to distribute |
| default-router | Gateway given to clients |
| dns-server | DNS server given to clients |
| excluded-address | Reserved addresses not distributed by DHCP |

---

## DHCP Configuration on R2 (for LAN2)

```
enable
configure terminal
ip dhcp pool LAN2
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1
dns-server 8.8.8.8
exit
ip dhcp excluded-address 192.168.2.1 192.168.2.10
end
wr
```

---

## Verification - show ip dhcp binding

![DHCP Binding R1](tp2_dhcp_binding.png)

```
IP address      Client-ID / Hardware address    Lease expiration    Type
192.168.1.11    000D.BDDB.E272                  --                  Automatic
```

PC0 successfully received the address 192.168.1.11 automatically via DHCP.

---

## Connectivity Test Results

![Ping Results](tp2_ping_results.png)

| Source | Destination | Result | Note |
|--------|-------------|--------|------|
| PC0 (192.168.1.11) | PC1 (192.168.2.11) | 100% | Inter-network communication OK |

Note : The first 2 packets were lost during ARP resolution on the first attempt. On the second attempt : 100% success. Completely normal behavior.

---

## Skills Learned

- Understanding the DHCP protocol and the DORA process
- Configuring a DHCP pool on a Cisco router with network, default-router, dns-server
- Excluding addresses with ip dhcp excluded-address
- Setting a PC to automatic DHCP mode in Packet Tracer
- Verifying DHCP assignments with show ip dhcp binding
- Combining DHCP and static routing on the same router

---

## DHCP Verification Commands

| Command | Description |
|---------|-------------|
| show ip dhcp binding | Displays IP addresses assigned to clients |
| show ip dhcp pool | Displays DHCP pool details |
| show ip dhcp conflict | Displays detected address conflicts |

---

*ALPHA Ziad Ramdan - L1 Cybersecurity - ISM Dakar - March 15, 2026*
