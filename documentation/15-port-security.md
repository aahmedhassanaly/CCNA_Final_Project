# Task 15 — Port Security

## Objective

Configure and verify Port Security on a switch access port.

The goal is to allow only one known MAC address and block unauthorized devices.

## Configuration

Port used:

    SW2 Et0/1

The port is configured as an access port in VLAN 10.

Port Security configuration:

    interface ethernet 0/1
     switchport port-security
     switchport port-security maximum 1
     switchport port-security mac-address sticky

The switch learned the MAC address of PC1:

    0050.7966.680b

The MAC address was saved as a Sticky MAC.

## Verification

Command:

    show port-security interface ethernet 0/1

Initial secure state:

    Port Security              : Enabled
    Port Status                : Secure-up
    Maximum MAC Addresses      : 1
    Sticky MAC Addresses       : 1
    Security Violation Count   : 0

The port allows only one secure MAC address.

## Security Violation Test

PC2 was connected to the same access port.

PC2 used a different MAC address:

    0050.7966.6801

The ping from PC2 failed because its MAC address was not allowed.

Port Security then detected a violation.

Verification showed:

    Port Status                : Secure-shutdown
    Violation Mode             : Shutdown
    Last Source Address:Vlan   : 0050.7966.6801:10
    Security Violation Count   : 1

The interface entered the `err-disabled` state.
<img width="849" height="529" alt="image" src="https://github.com/user-attachments/assets/9eecf781-c179-40a8-a5bb-f4346fe6fd7d" />


## Recovery

The port was recovered with:

    interface ethernet 0/1
     shutdown
     no shutdown

The interface returned to:

    up/up

## Key Points

- Port Security controls which MAC addresses can use an access port.
- Sticky MAC dynamically learns and saves a MAC address.
- Maximum MAC addresses limits the number of allowed devices.
- Shutdown mode places the port into an err-disabled state after a violation.
- A different MAC address can trigger a security violation.

## Verification Commands

    show port-security interface ethernet 0/1
    show running-config interface ethernet 0/1
    show mac address-table interface ethernet 0/1
    show interfaces ethernet 0/1

## Final Status

Task 15 — Port Security: **Completed**
