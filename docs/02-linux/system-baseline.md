# Linux System Baseline

## Purpose

This document establishes the initial configuration of the Kali Linux virtual machine after installation.

The baseline provides a known-good reference before security tools, services, and additional software are installed.

---

## System Information

| Field | Value |
|---|---|
| Hostname | aegis-lab-kali-01 |
| Distribution | |
| Kernel Version | 7.0.12+kali-amd64 |
| Architecture | x86_64 |
| Current User | aegisadmin |
| Uptime | 1 week |
| Time Zone | America/Denver |

---

## CPU

| Field | Value |
|---|---|
| CPU Model | Intel Core i7-8750H 2.20GHz |
| Logical CPUs | |
| Virtualization | VT-x |

---

## Memory

| Field | Value |
|---|---|
| Installed RAM | 6 Gb |
| Available RAM | 3 Gb |
| Swap | 2 Gb |

---

## Storage

| Mount Point | Size | Used | Available |
|-------------|------|------|-----------|
| /dev/sda1 | 38G | 15G | 21G |

---

## Network

| Field | Value |
|---|---|
| Interface | eth0 |
| IPv4 Address | [REDACTED-IP] |
| Gateway | 10.0.0.1 |
| DNS Server | 75.75.75.75 |

---

## Installed Core Software

- Git
- Nmap
- QEMU Guest Agent

---

## Validation Checklist

- [X] System information recorded
- [X] Network documented
- [X] Storage documented
- [X] Memory documented
- [X] Core software verified