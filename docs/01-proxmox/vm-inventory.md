# Project Aegis Virtual Machine Inventory

## Purpose

This document records all virtual machines and containers deployed in Project Aegis.

The inventory supports:

* Asset management
* Capacity planning
* Troubleshooting
* Security assessments
* Backup planning
* Change tracking
* Architecture documentation

---

## Current Inventory

| VM ID | Hostname               | Type | Operating system          | Purpose                               | vCPU |    RAM |  Disk | Storage     | Network                 | Status  |
| ----: | ---------------------- | ---- | ------------------------- | ------------------------------------- | ---: | -----: | ----: | ----------- | ----------------------- | ------- |
| `100` | `aegis-lab-kali-01`    | VM   | Kali Linux                | Security workstation                  |    2 |   4 GB | 40 GB | `local-lvm` | `vmbr0`                 | Active  |
| `120` | `aegis-lab-ubuntu-01`  | VM   | Ubuntu Server             | Linux server and vulnerability target |    2 |   2 GB | 25 GB | `local-lvm` | `vmbr0`                 | Planned |
| `140` | `aegis-lab-win-01`     | VM   | Windows                   | Windows workstation                   |    2 |   4 GB | 60 GB | `local-lvm` | `vmbr0`                 | Planned |
| `160` | `aegis-lab-dc-01`      | VM   | Windows Server            | Active Directory domain controller    |    2 |   4 GB | 60 GB | `local-lvm` | Future isolated network | Planned |
| `180` | `aegis-lab-pfsense-01` | VM   | pfSense                   | Firewall, routing, and segmentation   |    2 |   2 GB | 20 GB | `local-lvm` | Multiple bridges        | Planned |
| `200` | `aegis-lab-wazuh-01`   | VM   | Ubuntu or Wazuh appliance | SIEM and monitoring                   |  2–4 | 4–6 GB | 80 GB | `local-lvm` | Future isolated network | Planned |
| `101` | `aegis-lab-openvas-01` | VM   | Linux                     | Vulnerability scanner                 |  2–4 |   4 GB | 60 GB | `local-lvm` | Future security network | Planned |

Validation using `qm list` and `pct list` confirmed that Kali Linux is the only deployed virtual machine and that no LXC containers are currently deployed. Planned entries are retained for capacity and architecture planning but are not active assets.

---

## Inventory Field Definitions

| Field            | Description                                             |
| ---------------- | ------------------------------------------------------- |
| VM ID            | Unique Proxmox numeric identifier                       |
| Hostname         | Operating-system and Proxmox system name                |
| Type             | Full virtual machine or LXC container                   |
| Operating system | Installed operating system                              |
| Purpose          | Primary role in Project Aegis                           |
| vCPU             | Assigned virtual processors                             |
| RAM              | Assigned memory                                         |
| Disk             | Allocated virtual disk capacity                         |
| Storage          | Proxmox storage resource hosting the disk               |
| Network          | Connected bridge or network segment                     |
| Status           | Planned, Build, Active, Suspended, Archived, or Retired |

---

## Status Definitions

| Status    | Meaning                                      |
| --------- | -------------------------------------------- |
| Planned   | System has not been created                  |
| Build     | Installation or configuration is in progress |
| Active    | System is available for normal lab use       |
| Suspended | System is temporarily unavailable            |
| Archived  | System is retained but not normally used     |
| Retired   | System has been removed from service         |

---

## Active Asset Details

### `aegis-lab-kali-01`

| Field | Value |
|---|---|
| VM ID | `100` |
| System Type | Virtual machine |
| Operating System | Kali Linux |
| Primary Role | Security administration and authorized testing workstation |
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 40 GB |
| Storage | `local-lvm` |
| Network Bridge | `vmbr0` |
| IP Assignment | DHCP |
| Current Status | Active |
| Last Reviewed | 2026-07-25 |

#### Security Notes

* The VM is currently attached to the unsegmented home-network bridge `vmbr0`.
* Security testing is restricted to Project Aegis systems and other explicitly authorized assets.
* Higher-risk testing will wait until isolated networking and firewall controls are implemented.

#### Evidence

* [`Kali Linux System Baseline`](../02-linux/system-baseline.md)
* [`Kali ISO Preparation`](../04-kali/kali-iso-preparation.md)
* [`Project Aegis System Description`](../01-system-understanding/system-description.md)
* [`Proxmox Guest Inventory Validation`](../01-system-understanding/evidence/proxmox-guest-inventory-validation.md)

---

## Resource Planning

The host has approximately 16 GB of physical memory. All planned systems cannot operate simultaneously.

### Initial Vulnerability Management Lab

The following systems may run together:

| System               |                  RAM |
| -------------------- | -------------------: |
| Proxmox host reserve | Approximately 2–3 GB |
| Kali Linux           |                 4 GB |
| Ubuntu Server        |                 2 GB |
| Windows workstation  |                 4 GB |
| Remaining capacity   | Approximately 3–4 GB |

Greenbone/OpenVAS may require shutting down the Windows system or running the scanner separately.

### Active Directory Lab

A likely combination is:

| System                    |                  RAM |
| ------------------------- | -------------------: |
| Proxmox host reserve      | Approximately 2–3 GB |
| Windows domain controller |                 4 GB |
| Windows workstation       |                 4 GB |
| Ubuntu Server             |                 2 GB |
| pfSense                   |               1–2 GB |

### Monitoring Lab

Wazuh may require other systems to be powered off when not actively generating or forwarding logs.

---

## Asset Details Template

Create one subsection for each deployed system.

### `[HOSTNAME]`

| Field                  | Value                      |
| ---------------------- | -------------------------- |
| VM ID                  | `[ID]`                     |
| System type            | `[VM OR LXC]`              |
| Operating system       | `[OS]`                     |
| OS version             | `[VERSION]`                |
| Primary role           | `[ROLE]`                   |
| vCPU                   | `[COUNT]`                  |
| RAM                    | `[SIZE]`                   |
| Disk                   | `[SIZE]`                   |
| Storage                | `[STORAGE ID]`             |
| Network bridge         | `[BRIDGE]`                 |
| IP assignment          | `[DHCP OR STATIC]`         |
| IP address             | `[SANITIZED ADDRESS]`      |
| Default gateway        | `[SANITIZED GATEWAY]`      |
| DNS server             | `[SANITIZED DNS]`          |
| Administrative account | `[DO NOT RECORD PASSWORD]` |
| Backup status          | `[STATUS]`                 |
| Baseline snapshot      | `[SNAPSHOT NAME OR NONE]`  |
| Current status         | `[STATUS]`                 |
| Date created           | `[YYYY-MM-DD]`             |
| Last reviewed          | `[YYYY-MM-DD]`             |

#### Installed Services

| Service     |     Port | Purpose     | Required |
| ----------- | -------: | ----------- | -------- |
| `[SERVICE]` | `[PORT]` | `[PURPOSE]` | Yes/No   |

#### Security Notes

* `[SECURITY NOTE]`
* `[SECURITY NOTE]`

#### Dependencies

* `[DEPENDENCY]`

#### Evidence

* `[SCREENSHOT OR DOCUMENT LINK]`

---

## Inventory Maintenance Rules

Update the inventory when:

* A VM or container is created
* CPU, memory, or disk resources change
* A system is renamed
* A network interface changes
* A static IP address is assigned
* A service is installed or removed
* A backup is created
* A baseline snapshot is created
* A system is archived or deleted
* A major security configuration is changed

---

## Security Considerations

The public inventory must not include:

* Passwords
* Authentication tokens
* Private keys
* Public IP addresses
* Sensitive hostnames
* Employer information
* Personal account names
* Unredacted security findings

Private IP addresses may be sanitized using values such as:

```text
10.0.0.x
192.168.x.x
```

---

## Current Inventory Status

| Task                           | Status   |
| ------------------------------ | -------- |
| Naming standard created        | Complete |
| VM ID ranges assigned          | Complete |
| Planned systems recorded       | Complete |
| Kali Linux deployed            | Complete |
| Ubuntu Server deployed         | Pending  |
| Windows workstation deployed   | Pending  |
| Current guest inventory validated | Complete |

---

## Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 0.1 | 2026-07-16 | Javier Delgado | Initial planned virtual-machine inventory | Superseded |
| 0.2 | 2026-07-25 | Javier Delgado | Validated Kali VM ID, resources, bridge, DHCP assignment, and active status | Draft |
| 0.3 | 2026-07-25 | Javier Delgado | Confirmed one deployed QEMU VM, no LXC containers, and completed current guest inventory validation | Current |