# Proxmox Host Inventory

## Purpose

This document records the physical hardware, Proxmox installation, storage configuration, and network configuration used to host Project Aegis.

The inventory provides a baseline for capacity planning, troubleshooting, change tracking, and future architecture decisions.

---

## Host Summary

| Field               | Value                        |
| ------------------- | ---------------------------- |
| Project             | Project Aegis                |
| Host role           | Proxmox virtualization host  |
| Hardware platform   | ASUS TUF Gaming Laptop FX504 |
| Proxmox node name   | proxmox                      |
| Proxmox VE version  | 9.1.0                        |
| Operating system    | Debian GNU/Linux 13 (trixie) |
| Kernel version      | Linux 6.17.2-1-pve           |
| System architecture | x86-64                       |
| Installation type   | Bare-metal hypervisor        |
| Primary use         | Cybersecurity homelab        |

---

## Processor

| Field                  | Value                                                  |
| ---------------------- | ------------------------------------------------------ |
| Manufacturer           | Intel                                                  |
| Model                  | Intel Core i7-8750H @ 2.20GHz                          |
| Physical cores         | 6                                                      |
| Threads per core       | 2                                                      |
| Logical processors     | 12                                                     |
| Virtualization support | Intel VT-x                                             |
| VM workload strategy   | Multiple small VMs with limited simultaneous operation |

### Capacity Notes

The processor provides sufficient capacity for several small virtual machines. Virtual CPUs may be overcommitted because each VM does not continuously use all assigned processor time.

CPU utilization will be monitored when multiple systems are operating simultaneously.

---

## Memory

| Field                 | Value                              |
| --------------------- | ---------------------------------- |
| Installed memory      | Approximately 16 GB DDR4           |
| Proxmox host reserve  | Approximately 2–4 GB               |
| Estimated VM capacity | Approximately 10–12 GB at one time |
| Primary constraint    | Available memory                   |

### Memory Strategy

Memory is the primary resource constraint for this lab.

Only systems required for the active lab exercise should remain powered on. Large services such as Wazuh, Windows Server, and Greenbone may require other virtual machines to be shut down temporarily.

Initial planned allocations include:

| Virtual machine     | Planned RAM |
| ------------------- | ----------: |
| Kali Linux          |        4 GB |
| Ubuntu Server       |        2 GB |
| Windows workstation |        4 GB |
| Windows Server      |        4 GB |
| Wazuh               |      4–6 GB |
| pfSense             |      1–2 GB |

These systems will not all operate simultaneously with the current memory capacity.

---

## Physical Storage

| Device | Model | Approximate capacity | Media type | Current role | Planned purpose |
|---|---|---:|---|---|---|
| `/dev/nvme0n1` | Kingston RBUSNS8154P3256GJ | 238.5 GB | PCIe NVMe SSD | Proxmox system drive and LVM storage | Proxmox, active virtual machines, and performance-sensitive workloads |
| `/dev/sda` | Toshiba MQ04ABF100 | 931.5 GB | SATA HDD | Secondary disk; ext4 partition exists but is not currently mounted | ISO files, backups, archived VMs, scan exports, and less performance-sensitive data |

## Physical Disk Layout

### Primary NVMe SSD

The primary NVMe drive is configured with the standard Proxmox LVM layout.

| Device or volume | Size | Type | Filesystem | Mount point or purpose |
|---|---:|---|---|---|
| `/dev/nvme0n1` | 238.5 GB | Physical disk | — | Primary Proxmox system disk |
| `/dev/nvme0n1p1` | 1007 KB | Partition | — | Reserved system partition |
| `/dev/nvme0n1p2` | 1 GB | Partition | VFAT | `/boot/efi` |
| `/dev/nvme0n1p3` | 237.5 GB | Partition | LVM2 member | Proxmox LVM physical volume |
| `pve-swap` | 8 GB | Logical volume | Swap | System swap space |
| `pve-root` | 69.4 GB | Logical volume | ext4 | Proxmox root filesystem `/` |
| `pve-data` | 141.2 GB | LVM thin pool | LVM thin | Virtual-machine and container disk storage |

### Secondary HDD

| Device or partition | Size | Type | Filesystem | Mount point or purpose |
|---|---:|---|---|---|
| `/dev/sda` | 931.5 GB | Physical disk | — | Secondary Toshiba HDD |
| `/dev/sda1` | 512 MB | Partition | VFAT | Currently no mount point shown |
| `/dev/sda2` | 931 GB | Partition | ext4 | Currently no mount point shown |

The secondary HDD contains an ext4 partition, but the `lsblk` output does not show a mount point. This indicates that the disk is formatted but is not currently mounted in the Proxmox filesystem or configured as a Proxmox storage resource.

## Storage Strategy

The Kingston NVMe SSD currently hosts the Proxmox operating system and the LVM thin pool used for virtual-machine disks.

The NVMe SSD should host workloads that benefit from faster storage performance, including:

- Proxmox system files
- Windows virtual machines
- Kali Linux
- Wazuh
- Frequently used Ubuntu servers
- Active Directory services

The Toshiba HDD will be configured as secondary storage for:

- Installation ISO images
- VM and container backups
- Archived virtual machines
- Greenbone and Nmap report exports
- Project files
- Less performance-sensitive workloads

The secondary HDD should be mounted and added to Proxmox before it is used for backups or ISO storage.

## Proxmox Storage Configuration

| Storage name | Type | Location or device | Content types | Status |
|---|---|---|---|---|
| `local` | Directory storage | Located on the `pve-root` filesystem, normally `/var/lib/vz` | ISO images, container templates, backups, and snippets | To be confirmed |
| `local-lvm` | LVM-Thin | `pve/data` thin pool on `/dev/nvme0n1p3` | VM and container disk images | Active |
| Secondary HDD | Not yet configured | `/dev/sda2` | Planned for ISOs, backups, archives, and exports | Not mounted or added to Proxmox |

### Storage Observations

- Proxmox is installed on the 238.5 GB Kingston NVMe SSD.
- The Proxmox root logical volume provides 69.4 GB for the host operating system.
- The `pve-data` thin pool provides approximately 141.2 GB for virtual-machine and container disks.
- The system has an 8 GB swap logical volume.
- The 931 GB Toshiba HDD contains an ext4 partition but currently has no visible mount point.
- The secondary HDD must be mounted and added as a Proxmox storage resource before it can store ISO images, backups, or other Proxmox-managed content.

---

## Network Interfaces

| Interface | Type | Status | Address | Purpose |
|---|---|---|---|---|
| `nic0` | Physical Ethernet interface | Up | No address assigned directly | Physical network connection used by the Proxmox bridge |
| `wlo1` | Wireless interface | Down | No address assigned | Currently unused |
| `vmbr0` | Linux bridge | Up | `10.0.0.x/24` | Proxmox management access and initial VM connectivity |
| `lo` | Loopback interface | Unknown/active locally | `127.0.0.1/8`, `::1/128` | Local host communication |

### Interface Observations

- The Proxmox management IP address is assigned to `vmbr0`, not directly to the physical Ethernet interface.
- The physical interface `nic0` is active and is expected to be attached to `vmbr0`.
- The wireless interface `wlo1` is currently disabled.
- The Proxmox host also has an automatically generated IPv6 link-local address on `vmbr0`.communication                       |

## Routing Configuration

| Field | Value |
|---|---|
| Management network | `10.0.0.0/24` |
| Proxmox management address | `10.0.0.x/24` |
| Default gateway | `10.0.0.1` |
| Default-route interface | `vmbr0` |
| Connected route | `10.0.0.0/24` through `vmbr0` |
| Current network mode | Bridged to the home network |

### Route Details



default via 10.0.0.1 dev vmbr0



## Network Security Notes

The initial configuration prioritizes ease of setup and connectivity.

Current considerations include:

- Virtual machines attached to `vmbr0` may be able to communicate with other devices on the home network.
- Kali Linux and other security tools must only be used against authorized lab systems.
- Intentionally vulnerable machines should not remain connected to `vmbr0` after isolated lab networking is introduced.
- No Proxmox administrative ports should be forwarded through the home router.
- The Proxmox web interface should only be accessible from trusted internal devices.
- Future milestones will introduce pfSense and isolated virtual bridges.
- Firewall and segmentation testing should occur only inside the controlled lab network.

## Network Design

The Proxmox host currently uses a bridged network configuration.

The physical Ethernet interface provides connectivity to the home network, while `vmbr0` functions as a virtual Ethernet switch. The Proxmox management interface and any virtual machines connected to `vmbr0` can communicate through the physical adapter.

The current traffic path is:


Home Router
    |
Physical Ethernet Connection
    |
nic0
    |
vmbr0
    |
Proxmox Management Interface
and Connected Virtual Machines

## Current Network Architecture


Internet
   |
Home Router
Gateway: 10.0.0.1
   |
Home LAN: 10.0.0.0/24
   |
Physical Interface: nic0
   |
Linux Bridge: vmbr0
Management IP: 10.0.0.x/24
   |
Future Virtual Machines

---

## Administrative Access

| Access method         | Purpose                           | Security consideration                      |
| --------------------- | --------------------------------- | ------------------------------------------- |
| Proxmox web interface | Primary host administration       | Restrict access to the trusted home network |
| Proxmox shell         | Command-line administration       | Use only when necessary                     |
| SSH                   | Optional remote administration    | Disable or restrict when not required       |
| Physical console      | Recovery and local administration | Laptop should remain physically protected   |

### Administrative Security Baseline

* Use a strong, unique Proxmox administrator password.
* Do not expose TCP port `8006` directly to the internet.
* Do not publish credentials or authentication information in GitHub.
* Keep Proxmox updated.
* Use a non-root administrative account later when practical.
* Back up important configuration before major changes.
* Record significant changes in the project changelog.

---

## Current Resource Constraints

The current host has several limitations that affect the lab design:

1. The system contains approximately 16 GB of RAM.
2. Not all planned virtual machines can operate simultaneously.
3. The SSD has limited capacity.
4. The secondary HDD provides more capacity but lower performance.
5. The laptop has limited physical network-interface options.
6. Some networking exercises may require virtual interfaces or a USB Ethernet adapter.
7. The laptop battery provides short-term power protection but is not a replacement for a proper backup strategy.

---

## Risk Considerations

| Risk                                        | Potential impact                    | Planned control                          |
| ------------------------------------------- | ----------------------------------- | ---------------------------------------- |
| Vulnerable VM connected to the home network | Other devices may be exposed        | Introduce isolated lab networks          |
| Insufficient memory                         | Poor performance or failed services | Run only required VMs                    |
| SSD capacity exhaustion                     | VM or host instability              | Monitor storage and use HDD for archives |
| Accidental deletion                         | Loss of lab progress                | Use backups and GitHub documentation     |
| Credential exposure                         | Unauthorized access                 | Never commit credentials                 |
| Internet exposure                           | External compromise                 | Do not configure inbound port forwarding |
| Host failure                                | Loss of virtual machines            | Store backups on secondary storage       |

---

## Baseline Validation

The following checks were completed:

* [X] Proxmox web interface is accessible
* [X] Node status shows expected CPU and memory
* [X] Proxmox version is recorded
* [X] Physical storage devices are identified
* [X] Proxmox storage pools are identified
* [X] The primary Linux bridge is identified
* [X] The default gateway is recorded privately
* [X] No Proxmox administrative service is intentionally exposed to the internet
* [X] Host inventory information has been sanitized before publication

---

## Evidence to Capture

Store sanitized screenshots in:

```text
screenshots/proxmox/
```

Recommended screenshots:

1. Proxmox node summary
2. Storage overview
3. Network interface overview
4. Proxmox version information
5. Disk layout

Use descriptive filenames:

```text
proxmox-node-summary.png
proxmox-storage-overview.png
proxmox-network-overview.png
```

Review every screenshot before committing it to ensure it does not expose information you do not want public.

---

## Next Steps

1. Confirm the SSD and HDD configuration.
2. Create a formal storage plan.
3. Document the current Proxmox network bridge.
4. Establish a virtual-machine naming convention.
5. Create the initial VM inventory.
6. Capture sanitized screenshots.
