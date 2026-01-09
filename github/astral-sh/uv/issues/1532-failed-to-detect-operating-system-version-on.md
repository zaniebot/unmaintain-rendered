---
number: 1532
title: "Failed to detect operating system version on amazonlinux:2023 and rocky 8/9"
type: issue
state: closed
author: altendky
labels:
  - bug
  - linux
assignees: []
created_at: 2024-02-16T19:29:50Z
updated_at: 2024-02-16T19:48:55Z
url: https://github.com/astral-sh/uv/issues/1532
synced_at: 2026-01-07T13:12:16-06:00
---

# Failed to detect operating system version on amazonlinux:2023 and rocky 8/9

---

_Issue opened by @altendky on 2024-02-16 19:29_

https://github.com/Chia-Network/chia-blockchain/actions/runs/7933582391/job/21662665237?pr=17572#step:11:45
```
error: Failed to detect the operating system version: Couldn't parse /bin/ls for detecting the ld version: Invalid magic number: 0x622f7273752f2123
```

We test our installer script across several linux flavors via docker and as I switched to uv I started getting the above error on amazonlinux:2023, rocky8, and rocky9.  The Debian, Arch, Fedora, and Ubuntu images we test do not fail.

This may be related to https://github.com/astral-sh/uv/issues/1427 but I made a new issue so you can decide to merge them or not.

---

_Referenced in [Chia-Network/chia-blockchain#17572](../../Chia-Network/chia-blockchain/pulls/17572.md) on 2024-02-16 19:30_

---

_Label `bug` added by @zanieb on 2024-02-16 19:32_

---

_Label `linux` added by @zanieb on 2024-02-16 19:32_

---

_Referenced in [astral-sh/uv#1534](../../astral-sh/uv/pulls/1534.md) on 2024-02-16 19:43_

---

_Comment by @BurntSushi on 2024-02-16 19:43_

I'm pretty sure this is fixed by https://github.com/astral-sh/uv/pull/1493.

---

_Comment by @BurntSushi on 2024-02-16 19:48_

Yeah I've confirmed this is fixed locally in the `amazonlinux:2023` docker image.

---

_Closed by @BurntSushi on 2024-02-16 19:48_

---
