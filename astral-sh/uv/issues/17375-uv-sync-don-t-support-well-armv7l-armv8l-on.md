```yaml
number: 17375
title: "`uv sync` don't support well armv7l / armv8l on aarch64 cpu"
type: issue
state: closed
author: patryk4815
labels:
  - bug
assignees: []
created_at: 2026-01-09T13:06:09Z
updated_at: 2026-01-09T22:16:16Z
url: https://github.com/astral-sh/uv/issues/17375
synced_at: 2026-01-12T16:02:49Z
```

# `uv sync` don't support well armv7l / armv8l on aarch64 cpu

---

_@patryk4815_

### Summary

I am running `uv sync` inside an `ARMv7` Debian Docker container on a Raspberry Pi host (`aarch64`). Even though the `uv.lock` file contains a platform marker that **excludes** `armv7l`, `uv` still tries to install `lief`, which does not provide wheels for this platform.

For example see: https://github.com/pypa/packaging/pull/690 

## Error
```
(venv) root@9f83d1743cea:/pwndbg# uv sync --all-groups --all-extras
Resolved 150 packages in 11ms
error: Distribution `lief==0.17.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_41_armv7l`), but `lief` (v0.17.1) only has wheels for the following platforms: .....
```

## uv.lock snippet:
```
{ name = "lief", marker = "platform_machine == 'aarch64' or platform_machine == 'i686' or platform_machine == 'x86_64' or sys_platform != 'linux'", specifier = ">=0.17.1" },
```
From what I understand, the marker explicitly excludes `armv7l`, but `uv` still resolves and attempts to download lief. It appears this happens because the kernel reports aarch64, while the userspace and binaries (python+uv) are ARMv7.

## Environment:
- docker run --rm --platform linux/arm/v7 -it debian:13
- Host: Raspberry Pi (aarch64)

## Binaries
```
file /venv/bin/uv
# ELF 32-bit LSB pie executable, ARM...

file /usr/bin/python3.13
# ELF 32-bit LSB executable, ARM...

uname -m
# aarch64
```
So Python and uv are 32‑bit ARM, but the kernel reports aarch64.

## Question
How can I force uv to evaluate `platform_machine` as `armv7l` instead of `aarch64` so that the environment markers behave correctly?


Repro:
```bash
docker run --rm --platform linux/arm/v7 -it debian:13

apt install python3 python3-venv python3-pip git -y
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
git checkout 460914cd41febf15cf8cf155c29096af39e96d6e
python3 -m venv .venv
source ./.venv/bin/activate
pip install uv
uv sync --all-groups --all-extras
```

---


Even when is force 32bit mode with `setarch armv7l`
Uv still thinks is it 64bit aarch64?

```
(venv) root@9f83d1743cea:/pwndbg# setarch armv7l /bin/bash
root@9f83d1743cea:/pwndbg# uname -m
armv8l
root@9f83d1743cea:/pwndbg#
root@9f83d1743cea:/pwndbg# python3
Python 3.13.5 (main, Jun 25 2025, 18:55:22) [GCC 14.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import platform
>>> platform.machine()
'armv8l'
>>>
root@9f83d1743cea:/pwndbg# uv sync --all-groups --all-extras
Resolved 150 packages in 11ms
error: Distribution `lief==0.17.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_41_armv7l`), but `lief` (v0.17.1) only has wheels for the following platforms: `manylinux_2_28_i686`, `manylinux_2_28_x86_64`, `manylinux2014_aarch64`, `musllinux_1_2_aarch64`, `musllinux_1_2_i686`, `musllinux_1_2_x86_64`, `macosx_11_0_arm64`, `macosx_11_0_x86_64`, `win32`, `win_amd64`, `win_arm64`; consider adding "sys_platform == 'linux' and platform_machine == 'aarch64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```



### Platform

Debian 13 on raspberypi 4b

### Version

0.9.22

### Python version

Python 3.13.5

---

_Label `bug` added by @patryk4815 on 2026-01-09 13:06_

---

_Comment by @patryk4815 on 2026-01-09 13:24_

`setarch armv7l` + `pip install .` works fine ✅
`setarch armv7l` + `uv pip install .` don't work ❌
`setarch armv7l` + `uv sync` - don't work ❌


```
# uv sync
Resolved 150 packages in 4ms
      Built pwndbg @ file:///pwndbg
      Built intervaltree==3.1.0
      Built markupsafe==3.0.2
  x Failed to download and build `markupsafe==3.0.2`
  `-> The built wheel `markupsafe-3.0.2-cp313-cp313-linux_armv8l.whl` is not compatible with the current Python 3.13 on manylinux
      armv7l
  help: `markupsafe` (v3.0.2) was included because `pwndbg` (v2025.10.20) depends on `pwntools` (v4.14.1) which depends on `mako`
        (v1.3.10) which depends on `markupsafe`
```

---

_Comment by @zanieb on 2026-01-09 17:33_

What does `$(uv python find) -c "import platform; print(platform.machine())"` show?

---

_Comment by @zanieb on 2026-01-09 17:35_

That last error looks like a case from https://github.com/astral-sh/uv/pull/16074 which generally indicates a bug in the build backend? The build backend is the one building a wheel for the wrong platform, not uv. Maybe we should be considering armv8l as compatible with armv7l though?

---

_Comment by @zanieb on 2026-01-09 17:41_

I think pip does not validate that built wheel tags actually match the target environment tags.

---

_Comment by @zanieb on 2026-01-09 17:43_

I think per https://github.com/astral-sh/uv/pull/8881 we should consider these architectures compatible.

---

_Comment by @patryk4815 on 2026-01-09 18:08_

> `$(uv python find) -c "import platform; print(platform.machine())"`

shell spawned with `setarch armv7l /bin/bash`
```
(.venv) root@5ef08c5fb34c:/pwndbg# $(uv python find) -c "import platform; print(platform.machine())"
armv8l
```

---

_Closed by @zanieb on 2026-01-09 18:29_

---

_Comment by @patryk4815 on 2026-01-09 21:59_

@zanieb Thanks, it works fine now with uv==0.9.23! Btw, someone should verify whether on ARMv9 CPUs it is reported as 32-bit (`armv9l`). If yes, this should also be added as an alias?


---

_Comment by @zanieb on 2026-01-09 22:01_

It'd be great to know!

---

_Comment by @patryk4815 on 2026-01-09 22:16_

> It'd be great to know!

<img width="551" height="178" alt="Image" src="https://github.com/user-attachments/assets/a0022072-a347-4ff0-b3fb-fe27d5dcee17" />

<img width="430" height="132" alt="Image" src="https://github.com/user-attachments/assets/381200d2-3cc6-4785-89bb-f0d4768ae875" />

Ok soo, I check linux kernel source, on any 64bit ARM cpu it will be `armv8l` (for little-endian) and `armv8b` (for big-endian)

---
