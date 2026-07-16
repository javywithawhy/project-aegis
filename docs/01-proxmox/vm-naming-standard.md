# Project Aegis VM Naming Standard

## Purpose

This document defines the naming convention used for virtual machines and containers in Project Aegis.

A consistent naming standard makes systems easier to identify, document, troubleshoot, automate, and manage.

---

## Naming Format

Virtual machines will use the following format:

```text
aegis-[environment]-[role]-[number]
```

Example:

```text
aegis-lab-kali-01
```

---

## Naming Components

| Component   | Description               | Example |
| ----------- | ------------------------- | ------- |
| `aegis`     | Project identifier        | `aegis` |
| Environment | System environment        | `lab`   |
| Role        | Primary system function   | `kali`  |
| Number      | Two-digit instance number | `01`    |

---

## Approved Environment Codes

| Code     | Meaning                               |
| -------- | ------------------------------------- |
| `lab`    | General Project Aegis lab environment |
| `mgmt`   | Management systems                    |
| `srv`    | Server environment                    |
| `client` | Workstation or endpoint environment   |
| `sec`    | Security tooling environment          |
| `dmz`    | Perimeter or DMZ systems              |
| `test`   | Temporary testing systems             |

For the first phase, most systems will use `lab`.

---

## Approved Role Codes

| Role code | System purpose                  |
| --------- | ------------------------------- |
| `kali`    | Kali Linux security workstation |
| `ubuntu`  | Ubuntu Server                   |
| `win`     | Windows workstation             |
| `dc`      | Windows domain controller       |
| `pfsense` | pfSense firewall                |
| `wazuh`   | Wazuh SIEM server               |
| `openvas` | Greenbone/OpenVAS scanner       |
| `web`     | Web server                      |
| `db`      | Database server                 |
| `jump`    | Administrative jump host        |
| `files`   | File server                     |
| `backup`  | Backup server                   |
| `test`    | Temporary test system           |

---

## Planned System Names

| System                    | Planned hostname       |
| ------------------------- | ---------------------- |
| Kali Linux                | `aegis-lab-kali-01`    |
| Ubuntu Server             | `aegis-lab-ubuntu-01`  |
| Windows workstation       | `aegis-lab-win-01`     |
| Windows domain controller | `aegis-lab-dc-01`      |
| pfSense                   | `aegis-lab-pfsense-01` |
| Wazuh                     | `aegis-lab-wazuh-01`   |
| Greenbone/OpenVAS         | `aegis-lab-openvas-01` |

---

## Proxmox VM ID Standard

Proxmox assigns a numeric ID to every virtual machine and container.

Project Aegis will use the following ranges:

| VM ID range | Purpose                              |
| ----------- | ------------------------------------ |
| `100–119`   | Security tools                       |
| `120–139`   | Linux servers                        |
| `140–159`   | Windows workstations                 |
| `160–179`   | Windows servers and Active Directory |
| `180–199`   | Network appliances                   |
| `200–219`   | Monitoring systems                   |
| `900–999`   | Temporary or disposable test systems |

---

## Initial VM ID Assignments

| VM ID | Hostname               | Purpose                               |
| ----: | ---------------------- | ------------------------------------- |
| `100` | `aegis-lab-kali-01`    | Security workstation                  |
| `120` | `aegis-lab-ubuntu-01`  | Linux server and vulnerability target |
| `140` | `aegis-lab-win-01`     | Windows workstation                   |
| `160` | `aegis-lab-dc-01`      | Active Directory domain controller    |
| `180` | `aegis-lab-pfsense-01` | Firewall and router                   |
| `200` | `aegis-lab-wazuh-01`   | SIEM and monitoring                   |
| `101` | `aegis-lab-openvas-01` | Vulnerability scanner                 |

These assignments may change as the environment develops.

---

## Naming Rules

System names must:

* Use lowercase letters
* Use hyphens as separators
* Avoid spaces
* Avoid underscores
* Avoid personal names
* Avoid passwords or sensitive information
* Identify the system's primary role
* Use a two-digit sequence number
* Remain understandable without additional context

System names should not include:

* Public IP addresses
* Employer names
* Customer names
* Usernames
* Operating-system versions
* Temporary descriptions such as `new`, `final`, or `working`

---

## Examples

### Correct

```text
aegis-lab-kali-01
aegis-lab-ubuntu-01
aegis-lab-win-01
aegis-lab-dc-01
aegis-lab-wazuh-01
```

### Incorrect

```text
Kali VM
Javier-PC
WindowsFinal
Ubuntu_New
10.0.0.60
TestMachine
```

---

## Containers

Linux containers will follow the same naming format:

```text
aegis-lab-[role]-[number]
```

The asset inventory will identify whether the system is a VM or an LXC container.

---

## Temporary Systems

Temporary systems should use the `test` role or environment.

Example:

```text
aegis-test-ubuntu-01
```

Temporary systems should be removed when they are no longer required.

---

## Naming Change Process

Before renaming a system:

1. Record the existing name.
2. Confirm whether applications depend on the hostname.
3. Create a snapshot or backup.
4. Rename the operating system.
5. Rename the system in Proxmox.
6. Restart the system if required.
7. Verify network and service operation.
8. Update the asset inventory.
9. Commit the documentation change to GitHub.

---

## Current Standard Status

| Item                          | Status   |
| ----------------------------- | -------- |
| Naming format defined         | Complete |
| Environment codes defined     | Complete |
| Role codes defined            | Complete |
| VM ID ranges defined          | Complete |
| Initial system names assigned | Complete |
| Existing systems reviewed     | Pending  |
