```yaml
number: 1486
title: Failed to detect the operating system version on Rocky Linux 9.3
type: issue
state: closed
author: AtomBaf
labels:
  - bug
assignees: []
created_at: 2024-02-16T12:43:42Z
updated_at: 2024-02-16T14:44:48Z
url: https://github.com/astral-sh/uv/issues/1486
synced_at: 2026-01-12T15:58:28Z
```

# Failed to detect the operating system version on Rocky Linux 9.3

---

_@AtomBaf_

Hi there,

Doing an `uv venv` or any of the command will lead to this error:
```
× Failed to detect the operating system version: Couldn't parse /bin/ls for detecting the ld version: Invalid magic number: 0x622f7273752f2123
```

Context:
 - Rocky Linux 9.3
 - ldd (GNU libc) 2.34



---

_Label `bug` added by @BurntSushi on 2024-02-16 13:54_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 13:54_

---

_Comment by @BurntSushi on 2024-02-16 14:03_

I can reproduce this via the `rockylinux:9` docker image:

```
$ docker run --rm -it --mount type=bind,src=/home/andrew/astral/issues/uv/i1486,dst=/host rockylinux:9 bash
[root@b6f41d07c55e /]# yum update --assumeyes --quiet
[..snip..]
[root@b6f41d07c55e /]# yum install which --assumeyes --quiet
[..snip..]
[root@b6f41d07c55e /]# curl -LsSf https://astral.sh/uv/install.sh | sh
[..snip..]
[root@b6f41d07c55e /]# . ~/.cargo/env
[root@b6f41d07c55e /]# uv venv
  × Failed to detect the operating system version: Couldn't parse /bin/ls for detecting the ld version: Invalid magic number: 0x622f7273752f2123
```

---

_Comment by @BurntSushi on 2024-02-16 14:08_

Ah the problem is that `ls` is not actually a binary:

```
$ cat ~/astral/issues/uv/i1486/ls
#!/usr/bin/coreutils --coreutils-prog-shebang=ls
```

We assume it is and parse it as an ELF file. This is related to the concern @zanieb brought up in #1433 where everything will fail if parsing `ls` fails.

It looks like `sh` is a true binary though. And I think it kind of has to be?

```
$ file ~/astral/issues/uv/i1486/sh
/home/andrew/astral/issues/uv/i1486/sh: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=7acbb41bf6f1b7d977f1b44675bf3ed213776835, for GNU/Linux 3.2.0, stripped
```

I'll put up a PR that reverts #1433 and just checks the `/bin/sh` executable instead of attempting `/bin/ls`.

---

_Closed by @BurntSushi on 2024-02-16 14:44_

---
