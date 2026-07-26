# Project Aegis Security Lab — Asset Inventory

| Field | Value |
|---|---|
| System Name | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.2 |
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
| `PASL-NET-001` | Physical network interface | `nic0` | Physical Ethernet connection supporting Proxmox management and bridged VM connectivity | Installed in `PASL-HW-001` | Connected to the trusted home network | Operational | Partial — requires current interface-state validation |
| `PASL-NET-002` | Virtual network bridge | `vmbr0` | Linux bridge providing current management and VM connectivity | Configured on `PASL-HW-001` | Home-network bridged connection | Operational | Partial — current interface membership requires validation |
| `PASL-VM-001` | Virtual machine | `aegis-lab-kali-01` | Kali Linux security administration and authorized testing workstation | Proxmox VM ID `100` on `PASL-HW-001` | `vmbr0`; DHCP | Operational | Validated using `qm list` and prior configuration review |
| `PASL-SW-001` | Hypervisor platform | Proxmox Virtual Environment | Provides virtualization, virtual networking, storage integration, snapshots, and administrative management | Installed on `PASL-HW-001` | Managed through the Proxmox management interface | Operational | Validated — `pve-manager/9.1.1/42db4a6cf33dac83`, Debian 13, kernel `6.17.2-1-pve` |
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

## 6. Active Virtual Guest Summary

| Asset ID | VM ID | Hostname | Operating System | vCPU | RAM | Disk | Storage | Network | IP Assignment | Status |
|---|---:|---|---|---:|---:|---:|---|---|---|---|
| `PASL-VM-001` | `100` | `aegis-lab-kali-01` | Kali Linux | 2 | 4 GB | 40 GB | `local-lvm` | `vmbr0` | DHCP | Active |

Validation confirmed that Kali Linux is the only deployed QEMU virtual machine and that no LXC containers are currently deployed.

## 7. External Supporting Dependencies

These items support Project Aegis but are not managed as internal Project Aegis assets.

| Dependency | Purpose | Managed by Project Aegis? | Boundary Treatment |
|---|---|---|---|
| Home router | Gateway, DHCP, and internet connectivity | No | External dependency |
| Internet service provider | Internet connectivity | No | External dependency |
| GitHub platform | Public repository hosting and version control | No | External service supporting `PASL-INF-001` |
| Vendor update repositories | Operating-system and application updates | No | External software-supply dependency |
| Proxmox repositories | Hypervisor packages and updates | No | External software-supply dependency |

## 8. Planned Assets

Planned systems are tracked in project planning documents but are excluded from the active asset inventory until they are deployed and validated. These include Windows Server, Windows 11, Ubuntu Server, vulnerability-scanning services, centralized monitoring platforms, and a dedicated firewall or routing platform.

## 9. Inventory Maintenance Requirements

Update this inventory when:

- A physical device, VM, container, interface, storage resource, or security tool is added or removed.
- An asset changes hostname, VM ID, purpose, owner, location, network association, or operational status.
- CPU, memory, storage, operating-system, or software-version details materially change.
- An asset is suspended, archived, retired, replaced, or restored.
- A vulnerability, incident, or configuration review identifies an undocumented asset.
- The authorization boundary or inventory standard changes.

## 10. Current Validation Tasks

- [x] Validate the current Proxmox hostname, version, kernel, processor, and memory.
- [x] Validate physical disk models, capacities, and device assignments without publishing serial numbers.
- [ ] Validate active physical and virtual network interfaces.
- [ ] Validate the complete configuration of VM ID `100`.
- [ ] Confirm whether any additional managed software services are active on the Proxmox host or Kali VM.
- [ ] Reconcile stale host-inventory statements with the validated active configuration.
- [ ] Review and approve the active asset list.

## 11. Related Documentation

- [`System Description`](system-description.md)
- [`Phase 1 README`](README.md)
- [`Proxmox Host Inventory`](../01-proxmox/host-inventory.md)
- [`Virtual Machine Inventory`](../01-proxmox/vm-inventory.md)
- [`Current Network Architecture`](../01-proxmox/current-network-architecture.md)
- [`Proxmox Guest Inventory Validation`](evidence/proxmox-guest-inventory-validation.md)
- [`Proxmox Host Hardware Validation`](evidence/proxmox-host-hardware-validation.md)

## 12. Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 0.1 | 2026-07-25 | Javier Delgado | Created the initial asset inventory using validated Project Aegis system-description and guest-inventory evidence | In Progress |
| 0.2 | 2026-07-26 | Javier Delgado | Validated the Proxmox host identity, platform version, processor, memory, and physical storage devices | In Progress |