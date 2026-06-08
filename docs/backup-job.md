# Backup Job

## Overview

This document describes the backup job created during the Veeam Backup & Replication Proxmox lab.

The goal was to validate that Veeam could protect a Proxmox VM using the deployed Proxmox worker and the dedicated ReFS repository.

## Protected VM

| Setting | Value |
|---|---|
| VM name | debian-dmz-web01 |
| VMID | 330 |
| OS | Debian |
| Service | Nginx |
| Network | DMZ |
| IP address | 10.10.30.10 |

## Job Configuration

| Setting | Value |
|---|---|
| Job name | Debian-Web01-Test-Backup |
| Backup type | Virtual machine backup |
| Platform | Proxmox VE |
| Repository | Repo-Veeam-R-ReFS |
| Repository path | R:\VeeamRepository |
| Guest processing | Disabled |
| Schedule | Manual test run |

## Backup Result

The backup job completed successfully.

| Metric | Result |
|---|---:|
| Status | Success |
| Processed | 20 GB |
| Transferred | 951.1 MB |
| Duration | 02:56 |
| Warnings | 0 |
| Errors | 0 |
| Transport mode | HotAdd |

This confirmed that the Proxmox host, Veeam worker and repository were working correctly together.
