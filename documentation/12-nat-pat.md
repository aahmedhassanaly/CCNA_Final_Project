# Task 12 — NAT/PAT

## Objective

Configure PAT to allow internal private networks to access the Internet.

## NAT Interfaces

R3:

    Gi0/1 → NAT inside
    Gi0/0 → NAT outside

## NAT ACL

The internal networks were selected for NAT:

    access-list 1 permit 192.168.10.0 0.0.0.255
    access-list 1 permit 192.168.20.0 0.0.0.255
    access-list 1 permit 192.168.30.0 0.0.0.255
    access-list 1 permit 192.168.40.0 0.0.0.255

This ACL is used by NAT to identify traffic that should be translated.

## PAT Configuration

On R3:

    ip nat inside source list 1 interface gigabitEthernet 0/0 overload

`overload` enables PAT, allowing multiple internal devices to share the IP address of R3 Gi0/0.

## Routing

R2 needed a default route:

    ip route 0.0.0.0 0.0.0.0 10.0.0.1

R3 needed routes back to the internal VLANs through R1.

## Verification

From PC2:

    ping 8.8.8.8

Result:
<img width="1138" height="523" alt="image" src="https://github.com/user-attachments/assets/e4b5c495-111a-4043-a215-309fe40eebac" />


NAT translations were verified on R3:

    show ip nat translations

Example:

    Inside local: 192.168.20.11
    Inside global: 192.168.1.130
    Destination: 8.8.8.8

<img width="1037" height="515" alt="image" src="https://github.com/user-attachments/assets/d2f6410d-c407-4cb1-b21f-09f628d98f0f" />

This confirms that the private IP was translated to the R3 outside interface IP.

## Key Points

- NAT translates private IP addresses.
- PAT allows multiple devices to share one IP.
- `inside` and `outside` define the NAT boundaries.
- The NAT ACL identifies networks that need translation.
- Routing must be correct before NAT can work.


### Next Task

Task 13 — STP
