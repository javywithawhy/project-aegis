# Proxmox Storage Plan

## Purpose

This document defines how storage resources are allocated within Project Aegis.

The storage plan is intended to:

* Preserve performance for active virtual machines
* Provide sufficient capacity for backups and installation media
* Reduce the risk of filling the Proxmox system disk
* Separate active workloads from archived data
* Support recovery from failed configuration changes
* Establish consistent storage practices for future lab systems

---

## Storage Overview

Project Aegis uses two physical storage devices:

| Device         | Model                      | Capacity | Media type | Primary role                                 |
| -------------- | -------------------------- | -------: | ---------- | -------------------------------------------- |
| `/dev/nvme0n1` | Kingston RBUSNS8154P3256GJ | 238.5 GB | NVMe SSD   | Proxmox and active VM storage                |
| `/dev/sda`     | Toshiba MQ04ABF100         | 931.5 GB | SATA HDD   | Backups, ISO images, templates, and archives |

---

## Proxmox Storage Resources

| Storage ID  | Type      | Backing storage                             | Primary purpose                                                 |
| ----------- | --------- | ------------------------------------------- | --------------------------------------------------------------- |
| `local`     | Directory | Proxmox root filesystem                     | ISO images, container templates, backups, imports, and snippets |
| `local-lvm` | LVM-Thin  | Kingston NVMe SSD                           | Active VM and container disks                                   |
| `aegis-hdd` | Directory | Toshiba HDD mounted at `/mnt/pve/aegis-hdd` | Backups, ISO files, templates, snippets, and archived data      |

---

## Storage Assignment Strategy

### `local-lvm`

`local-lvm` is located on the faster NVMe SSD and will be used for active virtual-machine and container disks.

Planned workloads include:

* Kali Linux
* Ubuntu Server
* Windows workstation
* Windows Server
* Active Directory
* Wazuh
* pfSense
* Greenbone/OpenVAS

Not all systems will run simultaneously because the Proxmox host has limited memory.

### `local`

`local` is located on the Proxmox root filesystem.

It may be used for:

* Small ISO images
* Container templates
* Snippets
* Temporary backups
* Imports

Because `local` shares space with the Proxmox operating system, large files should not be stored there unnecessarily.

### `aegis-hdd`

`aegis-hdd` is located on the 1 TB Toshiba HDD.

It will be used for:

* Proxmox VM backups
* Installation ISO images
* Container templates
* Configuration archives
* Exported vulnerability reports
* Archived scan results
* Project documentation backups
* Virtual machines that do not require high storage performance

Active VM disks will remain on `local-lvm` unless capacity requires otherwise.

---

## Planned Content Placement

| Content             | Preferred storage      | Reason                                           |
| ------------------- | ---------------------- | ------------------------------------------------ |
| Active VM disks     | `local-lvm`            | Faster SSD performance                           |
| Windows VM disks    | `local-lvm`            | Windows benefits from faster random I/O          |
| Wazuh VM disk       | `local-lvm`            | Logging and indexing are storage-intensive       |
| Kali VM disk        | `local-lvm`            | Frequently used security workstation             |
| Ubuntu VM disk      | `local-lvm`            | Frequently used server                           |
| ISO images          | `aegis-hdd`            | Large files do not require SSD performance       |
| Container templates | `aegis-hdd`            | Low performance requirement                      |
| VM backups          | `aegis-hdd`            | Higher capacity and separation from active disks |
| Scan exports        | `aegis-hdd`            | Archival workload                                |
| Temporary files     | `local` or `aegis-hdd` | Depends on size and duration                     |
| Archived VMs        | `aegis-hdd`            | Capacity is more important than performance      |

---

## Backup Strategy

VM backups will be stored on `aegis-hdd`.

Initial backup practices will include:

* Create a backup before major configuration changes
* Back up important systems after completing a milestone
* Use descriptive backup notes
* Retain at least one known-good backup of critical systems
* Confirm backups complete successfully
* Periodically test that a backup can be restored
* Avoid storing the only copy of important documentation inside a VM

GitHub will provide version control for sanitized documentation, scripts, templates, and reports. It is not a backup for virtual-machine disks.

---

## Snapshot Strategy

Snapshots and backups serve different purposes.

### Snapshots

Snapshots will be used for short-term rollback before:

* Package upgrades
* Configuration changes
* Security hardening
* Agent installation
* Vulnerability remediation
* Detection-testing exercises

Snapshots should be removed after the system is confirmed stable.

### Backups

Backups will be used for longer-term recovery from:

* VM corruption
* Accidental deletion
* Failed upgrades
* Storage problems
* Major configuration errors
* Host reinstallation

Snapshots should not be treated as replacements for backups.

---

## Capacity Management

The following practices will be used to prevent storage exhaustion:

* Monitor storage utilization through the Proxmox dashboard
* Review `pvesm status` regularly
* Remove unused ISO images
* Remove obsolete snapshots
* Archive or delete unnecessary VM backups
* Avoid storing large files on the Proxmox root filesystem
* Keep sufficient free space available on `local-lvm`
* Investigate rapid or unexpected storage growth
* Document major storage changes

Suggested thresholds:

| Utilization | Response                                   |
| ----------- | ------------------------------------------ |
| Below 70%   | Normal operation                           |
| 70–80%      | Review growth and remove unnecessary files |
| 80–90%      | Take corrective action                     |
| Above 90%   | Treat as an urgent capacity issue          |

---

## Security Considerations

Storage-related security practices include:

* Do not store passwords or credentials in unencrypted files
* Sanitize reports before uploading them to GitHub
* Restrict Proxmox administrative access
* Review backup contents before sharing or exporting them
* Avoid storing employer or customer data in the lab
* Protect configuration exports containing network or identity information
* Do not expose Proxmox storage services directly to the internet
* Use descriptive filenames without including sensitive information

---

## Recovery Considerations

The 1 TB HDD is installed inside the same physical laptop as the NVMe SSD.

This provides protection against accidental VM deletion and some software failures, but it does not protect against:

* Laptop theft
* Fire or water damage
* Complete hardware failure
* Electrical damage
* Failure of both internal drives
* Ransomware affecting the host

Important documentation remains version-controlled in GitHub. A future improvement may include copying selected VM backups to an external drive.

---

## Current Configuration

| Item                              | Status             |
| --------------------------------- | ------------------ |
| NVMe SSD identified               | Complete           |
| Proxmox root storage documented   | Complete           |
| LVM-Thin storage documented       | Complete           |
| Secondary HDD inspected           | Complete           |
| Secondary HDD mounted permanently | Complete           |
| Secondary HDD added to Proxmox    | Complete           |
| Storage roles assigned            | Complete           |
| Backup schedule created           | Planned            |
| Restore testing completed         | Planned            |
| External backup implemented       | Future improvement |

---

## Validation Commands

The following commands can be used to review storage:

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL
```

Displays disks, partitions, filesystems, and mount points.

```bash
pvesm status
```

Displays Proxmox-managed storage resources and utilization.

```bash
df -hT
```

Displays mounted filesystem capacity and utilization.

```bash
findmnt /mnt/pve/aegis-hdd
```

Confirms the secondary HDD mount.

```bash
cat /etc/pve/storage.cfg
```

Displays the Proxmox storage configuration.

---

## Next Steps

1. Create the Project Aegis VM naming standard.
2. Create the initial VM inventory.
3. Document the current Proxmox network architecture.
4. Capture sanitized Proxmox screenshots.
5. Configure the first virtual machine.
