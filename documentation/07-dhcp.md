# Task 07 — DHCP

## Objective

Configure R2 as a DHCP server to automatically assign IPv4 addresses to clients in VLAN 10, VLAN 20, VLAN 30, and VLAN 40.

The goal is to replace static IP configuration on end devices with centralized DHCP address assignment.

---

## Network Design

| VLAN | Department | Network | Default Gateway |
|---|---|---|---|
| VLAN 10 | Administration | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | Sales | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | Servers | 192.168.40.0/24 | 192.168.40.1 |

R2 provides both:

- Inter-VLAN Routing
- DHCP Services

The DHCP server is located on the same router that provides the VLAN gateways.

---

## DHCP Design

The first 10 addresses in each subnet are reserved for infrastructure or future static devices.

Reserved addresses:

- `.1` → Default Gateway
- `.2 - .10` → Reserved

DHCP clients can receive addresses starting from `.11`.

---

## Configuration

### VLAN 10 DHCP Pool

    R2(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

    R2(config)# ip dhcp pool vlan 10
    R2(dhcp-config)# network 192.168.10.0 255.255.255.0
    R2(dhcp-config)# default-router 192.168.10.1

### VLAN 20 DHCP Pool

    R2(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.10

    R2(config)# ip dhcp pool vlan20
    R2(dhcp-config)# network 192.168.20.0 255.255.255.0
    R2(dhcp-config)# default-router 192.168.20.1

### VLAN 30 DHCP Pool

    R2(config)# ip dhcp excluded-address 192.168.30.1 192.168.30.10

    R2(config)# ip dhcp pool vlan30
    R2(dhcp-config)# network 192.168.30.0 255.255.255.0
    R2(dhcp-config)# default-router 192.168.30.1

### VLAN 40 DHCP Pool

    R2(config)# ip dhcp excluded-address 192.168.40.1 192.168.40.10

    R2(config)# ip dhcp pool vlan40
    R2(dhcp-config)# network 192.168.40.0 255.255.255.0
    R2(dhcp-config)# default-router 192.168.40.1

---

## DHCP Operation

When a client needs an IP address, it starts the DHCP process with a DHCP Discover message.

The message is initially broadcast within the client's VLAN.

For example:

    PC1 → SW2 → SW1 → R2

R2 receives the request through the corresponding router subinterface.

For VLAN 10:

    R2 Gi0/1.10
    192.168.10.1/24
    encapsulation dot1Q 10

R2 identifies the VLAN based on the incoming subinterface and selects the matching DHCP pool.

For VLAN 10, the DHCP pool uses:

    network 192.168.10.0 255.255.255.0
    default-router 192.168.10.1

R2 then assigns an available address to the client.

---

## Important Concept — DHCP Relay

An `ip helper-address` is not required in this topology.

The reason is that R2 is both:

- The DHCP Server
- The Default Gateway for the VLANs

`ip helper-address` is normally required when the DHCP server is located on another network and a Layer 3 device must relay DHCP requests to that server.

---

## Client Address Assignment

The clients were changed from static addressing to DHCP.

The resulting assignments were:

| Client / VLAN | Assigned IP | DHCP Status |
|---|---|---|
| VLAN 10 | 192.168.10.11 | Automatic |
| VLAN 20 | 192.168.20.11 | Automatic |
| VLAN 30 | 192.168.30.11 | Automatic |
| VLAN 40 | 192.168.40.11 | Automatic |

---

## Verification

### Verify DHCP Bindings

    R2# show ip dhcp binding

Expected result:

    Bindings from all pools not associated with VRF:
    IP address          Client-ID/              Lease expiration        Type
                        Hardware address/
                        User name
    192.168.10.11       0100.5079.6668.0b       Sep 08 2026 02:19 PM    Automatic
    192.168.20.11       0100.5079.6668.07       Sep 08 2026 02:22 PM    Automatic
    192.168.30.11       0100.5079.6668.03       Sep 08 2026 02:22 PM    Automatic
    192.168.40.11       0100.5079.6668.0a       Sep 08 2026 02:22 PM    Automatic

The `Automatic` type confirms that the addresses were dynamically assigned by DHCP.

---

## DHCP Configuration Verification

    R2# show running-config | section dhcp

Expected configuration:

    ip dhcp excluded-address 192.168.10.1 192.168.10.10
    ip dhcp excluded-address 192.168.20.1 192.168.20.10
    ip dhcp excluded-address 192.168.30.1 192.168.30.10
    ip dhcp excluded-address 192.168.40.1 192.168.40.10

    ip dhcp pool vlan 10
     network 192.168.10.0 255.255.255.0
     default-router 192.168.10.1

    ip dhcp pool vlan20
     network 192.168.20.0 255.255.255.0
     default-router 192.168.20.1

    ip dhcp pool vlan30
     network 192.168.30.0 255.255.255.0
     default-router 192.168.30.1

    ip dhcp pool vlan40
     network 192.168.40.0 255.255.255.0
     default-router 192.168.40.1

---

### DHCP Pool Has No Bindings

Remember:

Creating a DHCP pool does not create a lease.

A binding appears only after a client actually requests and receives an address.

---

## Real-World Relevance

DHCP is commonly used to automatically provide network configuration to end devices such as:

- User PCs
- Laptops
- Phones
- Printers
- Other network clients

Centralized DHCP reduces manual configuration and helps prevent duplicate IP addresses.

In larger enterprise environments, DHCP may be provided by dedicated servers such as Windows Server or Linux-based services, while routers or Layer 3 switches commonly act as DHCP relay agents.

---



---

## Final Verification

Task 07 is complete when:

- [x] DHCP pool exists for VLAN 10
- [x] DHCP pool exists for VLAN 20
- [x] DHCP pool exists for VLAN 30
- [x] DHCP pool exists for VLAN 40
- [x] Reserved addresses are excluded
- [x] Clients use DHCP
- [x] VLAN 10 client received an address
- [x] VLAN 20 client received an address
- [x] VLAN 30 client received an address
- [x] VLAN 40 client received an address
- [x] R2 shows DHCP bindings
- [x] DHCP addresses match the correct VLAN subnets

---

## Result

R2 is successfully providing DHCP services for all four VLANs.
<img width="828" height="517" alt="image" src="https://github.com/user-attachments/assets/ccfd7345-9d9f-4e87-980c-72e377f81bab" />


Each VLAN receives addresses from its own DHCP scope, while R2 remains the default gateway and performs inter-VLAN routing.

The lab now has:

    VLANs
    +
    Trunking
    +
    Router-on-a-Stick
    +
    DHCP
    =
    Functional multi-VLAN network with dynamic IP addressing

Next task: **Task 08 — Static & Default Routing**
