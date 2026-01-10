```yaml
number: 1493
title: only parse /bin/sh (not /bin/ls)
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-i1486
created_at: 2024-02-16T14:32:34Z
updated_at: 2024-02-16T14:44:48Z
url: https://github.com/astral-sh/uv/pull/1493
synced_at: 2026-01-10T15:33:24Z
```

# only parse /bin/sh (not /bin/ls)

---

_Pull request opened by @BurntSushi on 2024-02-16 14:32_

It turns out that /bin/ls can sometimes be plain text file. For
example, in Rocky Linux 9:

```
$ cat /bin/ls
#!/usr/bin/coreutils --coreutils-prog-shebang=ls
```

However, `/bin/sh` is an ELF binary:

```
$ file /bin/sh
/bin/sh: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=7acbb41bf6f1b7d977f1b44675bf3ed213776835, for GNU/Linux 3.2.0, stripped
```

In a related issue (#1433), @zanieb fixed #1395 where, on NixOS,
`/bin/ls` doesn't exist but `/bin/sh` does. However, the fix attempts
`/bin/ls` first and only tries `/bin/sh` if `/bin/ls` doesn't exist. If
`/bin/ls` exists but isn't a valid ELF file, then the entire enterprise
gives up and `uv` fails to detect the version of `libc` that is
installed.

Instead of tweaking the logic to keep trying `/bin/ls` and then
`/bin/sh` after even if parsing `/bin/ls` fails, we just switch over to
reading `/bin/sh` only. It seems like a more fundamental thing to sniff
and likely less error prone.

We can adjust this heuristic as needed if it provdes to be problematic.

I tested this fix manually on Rocky Linux 9 via Docker:

```
$ cross b -r -p uv --target x86_64-unknown-linux-musl
$ cp target/x86_64-unknown-linux-musl/release/uv ~/astral/issues/uv/i1486/uv
$ docker run --rm -it --mount type=bind,src=/home/andrew/astral/issues/uv/i1486,dst=/host rockylinux:9 bash
[root@df2baa65d2f8 /]# /host/uv venv
Using Python 3.9.18 interpreter at /usr/bin/python3.9
Creating virtualenv at: .venv
[root@df2baa65d2f8 /]#
```

Fixes #1486, Ref #1433


---

_Review requested from @zanieb by @BurntSushi on 2024-02-16 14:35_

---

_@zanieb approved on 2024-02-16 14:36_

Thanks! I wasn't sure about this in the first place.

---

_Label `bug` added by @zanieb on 2024-02-16 14:36_

---

_Merged by @BurntSushi on 2024-02-16 14:44_

---

_Closed by @BurntSushi on 2024-02-16 14:44_

---

_Branch deleted on 2024-02-16 14:44_

---
