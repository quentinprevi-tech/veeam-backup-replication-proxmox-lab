# Installation

## Overview

This document summarizes the installation steps for the Veeam Backup & Replication Proxmox lab.

The Veeam backup server was installed on a dedicated Windows Server 2022 VM.

VM details:

| Setting | Value |
|---|---|
| VM name | veeam-backup01 |
| VMID | 370 |
| OS | Windows Server 2022 |
| IP address | 10.10.20.70 |
| Network | SERVERS |

## Veeam Installation

Veeam Backup & Replication 13 was installed using the official Veeam installer.

Main installation choices:

| Setting | Value |
|---|---|
| Edition | Community / Trial |
| Service account | Local System |
| Database engine | PostgreSQL |
| Installation path | Default |
| Ports | Default |

## Proxmox Integration

The Proxmox host was added to Veeam as a virtualization platform.

| Setting | Value |
|---|---|
| Proxmox host | 192.168.0.156 |
| Authentication | Dedicated Proxmox user |
| User | veeam@pam |
| Privilege elevation | sudo |
| API port | TCP 8006 |
| SSH port | TCP 22 |

## Worker Deployment

A dedicated Proxmox worker VM was deployed by Veeam.

Final worker configuration:

| Setting | Value |
|---|---|
| Worker name | veeam-worker01 |
| VMID | 107 |
| IP address | 10.10.20.71 |
| Bridge | vmbr20 |
| Storage | fast-nvme |
| Max concurrent tasks | 4 |

The worker was validated successfully after firewall rules were adjusted.
