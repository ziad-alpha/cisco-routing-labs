# TP1 - Cisco Static Routing Configuration

**Author** : ALPHA Ziad Ramdan
**Field** : L1 Cybersecurity
**School** : Institut Superieur de Management (ISM) - Dakar, Senegal
**Date** : March 11, 2026
**Tool** : Cisco Packet Tracer 8.2.2 - Router 2911

---

## Description

Configuration of two Cisco 2911 routers interconnected via a point-to-point link, with one PC on each side simulating two distinct local area networks. The entire configuration was performed from PC0 via a console cable.

---

## Network Topology

![Network Topology](screenshots/topology.png)

```
PC0 (192.168.1.10)
        |
     [Gi0/0]
        R1              LAN1 : 192.168.1.0/24
     [Gi0/1]
        |
   10.0.0.0/30          Point-to-point link
        |
     [Gi0/0]
        R2              LAN2 : 192.168.2.0/24
     [Gi0/1]
        |
PC1 (192.168.2.10)
```

---

## IP Addressing Plan

| Device | Interface | IP Address | Mask | Gateway |
|--------|-----------|------------|------|---------|
| R1 | Gi0/0 | 192.168.1.1 | 255.255.255.0 /24 | - |
| R1 | Gi0/1 | 10.0.0.1 | 255.255.255.252 /30 | - |
| R2 | Gi0/0 | 10.0.0.2 | 255.255.255.252 /30 | - |
| R2 | Gi0/1 | 192.168.2.1 | 255.255.255.0 /24 | - |
| PC0 | Fa0 | 192.168.1.10 | 255.255.255.0 /24 | 192.168.1.1 |
| PC1 | Fa0 | 192.168.2.10 | 255.255.255.0 /24 | 192.168.2.1 |

Note : The /30 mask on the point-to-point link provides exactly 2 usable addresses. Using /24 here would have wasted addresses unnecessarily.

---

## Security Configuration

| Setting | Value |
|---------|-------|
| Privileged mode password | enable secret cisco |
| Console access password | password cisco1 |
| Remote VTY access 6 sessions | password cisco12 |
| Password encryption | service password-encryption |
| Warning banner | Acces strictement interdit |

---

## Static Routes

R1 - to reach LAN2 via R2 :
```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

R2 - to reach LAN1 via R1 :
```
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

---

## Verification - show ip interface brief

![Show IP Interface Brief R2](screenshots/show_ip_int_br_r2.png)

```
Interface             IP-Address     OK?  Method  Status                 Protocol
GigabitEthernet0/0    10.0.0.2       YES  manual  up                     up
GigabitEthernet0/1    192.168.2.1    YES  manual  up                     up
GigabitEthernet0/2    unassigned     YES  unset   administratively down  down
```

---

## Connectivity Test Results

![Ping Results](screenshots/ping_results.png)

| Source | Destination | Result | Note |
|--------|-------------|--------|------|
| PC0 (192.168.1.10) | R1 Gi0/0 (192.168.1.1) | 100% | Local communication OK |
| PC0 (192.168.1.10) | PC1 (192.168.2.10) | 100% | Inter-network communication OK |

Note : The first 2 packets of the initial ping to PC1 were lost during ARP resolution. On the second attempt, 100% success. This is completely normal behavior.

---

## Skills Learned

- Navigating Cisco IOS modes (User, Privileged, Configuration)
- Securing a router with multiple password levels
- Encrypting all passwords with service password-encryption
- Configuring GigabitEthernet interfaces and enabling them with no shutdown
- Building a coherent IP addressing plan with justified mask choices
- Understanding and configuring static routes between two networks
- Testing end-to-end connectivity with the ping command
- Saving configuration to non-volatile memory NVRAM
- Working via console cable from a PC

---

## Verification Commands

| Command | Shortcut | Description |
|---------|----------|-------------|
| show ip interface brief | sh ip int br | Status of all interfaces |
| show ip route | sh ip ro | Full routing table |
| show running-config | sh ru | Current running configuration |
| copy running-config startup-config | cop ru st | Save configuration to NVRAM |
| ping [address] | ping | Connectivity test |

---

## Repository Structure

```
cisco-routing-labs/
├── README-EN.md
├── README-FR.md
├── configs/
│   └── TP1_Configs_ALPHA_Ziad.txt
├── report/
│   └── TP1_Cisco_ALPHA_Ziad.docx
└── screenshots/
    ├── topology.png
    ├── show_ip_int_br_r2.png
    └── ping_results.png
```

---

*ALPHA Ziad Ramdan - L1 Cybersecurity - ISM Dakar - March 11, 2026*
