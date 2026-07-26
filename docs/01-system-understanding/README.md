# Phase 1 — System Understanding

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.1 |
| Status | Draft |
| Current Milestone | Establish system identity and scope |

## Objective

Define the system mission, components, dependencies, users, information flows, and authorization boundary before security controls are selected or assessed.

## Required Deliverables

- [ ] `system-description.md`
- [ ] `asset-inventory.md`
- [ ] `network-diagram.md`
- [ ] `authorization-boundary.md`
- [ ] `data-flow-diagram.md`
- [ ] `external-services-and-dependencies.md`

## Existing Evidence Available

The following existing repository artifacts may support Phase 1:

- `../01-proxmox/host-inventory.md`
- `../01-proxmox/vm-inventory.md`
- `../01-proxmox/vm-naming-standard.md`
- `../01-proxmox/storage-plan.md`
- `../01-proxmox/current-network-architecture.md`
- `../../diagrams/network/current-network-architecture.md`
- `../02-linux/system-baseline.md`
- `../04-kali/kali-iso-preparation.md`

These files are supporting evidence. They do not replace the Phase 1 governance deliverables.

## Phase Exit Criteria

Phase 1 is complete when:

- The system purpose and mission are clearly stated.
- In-scope and out-of-scope components are identified.
- Active assets and dependencies are inventoried.
- Current network architecture is accurate.
- Data flows are documented.
- The authorization boundary is documented and defensible.
- Contradictory or stale technical records have been corrected or marked superseded.
