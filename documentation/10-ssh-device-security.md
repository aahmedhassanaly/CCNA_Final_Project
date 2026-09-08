# Task 10 — SSH and Device Security

## Objective

In this task, we secure R1 and configure SSH for remote management.

The main goals are:

- Create a local administrator account.
- Generate RSA keys.
- Enable SSH Version 2.
- Secure VTY lines.
- Secure privileged EXEC mode.
- Secure console access.

---

## Why It Matters

Network devices should not be managed using insecure remote protocols such as Telnet.

SSH provides encrypted remote management.

Local authentication can be used for small environments and lab networks.

---

## Local User

A local user is an account stored directly on the Cisco device.

We created:

    username admin privilege 15 secret <lab-password>

The `admin` user has privilege level 15.

---

## SSH Configuration

The router was configured with:

- Hostname
- Domain name
- Local user
- RSA 2048-bit key
- SSH Version 2

Verification:

    R1# show ip ssh

Expected:

    SSH Enabled - version 2.0

---

## VTY Lines

VTY lines are virtual terminal lines used for remote management.

The configuration was applied to VTY lines 0 through 4:

    line vty 0 4
     login local
     transport input ssh

### Configuration Meaning

`line vty 0 4`

Selects VTY lines 0 through 4 for configuration.

`login local`

Uses the local username database for authentication.

`transport input ssh`

Allows SSH remote access and prevents Telnet access on these lines.

---

## Enable Secret

Privileged EXEC mode was protected using:

    enable secret <lab-password>

This protects access to privileged mode after using:

    enable

---

## Console Security

The console line was configured to use local authentication:

    line console 0
     login local

The console provides direct local access to the router, while VTY lines provide remote access.

---

## Verification

The following commands were used:

    R1# show ip ssh
    R1# show ssh
    R1# show running-config | section line vty
    R1# show running-config | section line con
    R1# show running-config | include enable secret

The SSH configuration was verified successfully.

`show ssh` showed no active SSH sessions because the VPCS device does not provide an SSH client.

Network connectivity was tested from the VPCS:

    VPCS> ping 10.0.1.1

The ping to R1 was successful.

---

## Troubleshooting

During verification, the VPCS returned:

    Bad command: "ssh -l admin 10.0.1.1"

The problem was not the R1 SSH configuration.

The VPCS image used in this EVE-NG lab does not provide the required SSH client command.

The network path was then tested separately:

    VPCS> ping 10.0.1.1

The ping was successful, proving that connectivity from the client network to R1 was working.

A real SSH client such as a Linux or Windows system can be used later for a complete end-to-end SSH login test.

---

## Key Points

- Console access is direct local access to the device.
- VTY lines are virtual lines used for remote access.
- SSH provides encrypted remote management.
- `login local` uses the local user database.
- `transport input ssh` allows SSH instead of Telnet.
- `enable secret` protects privileged EXEC mode.
- RSA keys are required for SSH server operation.

---

## Security Status

| Security Feature | Status |
|---|---|
| Local User | Completed |
| RSA Key | Completed |
| SSH Version 2 | Completed |
| VTY SSH Access | Completed |
| Enable Secret | Completed |
| Console Authentication | Completed |
| End-to-End SSH Client Test | Pending |

---

## Documentation Screenshots

Capture these real screenshots:

1. `show ip ssh`
   - Proves SSH Version 2 is enabled.

2. `show running-config | section line vty`
   - Proves VTY lines use local authentication and SSH only.

3. `show running-config | section line con`
   - Proves console authentication is configured.

4. Successful `ping 10.0.1.1` from VPCS
   - Proves network connectivity to R1.

Do not create fake SSH output. The final SSH login test should be documented after a real SSH client is available.

---

## Final Status

Task 10 — SSH and Device Security: **Completed with SSH client test pending**
<img width="820" height="556" alt="image" src="https://github.com/user-attachments/assets/3a148334-6823-418d-8110-733bcd326469" />
