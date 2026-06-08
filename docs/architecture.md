# Architecture

## Overview

This lab uses Veeam Backup & Replication 13 to protect virtual machines running on Proxmox VE.

The environment is segmented with OPNsense and contains separate networks for clients, servers and DMZ workloads.

The Veeam backup server is installed on a Windows Server VM, while a dedicated Veeam worker VM is deployed on Proxmox to process backup and restore operations.

## Main Components

| Component | Description |
|---|---|
| Proxmox VE | Hypervisor hosting the lab VMs |
| OPNsense | Firewall and router between lab networks |
| Veeam Backup Server | Windows Server VM running Veeam Backup & Replication |
| Veeam Worker | Dedicated worker VM used by Veeam for Proxmox operations |
| ReFS Repository | Dedicated backup repository on the Veeam server |
| Debian Web Server | Test VM protected by Veeam |

## Network Design

| Network | Purpose |
|---|---|
| Management / Home LAN | Proxmox management access |
| SERVERS | Windows Server, Veeam and infrastructure services |
| DMZ | Isolated web server network |

Key IP addresses:

| System | IP Address |
|---|---:|
| Proxmox VE Host | 192.168.0.156 |
| Veeam Backup Server | 10.10.20.70 |
| Veeam Worker | 10.10.20.71 |
| Active Directory / DNS | 10.10.20.10 |
| Debian DMZ Web Server | 10.10.30.10 |

## Backup Flow

The backup flow is:

1. Veeam Backup Server connects to the Proxmox host.
2. The Veeam worker processes the Proxmox VM backup.
3. Backup data is sent to the Veeam repository.
4. The restore test is performed from the repository back to Proxmox.

Final validated flow:

- Proxmox VM: debian-dmz-web01
- Backup repository: R:\VeeamRepository
- Restored VM: web01-veeam-restore-test
