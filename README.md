# Project Aegis Security Lab

## Simulated ISSO and Risk Management Framework Portfolio

Project Aegis Security Lab is a privately operated cybersecurity homelab designed to simulate the governance, documentation, security engineering, assessment, and continuous monitoring activities associated with a U.S. Government information system.

The project is being developed as a professional portfolio demonstrating practical Information System Security Officer responsibilities, including:

- Risk Management Framework activities
- System security documentation
- Security control implementation and assessment
- Vulnerability and POA&M management
- Continuous monitoring
- Incident response
- Change management
- Risk reporting
- Authorization to Operate preparation

> **Current phase:** Phase 1 — System Understanding  
> **Authorization status:** Not Authorized — System Definition in Progress

---

## System Identity

| Field | Value |
|---|---|
| System name | Project Aegis Security Lab |
| System identifier | PASL |
| System type | General-support cybersecurity training and testing environment |
| Hosting platform | Privately owned Proxmox VE homelab |
| Information permitted | Synthetic, public, and lab-generated information only |
| Information prohibited | Classified information, CUI, customer data, production credentials, and employer information |

### Mission

The mission of Project Aegis is to provide a realistic, repeatable, and professionally documented environment for developing practical ISSO, ISSE, and security architecture skills.

---

## Current Environment

| Component | Function | Status |
|---|---|---|
| Proxmox VE | Virtualization platform and system host | Operational |
| Kali Linux | Security testing and administrative workstation | Operational |
| Windows Server | Active Directory, DNS, DHCP, and administrative services | Planned |
| Windows 11 | Domain-joined user workstation | Planned |
| Ubuntu Server | Linux application or infrastructure server | Planned |
| Nessus Essentials | Vulnerability scanning | Planned |
| Wazuh | Endpoint monitoring and security analytics | Planned |
| Splunk | Centralized logging and analysis | Planned |

Existing Proxmox, Kali Linux, networking, storage, firewall, and Raspberry Pi documentation will be retained as technical evidence and migrated into the new RMF-centered structure.

---

## Governance Baseline

Project Aegis will primarily use:

- NIST SP 800-37 Rev. 2 — Risk Management Framework
- NIST SP 800-53 Rev. 5 — Security and Privacy Controls
- NIST SP 800-53A Rev. 5 — Control Assessment Procedures
- FIPS 199 — Security Categorization
- FIPS 200 — Minimum Security Requirements
- NIST SP 800-30 — Risk Assessment
- NIST SP 800-34 — Contingency Planning
- NIST SP 800-61 — Incident Handling
- NIST SP 800-128 — Configuration Management
- NIST SP 800-137 — Continuous Monitoring

Additional implementation guidance may include DISA STIGs, CIS Benchmarks, MITRE ATT&CK, CISA guidance, and vendor documentation.

This project simulates federal practices but does not claim formal compliance, certification, or authorization.

---

## Project Roles

Because this is a home lab, some roles are combined. This limitation will be documented and addressed through version control, evidence retention, separate user and administrator accounts, formal change records, and simulated independent assessment.

| Role | Assignment |
|---|---|
| System Owner | Javier Delgado |
| Information System Security Officer | Javier Delgado |
| System Administrator | Javier Delgado |
| Network Administrator | Javier Delgado |
| Information System Security Manager | Simulated mentor role |
| Security Control Assessor | Simulated independent auditor role |
| Authorizing Official | Simulated executive role |

---

## Project Phases

1. **System Understanding** — system description, asset inventory, network diagram, authorization boundary, and data-flow diagram.
2. **Security Documentation** — System Security Plan, roles, hardware and software inventories, categorization, and data classification.
3. **Risk Management** — risk assessment, risk register, threat assessment, and Business Impact Analysis.
4. **Security Controls** — implement, document, and map technical and administrative controls to NIST SP 800-53.
5. **Vulnerability Management** — scan, validate, prioritize, remediate, verify, and track findings through POA&Ms.
6. **Continuous Monitoring** — establish recurring reviews, metrics, reporting, and ongoing control awareness.
7. **Incident Response** — conduct tabletop and technical exercises and document outcomes.
8. **Change Management** — require security-impact analysis, testing, rollback, approval, and validation for major changes.
9. **Mock Audit** — perform an independent-style assessment and correct deficiencies.
10. **Mock Authorization** — prepare residual-risk reporting and a simulated authorization decision.

See [`ROADMAP.md`](ROADMAP.md) for the detailed project sequence.

---

## Repository Organization

```text
project-aegis/
├── README.md
├── ROADMAP.md
├── CHANGELOG.md
├── MIGRATION.md
├── LICENSE
├── docs/
│   ├── 00-project-governance/
│   ├── 01-system-understanding/
│   ├── 02-security-documentation/
│   ├── 03-risk-management/
│   ├── 04-security-controls/
│   ├── 05-vulnerability-management/
│   ├── 06-continuous-monitoring/
│   ├── 07-incident-response/
│   ├── 08-change-management/
│   ├── 09-mock-audit/
│   ├── 10-authorization/
│   └── 90-technical-evidence/
├── diagrams/
├── evidence/
├── templates/
└── scripts/
```

The structure will be implemented incrementally. Existing technical material will not be deleted merely because it predates the ISSO transition.

---

## Documentation Standard

Major documents should include, when applicable:

1. Objective
2. Scope
3. Roles and responsibilities
4. Assumptions
5. Procedures or methodology
6. Findings
7. Risk discussion
8. Recommendations
9. Evidence
10. Lessons learned
11. Revision history

Security claims must be supported by reproducible and sanitized evidence such as command output, configuration exports, logs, scan reports, diagrams, test results, and change records.

---

## Security and Privacy Rules

This public repository must not contain:

- Passwords, tokens, private keys, or secrets
- Classified information or Controlled Unclassified Information
- Employer or customer information
- Production credentials or production configurations
- Sensitive personally identifiable information
- Unredacted vulnerability reports
- Unnecessary public IP addresses, internal names, or device identifiers

All identities, records, and business data used in the lab will be fictional or synthetic.

---

## Professional Development Goals

Project Aegis supports preparation for roles including:

- Information System Security Officer
- Information Systems Security Engineer
- Infrastructure Security Engineer
- Security Control Assessor
- Security Engineer
- Security Architect

It also supports preparation for CompTIA Security+, ISC2 SSCP, ISC2 CGRC, and eventually CISSP.

The project demonstrates preparation and practical capability. It does not represent employment experience or an official government authorization.

---

## Disclaimer

Project Aegis Security Lab is an independent educational homelab. It is not a U.S. Government or Department of Defense information system, an official security assessment, a production system, or a formally authorized environment.

All testing is limited to systems owned by the project author or explicitly authorized for testing.
