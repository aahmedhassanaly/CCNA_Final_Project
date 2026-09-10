# Cisco Enterprise Network Lab

A practical Cisco networking lab built with **EVE-NG** to practice CCNA-level networking skills.

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
<img width="1374" height="1145" alt="image" src="https://github.com/user-attachments/assets/382940d4-9af6-4bba-bc90-539feb0947be" />

  
## Main Technologies

- IPv4 Addressing & Subnetting
- VLANs
- Access Ports
- 802.1Q Trunking
- Inter-VLAN Routing
- DHCP
- Static & Default Routing
- SSH
- ACLs
- NAT/PAT
- STP
- LACP EtherChannel
- Port Security

## VLANs

| VLAN | Name | Network |
|---|---|---|
| 10 | Administration | 192.168.10.0/24 |
| 20 | Sales | 192.168.20.0/24 |
| 30 | IT | 192.168.30.0/24 |
| 40 | Servers | 192.168.40.0/24 |

## What Was Practiced

- Built the network topology in EVE-NG.
- Configured VLANs and trunk links.
- Implemented Router-on-a-Stick for inter-VLAN routing.
- Configured DHCP for multiple VLANs.
- Configured static and default routing.
- Configured SSH for remote management.
- Used ACLs to control traffic between VLANs.
- Configured NAT/PAT for Internet access.
- Configured STP and tested link failure.
- Configured LACP EtherChannel.
- Configured Port Security and tested a MAC violation.
- Practiced troubleshooting during configuration problems.

## Verification

Common Cisco commands used:

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

## Project Status

**Completed**

IPv6 was reviewed but not implemented.

OSPF will be practiced in a **separate lab**.
