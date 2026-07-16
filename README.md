# Project Aegis

**A hands-on cybersecurity homelab and portfolio focused on infrastructure security, vulnerability management, detection engineering, and secure architecture.**

Project Aegis documents the design and construction of a small enterprise-style security environment running on Proxmox. The project combines technical labs with professional documentation, including architecture diagrams, security assessments, remediation plans, runbooks, and executive summaries.

The objective is not simply to install cybersecurity tools. Each phase examines why a technology is deployed, what security problem it addresses, how it should be configured, and how its effectiveness can be measured.

---

## Project Status

**Current version:** `v0.1`
**Current milestone:** Foundation
**Current activity:** Repository setup and Proxmox documentation

### Progress

* [x] Install Proxmox
* [x] Create the Project Aegis repository
* [x] Create the initial repository structure
* [ ] Document the Proxmox host
* [ ] Configure lab storage
* [ ] Document virtual networking
* [ ] Deploy Kali Linux
* [ ] Deploy Ubuntu Server
* [ ] Deploy a Windows workstation
* [ ] Perform initial network discovery
* [ ] Build the vulnerability management workflow
* [ ] Deploy Active Directory
* [ ] Deploy centralized logging and monitoring
* [ ] Implement network segmentation
* [ ] Complete the enterprise security capstone

---

## Project Objectives

Project Aegis is designed to demonstrate practical experience in:

* Proxmox virtualization
* Linux and Windows administration
* Network discovery and service enumeration
* Vulnerability scanning and remediation
* Security hardening
* Active Directory administration and security
* Network segmentation and firewall configuration
* Centralized logging and SIEM administration
* Detection engineering
* Threat hunting
* Incident investigation
* Risk assessment
* Security architecture
* Technical and executive reporting
* Git and GitHub documentation workflows

---

## Lab Hardware

| Component         | Specification                |
| ----------------- | ---------------------------- |
| Platform          | ASUS TUF Gaming Laptop FX504 |
| Processor         | Intel Core i7-8750H          |
| CPU capacity      | 6 cores / 12 threads         |
| Memory            | 16 GB DDR4                   |
| Primary storage   | 256 GB PCIe SSD              |
| Secondary storage | 1 TB HDD                     |
| Graphics          | NVIDIA GeForce GTX 1060 6 GB |
| Hypervisor        | Proxmox Virtual Environment  |

Because memory is the primary resource constraint, virtual machines will be powered on only when required for the current lab activity.

---

## Planned Lab Environment

The completed environment will include:

| System              | Purpose                                               |
| ------------------- | ----------------------------------------------------- |
| Proxmox VE          | Bare-metal virtualization platform                    |
| Kali Linux          | Security testing and assessment workstation           |
| Ubuntu Server       | Linux server and vulnerability target                 |
| Windows Workstation | Windows administration, logging, and endpoint testing |
| Windows Server      | Active Directory Domain Services                      |
| pfSense             | Routing, firewalling, and network segmentation        |
| Greenbone/OpenVAS   | Vulnerability scanning and management                 |
| Wazuh               | SIEM, endpoint monitoring, and alerting               |
| Sysmon              | Enhanced Windows security telemetry                   |
| Azure or AWS        | Cloud security and identity labs                      |

---

## Target Architecture

```text
                         Internet
                            |
                     Home Network
                            |
                     Proxmox Host
                            |
              +-------------+-------------+
              |                           |
       Management Network          Security Lab Network
              |                           |
         Proxmox GUI              pfSense Firewall
                                          |
                        +-----------------+-----------------+
                        |                 |                 |
                    Servers          Workstations       Monitoring
                        |                 |                 |
                 Ubuntu Server       Windows Client        Wazuh
                 Windows Server      Kali Linux
                 Active Directory
```

The architecture will evolve as networking, identity, monitoring, and segmentation are introduced.

---

## Project Roadmap

### Milestone 1: Foundation

* Document the Proxmox host
* Configure storage
* Understand virtual machines and containers
* Configure basic virtual networking
* Learn Git and Markdown workflows
* Create asset and VM inventories

### Milestone 2: Core Systems

* Deploy Kali Linux
* Deploy Ubuntu Server
* Deploy a Windows workstation
* Configure user accounts
* Apply operating-system updates
* Establish configuration baselines

### Milestone 3: Vulnerability Management

* Discover hosts with Nmap
* Enumerate open ports and services
* Deploy Greenbone/OpenVAS
* Run baseline vulnerability scans
* Review CVEs and CVSS scores
* Prioritize findings by risk
* Remediate selected vulnerabilities
* Perform validation scans
* Produce a final assessment report

### Milestone 4: Enterprise Identity

* Deploy Windows Server
* Configure Active Directory Domain Services
* Create organizational units, users, and groups
* Join Windows systems to the domain
* Configure Group Policy
* Review identity and privilege risks
* Harden the domain environment

### Milestone 5: Security Monitoring

* Deploy Wazuh
* Install endpoint agents
* Configure Sysmon
* Collect Linux and Windows logs
* Create dashboards
* Investigate security alerts
* Map activity to MITRE ATT&CK

### Milestone 6: Network Security

* Deploy pfSense
* Create isolated network segments
* Configure firewall policies
* Review NAT, DHCP, and DNS
* Restrict administrative access
* Document network trust boundaries

### Milestone 7: Detection and Threat Hunting

* Simulate failed-login activity
* Generate safe PowerShell telemetry
* Detect suspicious process execution
* Investigate persistence techniques
* Write basic detection rules
* Conduct structured threat hunts
* Produce incident reports

### Milestone 8: Cloud Security

* Configure a small Azure or AWS environment
* Review identity and access management
* Configure secure administrative access
* Enable logging and monitoring
* Review storage permissions
* Document a cloud security architecture

### Milestone 9: Enterprise Capstone

* Integrate identity, networking, monitoring, and vulnerability management
* Conduct a full security assessment
* Generate and investigate security events
* Remediate identified weaknesses
* Validate implemented controls
* Produce technical and executive reports

---

## Repository Structure

```text
project-aegis/
|
|-- README.md
|-- CHANGELOG.md
|-- LICENSE
|-- .gitignore
|
|-- docs/
|   |-- 00-introduction/
|   |-- 01-proxmox/
|   |-- 02-linux/
|   |-- 03-networking/
|   |-- 04-kali/
|   |-- 05-ubuntu/
|   |-- 06-windows/
|   |-- 07-vulnerability-management/
|   |-- 08-active-directory/
|   |-- 09-pfsense/
|   |-- 10-wazuh/
|   |-- 11-threat-hunting/
|   |-- 12-cloud-security/
|   `-- 13-capstone/
|
|-- diagrams/
|-- screenshots/
|-- reports/
|-- templates/
|-- scripts/
|-- scan-results/
|-- configs/
|-- assets/
|-- notes/
`-- resume/
```

---

## Documentation Standards

Each major lab phase should include:

1. **Purpose**
   What problem the technology or configuration addresses.

2. **Learning objectives**
   What knowledge or skills should be gained.

3. **Environment details**
   Systems, resources, network information, and dependencies.

4. **Implementation steps**
   Commands, configuration changes, and screenshots.

5. **Security considerations**
   Risks, trust boundaries, access requirements, and controls.

6. **Validation**
   Evidence that the system or control works as intended.

7. **Troubleshooting**
   Problems encountered and how they were resolved.

8. **Lessons learned**
   Technical and process improvements identified during the lab.

9. **Portfolio deliverables**
   Reports, diagrams, scripts, and sanitized results.

---

## Git Workflow

Changes are documented using focused commits with clear messages.

Examples:

```text
docs: add Proxmox host inventory
feat: deploy Ubuntu target server
config: document SSH hardening baseline
scan: add initial Nmap results
report: add vulnerability assessment summary
fix: correct virtual network configuration
```

Standard workflow:

```bash
git status
git add .
git commit -m "Describe the completed change"
git push
```

---

## Security and Privacy

This repository must not contain:

* Real passwords
* API keys
* Private keys
* Authentication tokens
* Sensitive personal information
* Public IP addresses
* Internal employer information
* Proprietary configurations
* Unredacted vulnerability reports containing sensitive data

Configuration files and scan results will be sanitized before being committed.

Intentionally vulnerable systems will be used only in an authorized lab environment. They should not be exposed directly to the internet or used against systems without explicit permission.

---

## Portfolio Deliverables

The project will eventually include:

* Network and architecture diagrams
* Asset inventories
* Vulnerability assessment reports
* Remediation plans
* Before-and-after security comparisons
* Security configuration baselines
* Architecture Decision Records
* Change records
* Runbooks
* Incident investigation reports
* Detection rules
* Threat-hunting reports
* Executive summaries
* Resume bullets
* Interview stories

---

## Skills Demonstrated

Project Aegis is intended to provide evidence of:

* Secure infrastructure design
* Systems engineering
* Vulnerability management
* Risk-based prioritization
* Security control implementation
* Technical troubleshooting
* Log analysis
* Security monitoring
* Identity and access management
* Network security
* Technical writing
* Executive communication
* Git-based documentation
* Continuous improvement

---

## Disclaimer

Project Aegis is an educational cybersecurity lab. All testing is performed on systems owned by the project author or explicitly authorized for testing.

The tools, techniques, and configurations documented here must not be used against systems without permission.

---

## License

This project is licensed under the MIT License unless otherwise noted.
