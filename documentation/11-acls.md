# Task 11 — ACLs

## Objective

Configure an Extended ACL to block VLAN 20 from reaching VLAN 40 while allowing other traffic.

## ACL Configuration

R2:

    ip access-list extended vlan-20-block-vlan40
    deny ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
    permit ip any any

    interface gigabitEthernet 0/1.20
    ip access-group vlan-20-block-vlan40 in

## Why?

The ACL is applied inbound on `Gi0/1.20` because this is where VLAN 20 traffic enters R2.

The Extended ACL checks both source and destination networks.

## Verification

Check the ACL:

    show ip access-lists vlan-20-block-vlan40

Test from PC2:

    ping 192.168.40.11

Result:

    FAIL — Communication administratively prohibited

This confirms that VLAN 20 cannot reach VLAN 40.

Test VLAN 20 to VLAN 30:

    ping 192.168.30.11

Result:

<img width="1097" height="573" alt="image" src="https://github.com/user-attachments/assets/eebf7a66-9173-42ca-93f7-b22d51bc781a" />

 
<img width="1123" height="509" alt="image" src="https://github.com/user-attachments/assets/0118b9b6-214b-4594-bba5-f1749b0aa3e5" />

This confirms that other traffic is still allowed.

## Key Points

- Extended ACLs can filter source and destination traffic.
- ACL direction is important.
- `in` checks traffic entering the interface.
- `permit ip any any` allows other traffic.
- ACLs have an implicit deny at the end.

