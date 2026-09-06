# Task 04 — Access Ports

## Objective

Assign switch ports to the correct VLAN.

Access ports are used to connect end devices to the network.

Each access port belongs to one VLAN.

---

## Port Plan

The following ports are used on SW1 and SW2.

| Port | Department | VLAN |
|---|---|---|
| Et0/1 | Administration | 10 |
| Et0/2 | Sales | 20 |
| Et0/3 | IT | 30 |
| Et1/0 | Servers | 40 |

The same port plan is used on both switches.

---

## SW1

The following ports were configured:

    Et0/1 → VLAN 10
    Et0/2 → VLAN 20
    Et0/3 → VLAN 30
    Et1/0 → VLAN 40

Each port was configured as an access port.

---

## SW2

The following ports were configured:

    Et0/1 → VLAN 10
    Et0/2 → VLAN 20
    Et0/3 → VLAN 30
    Et1/0 → VLAN 40

Each port was configured as an access port.

---

## Access Port Configuration

Example configuration:

    interface ethernet 0/1
     switchport mode access
     switchport access vlan 10

The same method was used for the other access ports with their correct VLAN IDs.

---

## Verification

The following command was used:

    show interfaces status

### SW1 Result

    Et0/1    connected    10
    Et0/2    connected    20
    Et0/3    connected    30
    Et1/0    connected    40

### SW2 Result

    Et0/1    connected    10
    Et0/2    connected    20
    Et0/3    connected    30
    Et1/0    connected    40

The results show that the ports are in the correct VLANs.

---

## Uplink Port

`Et0/0` was not configured as an access port.

It is connected to R2 and will be used as a trunk port.

The trunk configuration will be done in the next task.

---

## Unused Ports

Some switch ports are not used yet.

They remain in the default VLAN for now.

Port security and unused port configuration will be considered later when the network design is complete.

---

## Task Result

Task 04 was completed successfully.

<img width="1285" height="327" alt="image" src="https://github.com/user-attachments/assets/78154cdd-3de6-4b1f-a150-36af221fffd8" />

<img width="1851" height="350" alt="image" src="https://github.com/user-attachments/assets/083aac4e-b0bd-4617-bf62-448bcf6b7529" />


We configured access ports for:

- Administration
- Sales
- IT
- Servers

The ports were verified using `show interfaces status`.

---

## Next Task

Task 05 — Trunking

The next task will configure the links between R2 and the switches as 802.1Q trunk links.

The trunk links will carry VLAN 10, 20, 30, and 40.
