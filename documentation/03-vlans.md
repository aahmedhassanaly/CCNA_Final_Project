# Task 03 — VLANs

## Objective

Create the VLANs that will be used in the network.

VLANs are used to separate network traffic into different logical networks.

---

## VLAN Plan

The lab uses four VLANs:

| VLAN | Name | Department |
|---|---|---|
| 10 | admin | Administration |
| 20 | sales | Sales |
| 30 | it | IT |
| 40 | servers | Servers |

---

## Switches

The VLANs were created on:

- SW1
- SW2

Both switches use the same VLAN IDs and names.

---

## VLAN Configuration

The following VLANs were created on the switches:

    vlan 10
     name admin

    vlan 20
     name sales

    vlan 30
     name it

    vlan 40
     name servers

---

## Verification

The following command was used to check the VLANs:

    show vlan brief

The result showed:

    10   admin       active
    20   sales       active
    30   it          active
    40   servers     active

All four VLANs are active on SW1 and SW2.

---

## Current Port State

The switch ports are still in VLAN 1.

No access ports have been assigned to the new VLANs yet.

This is expected because access port configuration is part of the next task.

---

## Task Result

Task 03 was completed successfully.

<img width="1917" height="380" alt="image" src="https://github.com/user-attachments/assets/436cfec5-a8a4-4f67-9f5b-fc0d4542ffa3" />


<img width="1424" height="457" alt="image" src="https://github.com/user-attachments/assets/a8fe9d6f-a161-4651-8789-25f5a0d6d0e1" />

We created and verified:

- VLAN 10 — Administration
- VLAN 20 — Sales
- VLAN 30 — IT
- VLAN 40 — Servers

The same VLAN structure is now available on SW1 and SW2.

---

## Next Task

Task 04 — Access Ports

In the next task, switch ports will be assigned to the correct VLANs for end devices.
