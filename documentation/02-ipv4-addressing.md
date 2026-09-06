# Task 02 — IPv4 Addressing and Subnetting

## Objective

Create and configure the IPv4 addressing plan for the network.

The IP addresses are simple and organized to make the network easier to understand and troubleshoot.

---

## Lab Platform

- EVE-NG
- Cisco vIOS Router
- Cisco IOL L2 Switch

---

## VLAN Networks

The network will use four VLANs.

| VLAN | Department | Network | Default Gateway |
|---|---|---|---|
| 10 | Administration | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 |

The VLANs will be configured in the next tasks.

---

## Router-to-Router Links

The router-to-router links use `/30` networks.

A `/30` network gives two usable IP addresses. This is useful for a point-to-point link between two routers.

### R1 ↔ R2

Network:

    10.0.0.0/30

IP addresses:

| Device | Interface | IP Address |
|---|---|---|
| R1 | Gi0/1 | 10.0.0.1 |
| R2 | Gi0/0 | 10.0.0.2 |

Subnet Mask:

    255.255.255.252

---

### R1 ↔ R3

Network:

    10.0.1.0/30

IP addresses:

| Device | Interface | IP Address |
|---|---|---|
| R1 | Gi0/0 | 10.0.1.1 |
| R3 | Gi0/1 | 10.0.1.2 |

Subnet Mask:

    255.255.255.252

---

## Interface Connections

The following connections are used in the topology:

| Device | Interface | Connected To |
|---|---|---|
| R3 | Gi0/1 | R1 Gi0/0 |
| R1 | Gi0/1 | R2 Gi0/0 |
| R2 | Gi0/1 | SW1 e0/0 |
| R2 | Gi0/2 | SW2 e0/0 |

The links between R2 and the switches will be configured later as trunk links.

---

## R1 Configuration

R1 uses two interfaces for router-to-router connections.

    interface gi0/0
     ip address 10.0.1.1 255.255.255.252
     no shutdown

    interface gi0/1
     ip address 10.0.0.1 255.255.255.252
     no shutdown

---

## R2 Configuration

R2 uses Gi0/0 to connect to R1.

    interface gi0/0
     ip address 10.0.0.2 255.255.255.252
     no shutdown

---

## R3 Configuration

R3 uses Gi0/1 to connect to R1.

    interface gi0/1
     ip address 10.0.1.2 255.255.255.252
     no shutdown

---

## Verification

The following command was used to check the interfaces:

    show ip interface brief

The important interfaces should show `up/up`.

### R1

    Gi0/0    10.0.1.1    up    up
    Gi0/1    10.0.0.1    up    up

### R2

    Gi0/0    10.0.0.2    up    up

### R3

    Gi0/1    10.0.1.2    up    up

Unused interfaces are still administratively down because we do not use them yet.

---

## Connectivity Test

We tested the connection between R2 and R1.

From R2:

    ping 10.0.0.1

Result:

    Success rate is 80 percent (4/5)

The first packet was lost, but the next four packets were successful.

This can happen because the router needs to learn the MAC address using ARP before sending the traffic normally.

---

## Configuration Save

After the configuration was completed, the configuration was saved.

Command:

    write memory

Another command that can be used is:

    copy running-config startup-config

`running-config` is the current configuration.

`startup-config` is the saved configuration used after a reload.

---

## Task Result

Task 02 was completed successfully.



<img width="1236" height="218" alt="image" src="https://github.com/user-attachments/assets/b162e043-cfe0-41da-aef1-ef61d6502aa4" />



<img width="1312" height="265" alt="image" src="https://github.com/user-attachments/assets/87e93456-dfb3-4c87-a30d-48e57e052461" />



<img width="1281" height="243" alt="image" src="https://github.com/user-attachments/assets/427d8520-4839-4ae8-9224-7020b4210990" />



We completed:

- IPv4 addressing plan
- /24 networks for VLANs
- /30 networks for router links
- R1 configuration
- R2 configuration
- R3 configuration
- Interface verification
- Ping testing
- Configuration save

---

## Next Task

Task 03 — VLANs

We will create four VLANs:

- VLAN 10 — Administration
- VLAN 20 — Sales
- VLAN 30 — IT
- VLAN 40 — Servers

First, we will create and verify the VLANs on the switches.
