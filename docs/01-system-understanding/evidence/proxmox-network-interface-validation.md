# Proxmox Network Interface Validation

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Evidence Owner | Javier Delgado |
| Date | 2026-07-26 |
| Status | Complete |

## Objective

Validate the active physical and virtual network interfaces supporting the Project Aegis Proxmox host and Kali Linux VM.

## Commands Executed

The following read-only commands were executed from the Proxmox host shell:

```bash
ip -br link | awk '{print $1, $2}'
bridge link
grep -E '^(auto|iface|bridge-ports|bridge-stp|bridge-fd)' /etc/network/interfaces
ip route show default | sed -E \
's/via ([0-9]{1,3}\.){3}[0-9]{1,3}/via [SANITIZED]/g; s/src ([0-9]{1,3}\.){3}[0-9]{1,3}/src [SANITIZED]/g'
```

## Observed Interface States

```text
lo UNKNOWN
nic0 UP
wlo1 DOWN
vmbr0 UP
tap100i0 UNKNOWN
fwbr100i0 UP
fwpr100p0@fwln100i0 UP
fwln100i0@fwpr100p0 UP
```

## Observed Bridge Membership

```text
nic0 -> vmbr0
tap100i0 -> fwbr100i0
fwpr100p0 -> vmbr0
fwln100i0 -> fwbr100i0
```

## Persistent Configuration Summary

```text
auto lo
iface lo inet loopback
iface nic0 inet manual
auto vmbr0
iface vmbr0 inet static
iface nic1 inet manual
```

## Sanitized Default Route

```text
default via [SANITIZED] dev vmbr0 proto kernel onlink
```

## Validated Findings

- Physical Ethernet interface `nic0` is operational and attached to Linux bridge `vmbr0`.
- `vmbr0` is operational and carries the Proxmox host default route.
- The Proxmox management interface is configured statically on `vmbr0`.
- Wireless interface `wlo1` exists but is currently down and is not used by Project Aegis.
- VM ID `100` has an active Proxmox-generated tap and firewall bridge path.
- `tap100i0`, `fwbr100i0`, `fwpr100p0`, and `fwln100i0` are dynamic Proxmox-managed interfaces associated with VM ID `100`; they do not receive separate persistent Project Aegis asset identifiers.
- The persistent configuration contains an `iface nic1 inet manual` entry, but no active `nic1` interface appeared in the runtime interface list. This is recorded as a configuration-reconciliation item rather than an active asset.

## Security and Evidence Handling

- MAC addresses were intentionally excluded.
- The default gateway address was sanitized.
- No public IP addresses, credentials, tokens, or private keys were collected.
- Temporary Proxmox interface names are retained because they demonstrate the active VM firewall data path without exposing sensitive addressing.
