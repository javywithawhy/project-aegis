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
| `100` | `aegis-lab-kali-01`    | VM   | Kali Linux                | Security workstation                  |    2 |   4 GB | 40 GB | `local-lvm` | `vmbr0`                 | Planned |
| `120` | `aegis-lab-ubuntu-01`  | VM   | Ubuntu Server             | Linux server and vulnerability target |    2 |   2 GB | 25 GB | `local-lvm` | `vmbr0`                 | Planned |
| `140` | `aegis-lab-win-01`     | VM   | Windows                   | Windows workstation                   |    2 |   4 GB | 60 GB | `local-lvm` | `vmbr0`                 | Planned |
| `160` | `aegis-lab-dc-01`      | VM   | Windows Server            | Active Directory domain controller    |    2 |   4 GB | 60 GB | `local-lvm` | Future isolated network | Planned |
| `180` | `aegis-lab-pfsense-01` | VM   | pfSense                   | Firewall, routing, and segmentation   |    2 |   2 GB | 20 GB | `local-lvm` | Multiple bridges        | Planned |
| `200` | `aegis-lab-wazuh-01`   | VM   | Ubuntu or Wazuh appliance | SIEM and monitoring                   |  2–4 | 4–6 GB | 80 GB | `local-lvm` | Future isolated network | Planned |
| `101` | `aegis-lab-openvas-01` | VM   | Linux                     | Vulnerability scanner                 |  2–4 |   4 GB | 60 GB | `local-lvm` | Future security network | Planned |

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
| Kali Linux deployed            | Pending  |
| Ubuntu Server deployed         | Pending  |
| Windows workstation deployed   | Pending  |
| Inventory validation completed | Pending  |
