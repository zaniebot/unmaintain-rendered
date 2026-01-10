---
number: 16841
title: "`uv` tried to use wheels which don't exist"
type: issue
state: open
author: harshil21
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-11-25T03:37:16Z
updated_at: 2025-11-26T16:35:51Z
url: https://github.com/astral-sh/uv/issues/16841
synced_at: 2026-01-10T01:26:10Z
---

# `uv` tried to use wheels which don't exist

---

_Issue opened by @harshil21 on 2025-11-25 03:37_

### Summary

Hello,

I just had a really weird bug where `uv` failed to install a package because of a wheel whose metadata didn't have a name:

```bash
$ uv add --dev lgpio
Resolved 1 package in 1ms
error: Failed to install: lgpio-0.2.2.0-cp313-cp313-linux_aarch64.whl (lgpio==0.2.2.0)
  Caused by: The wheel is invalid: Metadata field Name not found
```

This was weird because I don't see any wheels for 3.13 published on PyPI: https://pypi.org/project/lgpio/#files

Note that I had successfully installed this many times before with uv, on the same python version and platform. I tried multiple different commands like: `uv add --dev lgpio --no-binary`, and `uv add --dev lgpio --reinstall`. Neither of them fixed it.

Finally, I did a `uv cache clean`, and then the package installed just fine. It sucks that I cannot reproduce this bug though.


Here's my current `pyproject.toml` if that helps. Note that I had a setuptools backend, and a hatchling backend, and they both had also failed.

```
[build-system]
requires = ["uv_build>=0.9.11,<0.10.0"]
build-backend = "uv_build"

[project]
name = "lewanlib"
version = "0.1.0"
description = "Library for controlling bus servos using the LewanSoul Bus Servo Communication Protocol"
readme = "README.md"
requires-python = ">=3.10"
authors = [
    {name = "NCSU-High-Powered-Rocketry-Club"}
]
license = {text = "MIT"}

dependencies = [
    "pyserial>=3.5",
]

[project.optional-dependencies]
dev = [
    "pytest>=9.0",
]

[dependency-groups]
dev = [
    "gpiod>=2.4.0",
    "gpiozero>=2.0.1",
    "lgpio>=0.2.2.0",
]
```

### Platform

Linux 6.12.47+rpt-rpi-2712 aarch64 GNU/Linux

### Version

0.9.11

### Python version

3.13.5 (system python)

---

_Label `bug` added by @harshil21 on 2025-11-25 03:37_

---

_Comment by @konstin on 2025-11-26 16:35_

Do you happen to still have the lockfile of the version with `cp313` or verbose log files? This would help a lot with tracing the source of this.

---

_Label `needs-mre` added by @konstin on 2025-11-26 16:35_

---
