---
title: "Config File Definitions"
description: >
  This page tells you how to configure Backy.
---

### Commands

The commands section is for defining commands. These can be run with or without a shell and on a host or locally.

See the [commands documentation](/config/commands) for further information.

```yaml
commands:
  stop-docker-container:
    cmd: docker
    args:
      - compose
      - -f /some/path/to/docker-compose.yaml
      - down
    # if host is not defined, cmd will be run locally
    host: some-host 
  backup-docker-container-script:
    cmd: /path/to/script
    # The host has to be defined in the config file
    host: some-host
    environment:
      - FOO=BAR
      - APP=$VAR # defined in .env file in config directory
  shell-cmd:
    cmd: rsync
    shell: bash
    args:
      - -av
      - some-host:/path/to/data 
      - ~/Docker/Backups/docker-data
  hostname:
    cmd: hostname
```

### Lists

To execute groups of commands in sequence, use a list configuration.

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
  hostname:
    name: hostname
    order:
      - hostname
    notifications:
      - prod-email
```

### Hosts

The hosts object may or may not be defined.

{{% notice info %}}
If any `host` from a commands object does not match any `host` object, the needed values will be checked in the default SSH config files.
{{% /notice %}}

```yaml
hosts:
  # any ssh_config(5) keys/values not listed here will be looked up in the config file or the default config file
  some-host:
    hostname: some-hostname
    config: ~/.ssh/config
    user: user
    privatekeypath: /path/to/private/key
    port: 22
    # can also be env:VAR or the password itself
    password: file:/path/to/file
    # can also be env:VAR or the password itself
    privatekeypassword: file:/path/to/file
    # only one is supported for now
    proxyjump: some-proxy-host
```

### Notifications

The notifications object can have two forms.

For more, [see the notification object documentation](/config/notifications). The top-level map key is id that has to be referenced by the `cmd-configs` key `notifications`.

```yaml
notifications:
  prod-email:
    type: mail
    host: yourhost.tld
    port: 587
    senderAddress: email@domain.tld
    recipients:
      - admin@domain.tld
    username: smtp-username@domain.tld
    password: your-password-here
  matrix:
    type: matrix
    home-server: your-home-server.tld
    room-id: room-id
    access-token: your-access-token
    user-id: your-user-id
```

### Logging

cmd-std-out controls whether commands output is echoed to StdOut.

If logfile is not defined, the log file will be written to the config directory in the file `backy.log`.

`console-disabled` controls whether the logging messages are echoed to StdOut. Default is false.

`verbose` basically does nothing as all necessary info is already output.

```yaml
logging:
  verbose: false
  file: path/to/log/file.log
  console-disabled: false
  cmd-std-out: false
```
