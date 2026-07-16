# Project Aegis Current Network Architecture

## Current State

```mermaid
flowchart TB
    Internet["Internet"]
    Router["Home Router<br/>Default Gateway: 10.0.0.1"]
    LAN["Home LAN<br/>10.0.0.0/24"]
    NIC["Proxmox Physical Ethernet<br/>nic0"]
    Bridge["Proxmox Linux Bridge<br/>vmbr0"]
    Management["Proxmox Management<br/>10.0.0.x/24"]

    subgraph FutureVMs["Initial Project Aegis Virtual Machines"]
        Kali["VM 100<br/>aegis-lab-kali-01"]
        Ubuntu["VM 120<br/>aegis-lab-ubuntu-01"]
        Windows["VM 140<br/>aegis-lab-win-01"]
    end

    Internet --> Router
    Router --> LAN
    LAN --> NIC
    NIC --> Bridge
    Bridge --> Management
    Bridge --> Kali
    Bridge --> Ubuntu
    Bridge --> Windows
```

## Current Design Notes

* `nic0` provides the physical Ethernet connection.
* `vmbr0` functions as the virtual switch.
* The Proxmox management interface is assigned to `vmbr0`.
* Initial virtual machines will connect directly to `vmbr0`.
* The home router currently provides DHCP, DNS forwarding, routing, and internet access.
* No dedicated lab segmentation currently exists.
* pfSense and isolated bridges will be introduced in a later milestone.

## Current Risk

All systems connected to `vmbr0` share the home-network trust zone.

Intentionally vulnerable machines and advanced security testing will be moved to an isolated network before those activities begin.
