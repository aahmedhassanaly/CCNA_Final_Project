# Task 06 — Inter-VLAN Routing

## Objective

Configure Inter-VLAN Routing using Router-on-a-Stick.

The router uses one physical interface with multiple subinterfaces.
Each subinterface is assigned to one VLAN.

## Topology

                 R2
                  |
                Trunk
                  |
                 SW1
                  |
                Trunk
                  |
                 SW2


<img width="1390" height="866" alt="image" src="https://github.com/user-attachments/assets/13e53ae0-fbfe-4a4c-a815-2d7887df3c46" />

SW1 and SW2 carry the required VLANs between the access ports and R2.

## VLANs and Gateways

| VLAN | Department | Network | Gateway |
|------|------------|---------|---------|
| 10 | Administration | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 |

## Router Configuration

R2 uses GigabitEthernet0/1 as the trunk interface toward SW1.

### VLAN 10

    interface GigabitEthernet0/1.10
     encapsulation dot1Q 10
     ip address 192.168.10.1 255.255.255.0

### VLAN 20

    interface GigabitEthernet0/1.20
     encapsulation dot1Q 20
     ip address 192.168.20.1 255.255.255.0

### VLAN 30

    interface GigabitEthernet0/1.30
     encapsulation dot1Q 30
     ip address 192.168.30.1 255.255.255.0

### VLAN 40

    interface GigabitEthernet0/1.40
     encapsulation dot1Q 40
     ip address 192.168.40.1 255.255.255.0

     
<img width="1919" height="769" alt="image" src="https://github.com/user-attachments/assets/754faaa7-2edf-4cb6-be42-11d424ed18fb" />

## Switch Trunks

The link between R2 and SW1 is configured as a trunk.

    interface Ethernet0/0
     switchport trunk encapsulation dot1q
     switchport mode trunk

The link between SW1 and SW2 is also configured as a trunk.

    interface Ethernet1/3
     switchport trunk encapsulation dot1q
     switchport mode trunk

The trunks carry VLANs 10, 20, 30, and 40.

## End Devices

| Device | VLAN | IP Address | Default Gateway |
|--------|------|------------|-----------------|
| PC1 | 10 | 192.168.10.10/24 | 192.168.10.1 |
| PC2 | 20 | 192.168.20.20/24 | 192.168.20.1 |
| PC3 | 30 | 192.168.30.30/24 | 192.168.30.1 |
| PC4 | 40 | 192.168.40.40/24 | 192.168.40.1 |

## How Router-on-a-Stick Works

R2 uses one physical interface:

    GigabitEthernet0/1

The interface has multiple subinterfaces:

    GigabitEthernet0/1.10 → VLAN 10
    GigabitEthernet0/1.20 → VLAN 20
    GigabitEthernet0/1.30 → VLAN 30
    GigabitEthernet0/1.40 → VLAN 40

Each subinterface uses 802.1Q encapsulation.

For example:

    encapsulation dot1Q 10

This associates the subinterface with VLAN 10.

R2 uses these subinterfaces as the default gateways and routes traffic between the VLANs.

## Traffic Flow

When PC1 communicates with PC2:

    PC1
    192.168.10.10
        |
    VLAN 10
        |
    SW2
        |
    Trunk
        |
    SW1
        |
    Trunk
        |
    R2 Gi0/1.10
    192.168.10.1
        |
    Layer 3 Routing
        |
    R2 Gi0/1.20
    192.168.20.1
        |
    Trunk
        |
    SW1
        |
    Trunk
        |
    SW2
        |
    VLAN 20
        |
    PC2
    192.168.20.20

R2 performs the Layer 3 routing between VLAN 10 and VLAN 20.

## Verification

Check the router interfaces:

    show ip interface brief

Expected:

    GigabitEthernet0/1       up    up
    GigabitEthernet0/1.10    up    up
    GigabitEthernet0/1.20    up    up
    GigabitEthernet0/1.30    up    up
    GigabitEthernet0/1.40    up    up

Check a subinterface:

    show running-config interface GigabitEthernet0/1.10

Expected:

    interface GigabitEthernet0/1.10
     encapsulation dot1Q 10
     ip address 192.168.10.1 255.255.255.0

Check the switch trunks:

    show interfaces trunk

VLANs 10, 20, 30, and 40 should be active on the trunks.

## Connectivity Testing

From PC1, test the local gateway:

    ping 192.168.10.1

Test another VLAN:

    ping 192.168.20.20

Test VLAN 30:

    ping 192.168.30.30

Test VLAN 40:

    ping 192.168.40.40

   <img width="1904" height="507" alt="image" src="https://github.com/user-attachments/assets/92187ad4-0da1-4375-b50f-57af60d421cd" />
 

Successful replies confirm that Inter-VLAN Routing is working.

## Troubleshooting

If Inter-VLAN Routing does not work, troubleshoot step by step.

### 1. Check the PC IP Configuration

Verify:

    IP address
    Subnet mask
    Default gateway

Example:

    PC1
    IP:      192.168.10.10
    Mask:    255.255.255.0
    Gateway: 192.168.10.1

### 2. Test the Default Gateway

From PC1:

    ping 192.168.10.1

If this fails, check the VLAN, access port, trunk, and router subinterface.

### 3. Check VLANs

On the switches:

    show vlan brief

Verify that the access ports are assigned to the correct VLANs.

### 4. Check Trunk Status

    show interfaces trunk

Verify that VLANs 10, 20, 30, and 40 are active on the trunks.

### 5. Check Router Subinterfaces

    show ip interface brief

The required subinterfaces should be up/up.

### 6. Check 802.1Q Configuration

    show running-config interface GigabitEthernet0/1.10

Verify:

    encapsulation dot1Q 10

The VLAN ID must match the VLAN.

## Important Design Note

This lab uses Router-on-a-Stick because the switches are Layer 2 switches.

In a larger enterprise network, Inter-VLAN Routing is commonly performed by a Layer 3 switch using SVIs.

Example:

    interface vlan 10
     ip address 192.168.10.1 255.255.255.0

Router-on-a-Stick is useful for understanding VLANs, trunks, 802.1Q, subinterfaces, and Layer 3 routing.

## Result

Inter-VLAN Routing was successfully configured using Router-on-a-Stick.

The lab supports communication between:

    VLAN 10
    VLAN 20
    VLAN 30
    VLAN 40

R2 provides the default gateway for each VLAN and performs Layer 3 routing between the VLANs.
