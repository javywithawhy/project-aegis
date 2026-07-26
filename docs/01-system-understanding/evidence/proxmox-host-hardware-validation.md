# Proxmox Host Hardware Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Collection Date | 2026-07-26 |
| Source | Proxmox host shell |
| Status | Validated |
| Classification | Public — Synthetic Lab Information |

## Objective

Validate the identity, operating platform, processor, memory, and physical storage devices of the Project Aegis Proxmox host without collecting hardware serial numbers or other unnecessary identifiers.

## Commands Executed

```bash
hostnamectl --static
pveversion
uname -r
grep -E '^(PRETTY_NAME|VERSION)=' /etc/os-release
lscpu | grep -E '^(CPU\(s\)|Model name|Thread\(s\) per core|Core\(s\) per socket|Socket\(s\)):'
free -h
lsblk -d -o NAME,MODEL,SIZE,ROTA,TRAN
```

## Observed Results

### Host and Operating Platform

| Field | Observed Value |
|---|---|
| Node hostname | `proxmox` |
| Proxmox VE | `pve-manager/9.1.1/42db4a6cf33dac83` |
| Running kernel | `6.17.2-1-pve` |
| Operating system | Debian GNU/Linux 13 (trixie) |

### Processor

| Field | Observed Value |
|---|---|
| Processor model | Intel Core i7-8750H CPU @ 2.20 GHz |
| CPU sockets | 1 |
| Physical cores | 6 |
| Threads per core | 2 |
| Logical processors | 12 |

### Memory Snapshot

| Field | Observed Value |
|---|---|
| Total memory reported by operating system | 15 GiB |
| Memory used at collection | 6.0 GiB |
| Memory available at collection | 9.5 GiB |
| Configured swap | 8.0 GiB |
| Swap used at collection | 0 B |

Memory utilization values are a point-in-time operational snapshot and will change as workloads start and stop.

### Physical Storage Devices

| Device | Model | Reported Size | Rotational | Transport | Assigned Role |
|---|---|---:|---|---|---|
| `/dev/nvme0n1` | Kingston RBUSNS8154P3256GJ | 238.5 GiB | No | NVMe | Proxmox system and active VM storage |
| `/dev/sda` | Toshiba MQ04ABF100 | 931.5 GiB | Yes | SATA | Secondary `aegis-hdd` storage |

## Validated Findings

- The active Proxmox node hostname is `proxmox`.
- The host is running Proxmox VE 9.1.1 on Debian GNU/Linux 13.
- The active kernel is `6.17.2-1-pve`.
- The host provides 6 physical CPU cores and 12 logical processors.
- The operating system reports 15 GiB of usable memory and 8 GiB of swap.
- The host contains one NVMe SSD and one SATA HDD.
- Hardware serial numbers were intentionally excluded from evidence collection.

## Related Assets

- `PASL-HW-001` — Proxmox virtualization host
- `PASL-STO-001` — Kingston NVMe primary storage
- `PASL-STO-002` — Toshiba SATA secondary storage
- `PASL-SW-001` — Proxmox Virtual Environment

## Evidence Handling

This evidence records only information needed for asset identification and capacity planning. Hardware serial numbers, MAC addresses, exact management addresses, credentials, and authentication information are not included.