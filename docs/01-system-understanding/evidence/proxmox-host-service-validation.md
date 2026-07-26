# Proxmox Host Service Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Validation Date | 2026-07-26 |
| Source System | `PASL-HW-001` — Proxmox host `proxmox` |
| Status | Validated with follow-up required for listener bind scope |

## Objective

Identify active Proxmox management, virtualization, firewall, scheduling, and remote-administration services; record relevant package versions; and establish a sanitized listening-port baseline.

## Commands Executed

The following read-only command groups were executed from the Proxmox host shell:

```bash
systemctl is-active <service>
systemctl list-units --type=service --state=running
dpkg-query -W
ss -lntupH
```

IP addresses were sanitized before the results were retained. Credentials, MAC addresses, hardware serial numbers, and public IP addresses were not collected.

## Core Service Status

| Service | Purpose | Observed Status |
|---|---|---|
| `pve-cluster` | Proxmox cluster filesystem and configuration database | Active |
| `pvedaemon` | Proxmox API daemon | Active |
| `pveproxy` | Proxmox HTTPS management proxy | Active |
| `pvestatd` | Proxmox node and guest status collection | Active |
| `pve-firewall` | Proxmox firewall service | Active |
| `pvefw-logger` | Proxmox firewall logging | Active |
| `qmeventd` | QEMU virtual-machine event handling | Active |
| `spiceproxy` | SPICE console proxy | Active |
| `ssh` | OpenSSH remote administration | Active |

## Additional Running Proxmox Services

The running-service review also identified:

- `pve-ha-crm` — cluster high-availability resource manager
- `pve-ha-lrm` — local high-availability resource manager
- `pve-lxc-syscalld` — LXC syscall daemon
- `pvescheduler` — Proxmox scheduled-task service

These services are part of the installed Proxmox platform. Their presence does not indicate that a multi-node cluster, high-availability workload, or LXC container is currently configured.

## Validated Package Versions

| Package | Version |
|---|---|
| `pve-manager` | `9.1.1` |
| `qemu-server` | `9.0.30` |
| `pve-firewall` | `6.0.4` |
| `openssh-server` | `1:10.0p1-7` |

## Sanitized Listening-Port Baseline

| Protocol | Port | Observed Process or Service | Preliminary Purpose | Bind-Scope Status |
|---|---:|---|---|---|
| TCP | 22 | `sshd` | SSH remote administration | Requires sanitized scope classification |
| TCP | 25 | Postfix `master` | Local mail transport and system notifications | Requires sanitized scope classification |
| TCP | 85 | `pvedaemon` | Proxmox API daemon communication | Requires sanitized scope classification |
| TCP | 111 | `rpcbind` | RPC service mapping | Requires sanitized scope classification |
| TCP | 3128 | `spiceproxy` | SPICE console proxy | Wildcard listener observed |
| TCP | 8006 | `pveproxy` | Proxmox web management interface | Wildcard listener observed |
| UDP | 111 | `rpcbind` | RPC service mapping | Requires sanitized scope classification |
| UDP | 323 | `chronyd` | Local time-synchronization command interface | Requires sanitized scope classification |

IPv4 and IPv6 listeners were observed for several services. Exact addresses were intentionally removed from the public evidence. A separate sanitized classification is required to distinguish loopback-only, host-LAN, wildcard, and link-local listeners without exposing the actual addresses.

## Security Interpretation

- The Proxmox management plane, VM event handling, scheduler, firewall service, SPICE proxy, and SSH service were operational at the time of validation.
- TCP port `8006` is the Proxmox web-management service and must remain restricted to trusted internal access.
- TCP port `3128` supports SPICE console access and should be treated as an administrative service.
- SSH is active and therefore belongs in the managed software and exposed-service baseline.
- `rpcbind` and Postfix listeners require bind-scope review because unnecessary network exposure would increase attack surface.
- An active `pve-firewall` service does not by itself prove that firewall enforcement or rules are enabled. Firewall policy state must be assessed separately.
- No evidence from this validation indicates intentional internet port forwarding. The home router remains an external dependency outside the Project Aegis boundary.

## Evidence Handling

Process identifiers were omitted because they are transient. IP addresses were replaced with sanitized labels. The evidence records service names, package versions, protocols, ports, and security-relevant findings without publishing local addressing details.