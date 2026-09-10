# Task 13 — STP

## Objective

Configure and verify Spanning Tree Protocol (STP) to prevent Layer 2 loops while keeping a backup path.

## Topology

Two links were configured between SW1 and SW2 to create Layer 2 redundancy.

    SW1
    |  \
    |   \
    |    \
    SW2

SW1 became the Root Bridge.

## STP Verification

On SW1:

    show spanning-tree

Result:

    This bridge is the root

All active SW1 ports were Designated and Forwarding. 
<img width="824" height="518" alt="image" src="https://github.com/user-attachments/assets/a639f2e5-8463-4b03-9f59-cc511da67941" />


On SW2:

    show spanning-tree

Initial result:

    Et1/2  Root  FWD
    Et1/3  Altn  BLK

This shows that:

- Et1/2 is the Root Port.
- Et1/3 is the Alternate Port.
- STP blocked Et1/3 to prevent a Layer 2 loop.
- <img width="858" height="500" alt="image" src="https://github.com/user-attachments/assets/5aae92ed-2292-47e3-a326-683b2f3b3ec3" />


## Failure Test

The primary link was disabled on SW1:

    interface ethernet 1/2
    shutdown

After the failure, SW2 changed the backup link:

    Et1/3  Root  FWD

This confirms that STP automatically selected the backup path.

The failed link was then restored:

    interface ethernet 1/2
    no shutdown

SW2 returned to:

    Et1/2  Root  FWD
    Et1/3  Altn  BLK

## Key Points

- STP prevents Layer 2 loops.
- The Root Bridge is selected using the lowest Bridge ID.
- Root Port is the best path to the Root Bridge.
- Alternate ports can be placed in Blocking state.
- STP keeps redundant links available as backup paths.
- STP can change the active path after a link failure.

## Verification Commands

    show spanning-tree
    show spanning-tree vlan 1

