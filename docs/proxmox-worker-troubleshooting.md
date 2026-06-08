# Proxmox Worker Troubleshooting

## Context

During the Veeam Backup & Replication lab, a Proxmox worker VM was required to process backup and restore operations.

The first worker deployment did not work correctly. Veeam was able to create the worker VM, but the worker test remained stuck and backup jobs were waiting for an available worker.

This document explains the issue, the troubleshooting steps and the final fix.

## Issue

The first Veeam worker was deployed on the Proxmox management bridge `vmbr0`.

It received the following IP address:

- Worker IP: 192.168.0.190
- Bridge: vmbr0

The worker VM was running and had an IP address, but Veeam could not fully validate it.

The backup job remained stuck on:

- Waiting for available workers

## Symptoms

The following symptoms were observed:

- The worker VM was created successfully in Proxmox.
- The worker VM received an IP address.
- The worker appeared in Veeam Backup Proxies.
- Backup jobs did not start processing the VM.
- Veeam remained stuck while waiting for an available worker.
- The worker test could not complete successfully.

## Root Cause

The Veeam backup server was located in the SERVERS network:

- Veeam Backup Server: 10.10.20.70

The first worker was deployed on another network:

- First Worker: 192.168.0.190

Because of this network mismatch, communication between the Veeam backup server, the worker and the Proxmox host was not reliable.

The worker was reachable at basic network level, but it was not usable by Veeam for backup operations.

## Checks Performed

Several checks were performed during troubleshooting:

- Confirmed that the worker VM existed in Proxmox.
- Confirmed that the worker VM was running.
- Checked the worker network configuration.
- Verified the IP address using QEMU Guest Agent.
- Tested connectivity between the Veeam backup server and the worker.
- Tested OPNsense firewall rules.
- Reviewed the Veeam worker test results.

## Final Fix

The worker was removed and redeployed on the SERVERS network.

Final worker configuration:

| Setting | Value |
|---|---|
| Worker name | veeam-worker01 |
| VMID | 107 |
| Bridge | vmbr20 |
| IP address | 10.10.20.71 |
| Storage | fast-nvme |
| Proxmox NIC firewall | Disabled |

After redeploying the worker on vmbr20, Veeam was able to obtain the worker IP address and connect to the worker service.

## Firewall Rules Added

Additional OPNsense rules were required on the SERVERS interface.

The following traffic was allowed:

| Source | Destination | Port | Purpose |
|---|---|---|---|
| 10.10.20.71 | 10.10.20.70 | Required Veeam services | Worker to backup server |
| 10.10.20.71 | 192.168.0.156 | TCP 8006 | Worker to Proxmox API |
| 10.10.20.71 | 192.168.0.156 | TCP 22 | Worker to Proxmox SSH |
| 10.10.20.71 | 10.10.20.10 | TCP/UDP 53 | Worker DNS resolution |
| 10.10.20.71 | Any | TCP 80/443 | Worker web updates |

Temporary broad rules were used during troubleshooting, then replaced with more restrictive rules.

## Successful Result

After the worker was redeployed on vmbr20 and the firewall rules were adjusted, the worker test completed successfully.

Validated results:

- Worker IP address obtained successfully.
- Connection to the worker service succeeded.
- Connection to the worker core service succeeded.
- Connection between the worker and backup server succeeded.
- Connection between the worker and Proxmox cluster succeeded.
- Backup job was able to use the worker.

## Lessons Learned

This issue showed the importance of placing backup infrastructure components on the correct network segment.

For this lab, the Veeam backup server and the Veeam worker needed to communicate reliably inside the SERVERS network.

The troubleshooting also showed that a successful VM deployment does not always mean that the worker is fully usable by Veeam. Worker service connectivity, firewall rules and routing must also be validated.
