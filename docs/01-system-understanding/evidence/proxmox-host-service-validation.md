# Proxmox Host Service Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Validation Date | 2026-07-26 |
| Source System | `PASL-HW-001` — Proxmox host `proxmox` |
| Status | Validated |

## Objective

Identify active Proxmox management, virtualization, firewall, scheduling, and remote-administration services; record relevant package versions; and establish a sanitized listening-port and bind-scope baseline.

## Commands Executed

The following read-only command groups were executed from the Proxmox host shell:

```bash
systemctl is-active <service>
systemctl list-units --type=service --state=running
dpkg-query -W
ss -lntupH
```

A local Python classification command was then used to categorize each retained listener as loopback-only, private-network, link-local, wildcard, or other/public without displaying the actual address.

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

| Protocol | Port | Observed Process or Service | Preliminary Purpose | Validated Bind Scope |
|---|---:|---|---|---|
| TCP | 22 | `sshd` | SSH remote administration | Wildcard — all host interfaces |
| TCP | 25 | Postfix `master` | Local mail transport and system notifications | Loopback only |
| TCP | 85 | `pvedaemon` | Proxmox API daemon communication | Loopback only |
| TCP | 111 | `rpcbind` | RPC service mapping | Wildcard — all host interfaces |
| TCP | 3128 | `spiceproxy` | SPICE console proxy | Wildcard — all host interfaces |
| TCP | 8006 | `pveproxy` | Proxmox web management interface | Wildcard — all host interfaces |
| UDP | 111 | `rpcbind` | RPC service mapping | Wildcard — all host interfaces |
| UDP | 323 | `chronyd` | Local time-synchronization command interface | Loopback only |

## Bind-Scope Interpretation

A wildcard listener accepts traffic addressed to any active interface on the Proxmox host. It does not by itself prove that the service is reachable from the internet. Actual reachability also depends on host firewall rules, upstream router configuration, network segmentation, and routing.

Loopback-only listeners accept connections only from the Proxmox host itself and are not directly reachable from other home-network devices.

## Security Interpretation

- The Proxmox management plane, VM event handling, scheduler, firewall service, SPICE proxy, and SSH service were operational at the time of validation.
- TCP port `8006` is the Proxmox web-management service and must remain restricted to trusted internal access.
- TCP port `3128` supports SPICE console access and should be treated as an administrative service.
- SSH on TCP port `22` is bound to all host interfaces. This may be acceptable for a trusted management network but should later be restricted through firewall rules, network segmentation, or service configuration as appropriate.
- `rpcbind` on TCP and UDP port `111` is bound to all host interfaces. Its operational necessity should be reviewed because an unnecessary wildcard RPC listener increases attack surface.
- Postfix TCP port `25`, `pvedaemon` TCP port `85`, and `chronyd` UDP port `323` are loopback-only and therefore locally scoped.
- An active `pve-firewall` service does not by itself prove that firewall enforcement or rules are enabled. Firewall policy state must be assessed separately.
- No evidence from this validation indicates intentional internet port forwarding. The home router remains an external dependency outside the Project Aegis boundary.

## Follow-Up Items

- Validate Proxmox firewall enforcement and effective policy separately.
- Determine whether wildcard `rpcbind` exposure is required.
- Confirm that SSH, SPICE, and Proxmox web-management access are restricted to trusted systems or networks.

## Evidence Handling

Process identifiers were omitted because they are transient. IP addresses were classified and removed rather than copied into the public evidence. The evidence records service names, package versions, protocols, ports, bind scopes, and security-relevant findings without publishing local addressing details.