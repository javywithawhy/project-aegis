# Proxmox Host Inventory

| Field | Value |
|---|---|
| Project | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 1.0 |
| Status | Current — Validated |
| Validation Date | 2026-07-26 |

## 1. Purpose

This document records the validated physical hardware, Proxmox platform, storage, network, and administrative-service configuration used to host Project Aegis.

It provides a current baseline for system understanding, capacity planning, configuration management, troubleshooting, security assessment, change control, and continuous monitoring.

Sensitive identifiers such as hardware serial numbers, MAC addresses, filesystem UUIDs, credentials, and exact management addresses are excluded from this public document.

## 2. Host Summary

| Field | Current Value |
|---|---|
| Asset ID | `PASL-HW-001` |
| Host role | Proxmox virtualization host |
| Hardware platform | ASUS TUF Gaming Laptop FX504 |
| Proxmox node name | `proxmox` |
| Proxmox VE version | 9.1.1 |
| Proxmox package identifier | `pve-manager/9.1.1/42db4a6cf33dac83` |
| Operating system | Debian GNU/Linux 13 (trixie) |
| Running kernel | `6.17.2-1-pve` |
| System architecture | x86-64 |
| Installation type | Bare-metal hypervisor |
| Primary use | Cybersecurity and ISSO/RMF homelab |

## 3. Processor and Memory

### 3.1 Processor

| Field | Current Value |
|---|---|
| Manufacturer | Intel |
| Model | Intel Core i7-8750H CPU @ 2.20 GHz |
| CPU sockets | 1 |
| Physical cores | 6 |
| Threads per core | 2 |
| Logical processors | 12 |
| Virtualization strategy | Multiple small VMs with controlled simultaneous operation |

### 3.2 Memory

| Field | Current Value |
|---|---|
| Installed memory | 16 GB nominal |
| Operating-system-reported memory | 15 GiB |
| Configured swap | 8 GiB |
| Primary capacity constraint | Available memory |

The difference between nominal installed memory and operating-system-reported memory is expected due to binary reporting and hardware reservations.

Only systems required for the active exercise should remain powered on. Planned Windows, monitoring, vulnerability-management, and routing workloads cannot all operate simultaneously with the current memory capacity.

## 4. Physical Storage

| Asset ID | Device | Model | Capacity | Media | Current Role | Status |
|---|---|---|---:|---|---|---|
| `PASL-STO-001` | `/dev/nvme0n1` | Kingston RBUSNS8154P3256GJ | 238.5 GiB | NVMe SSD | Proxmox system disk, root filesystem, swap, and LVM thin storage | Operational |
| `PASL-STO-002` | `/dev/sda` | Toshiba MQ04ABF100 | 931.5 GiB | SATA HDD | Secondary Proxmox directory storage through `aegis-hdd` | Operational |

Hardware serial numbers are intentionally omitted.

## 5. Primary NVMe Layout

| Device or Volume | Size | Type or Filesystem | Mount Point or Role |
|---|---:|---|---|
| `/dev/nvme0n1` | 238.5 GiB | Physical NVMe disk | Primary Proxmox system disk |
| `/dev/nvme0n1p1` | 1007 KiB | Reserved partition | System-reserved space |
| `/dev/nvme0n1p2` | 1 GiB | VFAT | `/boot/efi` |
| `/dev/nvme0n1p3` | 237.5 GiB | LVM2 member | Proxmox LVM physical volume |
| `pve-swap` | 8 GiB | Swap logical volume | System swap |
| `pve-root` | 69.4 GiB | ext4 logical volume | Root filesystem `/` |
| `pve-data` | 141.2 GiB | LVM thin pool | Backing storage for `local-lvm` |
| `pve-vm--100--disk--0` | 40 GiB | Thin logical volume | Kali VM ID `100` primary disk |

## 6. Secondary HDD Layout and Mount

| Device or Partition | Size | Filesystem | Mount Point or Role |
|---|---:|---|---|
| `/dev/sda` | 931.5 GiB | Physical SATA disk | Secondary Toshiba HDD |
| `/dev/sda1` | 512 MiB | VFAT | No active mount observed |
| `/dev/sda2` | 931 GiB | ext4 | `/mnt/pve/aegis-hdd` |

`/dev/sda2` is mounted read/write:

```text
/mnt/pve/aegis-hdd /dev/sda2 ext4 rw,relatime
```

The persistent mount is configured in `/etc/fstab` using a redacted filesystem UUID:

```text
UUID=[REDACTED] /mnt/pve/aegis-hdd ext4 defaults,nofail 0 2
```

The `nofail` option allows the Proxmox host to continue booting if the secondary disk is unavailable.

## 7. Proxmox Storage Resources

| Storage ID | Type | Status | Total (KiB) | Used (KiB) | Available (KiB) | Utilization |
|---|---|---|---:|---:|---:|---:|
| `aegis-hdd` | Directory | Active | 959,786,032 | 11,098,948 | 899,858,876 | 1.16% |
| `local` | Directory | Active | 71,017,632 | 5,173,128 | 62,191,284 | 7.28% |
| `local-lvm` | LVM thin | Active | 148,086,784 | 22,494,382 | 125,592,401 | 15.19% |

### 7.1 `aegis-hdd` Configuration

```text
dir: aegis-hdd
        path /mnt/pve/aegis-hdd
        content backup,snippets,iso,vztmpl
        prune-backups keep-all=1
        shared 0
```

`aegis-hdd` is a non-shared directory-storage resource supporting:

- VM and container backups
- ISO images
- Container templates
- Proxmox snippets

It is not currently configured to host VM disk images.

### 7.2 Storage Reconciliation

Previous statements that `/dev/sda2` was unmounted, absent from `/etc/fstab`, and unmanaged by Proxmox were stale. Validation confirmed that the partition is mounted persistently and is active as `aegis-hdd`.

The filesystem UUID previously included in the legacy inventory has been removed from the public record.

## 8. Network Interfaces

| Asset ID | Interface | Type | Runtime State | Purpose | Inventory Treatment |
|---|---|---|---|---|---|
| `PASL-NET-001` | `nic0` | Realtek wired Ethernet using `r8169` | Up | Physical connection for Proxmox management and bridged VM traffic | Active managed asset |
| `PASL-NET-002` | `vmbr0` | Linux bridge | Up | Proxmox management, default route, and current VM connectivity | Active managed asset |
| — | `wlo1` | Intel Wireless-AC 9560 using `iwlwifi` | Down | Not used by Project Aegis | Excluded from active inventory |
| — | `nic1` | Persistent configuration entry only | No runtime interface exists | No validated purpose | Stale or unused configuration entry |
| — | `lo` | Loopback | Active locally | Local host communication | System-managed interface |

`nic0` is attached to `vmbr0`. The Proxmox management address is statically configured on `vmbr0`, and the default route uses `vmbr0`. Exact addresses are withheld from the public inventory.

The persistent line `iface nic1 inet manual` does not correspond to a runtime interface or a second wired controller. It is excluded from the active asset inventory. No network configuration was changed during this validation.

## 9. Current Network Path

```text
External Home Router
        |
Trusted Home Network
        |
Realtek Ethernet — nic0
        |
Linux Bridge — vmbr0
        |---------------------------|
Proxmox Management          VM ID 100 Kali
```

Kali VM ID `100` is currently connected to `vmbr0` through a Proxmox-generated firewall bridge path. Virtual machines attached to `vmbr0` may communicate with other systems on the trusted home network, subject to firewall and routing controls.

Intentionally vulnerable systems must not be connected to `vmbr0` after isolated lab networking is introduced.

## 10. Active Virtual Guest

| Asset ID | VM ID | Name | vCPU | Memory | Disk | Storage | Network | Status |
|---|---:|---|---:|---:|---:|---|---|---|
| `PASL-VM-001` | `100` | `aegis-lab-kali-01` | 2 | 4096 MB | 40 GiB | `local-lvm` | `vmbr0` | Running during validation |

Kali is the only deployed QEMU virtual machine. No LXC containers are currently deployed.

## 11. Active Host Software and Administrative Services

| Asset or Component | Version or State | Security Relevance |
|---|---|---|
| Proxmox VE | 9.1.1 | Hypervisor and management platform |
| QEMU Server | 9.0.30 | Virtual-machine management |
| Proxmox Firewall | 6.0.4; service active | Firewall service state does not prove enforcement or rule coverage |
| OpenSSH Server | `1:10.0p1-7`; active | Remote administration through TCP port 22 |
| Proxmox web proxy | Active | Management interface through TCP port 8006 |
| SPICE proxy | Active | Administrative console service through TCP port 3128 |

Validated host listeners include:

| Protocol | Port | Service | Bind Scope |
|---|---:|---|---|
| TCP | 22 | SSH | Wildcard — all host interfaces |
| TCP | 25 | Postfix | Loopback only |
| TCP | 85 | Proxmox API daemon | Loopback only |
| TCP | 111 | `rpcbind` | Wildcard — all host interfaces |
| TCP | 3128 | SPICE proxy | Wildcard — all host interfaces |
| TCP | 8006 | Proxmox web management | Wildcard — all host interfaces |
| UDP | 111 | `rpcbind` | Wildcard — all host interfaces |
| UDP | 323 | `chronyd` command interface | Loopback only |

Wildcard binding does not prove internet exposure. Effective reachability depends on the host firewall, upstream router, segmentation, and routing. Administrative services must remain limited to trusted systems and networks. The need for wildcard `rpcbind` remains a later attack-surface review item.

## 12. Administrative Access

| Access Method | Purpose | Security Requirement |
|---|---|---|
| Proxmox web interface | Primary host administration | Restrict to trusted internal access; do not forward TCP 8006 from the internet |
| Proxmox shell | Local or browser-based command administration | Use only for authorized management activities |
| SSH | Remote command administration | Restrict to trusted systems and assess authentication controls |
| Physical console | Recovery and local administration | Maintain physical protection of the laptop |

Credentials, authentication material, and private keys must never be committed to the public repository.

## 13. Capacity and Risk Considerations

| Condition or Risk | Potential Impact | Current or Planned Treatment |
|---|---|---|
| Limited memory | VM performance degradation or failed service startup | Run only required VMs simultaneously |
| NVMe capacity exhaustion | Host or VM instability | Monitor `local` and `local-lvm`; use `aegis-hdd` for supported content |
| Secondary HDD failure | Loss of ISO, backup, or template data | Maintain backup planning and monitor storage health |
| VM attached to trusted home network | Exposure of non-lab devices | Introduce isolated lab networking before deploying vulnerable systems |
| Administrative wildcard listeners | Unauthorized internal access | Restrict with firewall and access-control implementation |
| Stale configuration records | Incorrect security or capacity decisions | Reconcile documentation with command-based evidence |
| Credential exposure | Unauthorized access | Sanitize evidence and never commit secrets |
| Host failure | Loss of active VM workloads | Maintain backups separate from VM snapshots |

## 14. Baseline Validation Status

- [x] Proxmox node identity and platform version validated.
- [x] Processor, memory, and swap validated.
- [x] Physical and logical storage layout validated.
- [x] `local`, `local-lvm`, and `aegis-hdd` status and utilization validated.
- [x] Secondary HDD mount and persistent `/etc/fstab` configuration validated with UUID redacted.
- [x] Active physical and virtual network interfaces validated.
- [x] Stale `nic1` entry reconciled as excluded from the active inventory.
- [x] Active VM and container inventory validated.
- [x] Core host services, package versions, listeners, and bind scopes validated.
- [x] Public inventory reviewed for removal of the exposed filesystem UUID and stale storage claims.

## 15. Related Evidence

- [`Proxmox Host Hardware Validation`](../01-system-understanding/evidence/proxmox-host-hardware-validation.md)
- [`Proxmox Network Interface Validation`](../01-system-understanding/evidence/proxmox-network-interface-validation.md)
- [`Proxmox Network Controller Reconciliation`](../01-system-understanding/evidence/proxmox-network-controller-reconciliation.md)
- [`Proxmox Storage Validation`](../01-system-understanding/evidence/proxmox-storage-validation.md)
- [`Proxmox Host Service Validation`](../01-system-understanding/evidence/proxmox-host-service-validation.md)
- [`Proxmox Guest Inventory Validation`](../01-system-understanding/evidence/proxmox-guest-inventory-validation.md)

## 16. Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 1.0 | 2026-07-26 | Javier Delgado | Replaced contradictory legacy host records with the validated Proxmox 9.1.1 hardware, storage, network, guest, and service baseline; removed the published filesystem UUID | Current |