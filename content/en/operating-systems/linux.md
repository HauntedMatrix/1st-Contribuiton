---
title: Linux
description: Core Linux commands for file, process, and permission management.
tags:
  - shell
  - system
---

## File System

```bash
ls -lah                    # list all files with details and sizes
cd <dir>                   # change directory
pwd                        # print working directory
mkdir -p <dir>             # create nested directories
rm -rf <dir>               # remove a directory recursively
find <dir> -name "*.log"   # find files by name
```

## Permissions

```bash
chmod 755 <file>      # owner rwx, group/others rx
chmod u+x <file>      # add execute permission for the owner
chown <user>:<group> <file>  # change owner and group
```

## Processes

```bash
ps aux                  # list all running processes
top                     # interactive process monitor
kill -9 <pid>           # force kill a process by pid
pgrep -l <name>         # find processes by name
```

## Network

```bash
ping <host>             # test connectivity
curl -I <url>           # fetch response headers
ss -tulpn               # list listening sockets
```

## Package Management

```bash
apt update && apt upgrade -y   # Debian/Ubuntu: update and upgrade
apt install <package>          # install a package
systemctl status <service>     # show service status
```

## References

- [Linux man-pages](https://man7.org/linux/man-pages/)
- [Arch Linux Wiki](https://wiki.archlinux.org)
