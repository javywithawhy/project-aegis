# Project Aegis Documentation Index

This directory contains the governance, security, assessment, and technical documentation for Project Aegis Security Lab.

## Primary RMF Documentation

| Phase | Directory | Purpose |
|---|---|---|
| 0 | `00-project-governance/` | Project charter, document standards, roles, assumptions, and governance records |
| 1 | `01-system-understanding/` | System description, asset inventory, network diagram, authorization boundary, and data flows |
| 2 | `02-security-documentation/` | System Security Plan, inventories, user roles, categorization, and classification |
| 3 | `03-risk-management/` | Risk assessment, risk register, threat assessment, and Business Impact Analysis |
| 4 | `04-security-controls/` | Control implementation statements, mappings, baselines, and evidence references |
| 5 | `05-vulnerability-management/` | Scan policy, findings, remediation, verification, and POA&Ms |
| 6 | `06-continuous-monitoring/` | Monitoring strategy, review schedules, metrics, and recurring reports |
| 7 | `07-incident-response/` | Incident plan, tabletop exercises, reports, timelines, and lessons learned |
| 8 | `08-change-management/` | Change requests, impact analyses, test plans, approvals, and validation |
| 9 | `09-mock-audit/` | Assessment plan, evidence requests, findings, and corrective actions |
| 10 | `10-authorization/` | Executive summary, residual risks, assessment summary, and authorization decision |
| 90 | `90-technical-evidence/` | Technical build records, configurations, platform baselines, and supporting implementation evidence |

## Legacy Technical Documentation

The repository currently contains technical directories created before the ISSO transition, including Proxmox, Linux, Kali, networking, Active Directory, pfSense, Wazuh, cloud-security, and Raspberry Pi material.

These documents remain valid project artifacts. They will be migrated or cross-referenced incrementally rather than deleted in bulk. See [`../MIGRATION.md`](../MIGRATION.md) for the disposition plan.

## Document Status Labels

Documents should use one of the following statuses:

- **Draft** — under development and not yet reviewed.
- **Under Review** — ready for mentor, assessor, or stakeholder review.
- **Approved** — accepted as the current project record.
- **Superseded** — retained for traceability but replaced by a newer document.
- **Archived** — no longer active but preserved as historical evidence.

## Minimum Document Header

New major documents should begin with:

```markdown
# Document Title

| Field | Value |
|---|---|
| System | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.1 |
| Status | Draft |
| Last Updated | YYYY-MM-DD |
```

## Evidence Handling

Evidence must be sanitized, clearly named, and referenced from the document it supports. Screenshots should not substitute for stronger evidence such as configuration exports, command output, logs, or test results when those are available.
