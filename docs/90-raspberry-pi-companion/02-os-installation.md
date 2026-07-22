# Raspberry Pi OS Installation

## Objective

Install a lightweight, headless operating system that will serve as the foundation for the Project Aegis Raspberry Pi companion appliance.

---

## Hardware

- Raspberry Pi 3 Model B
- 64 GB microSD card
- Wired Ethernet connection
- microSD card reader
- Compatible Raspberry Pi power supply

---

## Selected Operating System

```text
Raspberry Pi OS Lite
64-bit edition
```

Raspberry Pi OS Lite was selected because it does not include a graphical desktop environment.

---

## Why Raspberry Pi OS Lite?

The Lite edition provides:

- Lower memory use
- Lower CPU use
- Fewer installed packages
- Smaller attack surface
- Faster updates
- More resources for containers
- A server-oriented administration experience

This is especially important because the Raspberry Pi 3 Model B has 1 GB of RAM.

---

## Initial Configuration Plan

| Setting | Value |
|---|---|
| Hostname | `security-pi` |
| Administrative user | `javier` |
| Remote administration | SSH |
| Initial SSH authentication | Password |
| Desktop environment | None |
| Primary network connection | Wired Ethernet |
| Time zone | `America/Denver` |
| Keyboard layout | US |
| Locale | `en_US.UTF-8` |

Password-based SSH authentication will be used only for initial setup. SSH keys will be configured during the hardening phase.

---

## Installation Procedure

### 1. Install Raspberry Pi Imager

Download and install Raspberry Pi Imager from the official Raspberry Pi website.

Launch Raspberry Pi Imager after installation.

---

### 2. Insert the microSD Card

Insert the 64 GB microSD card into the computer using the available card reader.

Confirm that no important data remains on the card.

> Warning: The imaging process will erase the selected storage device.

---

### 3. Select the Raspberry Pi Device

In Raspberry Pi Imager, select:

```text
Raspberry Pi 3
```

---

### 4. Select the Operating System

Choose:

```text
Raspberry Pi OS (other)
```

Then select:

```text
Raspberry Pi OS Lite (64-bit)
```

Do not select the desktop edition.

---

### 5. Select Storage

Select the 64 GB microSD card.

Verify the selected storage device carefully before continuing.

---

### 6. Configure Operating System Settings

Open the operating system customization menu before writing the image.

Depending on the Raspberry Pi Imager version, the settings menu may appear automatically or may be opened through the interface.

Configure the following settings.

#### Hostname

```text
security-pi
```

#### Username

```text
javier
```

A unique administrative username is used instead of a default `pi` account.

#### Password

Create a strong, unique password.

Do not record the password in this repository.

#### SSH

Enable SSH.

Use password authentication temporarily for the initial connection.

#### Wi-Fi

Do not configure Wi-Fi unless it is required as a backup connection.

The primary network connection will be wired Ethernet.

#### Locale

```text
Locale: en_US.UTF-8
Time zone: America/Denver
Keyboard layout: US
```

#### Telemetry

Disable optional telemetry or analytics when presented with the choice.

---

### 7. Write the Image

Begin the imaging process.

Raspberry Pi Imager will:

1. Erase the microSD card.
2. Write Raspberry Pi OS Lite.
3. Apply the selected configuration.
4. Verify the written image.

Wait for the verification process to complete before removing the card.

---

### 8. Install the microSD Card

Safely eject the microSD card from the computer.

Insert it into the Raspberry Pi.

---

### 9. Connect the Raspberry Pi

Connect the following:

1. Ethernet cable
2. Power cable

A monitor and keyboard should not be necessary because SSH was enabled during imaging.

---

### 10. Allow the First Boot to Complete

Wait several minutes for the initial boot process.

During the first boot, the Raspberry Pi may:

- Expand the filesystem
- Apply configuration settings
- Generate SSH host keys
- Request a DHCP address
- Restart services

Do not interrupt power during this process.

---

## Initial Connectivity Test

From a Windows computer, open PowerShell.

Test hostname resolution:

```powershell
ping security-pi.local
```

A response is helpful, but some systems or firewalls may block ping even when the Raspberry Pi is online.

Attempt an SSH connection:

```powershell
ssh javier@security-pi.local
```

At the first connection, SSH may display a host authenticity warning.

Review the hostname and accept the new host key when appropriate.

Enter the password created in Raspberry Pi Imager.

---

## Alternative: Connect Using the IP Address

If `security-pi.local` does not resolve, locate the Raspberry Pi's address through the home router or DHCP client list.

Then connect using:

```powershell
ssh javier@192.168.1.X
```

Replace `192.168.1.X` with the assigned address.

---

## Initial Verification Commands

After signing in through SSH, run:

```bash
hostname
```

Expected result:

```text
security-pi
```

Confirm the operating system:

```bash
cat /etc/os-release
```

Confirm the architecture:

```bash
uname -m
```

Review the assigned network addresses:

```bash
hostname -I
```

Review storage:

```bash
df -h
```

Review memory:

```bash
free -h
```

Review system uptime:

```bash
uptime
```

---

## Suggested Evidence to Capture

Save screenshots or terminal output showing:

- Raspberry Pi Imager operating system selection
- Hostname configuration
- SSH configuration
- Successful SSH login
- Output of `hostname`
- Output of `cat /etc/os-release`
- Output of `hostname -I`
- Output of `df -h`
- Output of `free -h`

Do not capture or publish:

- Passwords
- Private SSH keys
- Public IP addresses
- Router administrative credentials
- Sensitive network information
- Personally identifying device details that are not needed

Suggested screenshot location:

```text
screenshots/90-raspberry-pi-companion/
```

---

## Security Considerations

The initial installation uses password-based SSH authentication for convenience.

This is temporary.

The hardening phase will include:

- Installing system updates
- Configuring SSH keys
- Disabling direct root login
- Reviewing password authentication
- Configuring the firewall
- Configuring automatic security updates
- Reviewing listening services
- Establishing a configuration baseline
- Assigning a stable network address

---

## Validation Checklist

- [X] Raspberry Pi OS Lite 64-bit was selected
- [X] The 64 GB microSD card was imaged successfully
- [X] Hostname was configured as `security-pi`
- [X] Administrative user was configured as `javier`
- [X] SSH was enabled
- [X] Wired Ethernet was connected
- [X] The Raspberry Pi booted successfully
- [X] The Raspberry Pi received an IP address
- [X] SSH access was successful
- [X] Operating system information was verified
- [X] Storage capacity was verified
- [X] Memory information was verified
- [X] Screenshots or command output were saved
- [X] No credentials were committed to GitHub

---

## Problems Encountered

Document installation or connectivity problems here.

```text
None documented yet.
```

---

## Lessons Learned

Complete this section after the installation.

Examples:

- How the Raspberry Pi was located on the network
- Whether `.local` hostname resolution worked
- Whether the first SSH connection succeeded
- Whether any imaging or power issues occurred
- Any differences between the instructions and the installed software

---

## Current Status

```text
Installation complete
```

Update this section after completing the installation.

---

## Next Step

Perform the initial server configuration and establish the Raspberry Pi security baseline.