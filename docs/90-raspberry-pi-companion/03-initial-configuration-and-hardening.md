# Initial Configuration and System Hardening

## Objective

Configure and harden the Raspberry Pi operating system before deploying application services or containers.

This phase established a secure administrative baseline for the Project Aegis Raspberry Pi companion appliance.

---

## System Summary

| Setting | Configured Value |
|---|---|
| Hostname | `security-pi` |
| Administrative user | `javier` |
| Operating system | Raspberry Pi OS Lite 64-bit |
| Base distribution | Debian GNU/Linux 13 (`trixie`) |
| Architecture | `aarch64` |
| IPv4 address | `10.0.0.79` |
| Network connection | Wired Ethernet |
| Time zone | `America/Denver` |
| Remote administration | SSH with public-key authentication |
| Host firewall | UFW |
| Automatic security updates | Enabled |

---

## Initial System Validation

The operating system installation was validated using:

```bash
hostname
whoami
cat /etc/os-release
uname -m
hostname -I
df -h /
free -h
uptime
vcgencmd measure_temp
vcgencmd get_throttled
```

### Validation Results

- Hostname correctly reported as `security-pi`.
- Administrative user correctly reported as `javier`.
- Operating system reported Debian GNU/Linux 13.
- Architecture reported as `aarch64`.
- The Raspberry Pi received the expected IPv4 address.
- The root filesystem had approximately 59 GB available.
- The system had approximately 754 MB of available memory while idle.
- Initial temperature was approximately 50.5°C.
- No CPU throttling or undervoltage conditions were detected.

The power and throttling status returned:

```text
throttled=0x0
```

---

## Operating System Updates

The system package lists and installed packages were updated using:

```bash
sudo apt update
sudo apt full-upgrade -y
```

The Raspberry Pi was rebooted after the upgrade.

The update status was verified after reconnecting.

---

## Stable Network Address

The Raspberry Pi was assigned a DHCP reservation through the home network gateway.

Reserved address:

```text
10.0.0.79
```

A router-side DHCP reservation was selected instead of manually configuring a static address on the operating system.

### Validation

The address was verified after reboot using:

```bash
hostname -I
ip route
```

Network and DNS connectivity were tested using:

```bash
ping -c 4 10.0.0.1
ping -c 4 raspberrypi.com
```

No port forwarding or public internet exposure was configured.

---

## SSH Host-Key Change

Because the Raspberry Pi operating system was reinstalled, the device generated new SSH host keys.

The previous key associated with the reused IP address was removed from the Windows SSH known-hosts file using:

```powershell
ssh-keygen -R 10.0.0.79
ssh-keygen -R security-pi.local
```

The new SSH fingerprint was reviewed before accepting the connection.

This demonstrated the importance of validating host-key changes rather than bypassing SSH security warnings.

---

## SSH Key Authentication

An Ed25519 SSH key was created on the administrative workstation.

Example command:

```powershell
ssh-keygen -t ed25519 -C "project-aegis"
```

The public key was copied to the Raspberry Pi and added to:

```text
~/.ssh/authorized_keys
```

Secure permissions were applied:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

The private SSH key remains on the administrative workstation and is not stored in the Project Aegis repository.

### Authentication Test

Public-key-only authentication was tested using:

```powershell
ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no javier@10.0.0.79
```

The connection succeeded without requesting the Raspberry Pi account password.

---

## SSH Hardening

A dedicated SSH hardening configuration was created:

```text
/etc/ssh/sshd_config.d/00-project-aegis-hardening.conf
```

Configuration:

```text
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
```

The configuration was validated using:

```bash
sudo sshd -t
```

The effective SSH settings were reviewed using:

```bash
sudo sshd -T | grep -E \
'^(pubkeyauthentication|passwordauthentication|kbdinteractiveauthentication|permitrootlogin) '
```

SSH was reloaded without interrupting the active administrative session:

```bash
sudo systemctl reload ssh
```

### Effective Controls

- Public-key authentication is enabled.
- Password-based SSH authentication is disabled.
- Keyboard-interactive authentication is disabled.
- Direct root login through SSH is disabled.
- Administrative access is limited to approved SSH keys.

A second SSH session was tested before closing the original session to reduce the risk of administrative lockout.

---

## Host Firewall

UFW was installed as the Raspberry Pi host firewall.

```bash
sudo apt install ufw -y
```

Default firewall policies were configured as:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny routed
```

SSH access was restricted to the local home subnet:

```bash
sudo ufw allow from 10.0.0.0/24 \
  to any port 22 proto tcp \
  comment 'SSH from home LAN'
```

Firewall logging was enabled:

```bash
sudo ufw logging low
```

The firewall was then enabled:

```bash
sudo ufw enable
```

### Firewall Policy

| Traffic Type | Policy |
|---|---|
| Unsolicited inbound traffic | Denied |
| Outbound traffic | Allowed |
| Routed traffic | Denied |
| SSH from `10.0.0.0/24` | Allowed |
| SSH from other networks | Denied |

### Validation

Firewall status was checked using:

```bash
sudo ufw status verbose
```

The configuration was tested after a reboot to confirm that:

- UFW remained enabled.
- SSH remained reachable from the local network.
- Unnecessary inbound traffic remained blocked.

> Docker networking behavior will be reviewed separately because published Docker ports may not follow normal UFW filtering behavior.

---

## Automatic Security Updates

Automatic security updates were configured using:

```bash
sudo apt install unattended-upgrades apt-listchanges -y
```

APT periodic updates were enabled in:

```text
/etc/apt/apt.conf.d/20auto-upgrades
```

Configuration:

```text
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

Project-specific unattended-upgrade settings were created in:

```text
/etc/apt/apt.conf.d/52project-aegis-unattended
```

Configuration:

```text
Unattended-Upgrade::Automatic-Reboot "false";
Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
Unattended-Upgrade::Remove-New-Unused-Dependencies "true";
```

Automatic rebooting was disabled so that service interruptions remain deliberate and documented.

### Validation

The configuration was tested using:

```bash
sudo unattended-upgrade --dry-run
echo $?
```

The dry run completed successfully with exit code:

```text
0
```

The following timers were verified as enabled:

```text
apt-daily.timer
apt-daily-upgrade.timer
```

---

## Disabled Wireless Radios

The Raspberry Pi will use wired Ethernet as its permanent network connection.

The unused Wi-Fi and Bluetooth radios were disabled in:

```text
/boot/firmware/config.txt
```

Configuration:

```text
# Project Aegis: disable unused onboard radios
dtoverlay=disable-wifi
dtoverlay=disable-bt
```

A backup of the original configuration was created before modification:

```text
/boot/firmware/config.txt.project-aegis-backup
```

### Validation

After rebooting, the available network interfaces were reviewed using:

```bash
ip -brief link
ls /sys/class/net
```

The remaining network interfaces were:

```text
eth0
lo
```

The wireless interface was no longer present.

---

## Time Synchronization

Accurate system time is necessary for reliable monitoring, log correlation, troubleshooting, and incident analysis.

Time synchronization was verified using:

```bash
timedatectl status
```

Validated settings:

```text
Time zone: America/Denver
System clock synchronized: yes
NTP service: active
```

The Raspberry Pi does not include a battery-backed real-time clock by default, so network time synchronization is especially important after restarts or power loss.

---

## Failed-Service Review

Failed system services were checked using:

```bash
systemctl --failed --no-pager
```

Result:

```text
0 loaded units listed.
```

No failed services were present during the baseline review.

---

## Listening-Port Audit

Listening network ports were reviewed using:

```bash
sudo ss -tulpn
```

Initially observed services included:

| Port | Protocol | Process | Purpose |
|---|---|---|---|
| 22 | TCP | `sshd` | Secure remote administration |
| 5353 | UDP | `avahi-daemon` | Multicast DNS |
| 546 | UDP | NetworkManager | DHCPv6 client |

SSH was required for administration.

The DHCPv6 client was part of normal network configuration.

Avahi was not required because the Raspberry Pi has a reserved IPv4 address and can be accessed directly.

---

## Avahi and mDNS

Avahi provided `.local` hostname discovery through multicast DNS.

Because the Raspberry Pi has a stable address, Avahi was disabled to remove an unnecessary listening service:

```bash
sudo systemctl disable --now avahi-daemon.service
sudo systemctl disable --now avahi-daemon.socket
```

The result was verified using:

```bash
systemctl is-active avahi-daemon.service
systemctl is-enabled avahi-daemon.service
sudo ss -tulpn
```

Expected state:

```text
inactive
disabled
```

UDP port 5353 was no longer listening after the change.

The Raspberry Pi will now be accessed using:

```powershell
ssh javier@10.0.0.79
```

---

## Security Controls Implemented

| Control | Status |
|---|---|
| Unique administrative account | Implemented |
| Current operating-system packages | Implemented |
| Stable network address | Implemented |
| SSH public-key authentication | Implemented |
| SSH password authentication disabled | Implemented |
| Direct root SSH login disabled | Implemented |
| Host firewall enabled | Implemented |
| SSH restricted to local subnet | Implemented |
| Automatic security updates | Implemented |
| Automatic rebooting disabled | Implemented |
| Unused wireless radios disabled | Implemented |
| Unnecessary mDNS service disabled | Implemented |
| Time synchronization verified | Implemented |
| Listening ports audited | Implemented |
| Failed services reviewed | Implemented |
| Power and throttling status checked | Implemented |

---

## Risks and Limitations

### microSD Card Storage

The operating system and future service data are stored on a microSD card.

Risks include:

- Storage wear
- Filesystem corruption after sudden power loss
- Reduced write endurance from excessive logging
- Complete service loss if the card fails

Mitigations will include:

- Conservative log-retention settings
- Configuration backups
- Monitoring filesystem usage
- Avoiding unnecessary write-heavy services
- Documenting rebuild and recovery procedures

### Single Network Interface

The Raspberry Pi has one active Ethernet interface.

It should not be treated as a firewall, router, or transparent intrusion-detection bridge without additional hardware and architecture changes.

### Physical Security

Anyone with physical access to the microSD card could potentially access or alter its contents.

The device should remain in a reasonably controlled location.

### Resource Constraints

The Raspberry Pi 3 Model B has approximately 1 GB of RAM.

Future services must be evaluated individually to avoid:

- Memory exhaustion
- Excessive swapping
- CPU saturation
- Storage exhaustion
- Service instability

---

## Validation Checklist

- [x] Operating system updated
- [x] Hostname verified
- [x] Administrative user verified
- [x] Stable IPv4 address reserved
- [x] SSH host key reviewed
- [x] SSH key authentication configured
- [x] Password-based SSH disabled
- [x] Direct root SSH login disabled
- [x] UFW installed and enabled
- [x] SSH restricted to the local subnet
- [x] Automatic security updates configured
- [x] Automatic rebooting disabled
- [x] Wi-Fi disabled
- [x] Bluetooth disabled
- [x] Time synchronization verified
- [x] Failed services reviewed
- [x] Listening ports audited
- [x] Avahi disabled
- [x] Ethernet and DNS connectivity verified
- [x] No credentials committed to GitHub

---

## Lessons Learned

- Reinstalling an operating system generates new SSH host keys.
- SSH host-key warnings should be investigated rather than bypassed.
- Public-key authentication allows password-based remote login to be disabled.
- Firewall access rules should be created before enabling the firewall.
- A firewall tool can be active even when its systemd unit does not behave like a continuously running daemon.
- Accurate timestamps are essential for security monitoring and incident analysis.
- Every listening network service should have a documented purpose.
- Services that are unnecessary for the system’s intended role should be disabled.
- Configuration changes should be tested in a second session before closing the original administrative connection.

---

## Current Status

```text
Phase 1 system foundation and hardening complete.
```

---

## Next Step

Install Docker Engine and Docker Compose to establish the container platform.