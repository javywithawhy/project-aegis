# Proxmox Guest Inventory Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Date | 2026-07-25 |
| Status | Complete |

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

## LXC Container Validation

The following command was executed from the Proxmox host shell:

```bash
pct list
```

The command returned no output. This indicates that no LXC containers were deployed on the host at the time of validation.

## Validated Findings

- One QEMU virtual machine is currently deployed.
- VM ID `100` is assigned to `aegis-lab-kali-01`.
- The Kali Linux VM was running at the time of validation.
- The VM has 4096 MB of assigned memory.
- The VM has a 40 GB boot disk.
- No other QEMU virtual machines were listed by `qm list`.
- No LXC containers were listed by `pct list`.
- The active Proxmox guest inventory consists only of the Kali Linux VM.

## Conclusion

The active guest inventory has been validated. Project Aegis currently contains one deployed virtual machine and no deployed Linux containers.

Planned Windows, Ubuntu, firewall, monitoring, and vulnerability-management systems are not yet deployed and remain outside the active guest inventory.

## Evidence Handling

The process identifier shown by `qm list` is transient operational data and is intentionally omitted from the retained evidence because it changes whenever the VM process restarts.