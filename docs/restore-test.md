# Restore Test

## Overview

This document describes the full VM restore test performed during the Veeam Backup & Replication Proxmox lab.

The goal was to validate that a VM backup could be restored successfully to Proxmox and that the restored workload was functional.

## Restore Scenario

The Debian/Nginx VM was restored to a new VM instead of overwriting the original VM.

| Original VM | Restored VM |
|---|---|
| debian-dmz-web01 | web01-veeam-restore-test |

This avoided modifying the original VM and allowed a safe restore validation.

## Restore Result

The restore completed successfully.

| Setting | Value |
|---|---|
| Restore type | Entire VM restore |
| Worker used | veeam-worker01 |
| Transport mode | HotAdd |
| Restored VMID | 108 |
| Restored VM name | web01-veeam-restore-test |
| Restore status | Success |

## Validation

After the restore, the VM was visible in Proxmox and was started for validation.

The restored VM received the expected IP address:

- 10.10.30.10

The Nginx web page was reachable from the Windows 11 lab client.

This confirmed that the restore was successful at both VM level and application level.

Important note:

The restored VM has the same IP address as the original VM. The original VM and restored VM should not be powered on at the same time unless the IP configuration is changed.
