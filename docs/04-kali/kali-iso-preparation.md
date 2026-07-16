# Kali Linux ISO Preparation

## Purpose

This document records the acquisition, integrity verification, and Proxmox upload of the Kali Linux installer used by Project Aegis.

Verifying the installation image before use reduces the risk of installing a corrupted or unauthorized image.

---

## Image Selection

| Field           | Value                           |
| --------------- | ------------------------------- |
| Distribution    | Kali Linux                      |
| Image type      | Installer                       |
| Architecture    | AMD64 / 64-bit                  |
| Filename        | kali-linux-2026.2-installer-amd64|
| Version         | 2026.2                          |
| Download source | Official Kali Linux website     |
| Download date   | 2026-07-16                      |

The standard Installer image was selected instead of a Live image, NetInstaller, ARM image, or prebuilt virtual machine.

This allows the operating system to be installed manually and documented within the Proxmox environment.

---

## Integrity Verification

The ISO was verified using SHA256.

### PowerShell Command

```powershell
Get-FileHash "$env:USERPROFILE\Downloads\kali-linux-2026.2-installer-amd64.iso" -Algorithm SHA256
```

### Verification Results

| Field               | Value                            |
| ------------------- | -------------------------------- |
| Hash algorithm      | SHA256                           |
| Published hash      | 6DBEFACC95E3B556C19C48E8BAE39B8B505E2D3A1ABA0BFB7AB62B036C3D2BA3  |
| Calculated hash     | 6DBEFACC95E3B556C19C48E8BAE39B8B505E2D3A1ABA0BFB7AB62B036C3D2BA3 |
| Hashes matched      | Yes                              |
| Verification status | Passed                           |

The calculated SHA256 value matched the value published for the official Kali Linux Installer image.

The ISO was not uploaded to Proxmox until verification was complete.

---

## Proxmox Upload

| Field                  | Value                              |
| ---------------------- | ---------------------------------- |
| Proxmox storage        | `aegis-hdd`                        |
| Content type           | ISO image                          |
| Storage location       | `/mnt/pve/aegis-hdd/template/iso/` |
| Upload method          | Proxmox web interface              |
| Upload status          | Complete                           |
| ISO visible in Proxmox | Yes                                |

### Upload Path

The image was uploaded through:

```text
Proxmox node
→ aegis-hdd
→ ISO Images
→ Upload
```

---

## Validation

The ISO was validated using:

```bash
pvesm list aegis-hdd --content iso
```

The uploaded Kali installer appeared in the list of ISO images available to Proxmox.

### Validation Checklist

* [x] ISO downloaded from the official Kali Linux source
* [x] Correct Installer image selected
* [x] AMD64 architecture selected
* [x] Published SHA256 value obtained
* [x] Local SHA256 value calculated
* [x] Hash values matched
* [x] ISO uploaded to `aegis-hdd`
* [x] ISO visible in Proxmox
* [x] Evidence captured
* [x] Screenshots reviewed before publication

---

## Security Considerations

* Installation images should be obtained only from trusted official sources.
* A mismatched hash must be treated as a failed verification.
* Unverified installation media should not be deployed.
* ISO images should not be committed to GitHub.
* Published screenshots should not expose personal file paths or unrelated information.
* The Kali system will only be used against authorized Project Aegis systems.

---

## Evidence

Evidence is stored in:

```text
screenshots/kali/
```

Files:

```text
kali-iso-sha256-verification.png
kali-iso-proxmox-storage.png
```

---

## Next Step

Create VM `100`, named:

```text
aegis-lab-kali-01
```

The VM will use the verified Kali Linux Installer ISO stored on `aegis-hdd`.
