# Proxmox Guest Inventory Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Date | 2026-07-25 |
| Status | In Progress |

## Objective

Validate the virtual machines and Linux containers currently deployed on the Project Aegis Proxmox host.

## QEMU Virtual Machine Validation

The following command was executed from the Proxmox host shell:

```bash
qm list
```

Observed output:

```text
VMID  NAME                 STATUS   MEM(MB)  BOOTDISK(GB)
100   aegis-lab-kali-01    running  4096     40.00
```

## Validated Findings

- One QEMU virtual machine is currently deployed.
- VM ID `100` is assigned to `aegis-lab-kali-01`.
- The Kali Linux VM was running at the time of validation.
- The VM has 4096 MB of assigned memory.
- The VM has a 40 GB boot disk.
- No other QEMU virtual machines were listed by `qm list`.

## Pending Validation

Linux containers must still be checked using:

```bash
pct list
```

The active guest inventory cannot be considered complete until the container check is recorded.

## Evidence Handling

The process identifier shown by `qm list` is transient operational data and is intentionally omitted from the retained evidence because it changes whenever the VM process restarts.
