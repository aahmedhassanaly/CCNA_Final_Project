# Cisco Enterprise Network Lab

**Portfolio Priority: #2 — Core Networking**

A practical Cisco networking lab built with **EVE-NG** covering switching, routing, VLANs, ACLs, NAT/PAT, STP, EtherChannel, port security, and troubleshooting.

## Lab Environment

- EVE-NG
- Cisco vIOS Router
- Cisco IOL L2 Switch

### Devices

- R1
- R2
- R3
- SW1
- SW2

## Topology

<img width="1374" height="1145" alt="Cisco enterprise network topology" src="https://github.com/user-attachments/assets/382940d4-9af6-4bba-bc90-539feb0947be" />

## VLANs

| VLAN | Name | Network |
|---|---|---|
| 10 | Administration | 192.168.10.0/24 |
| 20 | Sales | 192.168.20.0/24 |
| 30 | IT | 192.168.30.0/24 |
| 40 | Servers | 192.168.40.0/24 |

## Skills Demonstrated

- IPv4 addressing and subnetting
- VLANs and access ports
- 802.1Q trunking
- Inter-VLAN routing / Router-on-a-Stick
- DHCP
- Static and default routing
- SSH management
- ACLs
- NAT/PAT
- STP and link-failure testing
- LACP EtherChannel
- Port Security and MAC-violation testing
- Troubleshooting and verification

## Verification

Common verification commands:

```text
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip dhcp binding
show access-lists
show ip nat translations
show spanning-tree
show etherchannel summary
show port-security interface ethernet 0/1
```

## Tasks

| # | Task | Status |
|---:|---|:---:|
| 01 | [Device & Topology Preparation](documentation/01-device-topology-preparation.md) | ✅ |
| 02 | [IPv4 Addressing](documentation/02-ipv4-addressing.md) | ✅ |
| 03 | [VLANs](documentation/03-vlans.md) | ✅ |
| 04 | [Access Ports](documentation/04-access-ports.md) | ✅ |
| 05 | [Trunking](documentation/05-trunking.md) | ✅ |
| 06 | [Inter-VLAN Routing](documentation/06-inter-vlan-routing.md) | ✅ |
| 07 | [DHCP](documentation/07-dhcp.md) | ✅ |
| 08 | [Static & Default Routing](documentation/08-static-default-routing.md) | ✅ |
| 09 | [Internet Simulation](documentation/09-internet-simulation.md) | ✅ |
| 10 | [SSH Device Security](documentation/10-ssh-device-security.md) | ✅ |
| 11 | [ACLs](documentation/11-acls.md) | ✅ |
| 12 | [NAT/PAT](documentation/12-nat-pat.md) | ✅ |
| 13 | [STP](documentation/13-stp.md) | ✅ |
| 14 | [EtherChannel](documentation/14-etherchannel.md) | ✅ |
| 15 | [Port Security](documentation/15-port-security.md) | ✅ |

Detailed implementation notes: [Documentation](documentation/).

## Project Status

**Completed**

IPv6 was reviewed but not implemented.

OSPF is intentionally kept for a separate routing lab rather than duplicating the project scope.
