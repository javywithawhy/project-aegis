# Raspberry Pi Hardware Inventory

## Objective

Document the physical hardware that will support the Raspberry Pi companion project before installing or configuring any software.

---

## Hardware Inventory

| Component | Details |
|---|---|
| Raspberry Pi | Raspberry Pi 3 Model B |
| Storage | 64 GB microSD card |
| Network | Wired Ethernet |
| SD Card Reader | Available |
| Existing Data | None requiring backup |
| Intended Use | Project Aegis companion appliance |

---

## Raspberry Pi Model

The system is a:

```text
Raspberry Pi 3 Model B
```

This model includes:

- Quad-core ARM processor
- 1 GB RAM
- Built-in Ethernet
- Built-in Wi-Fi
- USB ports
- HDMI output
- GPIO connectivity

For this project, wired Ethernet will be used instead of Wi-Fi whenever possible.

---

## Storage

The Raspberry Pi will use a:

```text
64 GB microSD card
```

This provides sufficient capacity for:

- Raspberry Pi OS
- Docker images
- Container configuration files
- Monitoring data
- Selected log storage
- Configuration backups
- Project documentation

Because monitoring and logging can generate frequent storage writes, retention settings will be configured conservatively.

---

## Network Connection

The Raspberry Pi will remain connected by wired Ethernet.

Preferred connection:

```text
Home Router or Network Switch
              │
              │ Ethernet
              │
      Raspberry Pi 3 Model B
```

Wired Ethernet was selected because it generally provides:

- More stable connectivity
- Lower latency
- More predictable availability
- Easier troubleshooting
- Better suitability for monitoring services

---

## Existing Data

No important data is currently stored on the microSD card.

The card can therefore be erased and reimaged without requiring a backup.

---

## Power Requirements

The Raspberry Pi 3 Model B should use a stable power supply rated approximately:

```text
5 volts
2.5 amps
```

An inadequate power supply may cause:

- Random reboots
- Storage corruption
- USB instability
- Network interruptions
- Performance throttling
- Undervoltage warnings

The power supply should be inspected before long-term operation.

---

## Cooling and Placement

The Raspberry Pi should be placed in a ventilated location.

Recommended conditions:

- Use a protective case.
- Avoid enclosed spaces with poor airflow.
- Keep the device away from direct heat sources.
- Keep the device away from liquids.
- Avoid placing it directly on carpet.
- Use small heatsinks if temperatures become excessive.
- Leave enough space to access the microSD card and network cable.

---

## Planned Role

The Raspberry Pi will operate as a companion security and monitoring appliance alongside the Project Aegis Proxmox environment.

Its planned responsibilities include:

- Monitoring infrastructure availability
- Collecting system metrics
- Hosting dashboards
- Collecting selected logs
- Providing DNS filtering
- Supporting administrative automation
- Monitoring the Proxmox host and virtual machines

---

## Resource Considerations

The Raspberry Pi 3 Model B has 1 GB of RAM, so services must be introduced carefully.

The following practices will be used:

- Install only required software.
- Use Raspberry Pi OS Lite without a desktop.
- Prefer lightweight containers.
- Monitor memory usage after each service is deployed.
- Avoid running multiple resource-intensive services simultaneously.
- Move heavy workloads to Proxmox when necessary.
- Configure log and metric retention limits.

---

## Inventory Status

| Check | Status |
|---|---|
| Raspberry Pi model identified | Complete |
| Storage capacity identified | Complete |
| Existing data reviewed | Complete |
| SD card reader available | Complete |
| Wired Ethernet available | Complete |
| Power supply rating verified | Pending |
| Case and ventilation reviewed | Pending |

---

## Next Step

Install Raspberry Pi OS Lite on the 64 GB microSD card.