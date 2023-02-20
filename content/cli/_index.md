---
title: "CLI"
weight: 4
---

This page lists documentation for the CLI.

## Subcommands

### backy backup

```
Backup executes commands defined in config file.
Use the --lists flag to execute the specified commands. If not specified, all lists will be executed.

Usage:
  backy backup [--lists=list1,list2] [flags]

Flags:
  -h, --help            help for backup
  -l, --lists strings   Accepts comma-separated names of command lists to execute.

Global Flags:
  -f, --config string   config file to read from
  -v, --verbose         Sets verbose level
```

### backy exec

```
Exec executes commands defined in config file.

Usage:
  backy exec command ... [flags]

Flags:
  -h, --help   help for exec

Global Flags:
  -f, --config string   config file to read from
  -v, --verbose         Sets verbose level
```

### backy cron

```
Cron starts a scheduler that executes command lists at the time defined in config file.

Usage:
  backy cron [flags]

Flags:
  -h, --help   help for cron

Global Flags:
  -f, --config string   config file to read from
  -v, --verbose         Sets verbose level
```

### backy version

```
Prints the version and exits.

Usage:
  backy version [flags]

Flags:
  -h, --help   help for version
```