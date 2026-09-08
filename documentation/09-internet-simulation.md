# Task 09 — Internet Simulation

## Objective

In this task, we simulate Internet connectivity and practice default routing.

The main goals are:

- Configure an external connection.
- Configure default routes.
- Verify Internet connectivity.
- Understand return traffic.
- Understand why NAT is needed for private IP addresses.

---

## Topology

    PC
     |
    SW2
     |
    SW1
     |
    R2
     |
    R1
     |
    R3
     |
    Cloud0
     |
    Internet

R3 is the edge router connected to the external network.

---

## Addressing

| Device | Interface | IP Address |
|---|---|---|
| R1 | Gi0/0 | 10.0.1.1/30 |
| R3 | Gi0/1 | 10.0.1.2/30 |
| R3 | Gi0/0 | 192.168.1.130/24 |
| Gateway | — | 192.168.1.1 |

---

## Configure R3 External Interface

R3 Gi0/0 was connected to the EVE-NG Cloud0 network.

    R3# configure terminal
    R3(config)# interface gi0/0
    R3(config-if)# ip address 192.168.1.130 255.255.255.0
    R3(config-if)# no shutdown
    R3(config-if)# end

Verify:

    R3# show ip interface brief

The interface should be `up/up`.

---

## Configure R3 Default Route

R3 needs a default route to reach external networks.

    R3# configure terminal
    R3(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1
    R3(config)# end

Verify:

    R3# show ip route

Expected:

    S* 0.0.0.0/0 [1/0] via 192.168.1.1

---

## Verify R3 Internet Access

Test the external gateway:

    R3# ping 192.168.1.1

Test Internet connectivity:

    R3# ping 8.8.8.8

Both tests were successful.

This confirms that R3 can reach the external network and the Internet.

---

## Configure R1 Default Route

R1 needs to send unknown traffic to R3.

    R1# configure terminal
    R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.1.2
    R1(config)# end

Verify:

    R1# show ip route

Expected:

    S* 0.0.0.0/0 [1/0] via 10.0.1.2

---

## Test R1 Connectivity

R1 can reach R3:

    R1# ping 10.0.1.2

Result:

    Success rate is 100 percent

However, this test failed:

    R1# ping 8.8.8.8

Result:

    Success rate is 0 percent

---

## Why Does R1 Fail?

R1 uses a private IP address such as:

    10.0.1.1

The Internet does not have a return route to this private network.

The traffic path is:

    R1 → R3 → Internet

But the return traffic cannot reach:

    10.0.1.0/30

This shows that a default route alone is not enough for Internet connectivity.

---

## NAT and Return Traffic

NAT can translate a private source address into an external address.

Example:

    Private IP
        ↓
       NAT
        ↓
    External IP
        ↓
    Internet

NAT/PAT will be configured in a later task.

---

## Troubleshooting

During the lab, R3 had an old default route from the previous task.

The routing table showed two possible default routes.

The old route was removed:

    R3(config)# no ip route 0.0.0.0 0.0.0.0 10.0.1.1

After that, R3 used:

    S* 0.0.0.0/0 via 192.168.1.1

This shows why checking the routing table is important when troubleshooting.

---

## Verification

| Test | Result |
|---|---|
| R3 → 192.168.1.1 | PASS |
| R3 → 8.8.8.8 | PASS |
| R1 → R3 | PASS |
| R1 → 8.8.8.8 | FAIL — return path/NAT required |

---

## Key Points

- A default route sends unknown traffic to a next-hop router.
- Internet traffic needs a return path.
- Private IP addresses cannot normally be used directly on the public Internet.
- NAT/PAT allows private networks to access external networks.
- The routing table is an important troubleshooting tool.

---

## Final Status

Task 09 — Internet Simulation: **Completed**
<img width="807" height="229" alt="image" src="https://github.com/user-attachments/assets/85580562-7c13-4cbb-99f5-32aea1a675f9" />


