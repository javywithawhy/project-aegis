# Kali Linux Service and Listener Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Validation Date | 2026-07-26 |
| Source Asset | `PASL-VM-001` — `aegis-lab-kali-01` |
| Status | Complete |

## Objective

Validate the Kali Linux guest identity, security-relevant service state, selected package versions, and listening network services without collecting local IP addresses, MAC addresses, credentials, or process identifiers.

## Validated System Identity

| Field | Observed Value |
|---|---|
| Hostname | `aegis-lab-kali-01` |
| Operating system | Kali GNU/Linux Rolling |
| Kernel | `7.0.12+kali-amd64` |

## Security-Relevant Service Status

| Service | Purpose | Observed Status |
|---|---|---|
| `qemu-guest-agent` | Proxmox guest integration and management support | Active |
| `NetworkManager` | Guest network configuration and connectivity | Active |
| `ssh` | Remote shell service | Inactive |
| `cron` | Scheduled task execution | Active |
| `rsyslog` | Traditional system logging service | Not installed |
| `auditd` | Linux audit framework daemon | Not installed |
| `ufw` | Host firewall management service | No service unit installed |
| `firewalld` | Dynamic host firewall service | Not installed |

## Validated Package Versions

| Package | Version | Role |
|---|---|---|
| `qemu-guest-agent` | `1:11.0.1+ds-1` | Hypervisor guest integration |
| `openssh-server` | `1:10.3p1-5` | Installed remote administration capability; service inactive |
| `nmap` | `7.99+dfsg-1kali1` | Authorized network discovery and assessment tool |

The package query returned an incomplete `ufw` entry without a validated version. Because the `ufw` service unit was not present and no listener was observed, UFW is not treated as an active managed service in this evidence.

## Listening-Service Validation

The sanitized listener review returned:

```text
No listening TCP or UDP services found.
```

This means no TCP or UDP server process was listening at the time of validation. In particular, the installed OpenSSH Server package was not exposing TCP port 22 because the `ssh` service was inactive.

## Security Interpretation

- The Kali VM currently presents no listening TCP or UDP services, reducing its inbound network attack surface.
- The QEMU Guest Agent and NetworkManager are active and support expected VM operation.
- OpenSSH Server is installed but inactive. Any future activation should be documented and followed by access-control and listener-scope validation.
- `auditd` and `rsyslog` are not installed. Their absence should be evaluated during later logging, auditing, and continuous-monitoring control implementation.
- No host firewall management service was active inside Kali at the time of validation. The VM network interface has Proxmox firewall support enabled, but guest-level firewall requirements remain a separate control decision.
- Nmap is installed as an authorized assessment tool and must only be used against Project Aegis assets or other explicitly authorized systems.

## Evidence Handling

The validation recorded service names, package versions, and listener results. Local addresses, credentials, MAC addresses, and transient process identifiers were excluded from the public evidence.