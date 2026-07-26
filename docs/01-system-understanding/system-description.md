# Project Aegis Security Lab — System Description

| Field | Value |
|---|---|
| System Name | Project Aegis Security Lab |
| System Identifier | PASL |
| Document Owner | Javier Delgado |
| Version | 0.1 |
| Status | Draft |
| Date | 2026-07-25 |
| Authorization Status | Not Authorized — System Definition in Progress |

## 1. Objective

This document provides a high-level description of Project Aegis Security Lab, including its purpose, operating environment, users, system components, dependencies, information types, and major security limitations.

The document establishes the system context needed to support later Risk Management Framework activities, including security categorization, control selection, control implementation, assessment, authorization, and continuous monitoring.

## 2. System Purpose

Project Aegis Security Lab is a privately operated cybersecurity training and assessment environment hosted on a Proxmox VE virtualization platform.

The system is designed to simulate the governance and technical activities associated with an Information System Security Officer supporting a U.S. Government or Department of Defense information system.

Project Aegis supports practical exercises involving:

- Risk Management Framework activities
- Security documentation development
- Security control implementation and validation
- Identity and access management
- Vulnerability scanning and remediation
- Centralized logging and monitoring
- Incident-response exercises
- Configuration and change management
- Risk tracking and POA&M development
- Mock security assessment and authorization preparation

## 3. Mission and Business Functions

The mission of Project Aegis is to provide a realistic, repeatable, and professionally documented environment for developing practical ISSO, ISSE, and security architecture skills.

The system supports the following simulated business functions:

1. Maintain a controlled virtualized cybersecurity environment.
2. Deploy and administer Windows and Linux systems.
3. Implement and document security controls.
4. Collect and analyze security logs and vulnerability information.
5. Conduct authorized security testing against lab-owned assets.
6. Track findings, risks, remediation actions, and system changes.
7. Produce evidence and documentation for a mock authorization package.

## 4. System Environment

Project Aegis is hosted on privately owned consumer hardware running Proxmox Virtual Environment as a bare-metal hypervisor.

The current physical host is an ASUS TUF Gaming Laptop FX504 with:

- Intel Core i7-8750H processor
- 6 physical cores and 12 logical processors
- Approximately 16 GB DDR4 memory
- 256 GB NVMe primary storage
- 1 TB secondary HDD
- One active Ethernet interface used for Proxmox and virtual-machine connectivity

Memory is the primary resource constraint. Virtual machines and security platforms will be operated in limited combinations rather than all at once.

## 5. Current System Components

| Component | Role | Current Status |
|---|---|---|
| Proxmox VE Host | Virtualization platform and system management | Operational |
| Kali Linux VM | Security administration and authorized testing workstation | Operational |
| Proxmox Linux Bridge `vmbr0` | Current virtual network connectivity | Operational |
| Secondary HDD Storage | ISO, backup, archive, and supporting storage | Operational; documentation requires validation |
| GitHub Repository | Public portfolio documentation and version control | Operational |

## 6. Planned System Components

| Component | Planned Role | Status |
|---|---|---|
| Windows Server | Active Directory, DNS, DHCP, and centralized identity services | Planned |
| Windows 11 VM | Domain-joined standard-user workstation | Planned |
| Ubuntu Server | Linux infrastructure server and assessment target | Planned |
| Nessus Essentials | Vulnerability scanning and reporting | Planned |
| Wazuh | Endpoint telemetry, security monitoring, and alerting | Planned |
| Splunk | Centralized log collection and analysis | Planned |
| Microsoft Defender | Windows endpoint protection and security telemetry | Planned |
| pfSense or equivalent | Routing, firewall enforcement, and segmentation | Planned |
| Backup Repository | Recovery of system configurations and selected virtual machines | Planned |
| Security Onion | Network security monitoring | Future consideration |

Planned components are not part of the active system until they are deployed, documented, validated, and approved through the project change-management process.

## 7. Current Network Environment

The current system uses a bridged network configuration.

The Proxmox host and connected virtual machines use the Linux bridge `vmbr0`, which connects to the trusted home network through the physical Ethernet interface.

Current characteristics include:

- Home-router-provided DHCP
- Internet access for updates and installation
- No dedicated lab firewall
- No enforced segmentation between lab assets and other home-network devices
- No externally forwarded Proxmox administrative ports
- Sanitized addressing in public documentation

This design is temporary. Sensitive testing and intentionally vulnerable systems will not be introduced until isolated networking and firewall enforcement are implemented.

## 8. Users and Roles

| Role | Assignment | Access or Responsibility |
|---|---|---|
| System Owner | Javier Delgado | Defines system mission, resources, and acceptable risk |
| ISSO | Javier Delgado | Maintains security documentation, findings, and security posture |
| System Administrator | Javier Delgado | Configures and maintains system components |
| Network Administrator | Javier Delgado | Maintains virtual networking and firewall configurations |
| Privileged Administrator | Dedicated lab administrative identity | Performs authorized privileged operations |
| General User | Fictional lab identity | Performs standard user activities |
| ISSM | Simulated mentor role | Provides governance review and oversight |
| Security Control Assessor | Simulated independent assessor role | Evaluates controls and evidence |
| Authorizing Official | Simulated executive role | Makes the mock authorization decision |

Because this is a home lab, multiple normally separated duties are performed by the same person. This limitation will be tracked and addressed through documented approvals, version control, separate account types, evidence retention, and mock independent review.

## 9. Information Types

Project Aegis processes only synthetic, publicly available, or lab-generated information.

Permitted information includes:

- Fictional user and administrator accounts
- Synthetic personnel and business records
- Test files and documents
- Public vulnerability and threat information
- Lab-generated logs and network traffic
- Sanitized screenshots and configuration excerpts
- Security assessment evidence produced within the lab

Project Aegis is not authorized to process:

- Classified information
- Controlled Unclassified Information
- Export-controlled information
- Proprietary customer or employer information
- Real government system data
- Production credentials, tokens, or private keys
- Sensitive personally identifiable information
- Nonpublic network diagrams or customer configurations

## 10. External Services and Dependencies

Project Aegis currently depends on:

| Dependency | Purpose | Managed Within Boundary? |
|---|---|---|
| Home Router | Gateway, DHCP, and internet connectivity | No |
| Internet Service Provider | External network connectivity | No |
| GitHub | Public documentation and version control | No |
| Vendor Update Repositories | Operating-system and software updates | No |
| Proxmox Project Repositories | Hypervisor packages and updates | No |
| NIST, CISA, MITRE, and vendor guidance | Security standards and reference material | No |

External services support the lab but are not administered as Project Aegis system components. Their trust relationships and effects on the authorization boundary will be documented separately.

## 11. Preliminary System Scope

### In Scope

- Project Aegis Proxmox host
- Virtual machines and virtual networks assigned to the project
- Project-specific administrative and test accounts
- Security tools deployed for the project
- Project-generated logs, findings, configurations, and evidence
- Project documentation and sanitized GitHub artifacts
- Project backup configurations and selected backup data

### Out of Scope

- Personal family devices
- Employer-owned or customer-owned systems
- Home entertainment and personal financial systems
- Internet service provider infrastructure
- Production cloud environments
- External systems not administered as part of Project Aegis
- Real government information or operational data

The detailed authorization boundary will be developed as a separate Phase 1 deliverable.

## 12. Security Considerations

Current security concerns include:

- The lab is not yet segmented from the home network.
- Proxmox and Kali Linux currently share the home-network trust zone.
- The same individual performs several governance and administrative roles.
- Consumer hardware limits redundancy, availability, and physical-security controls.
- Some existing technical records contain stale or contradictory status information.
- The environment is not authorized for sensitive or operational data.

Compensating practices include:

- Restricting testing to lab-owned systems
- Avoiding internet exposure of administrative services
- Sanitizing all public evidence
- Using strong, unique administrative credentials
- Maintaining version-controlled documentation
- Delaying higher-risk exercises until segmentation is implemented
- Recording significant changes and risks

## 13. Assumptions and Limitations

- Project Aegis is an educational simulation and not an official government information system.
- Organizational and personnel controls may be represented through documentation rather than enterprise services.
- Some controls may be tailored, simulated, inherited, or marked not applicable.
- The environment does not provide production-level availability or geographic redundancy.
- Planned systems may change based on hardware, licensing, and project requirements.
- Current technical documentation will be validated before it is treated as authoritative evidence.

## 14. Related Documentation

- [`README.md`](../../README.md)
- [`ROADMAP.md`](../../ROADMAP.md)
- [`MIGRATION.md`](../../MIGRATION.md)
- [`Phase 1 README`](README.md)
- [`Proxmox Host Inventory`](../01-proxmox/host-inventory.md)
- [`Virtual Machine Inventory`](../01-proxmox/vm-inventory.md)
- [`Current Network Architecture`](../01-proxmox/current-network-architecture.md)
- [`Kali Linux System Baseline`](../02-linux/system-baseline.md)

## 15. Open Validation Items

The following items must be verified before this document can be approved:

- [ ] Confirm the exact operational status of the secondary HDD storage.
- [ ] Confirm Kali Linux's current VM ID, assigned RAM, disk size, and network bridge.
- [ ] Confirm whether the Kali VM uses DHCP or a reserved/static address.
- [ ] Confirm whether any services other than Proxmox and Kali Linux are currently active.
- [ ] Resolve stale `Planned` entries in the existing virtual-machine inventory.

## 16. Revision History

| Version | Date | Author | Change Summary | Status |
|---|---|---|---|---|
| 0.1 | 2026-07-25 | Javier Delgado | Initial system-description draft | Draft |
