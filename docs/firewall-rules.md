# Firewall Rules

## Overview

This document summarizes the OPNsense firewall rules required for the Veeam Backup & Replication Proxmox lab.

The lab uses network segmentation, so Veeam traffic between the SERVERS network and the Proxmox management network must be explicitly allowed.

## Systems

| System | IP Address | Role |
|---|---:|---|
| Veeam Backup Server | 10.10.20.70 | Veeam B&R server |
| Veeam Worker | 10.10.20.71 | Proxmox backup worker |
| Proxmox Host | 192.168.0.156 | Virtualization host |
| Active Directory / DNS | 10.10.20.10 | Internal DNS |

## Final Rules

| Source | Destination | Port / Protocol | Purpose |
|---|---|---|---|
| 10.10.20.70 | 192.168.0.156 | TCP 8006 | Veeam server to Proxmox API |
| 10.10.20.70 | 192.168.0.156 | TCP 22 | Veeam server to Proxmox SSH |
| 10.10.20.70 | Any | TCP 80/443 | Veeam server web access |
| 10.10.20.71 | Any | TCP 80/443 | Veeam worker web updates |
| 10.10.20.71 | 10.10.20.10 | TCP/UDP 53 | Veeam worker DNS |
| 10.10.20.71 | 192.168.0.156 | TCP 8006 | Veeam worker to Proxmox API |
| 10.10.20.71 | 192.168.0.156 | TCP 22 | Veeam worker to Proxmox SSH |

## Notes

During troubleshooting, temporary broad rules were used to identify blocked traffic.

After the worker test succeeded, these temporary rules were replaced with more restrictive rules.

The final worker test was successful after firewall hardening, which confirmed that the restricted rules were sufficient.
