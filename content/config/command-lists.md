---
title: "Command Lists"
weight: 2
description: >
  This page tells you how to get started with Backy.
---

Command lists are for executing commands in sequence and getting notifications from them.

The top-level object key can be anything you want.

| key | description | type | required
| --- | --- | --- | --- |
| `order` | Defines the sequence of commands to execute | `[]string` | yes |
| `notifications` | The notification IDs to use on success and failure | `[]string` | no |
| `name` | Optional name of the list | `string` | no |
| `cron` | Time at which to schedule the list. | `string` | no |

### Order

The order is an array of commands to execute in order. Each command must be defined.

### Notifications

An array of notification IDs to use on success and failure. Must match any of the `notifications` object map keys.

### Name

Name is optional for logging. If name is not defined, name will be the object's map key.

### Cron mode

Backy also has a cron mode, so one can run `backy cron` and start a process that schedules jobs to run at times defined in the configuration file.

Adding `cron: 0 0 1 * * *` to a `cmd-configs` object will schedule the list at 1 in the morning. See [https://crontab.guru/](https://crontab.guru/) for reference.

{{% notice tip %}}
Note: Backy uses the second field of cron, so add anything except * to the beginning of a regular cron expression.
{{% /notice %}}

```yaml
cmd-configs:
  cmds-to-run: # this can be any name you want
    # all commands have to be defined
    order:
      - stop-docker-container
      - backup-docker-container-script
      - shell-cmd
      - hostname
    notifications:
      - matrix
    name: backup-some-server
    cron: "0 0 1 * * *"
  hostname:
    name: hostname
    order:
      - hostname
    notifications:
      - prod-email
```
