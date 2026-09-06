# Task 05 — Trunking

## Objective

Configure trunk links between the access switches and the router.

The trunk links will carry traffic for multiple VLANs.

This configuration is required before configuring Inter-VLAN Routing.

---

## Lab Platform

- EVE-NG
- Cisco IOL L2 Switches
- Cisco vIOS Routers

---

## Trunk Links

| Device | Interface | Connected To | Mode |
|---|---|---|---|
| SW1 | Et0/0 | R2 | Trunk |
| SW2 | Et0/0 | R2 | Trunk |

The following VLANs will use the trunk links:

- VLAN 10 — Administration
- VLAN 20 — Sales
- VLAN 30 — IT
- VLAN 40 — Servers

---

## SW1 Configuration

Configure Et0/0 as a trunk port.

    enable
    configure terminal

    interface ethernet 0/0
     switchport trunk encapsulation dot1q
     switchport mode trunk
    exit

    end

---

## SW2 Configuration

Configure Et0/0 as a trunk port.

    enable
    configure terminal

    interface ethernet 0/0
     switchport trunk encapsulation dot1q
     switchport mode trunk
    exit

    end

---

## Verification

### SW1

Check the trunk configuration:

    show interfaces ethernet 0/0 switchport

Expected result:

    Administrative Mode: trunk
    Operational Mode: trunk
    Administrative Trunking Encapsulation: dot1q
    Operational Trunking Encapsulation: dot1q

### SW2

Check the trunk configuration:

    show interfaces ethernet 0/0 switchport

Expected result:

    Administrative Mode: trunk
    Operational Mode: trunk
    Administrative Trunking Encapsulation: dot1q
    Operational Trunking Encapsulation: dot1q

---

## Current Trunk Settings

The trunk ports are using:

- Trunk Mode: Enabled
- Encapsulation: 802.1Q (dot1q)
- Native VLAN: VLAN 1
- Trunk Negotiation: On

The native VLAN was not changed at this stage.

Security hardening for trunk ports can be reviewed later if needed.

---

## Task Result

Task 05 is completed.

SW1 Et0/0 and SW2 Et0/0 are operating as 802.1Q trunk ports.

The switches are now ready for the Inter-VLAN Routing configuration.

<img width="1517" height="279" alt="image" src="https://github.com/user-attachments/assets/1a99cbf2-9734-4983-bde7-efc57c7afc6c" />

<img width="1584" height="270" alt="image" src="https://github.com/user-attachments/assets/d3503e8a-01f2-4633-9802-7d2b29247c2f" />

---

## Next Task

**Task 06 — Inter-VLAN Routing**

In the next task, R2 will use Router-on-a-Stick with subinterfaces for VLAN 10, VLAN 20, VLAN 30, and VLAN 40.

End devices will also be added to the topology and used for connectivity testing.
