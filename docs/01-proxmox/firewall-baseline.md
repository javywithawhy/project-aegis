# Proxmox Firewall Baseline

## Purpose

This document records the initial firewall state of the Project Aegis Proxmox host before firewall rules or security-policy changes are introduced.

The baseline supports:

* Change tracking
* Troubleshooting
* Security review
* Validation of future firewall changes
* Recovery from accidental loss of administrative access
* Comparison between the initial and future secured configurations

---

## Firewall Scope

Proxmox firewall controls may be applied at multiple levels:

| Level           | Purpose                                                                   |
| --------------- | ------------------------------------------------------------------------- |
| Datacenter      | Provides settings and rules that can apply across the Proxmox environment |
| Node            | Protects the Proxmox host and its administrative services                 |
| VM or container | Filters traffic entering or leaving an individual guest system            |

Project Aegis currently contains one Proxmox node and no deployed virtual machines.

---

## Current Firewall Status

| Check                                | Result                                           |
| ------------------------------------ | ------------------------------------------------ |
| `pve-firewall status`                | `Status: disabled/running`                       |
| Firewall service state               | Active and running                               |
| Firewall service startup state       | Enabled at boot                                  |
| Datacenter firewall option           | No                                               |
| Node-specific firewall configuration | None                                             |
| ebtables                             | Yes                                              |
| Input policy                         | `DROP`                                           |
| Output policy                        | `ACCEPT`                                         |
| Forward policy                       | `ACCEPT`                                         |
| Log rate limit                       | Default: enabled, 1 event per second, burst of 5 |
| Datacenter rules configured          | No                                               |
| Node rules configured                | No                                               |
| VM firewall rules configured         | Not applicable; no VMs deployed                  |

---

## Status Interpretation

The `pve-firewall` systemd service is active and enabled. This means the firewall daemon starts automatically and is available to process Proxmox firewall configuration.

However, the command:

```text
Status: disabled/running
```

indicates that the firewall service is running while firewall enforcement is disabled.

The firewall options page also shows:

```text
Firewall: No
```

Therefore, the configured default policies are not currently being enforced.

A running firewall service does not necessarily mean that firewall filtering is active. The service can remain available while enforcement is disabled at the Proxmox configuration level.

---

## Default Policies

| Traffic direction | Default policy | Meaning if firewall enforcement is enabled            |
| ----------------- | -------------- | ----------------------------------------------------- |
| Input             | `DROP`         | Incoming traffic is denied unless explicitly allowed  |
| Output            | `ACCEPT`       | Outbound traffic is allowed unless explicitly denied  |
| Forward           | `ACCEPT`       | Forwarded traffic is allowed unless explicitly denied |

The input policy presents the most immediate administrative risk.

If firewall enforcement is enabled before explicit management allow rules are created, access to the Proxmox web interface and SSH could be interrupted.

---

## Firewall Service Details

The Proxmox firewall service was reviewed with:

```bash
systemctl status pve-firewall --no-pager
```

The service was observed as:

* Loaded
* Enabled
* Active
* Running
* Successfully started
* Configured to start automatically at boot

Observed service name:

```text
pve-firewall.service - Proxmox VE firewall
```

The active daemon does not mean that firewall rules are currently being enforced. Enforcement depends on the Proxmox firewall enable settings and configured rule sets.

---

## Datacenter Firewall Configuration

The datacenter firewall configuration directory was reviewed:

```text
/etc/pve/firewall/
```

The directory exists but contains no firewall configuration files.

Observed result:

```text
total 0
```

This indicates that no datacenter-level firewall rule file has been created.

No datacenter firewall rules are currently configured.

---

## Node Firewall Configuration

The node-specific firewall configuration file was checked at:

```text
/etc/pve/nodes/[NODE-NAME]/host.fw
```

The command used was:

```bash
ls -lah /etc/pve/nodes/$(hostname)/host.fw 2>/dev/null || echo "No node firewall file found"
```

The result was:

```text
No node firewall file found
```

This confirms that no node-level firewall configuration file or node-specific firewall rules are currently configured.

---

## VM and Container Firewall Configuration

No virtual machines or containers have been deployed yet.

Therefore:

* No VM-level firewall rules exist.
* No container-level firewall rules exist.
* No guest network interfaces currently use Proxmox firewall filtering.
* Guest-specific firewall configuration will be documented after the first systems are created.

---

## Current Administrative Services

| Service               |   Default port | Current purpose                          | Required during initial setup    |
| --------------------- | -------------: | ---------------------------------------- | -------------------------------- |
| Proxmox web interface |       TCP 8006 | Primary browser-based administration     | Yes                              |
| SSH                   |         TCP 22 | Command-line administration              | Yes during initial configuration |
| ICMP                  | Not port-based | Connectivity testing and troubleshooting | Recommended                      |

The Proxmox host is currently managed from the trusted home network.

No Proxmox administrative service should be exposed through inbound internet port forwarding.

If the firewall is enabled later, explicit rules must allow trusted management access to TCP ports `8006` and `22` before the default input policy of `DROP` is enforced.

---

## Current Network Context

| Field                       | Value               |
| --------------------------- | ------------------- |
| Management network          | `10.0.0.0/24`       |
| Proxmox management address  | `10.0.0.x/24`       |
| Management interface        | `vmbr0`             |
| Physical Ethernet interface | `nic0`              |
| Default gateway             | `10.0.0.1`          |
| Dedicated management VLAN   | No                  |
| Dedicated lab firewall      | Not yet deployed    |
| Network segmentation        | Not yet implemented |
| Current trust zone          | Home network        |

The exact Proxmox management address is sanitized in public documentation.

---

## Baseline Findings

1. The Proxmox firewall daemon is installed and operating normally.
2. The firewall service is active and running.
3. The firewall service is configured to start automatically.
4. Firewall enforcement is currently disabled.
5. The datacenter firewall option is set to `No`.
6. The default input policy is configured as `DROP`.
7. The default output policy is configured as `ACCEPT`.
8. The default forward policy is configured as `ACCEPT`.
9. ebtables support is enabled.
10. The default log rate limit is enabled.
11. No datacenter-level firewall rules are configured.
12. No node-level firewall rules are configured.
13. No VM-level firewall rules exist because no VMs have been deployed.
14. Enabling the firewall without management allow rules could interrupt web-interface and SSH access.

---

## Current Risks

| Risk                                         | Potential impact                                             | Planned response                                            |
| -------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| Management interface shares the home network | Other home-network devices may attempt to reach Proxmox      | Restrict administration to trusted systems                  |
| Firewall enabled without an allow rule       | Administrative access could be interrupted                   | Create and verify management allow rules before enabling    |
| Default input policy is `DROP`               | Required administrative traffic may be blocked               | Explicitly allow ports `8006` and `22` from trusted sources |
| SSH is reachable from the home network       | Increased administrative attack surface                      | Restrict SSH or disable it when unnecessary                 |
| No dedicated management segment              | Management traffic is not isolated                           | Introduce segmentation in a later milestone                 |
| Misconfigured firewall rules                 | Loss of connectivity or unintended exposure                  | Use staged changes and maintain physical console access     |
| No current firewall enforcement              | Host depends primarily on network trust and service security | Introduce controlled firewall enforcement later             |
| Administrative port forwarding               | Could expose Proxmox to the internet                         | Confirm no inbound forwarding exists                        |

---

## Current Security Controls

The following controls currently reduce risk:

* Proxmox is located on a private home network.
* The management address uses an RFC1918 private IP address.
* The Proxmox web interface is intended for internal access only.
* Firewall enforcement has not been enabled without tested allow rules.
* Physical access to the Proxmox laptop is available for recovery.
* No virtual machines or intentionally vulnerable systems have been deployed.
* No datacenter, node, or VM firewall rules currently exist.
* Firewall status and default policies are documented before modification.
* Sensitive management information is sanitized before being published to GitHub.

---

## Initial Security Approach

No firewall enforcement changes will be made until:

1. The trusted management source address or network is identified.
2. TCP port `8006` is explicitly allowed from the trusted management source.
3. TCP port `22` is explicitly allowed if SSH administration remains required.
4. ICMP behavior is documented.
5. Physical console access is available.
6. The current firewall configuration is backed up.
7. A rollback procedure is documented.
8. Rules are added and tested individually.
9. Browser and SSH connectivity are verified after each change.
10. The home router is checked for unwanted inbound port-forwarding rules.

This staged approach reduces the risk of locking the administrator out of the Proxmox host.

---

## Planned Management Access Policy

A future management-access policy should follow this general model:

| Traffic                               | Source                              | Destination                                 | Action              |
| ------------------------------------- | ----------------------------------- | ------------------------------------------- | ------------------- |
| Proxmox web administration            | Trusted management device or subnet | Proxmox TCP 8006                            | Allow               |
| SSH administration                    | Trusted management device or subnet | Proxmox TCP 22                              | Allow when required |
| ICMP testing                          | Trusted home or management network  | Proxmox host                                | Allow or rate-limit |
| Unapproved inbound management traffic | Any other source                    | Proxmox host                                | Drop and log        |
| Outbound host traffic                 | Proxmox host                        | Required update and infrastructure services | Allow               |

Exact rules will be created only after the trusted management source has been identified and a rollback process is available.

---

## Rollback Considerations

Before enabling firewall enforcement:

1. Confirm direct physical access to the Proxmox laptop.
2. Keep the Proxmox web interface open on a trusted device.
3. Keep an existing SSH session open when practical.
4. Back up relevant firewall configuration files.
5. Add management allow rules before enabling enforcement.
6. Apply one change at a time.
7. Test TCP port `8006`.
8. Test TCP port `22` if SSH remains enabled.
9. Disable the firewall from the local console if access is lost.
10. Document any failed rule or recovery action.

---

## Baseline Validation

* [x] `pve-firewall status` reviewed
* [x] Firewall service state recorded
* [x] Firewall startup state recorded
* [x] Datacenter firewall option reviewed
* [x] ebtables status reviewed
* [x] Default input policy documented
* [x] Default output policy documented
* [x] Default forward policy documented
* [x] Log rate limit documented
* [x] Existing datacenter rules reviewed
* [x] Existing node rules confirmed
* [x] VM firewall status documented
* [ ] No administrative port forwarding confirmed
* [x] Screenshots reviewed for sensitive information
* [x] Baseline documentation completed

---

## Evidence

Evidence should be stored in:

```text
screenshots/proxmox/
```

Recommended filenames:

```text
proxmox-firewall-options.png
proxmox-firewall-directory.png
proxmox-firewall-status.png
proxmox-firewall-service-status.png
proxmox-node-firewall-file-check.png
```

The collected evidence shows:

* Firewall enforcement disabled
* Firewall daemon running successfully
* Firewall service enabled at boot
* Default firewall policies
* ebtables enabled
* Default logging rate limit
* Empty datacenter firewall configuration directory
* No node-specific firewall configuration file
* No VM-level firewall configuration because no VMs exist

Screenshots must be reviewed before publication to ensure they do not expose unnecessary host, account, or network information.

---

## Planned Future State

A future Proxmox firewall configuration may include:

* Explicit management access from trusted source systems
* Restricted SSH access
* Logging for blocked administrative attempts
* Guest-level firewall policies
* Per-VM security groups
* pfSense-based lab segmentation
* Separate management and security-testing networks
* Default-deny inbound management access
* Documented firewall rule reviews
* Validation testing after every firewall change

No firewall rules will be activated until required traffic paths and recovery procedures have been validated.

---

## Current Baseline Conclusion

The Proxmox firewall infrastructure is installed, enabled as a system service, and operating normally, but firewall enforcement is currently disabled.

No datacenter, node, VM, or container firewall rules are configured.

This is acceptable during the initial Project Aegis setup because no intentionally vulnerable systems have been deployed. Firewall enforcement will be introduced through a controlled change after trusted management access rules, testing procedures, and rollback steps have been documented.

---

## Next Steps

1. Confirm that the home router has no inbound port forwarding to TCP ports `22` or `8006`.
2. Record the trusted management device or subnet.
3. Document a firewall change and rollback procedure.
4. Prepare explicit management allow rules.
5. Keep firewall enforcement disabled until those rules are validated.
6. Download and verify the Kali Linux installation image.
7. Deploy the first Project Aegis virtual machine.
