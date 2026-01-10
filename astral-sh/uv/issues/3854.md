```yaml
number: 3854
title: "Architecture not supported: arm"
type: issue
state: closed
author: bulletmark
labels:
  - bug
assignees: []
created_at: 2024-05-27T02:33:32Z
updated_at: 2024-05-27T03:47:33Z
url: https://github.com/astral-sh/uv/issues/3854
synced_at: 2026-01-10T05:31:37Z
```

# Architecture not supported: arm

---

_Issue opened by @bulletmark on 2024-05-27 02:33_


Using last uv version 1 works fine on my Raspberry Pi (running Arch):
```
$ uv -V
uv 0.1.7

$ uv venv
Using Python 3.12.3 interpreter at /usr/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```
Using current uv version 2 can't create venv:
```
$ uv -V
uv 0.2.3

$ uv venv
  × Architecture not supported: arm
```
```
$ uname -a
Linux pi2 6.6.31-2-rpi #1 SMP Fri May 24 17:26:41 MDT 2024 armv7l GNU/Linux
```



---

_Comment by @charliermarsh on 2024-05-27 02:37_

By the way, `uv 0.1.7` is very old (at least in uv terms), that's like 40 versions behind.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-27 02:50_

---

_Label `bug` added by @charliermarsh on 2024-05-27 02:50_

---

_Comment by @bulletmark on 2024-05-27 03:20_

Yes, sorry Arch on ARM is poorly supported so uv 0.1.7 was the easiest version 1 available for me for me to easily restore from the standard packages.  I just hand installed uv 0.1.45 and that worked fine so clearly this is a version 2 issue.

---

_Closed by @charliermarsh on 2024-05-27 03:33_

---

_Comment by @charliermarsh on 2024-05-27 03:47_

Yup all good. Should be fixed in the next release...

---
