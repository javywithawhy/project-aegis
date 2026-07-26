# Kali VM Configuration Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Date | 2026-07-26 |
| Status | Complete |

## Objective

Validate the current Proxmox configuration and snapshot state of Kali Linux virtual machine ID `100` without retaining the virtual network adapter MAC address or other unnecessary identifiers.

## Commands Executed

The following read-only commands were executed from the Proxmox host shell:

```bash
qm config 100 \
| grep -E '^(agent|bios|boot|bootdisk|cores|cpu|ide[0-9]+|machine|memory|name|net[0-9]+|numa|onboot|ostype|protection|scsi[0-9]+|sata[0-9]+|scsihw|sockets|tags|vga):' \
| sed -E 's/((virtio|e1000|rtl8139|vmxnet3)=)[0-9A-Fa-f:]+/\1[SANITIZED-MAC]/g'

qm listsnapshot 100
```

## Sanitized Configuration Output

```text
agent: 1
boot: order=scsi0;ide2;net0
cores: 2
cpu: host
ide2: aegis-hdd:iso/kali-linux-2026.2-installer-amd64.iso,media=cdrom,size=4689972K
memory: 4096
name: aegis-lab-kali-01
net0: virtio=[SANITIZED-MAC],bridge=vmbr0,firewall=1
numa: 0
ostype: l26
scsi0: local-lvm:vm-100-disk-0,discard=on,iothread=1,size=40G,ssd=1
scsihw: virtio-scsi-single
sockets: 1
```

## Snapshot Output

```text
`-> baseline-clean-install      2025-10-01 16:01:30     Clean Kali installation after updates, guest-agent installation, and initial validation
 `-> current                                            You are here!
```

## Validated Findings

- VM ID `100` is named `aegis-lab-kali-01`.
- The VM is configured with one socket, two virtual CPU cores, and host CPU passthrough.
- The VM is assigned 4096 MB of memory.
- QEMU Guest Agent support is enabled.
- The operating-system type is configured as Linux 2.6 or newer (`l26`).
- The primary virtual disk is a 40 GB SCSI disk on `local-lvm`.
- The virtual disk uses the `virtio-scsi-single` controller with discard, I/O thread, and SSD emulation enabled.
- The VM uses a VirtIO network adapter connected to `vmbr0`.
- Proxmox firewall processing is enabled for the VM network interface.
- The Kali 2026.2 installer ISO remains attached as virtual CD-ROM media from `aegis-hdd`.
- The configured boot order is the primary SCSI disk, the virtual CD-ROM, and then the network adapter.
- The baseline snapshot `baseline-clean-install` exists and records the clean post-installation state.

## Security and Configuration Notes

The virtual network adapter MAC address is intentionally sanitized from public evidence.

The attached installer ISO is not a security finding, but removable installation media should be reviewed periodically and detached when it is no longer operationally required.

The Proxmox snapshot provides a convenient rollback point but is not a substitute for an independent backup stored on separate media.
