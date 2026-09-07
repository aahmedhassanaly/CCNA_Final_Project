# Task 08 — Static & Default Routing

## Objective

Configure static and default routes between R2, R1, and R3.

The goal is to allow traffic to move between the internal VLAN networks and the simulated ISP network.

---

## Topology

    VLANs
      |
     SW1
      |
     R2
      |
     R1
      |
     R3 (ISP)

---

## Network Addressing

| Device | Interface | IP Address |
|---|---|---|
| R2 | Gi0/0 | 10.0.0.2/30 |
| R1 | Gi0/1 | 10.0.0.1/30 |
| R1 | Gi0/0 | 10.0.1.1/30 |
| R3 | Gi0/1 | 10.0.1.2/30 |

Internal VLAN networks:

| VLAN | Network | Gateway |
|---|---|---|
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | 192.168.40.0/24 | 192.168.40.1 |

---

## Static Routing on R2

R2 is directly connected to R1 through:

    R2 Gi0/0 = 10.0.0.2
    R1 Gi0/1 = 10.0.0.1

R2 does not know the network `10.0.1.0/30`, which is behind R1.

A static route was added:

    R2(config)# ip route 10.0.1.0 255.255.255.252 10.0.0.1

This tells R2 to send traffic for `10.0.1.0/30` to R1.

### Verification

    R2# show ip route

Expected result:

    S 10.0.1.0/30 [1/0] via 10.0.0.1

`S` means Static Route.

---

## Static Routing on R1

R1 is directly connected to R2 and R3, but it does not know the internal VLAN networks behind R2.

Static routes were added for the four VLAN networks:

    R1(config)# ip route 192.168.10.0 255.255.255.0 10.0.0.2
    R1(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2
    R1(config)# ip route 192.168.30.0 255.255.255.0 10.0.0.2
    R1(config)# ip route 192.168.40.0 255.255.255.0 10.0.0.2

The next-hop is `10.0.0.2`, which is R2.

### Verification

    R1# show ip route

Expected result:

    S 192.168.10.0/24 [1/0] via 10.0.0.2
    S 192.168.20.0/24 [1/0] via 10.0.0.2
    S 192.168.30.0/24 [1/0] via 10.0.0.2
    S 192.168.40.0/24 [1/0] via 10.0.0.2

---

## R3 Interface Configuration

R3 represents the ISP in this lab.

The connection between R1 and R3 uses:

    R1 Gi0/0 = 10.0.1.1
    R3 Gi0/1 = 10.0.1.2

R3 Gi0/1 was configured and enabled:

    R3(config)# interface GigabitEthernet0/1
    R3(config-if)# ip address 10.0.1.2 255.255.255.252
    R3(config-if)# no shutdown

Verification:

    R3# show ip interface brief

Expected result:

    GigabitEthernet0/1    10.0.1.2    up    up

---

## Default Route on R3

R3 is used as the ISP side of the lab.

Instead of adding a separate route for every internal network, a default route was configured.

    R3(config)# ip route 0.0.0.0 0.0.0.0 10.0.1.1

This tells R3 to send unknown destinations to R1.

### Verification

    R3# show ip route

Expected result:

    Gateway of last resort is 10.0.1.1 to network 0.0.0.0

    S* 0.0.0.0/0 [1/0] via 10.0.1.1

`S*` means the route is a static default route.

---

## Routing Flow

Traffic from an internal VLAN to R3 follows this path:

    VLAN Client
        ↓
    R2
        ↓
    R1
        ↓
    R3

For example, traffic from VLAN 10 to R3:

    192.168.10.11
        ↓
    192.168.10.1 (R2)
        ↓
    10.0.0.1 (R1)
        ↓
    10.0.1.2 (R3)

The return traffic needs a valid return path as well.

---

## Verification

The routing configuration was tested in both directions.

From R2:

    R2# ping 10.0.1.2

From R3:

    R3# ping 192.168.10.1

The tests confirmed connectivity between the internal router and the simulated ISP router.

---

## Troubleshooting Approach

When routing fails, do not immediately add more routes.

Use this process:

### 1. Problem

Identify the destination that cannot be reached.

### 2. Evidence

Check the routing table:

    show ip route

Check the interface status:

    show ip interface brief

Test the next-hop:

    ping <next-hop>

### 3. Hypothesis

Possible problems include:

- Missing route
- Wrong next-hop
- Down interface
- Wrong IP address
- Missing return route

### 4. Test

Test each hop separately to find where the traffic stops.

### 5. Fix

Correct the confirmed routing or interface problem.

### 6. Verify

Repeat the ping and check the routing table again.

---

## Key Concepts

### Static Route

A static route is manually configured by an administrator for a specific destination network.

### Default Route

A default route is used when there is no more specific route for the destination.

### Next-Hop

The next-hop is the IP address of the router that should receive the packet next.

### Return Path

The destination must also know how to send the reply back.

A route in only one direction is not enough for successful end-to-end communication.

---

## Real-World Relevance

Static routes are useful for:

- Small networks
- Simple point-to-point links
- Specific network paths
- Backup routes

Default routes are commonly used when a router has one main path to external networks, such as an ISP or upstream router.

In larger networks, dynamic routing protocols are often used instead of many manual static routes.

---

## Final Verification

- [x] R2 has a route to `10.0.1.0/30`
- [x] R1 has routes to all four VLAN networks
- [x] R3 Gi0/1 is `up/up`
- [x] R3 has a default route
- [x] R2 can reach R3
- [x] R3 can reach the internal router
- [x] Return paths were considered
- [x] Routing tables were verified

---

## Result

Static routing is configured between R2 and R1, and a default route is configured on R3.
<img width="833" height="529" alt="image" src="https://github.com/user-attachments/assets/4aad91c8-57b8-451c-94b6-179f469d9504" />


The lab now has working routing between the internal VLAN environment and the simulated ISP network.

Next task: **Task 09 — Internet Simulation**
