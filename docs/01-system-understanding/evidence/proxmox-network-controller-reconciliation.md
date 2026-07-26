# Proxmox Network Controller Reconciliation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Validation Date | 2026-07-26 |
| Source System | `PASL-HW-001` — Proxmox host `proxmox` |
| Status | Validated — stale configuration entry identified |

## Objective

Reconcile the persistent `nic1` network configuration entry with the network interfaces and hardware controllers currently present on the Proxmox host.

## Commands Executed

The following read-only checks were executed from the Proxmox host shell:

```bash
grep -nE '^[[:space:]]*(auto|allow-hotplug|iface|bridge-ports|bridge-stp|bridge-fd|source|source-directory)' /etc/network/interfaces
ip link show nic1
readlink -f /sys/class/net/*/device/driver
lspci -nnk
```

The collected output excluded MAC addresses and IP addresses.

## Persistent Network Configuration

| Configuration Entry | Observed State |
|---|---|
| `lo` | Configured as loopback |
| `nic0` | Configured as a manual physical interface |
| `vmbr0` | Configured as the static Proxmox management bridge |
| `bridge-ports nic0` | Confirms that `nic0` is the physical member of `vmbr0` |
| `nic1` | Present as `iface nic1 inet manual`, but not present at runtime |
| `/etc/network/interfaces.d/*` | Included through the source directive |

## Runtime and Hardware Findings

| Interface or Controller | Type | Driver | Current Treatment |
|---|---|---|---|
| `nic0` | Physical Realtek Gigabit Ethernet interface | `r8169` | Active Project Aegis network asset `PASL-NET-001` |
| `vmbr0` | Linux software bridge | Not applicable | Active Project Aegis network asset `PASL-NET-002` |
| `wlo1` | Physical Intel Wireless-AC 9560 interface | `iwlwifi` | Present but down and excluded from active Project Aegis networking |
| `nic1` | No matching runtime interface or hardware controller | None | Stale or unused persistent configuration entry |
| `bonding_masters` | Kernel virtual control interface | Not applicable | Not treated as a Project Aegis network asset |

The physical controllers identified were:

- Intel Wireless-AC 9560 using the `iwlwifi` driver
- Realtek RTL8111/8168-family Gigabit Ethernet controller using the `r8169` driver

No second wired Ethernet controller matching `nic1` was present.

## Conclusion

The `iface nic1 inet manual` line does not represent an active physical or virtual interface. It is classified as a stale or unused configuration entry and is excluded from the active asset inventory.

No change was made to `/etc/network/interfaces` during this validation. Removal of the stale line is optional configuration cleanup and should only be performed after creating a backup and confirming that no external adapter is expected to use the `nic1` name.

## ISSO Relevance

This reconciliation demonstrates the distinction between configured objects and active assets. An ISSO must investigate inventory discrepancies rather than treating every configuration entry as evidence that an asset exists. The stale entry remains a configuration-management observation until it is removed or formally retained.