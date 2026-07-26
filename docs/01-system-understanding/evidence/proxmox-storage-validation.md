# Proxmox Storage Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Validation Date | 2026-07-26 |
| Source System | `PASL-HW-001` — Proxmox host `proxmox` |
| Status | Complete |

## Objective

Validate the physical and logical storage layout, confirm that the secondary Toshiba HDD is mounted read/write, verify its persistent mount configuration, and confirm that it is actively managed by Proxmox as `aegis-hdd`.

## Commands Executed

```bash
pvesm status
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
findmnt /mnt/pve/aegis-hdd
grep -A 6 '^dir: aegis-hdd$' /etc/pve/storage.cfg
grep '/mnt/pve/aegis-hdd' /etc/fstab \
| sed -E 's/UUID=[^[:space:]]+/UUID=[REDACTED]/'
```

The commands were read-only. The filesystem UUID was redacted before publication.

## Proxmox Storage Status

| Storage ID | Type | Status | Total (KiB) | Used (KiB) | Available (KiB) | Utilization |
|---|---|---|---:|---:|---:|---:|
| `aegis-hdd` | Directory | Active | 959,786,032 | 11,098,948 | 899,858,876 | 1.16% |
| `local` | Directory | Active | 71,017,632 | 5,173,128 | 62,191,284 | 7.28% |
| `local-lvm` | LVM thin | Active | 148,086,784 | 22,494,382 | 125,592,401 | 15.19% |

## Physical and Logical Layout

| Device or Volume | Size | Type or Filesystem | Mount Point or Role |
|---|---:|---|---|
| `/dev/nvme0n1` | 238.5 GiB | NVMe physical disk | Primary Proxmox system disk |
| `/dev/nvme0n1p2` | 1 GiB | VFAT | `/boot/efi` |
| `/dev/nvme0n1p3` | 237.5 GiB | LVM2 member | Proxmox volume group |
| `pve-root` | 69.4 GiB | ext4 logical volume | `/` |
| `pve-swap` | 8 GiB | Swap logical volume | System swap |
| `pve-data` | 141.2 GiB | LVM thin pool | `local-lvm` backing storage |
| `pve-vm--100--disk--0` | 40 GiB | Thin logical volume | Kali VM ID `100` primary disk |
| `/dev/sda` | 931.5 GiB | SATA physical disk | Secondary Toshiba HDD |
| `/dev/sda1` | 512 MiB | VFAT | No active mount observed |
| `/dev/sda2` | 931 GiB | ext4 | `/mnt/pve/aegis-hdd` |

## Secondary HDD Mount Validation

`findmnt` confirmed:

```text
TARGET             SOURCE    FSTYPE OPTIONS
/mnt/pve/aegis-hdd /dev/sda2 ext4   rw,relatime
```

The secondary partition is therefore mounted read/write as ext4.

## Proxmox Storage Configuration

```text
dir: aegis-hdd
        path /mnt/pve/aegis-hdd
        content backup,snippets,iso,vztmpl
        prune-backups keep-all=1
        shared 0
```

This confirms that `aegis-hdd` is an active, non-shared Proxmox directory-storage resource supporting backups, snippets, ISO images, and container templates.

## Persistent Mount Configuration

The sanitized `/etc/fstab` entry is:

```text
UUID=[REDACTED] /mnt/pve/aegis-hdd ext4 defaults,nofail 0 2
```

The disk is mounted persistently by filesystem UUID. The `nofail` option allows the Proxmox host to continue booting if the secondary disk is unavailable.

## Findings

- The legacy statements that `/dev/sda2` was unmounted, absent from `/etc/fstab`, and unmanaged by Proxmox were stale and incorrect.
- `/dev/sda2` is mounted at `/mnt/pve/aegis-hdd` using ext4 with read/write access.
- `aegis-hdd` is active in Proxmox and is currently lightly utilized.
- Kali VM ID `100` uses a 40 GiB thin-provisioned disk on `local-lvm`.
- The filesystem UUID is intentionally excluded from the public repository.

## ISSO Relevance

This validation reconciles configuration records with the deployed environment. Accurate storage inventory supports configuration management, backup planning, capacity monitoring, incident response, and assessment evidence. Stale records can cause incorrect risk decisions and must be corrected when discovered.