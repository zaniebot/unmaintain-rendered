```yaml
number: 2896
title: Running rg with sudo on / causes system hang
type: issue
state: closed
author: mgiv
labels: []
assignees: []
created_at: 2024-09-18T19:38:10Z
updated_at: 2024-09-18T21:13:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2896
synced_at: 2026-01-12T16:13:25Z
```

# Running rg with sudo on / causes system hang

---

_@mgiv_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

Arch Linux

### Describe your bug.

Without using sudo, searching in / will come up with errors such as:
```
rg: /proc/1411/task/1411/net/nf_conntrack_expect: Permission denied (os error 13)
```
However, there seems to be others, such as Input/output error, Invalid argument, etc.

To avoid this I added sudo, which works on things like find. However with rg it completely hangs the system and gets stuck on something in /proc

This is only recoverable by force restarting the computer


### What are the steps to reproduce the behavior?

`sudo rg foobar /` from anywhere in the filesystem

### What is the actual behavior?

[Logs](https://gist.github.com/bluescorpion35/66d5ad0285ad833affe768e341585292)

Note: the actual output is longer, the terminal only showed a certain amount of it. Also I killed rg after a few seconds, in case it crashed the system again

### What is the expected behavior?

Either not shown the errors, or at least not crash the entire system

I think rg should entirely ignore /proc unless an option is specified

---

_Comment by @mgiv on 2024-09-18 19:47_

Error logs from journalctl
```
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: System Fatal error.
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: CPU:2 (19:21:0) MC5_STATUS[-|UE|MiscV|AddrV|PCC|TCC|SyndV|-|-|-]: 0xbea0000001000108
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: Error Addr: 0x00ffffffc1315e4e
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: IPID: 0x000500b000000000, Syndrome: 0x000000004d000000
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: Execution Unit Ext. Error Code: 0
Sep 18 20:41:10 archlinux kernel: [Hardware Error]: cache level: RESV, tx: GEN, mem-tx: GEN
```

---

_Locked by @ghost on 2024-09-18 21:13_

---
