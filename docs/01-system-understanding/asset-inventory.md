# Project Aegis Security Lab — Asset Inventory

| Field | Value |
|---|---|
| System Name | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.6 |
| Status | In Progress |
| Date | 2026-07-26 |
| Authorization Status | Not Authorized — System Definition in Progress |

## 1. Objective

This document identifies and tracks the hardware, virtual machines, storage, networking components, software platforms, and information assets used by Project Aegis Security Lab.

The inventory supports system-boundary definition, configuration management, vulnerability management, security assessment, incident response, change control, and continuous monitoring.

## 2. Inventory Rules

An item is entered in the active inventory when it is deployed, assigned to Project Aegis, and available for use. Planned systems are not treated as active assets until deployment and validation are complete.

Each managed asset receives a stable Project Aegis asset identifier. Asset identifiers should remain associated with the asset throughout its lifecycle, including suspension, retirement, or replacement.

The public inventory must not contain passwords, tokens, private keys, public IP addresses, hardware serial numbers, MAC addresses, personal account names, or unsanitized sensitive findings.

## 3. Asset Identifier Standard

| Prefix | Asset Category | Example |
|---|---|---|
| `PASL-HW` | Physical computing hardware | `PASL-HW-001` |
| `PASL-STO` | Storage device or storage service | `PASL-STO-001` |
| `PASL-NET` | Network interface, bridge, firewall, or network service | `PASL-NET-001` |
| `PASL-VM` | Virtual machine | `PASL-VM-001` |
| `PASL-SW` | Managed software platform or security tool | `PASL-SW-001` |
| `PASL-INF` | Information, documentation, logs, or evidence | `PASL-INF-001` |

## 4. Active Managed Assets

| Asset ID | Category | Asset Name | Description and Role | Location or Host | Network Association | Operational Status | Validation Status |
|---|---|---|---|---|---|---|---|
| `PASL-HW-001` | Physical host | Proxmox virtualization host | ASUS TUF Gaming Laptop FX504 used as the bare-metal platform for Project Aegis virtual machines, virtual networking, and storage services | Privately controlled physical location | Management through `vmbr0` | Operational | Validated — hostname, operating platform, processor, memory, and disks confirmed on 2026-07-26 |
| `PASL-STO-001` | Primary storage | Kingston RBUSNS8154P3256GJ NVMe SSD | Hosts Proxmox system files, LVM storage, and active VM disks | `/dev/nvme0n1` in `PASL-HW-001` | Not directly networked | Operational | Validated — 238.5 GiB NVMe device confirmed on 2026-07-26 |
| `PASL-STO-002` | Secondary storage | Toshiba MQ04ABF100 / `aegis-hdd` | Directory storage for ISO files, backups, templates, archives, and supporting project data | `/dev/sda2` mounted at `/mnt/pve/aegis-hdd` | Available through Proxmox storage services | Operational | Validated — 931.5 GiB SATA device and active Proxmox storage confirmed |
| `PASL-NET-001` | Physical network interface | `nic0` | Physical Ethernet connection supporting Proxmox management and bridged VM connectivity | Installed in `PASL-HW-001` | Member of `vmbr0`; connected to the trusted home network | Operational | Validated — interface up and forwarding through `vmbr0` on 2026-07-26 |
| `PASL-NET-002` | Virtual network bridge | `vmbr0` | Linux bridge providing current Proxmox management, default-route, and VM connectivity | Configured on `PASL-HW-001` | Home-network bridged connection | Operational | Validated — bridge up, `nic0` attached, and default route present on 2026-07-26 |
| `PASL-VM-001` | Virtual machine | `aegis-lab-kali-01` | Kali Linux security administration and authorized testing workstation | Proxmox VM ID `100` on `PASL-HW-001` | VirtIO adapter on `vmbr0`; DHCP; Proxmox firewall enabled | Operational | Validated — CPU, memory, disk, network, guest agent, boot media, and snapshot state confirmed on 2026-07-26 |
| `PASL-SW-001` | Hypervisor platform | Proxmox Virtual Environment | Provides virtualization, virtual networking, storage integration, snapshots, administrative management, scheduling, console access, and host firewall services | Installed on `PASL-HW-001` | Management services available through `vmbr0` | Operational | Validated — Proxmox 9.1.1 platform, core services, package versions, listening ports, and bind scopes recorded on 2026-07-26 |
| `PASL-SW-002` | Remote administration service | OpenSSH Server | Provides command-line remote administration of the Proxmox host | Installed on `PASL-HW-001` | TCP port 22; wildcard listener on all host interfaces | Operational | Validated — service active; package version `1:10.0p1-7`; access restrictions require later control assessment |
| `PASL-INF-001` | Information asset | Project documentation and evidence | System documentation, diagrams, configuration records, findings, screenshots, and assessment evidence | GitHub repository and approved local working copies | External hosted repository | Operational | Version controlled; public-content sanitization required |

## 5. Validated Host Configuration

| Field | Current Value |
|---|---|
| Asset ID | `PASL-HW-001` |
| Proxmox node hostname | `proxmox` |
| Hardware platform | ASUS TUF Gaming Laptop FX504 |
| Proxmox VE version | 9.1.1 |
| Proxmox package identifier | `pve-manager/9.1.1/42db4a6cf33dac83` |
| Operating system | Debian GNU/Linux 13 (trixie) |
| Running kernel | `6.17.2-1-pve` |
| Processor | Intel Core i7-8750H CPU @ 2.20 GHz |
| CPU sockets | 1 |
| Physical cores | 6 |
| Threads per core | 2 |
| Logical processors | 12 |
| Operating-system-reported memory | 15 GiB |
| Configured swap | 8 GiB |

Point-in-time memory utilization is retained in the supporting evidence but is not treated as a fixed asset characteristic.

## 6. Validated Network Configuration

| Field | Current Value |
|---|---|
| Physical Ethernet asset | `PASL-NET-001` — `nic0` |
| Physical interface state | Up |
| Primary bridge asset | `PASL-NET-002` — `vmbr0` |
| Bridge state | Up |
| Physical bridge membership | `nic0` attached to `vmbr0` |
| Proxmox management addressing | Static configuration on `vmbr0`; address sanitized from public evidence |
| Default route | Through `vmbr0`; gateway sanitized from public evidence |
| Wireless interface | `wlo1` exists but is down and not used by Project Aegis |
| VM ID `100` runtime path | `tap100i0` → `fwbr100i0` → firewall link pair → `vmbr0` |

The VM-specific interfaces `tap100i0`, `fwbr100i0`, `fwpr100p0`, and `fwln100i0` are generated and maintained by Proxmox. They demonstrate an active firewall bridge path for VM ID `100`, but they are not assigned independent persistent asset identifiers.

The persistent network configuration contains an `iface nic1 inet manual` entry, although no `nic1` interface appeared in the runtime interface list. This discrepancy will be reconciled during legacy host-inventory cleanup.

## 7. Active Virtual Guest Summary

| Asset ID | VM ID | Hostname | Operating System | vCPU | RAM | Disk | Storage | Network | IP Assignment | Status |
|---|---:|---|---|---:|---:|---:|---|---|---|---|
| `PASL-VM-001` | `100` | `aegis-lab-kali-01` | Kali Linux | 2 | 4 GB | 40 GB | `local-lvm` | `vmbr0` | DHCP | Active |

Validation confirmed that Kali Linux is the only deployed QEMU virtual machine and that no LXC containers are currently deployed.

## 8. Validated Kali VM Configuration

| Field | Current Value |
|---|---|
| Asset ID | `PASL-VM-001` |
| Proxmox VM ID | `100` |
| Name | `aegis-lab-kali-01` |
| Operating-system type | Linux 2.6 or newer (`l26`) |
| CPU configuration | 1 socket, 2 cores, host CPU passthrough |
| Assigned memory | 4096 MB |
| QEMU Guest Agent | Enabled |
| Primary disk | 40 GB SCSI disk on `local-lvm` |
| SCSI controller | `virtio-scsi-single` |
| Disk options | Discard enabled, I/O thread enabled, SSD emulation enabled |
| Network adapter | VirtIO adapter on `vmbr0` |
| Proxmox network firewall | Enabled |
| IP assignment | DHCP |
| Attached installation media | Kali Linux 2026.2 installer ISO from `aegis-hdd` |
| Boot order | SCSI disk, virtual CD-ROM, network adapter |
| Baseline snapshot | `baseline-clean-install` |
| Snapshot date | 2025-10-01 |

The virtual network adapter MAC address is intentionally excluded from the public inventory. The installer ISO remains attached and should be reviewed for removal when no longer operationally required. The baseline snapshot is a rollback aid and is not treated as an independent backup.

## 9. Validated Proxmox Host Software and Services

### 9.1 Core Service Status

| Service | Purpose | Status |
|---|---|---|
| `pve-cluster` | Proxmox configuration filesystem | Active |
| `pvedaemon` | Proxmox API daemon | Active |
| `pveproxy` | Proxmox HTTPS management proxy | Active |
| `pvestatd` | Node and guest status collection | Active |
| `pve-firewall` | Proxmox firewall service | Active |
| `pvefw-logger` | Firewall logging | Active |
| `qmeventd` | QEMU event handling | Active |
| `spiceproxy` | SPICE console proxy | Active |
| `ssh` | OpenSSH remote administration | Active |

Additional running Proxmox services include `pve-ha-crm`, `pve-ha-lrm`, `pve-lxc-syscalld`, and `pvescheduler`. Their presence is part of the installed Proxmox platform and does not prove that a multi-node cluster, high-availability workload, or LXC container is configured.

### 9.2 Package Versions

| Package | Version |
|---|---|
| `pve-manager` | `9.1.1` |
| `qemu-server` | `9.0.30` |
| `pve-firewall` | `6.0.4` |
| `openssh-server` | `1:10.0p1-7` |

### 9.3 Sanitized Listening-Port and Bind-Scope Baseline

| Protocol | Port | Service or Process | Preliminary Purpose | Validated Bind Scope |
|---|---:|---|---|---|
| TCP | 22 | `sshd` | SSH administration | Wildcard — all host interfaces |
| TCP | 25 | Postfix `master` | Local mail and system notifications | Loopback only |
| TCP | 85 | `pvedaemon` | Proxmox API daemon communication | Loopback only |
| TCP | 111 | `rpcbind` | RPC service mapping | Wildcard — all host interfaces |
| TCP | 3128 | `spiceproxy` | SPICE console proxy | Wildcard — all host interfaces |
| TCP | 8006 | `pveproxy` | Proxmox web-management interface | Wildcard — all host interfaces |
| UDP | 111 | `rpcbind` | RPC service mapping | Wildcard — all host interfaces |
| UDP | 323 | `chronyd` | Time-synchronization command interface | Loopback only |

A wildcard listener accepts traffic addressed to any active interface on the Proxmox host. It does not by itself prove internet exposure; actual reachability also depends on firewall rules, upstream router configuration, segmentation, and routing.

The wildcard listeners on SSH, SPICE, and the Proxmox web interface are administrative attack-surface items that must remain limited to trusted systems or networks. The operational need for wildcard `rpcbind` on TCP and UDP port `111` should be reviewed. Postfix, `pvedaemon`, and the `chronyd` command interface are loopback-only.

The active state of `pve-firewall` confirms that the service is running, but it does not by itself confirm that firewall enforcement or a protective ruleset is enabled. Firewall policy state remains a separate security-baseline item.

## 10. External Supporting Dependencies

These items support Project Aegis but are not managed as internal Project Aegis assets.

| Dependency | Purpose | Managed by Project Aegis? | Boundary Treatment |
|---|---|---|---|
| Home router | Gateway, DHCP, and internet connectivity | No | External dependency |
| Internet service provider | Internet connectivity | No | External dependency |
| GitHub platform | Public repository hosting and version control | No | External service supporting `PASL-INF-001` |
| Vendor update repositories | Operating-system and application updates | No | External software-supply dependency |
| Proxmox repositories | Hypervisor packages and updates | No | External software-supply dependency |

## 11. Planned Assets

Planned systems are tracked in project planning documents but are excluded from the active asset inventory until they are deployed and validated. These include Windows Server, Windows 11, Ubuntu Server, vulnerability-scanning services, centralized monitoring platforms, and a dedicated firewall or routing platform.

## 12. Inventory Maintenance Requirements

Update this inventory when:

- A physical device, VM, container, interface, storage resource, or security tool is added or removed.
- An asset changes hostname, VM ID, purpose, owner, location, network association, or operational status.
- CPU, memory, storage, operating-system, or software-version details materially change.
- An asset is suspended, archived, retired, replaced, or restored.
- A vulnerability, incident, or configuration review identifies an undocumented asset.
- The authorization boundary or inventory standard changes.

## 13. Current Validation Tasks

- [x] Validate the current Proxmox hostname, version, kernel, processor, and memory.
- [x] Validate physical disk models, capacities, and device assignments without publishing serial numbers.
- [x] Validate active physical and virtual network interfaces.
- [x] Validate the complete configuration of VM ID `100`.
- [x] Validate active managed software services on the Proxmox host.
- [x] Classify Proxmox listener bind scopes without publishing local IP addresses.
- [ ] Validate active managed software and listening services inside the Kali VM.
- [ ] Reconcile stale host-inventory statements and the unused `nic1` configuration entry with the validated active configuration.
- [ ] Review and approve the active asset list.

## 14. Related Documentation

- [`System Description`](system-description.md)
- [`Phase 1 README`](README.md)
- [`Proxmox Host Inventory`](../01-proxmox/host-inventory.md)
- [`Virtual Machine Inventory`](../01-proxmox/vm-inventory.md)
- [`Current Network Architecture`](../01-proxmox/current-network-architecture.md)
- [`Proxmox Guest Inventory Validation`](evidence/proxmox-guest-inventory-validation.md)
- [`Proxmox Host Hardware Validation`](evidence/proxmox-host-hardware-validation.md)
- [`Proxmox Network Interface Validation`](evidence/proxmox-network-interface-validation.md)
- [`Kali VM Configuration Validation`](evidence/kali-vm-configuration-validation.md)
- [`Proxmox Host Service Validation`](evidence/proxmox-host-service-validation.md)

## 15. Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 0.1 | 2026-07-25 | Javier Delgado | Created the initial asset inventory using validated Project Aegis system-description and guest-inventory evidence | In Progress |
| 0.2 | 2026-07-26 | Javier Delgado | Validated the Proxmox host identity, platform version, processor, memory, and physical storage devices | In Progress |
| 0.3 | 2026-07-26 | Javier Delgado | Validated `nic0`, `vmbr0`, the default-route path, the VM ID `100` firewall bridge path, and recorded the inactive wireless and unmatched `nic1` configuration | In Progress |
| 0.4 | 2026-07-26 | Javier Delgado | Validated the complete VM ID `100` compute, storage, network, guest-agent, boot-media, and snapshot configuration | In Progress |
| 0.5 | 2026-07-26 | Javier Delgado | Validated Proxmox core services, software package versions, OpenSSH, and the sanitized host listening-port baseline | In Progress |
| 0.6 | 2026-07-26 | Javier Delgado | Classified Proxmox listener bind scopes and identified wildcard administrative and RPC attack-surface items | In Progress |