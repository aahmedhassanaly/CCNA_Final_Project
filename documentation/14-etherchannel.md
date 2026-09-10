# Task 14 — EtherChannel

## Objective

Configure an LACP EtherChannel between SW1 and SW2.

## Configuration

Two links were used between SW1 and SW2:

    SW1 Et1/2 ↔ SW2 Et1/2
    SW1 Et1/3 ↔ SW2 Et1/3

LACP was configured using:

    interface range ethernet 1/2 - 3
    channel-group 1 mode active

The member interfaces were configured as trunks.

## Troubleshooting

Initially, one link was suspended because the interfaces had different Layer 2 settings.

Verification showed:

    Et1/2 - access
    Et1/3 - trunk

The interfaces were corrected to use 802.1Q trunking.

After fixing the mismatch, both links joined the EtherChannel.

## Verification

Command:

    show etherchannel summary

Result:

    Po1(SU)  LACP
    Et1/2(P)
    Et1/3(P)

`P` means the interface is bundled in the Port-Channel.

Command:

    show interfaces port-channel 1

Result:

    Port-channel1 is up, line protocol is up
    Members in this channel: Et1/2 Et1/3

This confirms that both physical links are working as one logical EtherChannel.

## Key Points

- EtherChannel combines multiple physical links into one logical link.
- LACP is used to negotiate the EtherChannel.
- Member interfaces must have compatible Layer 2 settings.
- Both links are now members of Port-Channel 1.
- EtherChannel provides redundancy and more bandwidth.

## Verification Commands

    show etherchannel summary
    show interfaces port-channel 1
<img width="812" height="637" alt="image" src="https://github.com/user-attachments/assets/c557fbf3-5b49-4751-8cef-f33352f29af3" />

