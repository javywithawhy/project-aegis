# Raspberry Pi Companion Project

## Project Overview

This companion project adds a Raspberry Pi 3 Model B to Project Aegis as a dedicated security management and monitoring appliance.

The Raspberry Pi will operate alongside the Proxmox homelab rather than replacing any existing Project Aegis components. Its purpose is to provide lightweight infrastructure services that remain available independently of the virtual machines running on Proxmox.

---

## Project Purpose

The Raspberry Pi companion project will provide hands-on experience with:

- Linux server administration
- System hardening
- Docker and container management
- Infrastructure monitoring
- Centralized logging
- DNS security
- Availability monitoring
- Security automation
- Configuration management
- Technical documentation

---

## Planned Role

The Raspberry Pi will act as a dedicated management appliance for Project Aegis.

Planned services may include:

- Prometheus
- Grafana
- Loki
- Node Exporter
- Uptime Kuma
- Pi-hole
- Ansible
- Syslog collection
- Automated configuration backups

Services will be introduced gradually based on the Raspberry Pi 3's available CPU and memory.

---

## Relationship to Project Aegis

Project Aegis uses Proxmox as its primary virtualization platform.

The Raspberry Pi will support that environment by providing services that do not need to run as Proxmox virtual machines.

```text
Home Network
│
├── Raspberry Pi 3 Model B
│   ├── Monitoring
│   ├── Logging
│   ├── DNS security
│   ├── Availability checks
│   └── Automation
│
└── Proxmox Host
    ├── Kali Linux
    ├── Ubuntu Server
    ├── Windows systems
    ├── Active Directory
    ├── pfSense
    ├── Wazuh
    └── Security testing workloads
```

This separation provides an independent platform for monitoring the Proxmox host and its virtual machines.

---

## Project Objectives

1. Install a minimal Raspberry Pi operating system.
2. Configure secure remote administration.
3. Establish a documented security baseline.
4. Configure stable wired network connectivity.
5. Install Docker and Docker Compose.
6. Deploy lightweight monitoring services.
7. Monitor the Raspberry Pi and Proxmox environment.
8. Centralize selected system logs.
9. Add DNS security and availability monitoring.
10. Automate selected administrative and security tasks.

---

## Project Phases

### Phase 1 — System Foundation

- Document the hardware
- Install Raspberry Pi OS Lite
- Configure the hostname
- Enable SSH
- Update the operating system
- Configure networking
- Apply baseline hardening
- Configure automatic security updates
- Configure the host firewall

### Phase 2 — Container Platform

- Install Docker
- Install Docker Compose
- Create persistent container directories
- Deploy Portainer if system resources permit
- Document backup and recovery procedures

### Phase 3 — Monitoring

- Install Node Exporter
- Install Prometheus
- Install Grafana
- Monitor Raspberry Pi resource usage
- Monitor the Proxmox host
- Monitor supported Linux virtual machines

### Phase 4 — Centralized Logging

- Deploy Loki
- Install compatible log collectors
- Collect Raspberry Pi logs
- Collect selected Proxmox and Linux logs
- Create security-focused dashboards
- Configure basic alerts

### Phase 5 — Network Services

- Deploy Pi-hole
- Configure DNS filtering
- Configure malicious-domain blocklists
- Review DNS query logs
- Document privacy and availability considerations

### Phase 6 — Availability and Automation

- Deploy Uptime Kuma
- Monitor Project Aegis services
- Test outage detection
- Configure notifications
- Introduce Ansible
- Automate selected maintenance and hardening checks

---

## Constraints

The Raspberry Pi 3 Model B has limited processing power and memory compared with the Proxmox host.

The project will therefore follow these constraints:

- Prefer lightweight services.
- Avoid running unnecessary containers.
- Review memory and CPU use after each deployment.
- Add only one major service at a time.
- Maintain backups of configuration files.
- Avoid storing unnecessary sensitive information.
- Monitor microSD card storage and write activity.
- Move resource-intensive workloads to Proxmox when appropriate.

---

## Security Design Principles

The companion project will follow these principles:

- Least privilege
- Minimal software installation
- Secure remote administration
- Regular patching
- Documented configuration changes
- Separate service accounts when practical
- Persistent configuration backups
- Limited network exposure
- Monitoring and alerting
- Recovery planning

---

## Planned Deliverables

Each major implementation will include:

- Purpose and scope
- Installation procedure
- Configuration summary
- Security considerations
- Validation results
- Screenshots or command output
- Risks and limitations
- Lessons learned
- Future improvements

---

## Current Status

- Raspberry Pi hardware documented
- Raspberry Pi OS Lite installed
- Wired network connectivity configured
- Stable IPv4 address reserved
- Operating system fully updated
- SSH key authentication configured
- Password-based SSH access disabled
- Host firewall configured
- Automatic security updates enabled
- Unused wireless radios disabled
- Time synchronization verified
- Listening network services audited
- Initial system hardening complete

---

## Next Step

Install Docker Engine and Docker Compose.