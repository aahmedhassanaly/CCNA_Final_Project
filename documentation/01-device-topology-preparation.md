# Task 01 — Device & Topology Preparation

## Objective

Prepare the initial enterprise network lab in EVE-NG before starting IP addressing, VLANs, routing, and network services.

The objectives of this task are:

- Build the initial network topology.
- Assign clear hostnames to all devices.
- Identify the role of each device.
- Verify the available interfaces.
- Verify the physical connections.
- Confirm that the lab is ready for network configuration.

## Lab Platform

- EVE-NG

## Cisco Images

### Routers

- Cisco vIOS Router
- Image: `vios-adventerprisek9-m-15.6.2T`

### Switches

- Cisco IOL L2

## Devices

| Device | Role | Image |
|---|---|---|
| R1 | Edge Router | Cisco vIOS Router |
| R2 | Core Router | Cisco vIOS Router |
| R3 | ISP Router | Cisco vIOS Router |
| SW1 | Access Switch | Cisco IOL L2 |
| SW2 | Access Switch | Cisco IOL L2 |

## Network Topology

                 INTERNET / ISP
                       |
                      R3
                       |
                      R1
                       |
                      R2
                    /    \
                   /      \
                 SW1      SW2

### Device Roles

**R3 — ISP Router**

Represents the external ISP/Internet side of the lab.

**R1 — Edge Router**

Acts as the enterprise edge router between the internal network and the ISP.

**R2 — Core Router**

Acts as the internal routing device and will provide connectivity between internal network segments.

**SW1 / SW2 — Access Switches**

Provide Layer 2 connectivity for internal end devices.

## Physical Connections

| Device A | Interface | Device B | Interface |
|---|---|---|---|
| R3 | Gi0/0 | Internet | — |
| R3 | Gi0/1 | R1 | Gi0/0 |
| R1 | Gi0/1 | R2 | Gi0/0 |
| R2 | Gi0/1 | SW1 | e0/0 |
| R2 | Gi0/2 | SW2 | e0/0 |

The interface assignments were verified from the actual EVE-NG devices.

## Basic Device Configuration

Each device was assigned a unique hostname.

R1:

    hostname R1

R2:

    hostname R2

R3:

    hostname R3

SW1:

    hostname SW1

SW2:

    hostname SW2

Using unique hostnames makes it easier to identify the device being configured and reduces the risk of applying configuration to the wrong device.

## Interface Verification

Router interfaces were verified using:

    show ip interface brief

Switch interfaces were verified using:

    show interfaces status

The purpose of these commands was to confirm the available interfaces and verify the initial state of the devices before network configuration.

## Initial Interface State

At the beginning of the lab, interfaces were not configured with IP addresses.

Unused or not-yet-enabled interfaces showed:

    administratively down

This is expected during the initial preparation stage.

Interfaces will be enabled and configured later when they are required by the network design.

## Configuration Scope

The following technologies were intentionally not configured during this task:

- IPv4 addressing
- Subnetting
- VLANs
- Access ports
- Trunking
- Inter-VLAN routing
- DHCP
- Static routing
- Default routing
- NAT/PAT
- ACLs
- SSH
- IPv6
- STP
- EtherChannel
- Port Security

These technologies will be introduced in later tasks when they are required.

## Verification

The following commands were used to verify the initial device state.

### Routers

    show ip interface brief

### Switches

    show interfaces status

The verification confirmed that the required interfaces were available and that the initial topology was ready for further configuration.

## Task Result

Task 01 successfully completed.

The EVE-NG environment is prepared and ready for the next phase:
<img width="1913" height="876" alt="image" src="https://github.com/user-attachments/assets/c52f70c0-af80-4fd9-a7df-051f177b534a" />
