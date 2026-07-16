# Project Aegis Roadmap

## Overview

Project Aegis is organized into milestones that gradually build a small enterprise-style cybersecurity environment.

Each milestone includes:

* Technical implementation
* Security concepts
* Documentation
* Validation
* Troubleshooting
* GitHub updates
* Portfolio deliverables
* Resume and interview material

The project is designed to move from foundational infrastructure skills into vulnerability management, identity security, monitoring, detection engineering, cloud security, and security architecture.

---

## Milestone 0: Repository and Documentation Foundation

### Status

**Complete**

### Objectives

* Create the Project Aegis GitHub repository
* Establish a professional folder structure
* Create the main project README
* Document the project overview
* Define learning objectives
* Create the project roadmap
* Establish commit-message conventions

### Deliverables

* Root `README.md`
* `project-overview.md`
* `learning-objectives.md`
* `roadmap.md`
* Initial repository structure
* Initial Git history

### Example Commit Messages

```text
docs: add professional project README
docs: add Project Aegis overview
docs: add Project Aegis learning objectives
docs: add Project Aegis roadmap
```

---

## Milestone 1: Proxmox Foundation

### Goal

Document and organize the Proxmox host before deploying the core lab systems.

### Objectives

* Review the Proxmox interface
* Document the physical host
* Identify available storage
* Review network interfaces and bridges
* Understand VM and container concepts
* Understand CPU, memory, and storage allocation
* Create a virtual machine naming convention
* Create an asset inventory
* Learn how snapshots and backups differ
* Establish a basic change-management process

### Planned Tasks

* Record the Proxmox version
* Record the host IP address privately
* Document CPU, memory, disks, and network interfaces
* Review the default `vmbr0` bridge
* Identify SSD and HDD storage roles
* Create a VM inventory template
* Create a host inventory document
* Take screenshots of the Proxmox dashboard
* Create the first architecture diagram

### Deliverables

* Proxmox host inventory
* Storage plan
* Network overview
* VM naming standard
* Initial architecture diagram
* Screenshot evidence
* Proxmox troubleshooting notes

### Portfolio Skills

* Virtualization
* Infrastructure documentation
* Capacity planning
* Change management
* Technical writing

---

## Milestone 2: Core Systems Deployment

### Goal

Deploy the foundational Linux and Windows systems used throughout the project.

### Systems

* Kali Linux
* Ubuntu Server
* Windows workstation

### Objectives

* Upload ISO images
* Create virtual machines
* Allocate CPU, memory, disk, and network resources
* Install operating systems
* Apply updates
* Create administrative and standard user accounts
* Configure hostnames
* Verify network connectivity
* Create baseline snapshots
* Document each installation

### Planned Tasks

#### Kali Linux

* Create the Kali VM
* Install Kali Linux
* Apply updates
* Install Git and Nmap
* Verify internet access
* Take a clean snapshot

#### Ubuntu Server

* Create the Ubuntu VM
* Install Ubuntu Server
* Enable OpenSSH
* Install Apache
* Verify SSH access
* Verify the Apache test page
* Take a clean snapshot

#### Windows Workstation

* Create the Windows VM
* Install Windows
* Apply updates
* Review Windows Defender Firewall
* Review Event Viewer
* Install basic administration tools
* Take a clean snapshot

### Deliverables

* VM inventory
* Installation notes
* Baseline screenshots
* Network connectivity tests
* Operating-system baseline documentation
* Lessons learned

### Portfolio Skills

* Linux administration
* Windows administration
* Virtual machine deployment
* Systems documentation
* Troubleshooting

---

## Milestone 3: Networking Fundamentals

### Goal

Understand and document how systems communicate inside the lab.

### Objectives

* Understand IP addressing
* Understand subnet masks and gateways
* Understand DNS and DHCP
* Understand TCP and UDP
* Identify common ports and protocols
* Review Proxmox virtual networking
* Test connectivity between systems
* Document network paths
* Identify trust boundaries
* Create an updated network diagram

### Planned Tasks

* Record each VM hostname
* Record private lab IP addresses
* Test connectivity with `ping`
* Review routes
* Review DNS resolution
* Identify listening services
* Review firewall behavior
* Document permitted communication paths
* Create a network flow diagram

### Deliverables

* IP inventory
* Network diagram
* Service inventory
* Common ports reference
* Connectivity test results
* Network troubleshooting notes

### Portfolio Skills

* TCP/IP fundamentals
* Network documentation
* Service enumeration
* Trust-boundary identification
* Infrastructure troubleshooting

---

## Milestone 4: Vulnerability Management

### Goal

Build a complete vulnerability management workflow.

### Objectives

* Discover hosts
* Enumerate ports and services
* Install Greenbone/OpenVAS
* Run baseline scans
* Interpret CVEs and CVSS scores
* Prioritize findings
* Create remediation plans
* Apply fixes
* Rescan systems
* Measure improvement
* Produce technical and executive reports

### Planned Tasks

* Run initial Nmap scans
* Save scan results
* Install Greenbone/OpenVAS
* Configure scan targets
* Perform vulnerability scans
* Export reports
* Select findings for remediation
* Patch Ubuntu
* Patch Windows
* Review exposed services
* Repeat Nmap and Greenbone scans
* Compare before-and-after results

### Deliverables

* Nmap scan results
* Greenbone scan summaries
* Vulnerability prioritization worksheet
* Remediation notes
* Before-and-after comparison
* Vulnerability assessment report
* Executive summary

### Portfolio Skills

* Vulnerability assessment
* Risk prioritization
* Patch management
* Security hardening
* Validation testing
* Security reporting

---

## Milestone 5: Active Directory and Identity Security

### Goal

Build and secure a small Windows domain.

### Systems

* Windows Server
* Active Directory Domain Services
* Domain-joined Windows workstation

### Objectives

* Deploy Windows Server
* Configure Active Directory Domain Services
* Create a domain
* Create organizational units
* Create users and groups
* Join systems to the domain
* Configure Group Policy
* Review administrative privileges
* Apply least privilege
* Document identity risks

### Planned Tasks

* Install Windows Server
* Promote the server to a domain controller
* Configure DNS
* Create organizational units
* Create test users
* Create security groups
* Join the Windows workstation to the domain
* Configure password policies
* Configure account lockout policies
* Review privileged groups
* Document identity architecture

### Deliverables

* Active Directory diagram
* User and group inventory
* Group Policy documentation
* Identity risk register
* Administrative access review
* Hardening recommendations

### Portfolio Skills

* Identity and access management
* Active Directory
* Group Policy
* Least privilege
* Security architecture
* Access-control documentation

---

## Milestone 6: Network Security and Segmentation

### Goal

Introduce controlled network boundaries and firewall policies.

### System

* pfSense

### Objectives

* Deploy pfSense
* Understand routing and NAT
* Create network segments
* Configure firewall rules
* Restrict administrative access
* Document allowed network flows
* Test segmentation
* Apply default-deny principles

### Planned Segments

* Management
* Servers
* Workstations
* Security tools
* DMZ
* Future cloud or test segment

### Planned Tasks

* Deploy pfSense
* Configure WAN and LAN interfaces
* Create isolated lab networks
* Configure DHCP
* Configure DNS
* Create firewall policies
* Test permitted traffic
* Test blocked traffic
* Document trust boundaries

### Deliverables

* Segmented network diagram
* Firewall rule table
* Network flow matrix
* Segmentation test results
* Architecture Decision Record
* Risk and control analysis

### Portfolio Skills

* Firewall administration
* Network segmentation
* Routing
* NAT
* Trust-boundary design
* Secure architecture

---

## Milestone 7: Centralized Logging and SIEM

### Goal

Collect and analyze security telemetry from the lab.

### Systems

* Wazuh
* Sysmon
* Windows agent
* Linux agent

### Objectives

* Deploy Wazuh
* Install endpoint agents
* Collect Windows and Linux logs
* Configure Sysmon
* Review alerts
* Build dashboards
* Understand event correlation
* Document monitoring coverage

### Planned Tasks

* Install Wazuh
* Connect Ubuntu
* Connect Windows
* Install Sysmon
* Review authentication logs
* Review process execution
* Review file-integrity events
* Build basic dashboards
* Test alert generation

### Deliverables

* Monitoring architecture diagram
* Log-source inventory
* Agent deployment notes
* Dashboard screenshots
* Alert investigation notes
* Monitoring-gap assessment

### Portfolio Skills

* SIEM administration
* Endpoint monitoring
* Log analysis
* Windows telemetry
* Linux logging
* Security operations

---

## Milestone 8: Detection Engineering

### Goal

Create and validate security detections.

### Objectives

* Understand detection logic
* Identify useful telemetry
* Generate safe test activity
* Create detection rules
* Validate alert behavior
* Reduce false positives
* Map detections to MITRE ATT&CK

### Planned Detection Scenarios

* Repeated failed logins
* New local administrator
* Suspicious PowerShell activity
* Unexpected service creation
* New scheduled task
* Security-log clearing
* Sensitive file modification

### Deliverables

* Detection rules
* Test procedures
* Validation screenshots
* False-positive notes
* MITRE ATT&CK mappings
* Detection coverage matrix

### Portfolio Skills

* Detection engineering
* Rule development
* Alert validation
* MITRE ATT&CK
* Security analytics
* Technical documentation

---

## Milestone 9: Threat Hunting and Incident Response

### Goal

Investigate suspicious activity and produce structured reports.

### Objectives

* Develop threat-hunting hypotheses
* Search logs
* Build timelines
* Identify suspicious behavior
* Determine scope and impact
* Recommend containment and remediation
* Write technical and executive reports

### Planned Scenarios

* Brute-force authentication attempt
* Suspicious PowerShell execution
* Unauthorized privilege change
* Persistence through scheduled tasks
* Unexpected network connection
* Simulated account compromise

### Deliverables

* Threat-hunting reports
* Investigation timelines
* Incident reports
* Evidence tables
* Containment recommendations
* Lessons-learned reports
* Executive summaries

### Portfolio Skills

* Threat hunting
* Incident investigation
* Evidence analysis
* Timeline development
* Incident reporting
* Executive communication

---

## Milestone 10: Cloud Security

### Goal

Extend Project Aegis into a small cloud environment.

### Platform

One of the following:

* Microsoft Azure
* Amazon Web Services

### Objectives

* Understand the shared responsibility model
* Configure cloud identity
* Apply least privilege
* Secure cloud storage
* Enable logging
* Review network exposure
* Document cloud architecture
* Assess cloud security posture

### Planned Tasks

* Create a small cloud environment
* Create users and roles
* Configure MFA
* Review storage permissions
* Enable cloud logging
* Review security recommendations
* Document data flows
* Identify misconfigurations
* Produce a cloud security assessment

### Deliverables

* Cloud architecture diagram
* IAM review
* Security configuration notes
* Logging documentation
* Cloud risk register
* Cloud assessment report

### Portfolio Skills

* Cloud security
* Identity and access management
* Security posture management
* Cloud logging
* Architecture documentation
* Risk assessment

---

## Milestone 11: Security Architecture

### Goal

Evaluate the entire environment from a security-architecture perspective.

### Objectives

* Identify assets and trust boundaries
* Document security requirements
* Review data flows
* Identify risks
* Select controls
* Explain design tradeoffs
* Create Architecture Decision Records
* Recommend future improvements

### Planned Tasks

* Review the lab architecture
* Identify critical assets
* Map data flows
* Review identity boundaries
* Review administrative access
* Review logging coverage
* Review vulnerability-management coverage
* Review recovery assumptions
* Document accepted risks
* Produce a target-state architecture

### Deliverables

* Current-state architecture
* Target-state architecture
* Risk register
* Control mapping
* Architecture Decision Records
* Security architecture review
* Executive recommendation summary

### Portfolio Skills

* Security architecture
* Risk analysis
* Control selection
* Design review
* Tradeoff analysis
* Executive communication

---

## Milestone 12: Enterprise Capstone

### Goal

Combine the entire Project Aegis environment into a complete security-engineering exercise.

### Capstone Activities

* Review the asset inventory
* Review the architecture
* Conduct network discovery
* Perform vulnerability scans
* Generate security events
* Investigate alerts
* Conduct a threat hunt
* Apply remediation
* Validate controls
* Review remaining risks
* Produce final reports

### Final Deliverables

* Complete architecture diagram
* Asset inventory
* Vulnerability assessment
* Remediation plan
* Validation report
* Threat-hunting report
* Incident report
* Risk register
* Security architecture review
* Executive summary
* Final project presentation
* Resume bullets
* Interview stories

### Definition of Completion

Project Aegis will be complete when the lab can be:

1. Explained
2. Rebuilt
3. Secured
4. Monitored
5. Assessed
6. Tested
7. Remediated
8. Documented
9. Presented to technical and nontechnical audiences

---

## Version Roadmap

| Version | Milestone                               |
| ------- | --------------------------------------- |
| `v0.1`  | Repository and documentation foundation |
| `v0.2`  | Proxmox foundation                      |
| `v0.3`  | Core systems deployed                   |
| `v0.4`  | Networking documented                   |
| `v0.5`  | Vulnerability management complete       |
| `v0.6`  | Active Directory complete               |
| `v0.7`  | Network segmentation complete           |
| `v0.8`  | Wazuh monitoring complete               |
| `v0.9`  | Detection and threat hunting complete   |
| `v1.0`  | Enterprise capstone complete            |

---

## Current Focus

**Current milestone:** Milestone 1 — Proxmox Foundation

**Next task:** Document the Proxmox host, storage, and network configuration.
