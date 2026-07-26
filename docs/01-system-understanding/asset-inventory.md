# Project Aegis Security Lab — Asset Inventory

| Field | Value |
|---|---|
| System Name | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.1 |
| Status | In Progress |
| Date | 2026-07-25 |
| Authorization Status | Not Authorized — System Definition in Progress |

## 1. Objective

This document identifies and tracks the hardware, virtual machines, storage, networking components, software platforms, and information assets used by Project Aegis Security Lab.

The inventory supports system-boundary definition, configuration management, vulnerability management, security assessment, incident response, change control, and continuous monitoring.

## 2. Inventory Rules

An item is entered in the active inventory when it is deployed, assigned to Project Aegis, and available for use. Planned systems are not treated as active assets until deployment and validation are complete.

Each managed asset receives a stable Project Aegis asset identifier. Asset identifiers should remain associated with the asset throughout its lifecycle, including suspension, retirement, or replacement.

The public inventory must not contain passwords, tokens, private keys, public IP addresses, hardware serial numbers, personal account names, or unsanitized sensitive findings.

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
| `PASL-HW-001` | Physical host | Proxmox virtualization host | Bare-metal platform that hosts Project Aegis virtual machines, virtual networking, and storage services | Privately controlled physical location | Management through `vmbr0` | Operational | Partial — hardware and operating-system details require current command validation |
| `PASL-STO-001` | Primary storage | Kingston NVMe SSD | Hosts Proxmox system files, LVM storage, and active VM disks | Installed in `PASL-HW-001` | Not directly networked | Operational | Partial — model and capacity documented; current device details require validation |
| `PASL-STO-002` | Secondary storage | `aegis-hdd` | Directory storage for ISO files, backups, templates, archives, and supporting project data | `/dev/sda2` mounted at `/mnt/pve/aegis-hdd` | Available through Proxmox storage services | Operational | Validated using `pvesm status`, `findmnt`, and `lsblk` |
| `PASL-NET-001` | Physical network interface | `nic0` | Physical Ethernet connection supporting Proxmox management and bridged VM connectivity | Installed in `PASL-HW-001` | Connected to the trusted home network | Operational | Partial — requires current interface validation |
| `PASL-NET-002` | Virtual network bridge | `vmbr0` | Linux bridge providing current management and VM connectivity | Configured on `PASL-HW-001` | Home-network bridged connection | Operational | Partial — current address and interface membership require validation |
| `PASL-VM-001` | Virtual machine | `aegis-lab-kali-01` | Kali Linux security administration and authorized testing workstation | Proxmox VM ID `100` on `PASL-HW-001` | `vmbr0`; DHCP | Operational | Validated using `qm list` and prior configuration review |
| `PASL-SW-001` | Hypervisor platform | Proxmox Virtual Environment | Provides virtualization, virtual networking, storage integration, snapshots, and administrative management | Installed on `PASL-HW-001` | Managed through the Proxmox management interface | Operational | Partial — current package version requires validation |
| `PASL-INF-001` | Information asset | Project documentation and evidence | System documentation, diagrams, configuration records, findings, screenshots, and assessment evidence | GitHub repository and approved local working copies | External hosted repository | Operational | Version controlled; public-content sanitization required |

## 5. Active Virtual Guest Summary

| Asset ID | VM ID | Hostname | Operating System | vCPU | RAM | Disk | Storage | Network | IP Assignment | Status |
|---|---:|---|---|---:|---:|---:|---|---|---|---|
| `PASL-VM-001` | `100` | `aegis-lab-kali-01` | Kali Linux | 2 | 4 GB | 40 GB | `local-lvm` | `vmbr0` | DHCP | Active |

Validation confirmed that Kali Linux is the only deployed QEMU virtual machine and that no LXC containers are currently deployed.

## 6. External Supporting Dependencies

These items support Project Aegis but are not managed as internal Project Aegis assets.

| Dependency | Purpose | Managed by Project Aegis? | Boundary Treatment |
|---|---|---|---|
| Home router | Gateway, DHCP, and internet connectivity | No | External dependency |
| Internet service provider | Internet connectivity | No | External dependency |
| GitHub platform | Public repository hosting and version control | No | External service supporting `PASL-INF-001` |
| Vendor update repositories | Operating-system and application updates | No | External software-supply dependency |
| Proxmox repositories | Hypervisor packages and updates | No | External software-supply dependency |

## 7. Planned Assets

Planned systems are tracked in project planning documents but are excluded from the active asset inventory until they are deployed and validated. These include Windows Server, Windows 11, Ubuntu Server, vulnerability-scanning services, centralized monitoring platforms, and a dedicated firewall or routing platform.

## 8. Inventory Maintenance Requirements

Update this inventory when:

- A physical device, VM, container, interface, storage resource, or security tool is added or removed.
- An asset changes hostname, VM ID, purpose, owner, location, network association, or operational status.
- CPU, memory, storage, operating-system, or software-version details materially change.
- An asset is suspended, archived, retired, replaced, or restored.
- A vulnerability, incident, or configuration review identifies an undocumented asset.
- The authorization boundary or inventory standard changes.

## 9. Current Validation Tasks

- [ ] Validate the current Proxmox hostname, version, kernel, processor, and memory.
- [ ] Validate physical disk models, capacities, and device assignments without publishing serial numbers.
- [ ] Validate active physical and virtual network interfaces.
- [ ] Validate the complete configuration of VM ID `100`.
- [ ] Confirm whether any additional managed software services are active on the Proxmox host or Kali VM.
- [ ] Review and approve the active asset list.

## 10. Related Documentation

- [`System Description`](system-description.md)
- [`Phase 1 README`](README.md)
- [`Proxmox Host Inventory`](../01-proxmox/host-inventory.md)
- [`Virtual Machine Inventory`](../01-proxmox/vm-inventory.md)
- [`Current Network Architecture`](../01-proxmox/current-network-architecture.md)
- [`Proxmox Guest Inventory Validation`](evidence/proxmox-guest-inventory-validation.md)

## 11. Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 0.1 | 2026-07-25 | Javier Delgado | Created the initial asset inventory using validated Project Aegis system-description and guest-inventory evidence | In Progress |
