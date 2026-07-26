# Project Aegis Repository Migration Plan

## Objective

Transition the existing Project Aegis cybersecurity homelab into an RMF-centered ISSO portfolio without discarding useful technical evidence.

## Migration Principles

1. Preserve valid technical work.
2. Separate governance documents from technical evidence.
3. Remove obsolete placeholders only after replacement content exists.
4. Avoid publishing secrets, sensitive identifiers, employer information, or customer information.
5. Keep changes traceable through focused Git commits.
6. Do not rewrite Git history unless a secret must be removed.

## Existing Content Review

The current repository already contains useful material, including:

- Proxmox host inventory
- Storage documentation
- VM inventory and naming standards
- Current network architecture
- Proxmox firewall baseline
- Kali Linux ISO preparation and system baseline
- Raspberry Pi companion-project documentation
- Screenshots supporting technical implementation

These materials are useful evidence for future RMF documentation, especially asset management, configuration management, boundary definition, network architecture, and control implementation.

## Content Disposition

| Existing Area | Disposition | Future Location or Use |
|---|---|---|
| `docs/00-introduction/` | Supersede and archive | Relevant material incorporated into root README, ROADMAP, and governance documents |
| `docs/01-proxmox/` | Retain | Technical evidence for system inventory, boundary, configuration management, and controls |
| `docs/02-linux/` | Retain | Linux baseline and assessment evidence |
| `docs/03-networking/` | Retain or consolidate | Technical network implementation evidence |
| `docs/04-kali/` | Retain | Security workstation build evidence |
| `docs/05-ubuntu/` | Retain for future work | Ubuntu implementation evidence |
| `docs/06-windows/` | Retain for future work | Windows implementation evidence |
| `docs/07-vulnerability-management/` | Migrate | Replace with RMF Phase 5 documentation |
| `docs/08-active-directory/` | Retain for future work | Identity and access-control implementation evidence |
| `docs/09-pfsense/` | Retain for future work | Firewall and segmentation evidence |
| `docs/10-wazuh/` | Retain for future work | Logging and continuous-monitoring evidence |
| `docs/11-threat-hunting/` | Reclassify | Supporting incident-response and monitoring exercises |
| `docs/12-cloud-security/` | Defer | Future extension outside the initial authorization boundary unless formally added |
| `docs/13-capstone/` | Supersede | Replaced by mock audit and authorization phases |
| `docs/90-raspberry-pi-companion/` | Keep separate | Out-of-boundary companion project unless later incorporated |
| `reports/` | Consolidate gradually | Assessment reports, incident reports, executive summaries, and authorization outputs |
| `scan-results/` | Rename conceptually to evidence | Sanitized vulnerability-scan evidence only |
| `screenshots/` | Retain | Sanitized implementation and assessment evidence |
| `configs/` | Retain | Sanitized configuration evidence |
| `scripts/` | Retain | Inventory, validation, auditing, and maintenance automation |

## Target Documentation Structure

```text
docs/
├── 00-project-governance/
├── 01-system-understanding/
├── 02-security-documentation/
├── 03-risk-management/
├── 04-security-controls/
├── 05-vulnerability-management/
├── 06-continuous-monitoring/
├── 07-incident-response/
├── 08-change-management/
├── 09-mock-audit/
├── 10-authorization/
└── 90-technical-evidence/
```

## Cleanup Actions

### Completed in Transition Branch

- Created a dedicated transition branch.
- Replaced the general cybersecurity README with an ISSO/RMF-focused README.
- Added a ten-phase ISSO roadmap.
- Added this migration plan.

### Remaining Cleanup

- Create the new phase directories as work begins.
- Add a repository documentation index.
- Move or cross-reference existing technical documents only when their target phase is active.
- Review existing documents for contradictions, stale statements, and incomplete fields.
- Remove empty legacy directories after confirming they are no longer needed.
- Review screenshots and configuration files for sensitive data.
- Standardize document headers, revision histories, and evidence references.

## Known Documentation Issues to Correct

The initial review identified several issues that should be corrected during migration:

- Some documents mix planned and current-state information.
- The Proxmox host inventory contains contradictory statements about whether the secondary HDD is mounted and configured.
- Some baseline fields are incomplete, such as Linux distribution and logical CPU count.
- Some technical documents contain formatting artifacts or duplicated narrative.
- The existing roadmap is infrastructure-oriented rather than RMF-oriented.
- Several empty placeholder directories no longer match the new project phases.

These are documentation-quality findings, not failures. They will be tracked and corrected as each source document is incorporated into the ISSO portfolio.

## Approval Status

**Status:** Proposed transition structure  
**Branch:** `chore/isso-project-transition`  
**Approval:** Pending repository-owner review and merge
