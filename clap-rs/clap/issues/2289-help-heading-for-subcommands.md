```yaml
number: 2289
title: "`help_heading` for subcommands"
type: issue
state: closed
author: tailhook
labels:
  - C-enhancement
  - A-help
  - S-duplicate
assignees: []
created_at: 2021-01-06T14:30:14Z
updated_at: 2022-01-11T18:45:07Z
url: https://github.com/clap-rs/clap/issues/2289
synced_at: 2026-01-12T16:14:13Z
```

# `help_heading` for subcommands

---

_@tailhook_

It would be nice to have `help_heading` equivalent for the subcommands. Currently there is `App::help_heading` but it does something different (default `help_heading` for next arguments).

This is needed for the commands having a lot of of subcommands, to show you one example, here is a snippet from [`virsh`](https://linux.die.net/man/1/virsh) (command-line tool for `libvirt`):
```
 Domain Management (help keyword 'domain')
    attach-device                  attach device from an XML file
    attach-disk                    attach disk device
    attach-interface               attach network interface
    [.. snip ..]
 Domain Monitoring (help keyword 'monitor')
    domblkerror                    Show errors on block devices
    domblkinfo                     domain block device size information
    domblklist                     list all domain blocks
    domblkstat                     get device block stats for a domain
    domcontrol                     domain control interface state
    domif-getlink                  get link state of a virtual interface
    [.. snip ..]
 Host and Hypervisor (help keyword 'host')
    allocpages                     Manipulate pages pool size
    capabilities                   capabilities
    cpu-baseline                   compute baseline CPU
    cpu-compare                    compare host CPU with a CPU described by an XML file
    cpu-models                     CPU models
    [.. snip ..]
```


---

_Label `T: bug` added by @tailhook on 2021-01-06 14:30_

---

_Label `T: bug` removed by @pksunkara on 2021-01-07 14:05_

---

_Label `C: help message` added by @pksunkara on 2021-01-07 14:06_

---

_Label `T: new feature` added by @pksunkara on 2021-01-07 14:06_

---

_Added to milestone `3.1` by @pksunkara on 2021-01-07 14:06_

---

_Referenced in [epage/clapng#172](../../epage/clapng/issues/172.md) on 2021-12-06 20:19_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:11_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:11_

---

_Comment by @epage on 2021-12-09 20:18_

Looks like this is a duplicate of #1553 .  If this is by mistake, feel free to comment, letting us know!

---

_Closed by @epage on 2021-12-09 20:18_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:45_

---
