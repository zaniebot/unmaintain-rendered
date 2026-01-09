---
number: 12782
title: "`fork-strategy` `fewest` does not limit versions when combined with `required-environments`"
type: issue
state: open
author: johnthagen
labels:
  - bug
assignees: []
created_at: 2025-04-09T14:11:39Z
updated_at: 2025-04-10T14:04:42Z
url: https://github.com/astral-sh/uv/issues/12782
synced_at: 2026-01-07T13:12:18-06:00
---

# `fork-strategy` `fewest` does not limit versions when combined with `required-environments`

---

_Issue opened by @johnthagen on 2025-04-09 14:11_

### Summary

This `pyproject.toml`

```toml
[project]
name = "test"
version = "1"
requires-python = ">=3.11, <3.12"
dependencies = [
    "open3d",
]

[tool.uv]
fork-strategy = "fewest"
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'aarch64'",
]
```

Locks `open3d` to

```toml
[[package]]
name = "open3d"
version = "0.18.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_machine == 'aarch64' and sys_platform == 'linux'",
]
dependencies = [
    { name = "addict", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "configargparse", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "dash", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "matplotlib", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "nbformat", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "numpy", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "pandas", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "pillow", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "pyquaternion", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "pyyaml", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "scikit-learn", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "tqdm", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "werkzeug", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/c5/c7/97a2de0176cf9599aa76b3eb5c38943a4bc3e70ada824c8d5d6773108713/open3d-0.18.0-cp311-cp311-manylinux_2_27_aarch64.whl", hash = "sha256:882f1e5039a3c1c5ec05183eb650537fd7431238b7ccb2b742ca5479f02f705b", size = 58431474 },
]

[[package]]
name = "open3d"
version = "0.19.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_machine != 'aarch64' or sys_platform != 'linux'",
]
dependencies = [
    { name = "addict", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "configargparse", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "dash", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "flask", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "matplotlib", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "nbformat", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "numpy", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "pandas", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "pillow", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "pyquaternion", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "pyyaml", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "scikit-learn", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "tqdm", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "werkzeug", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/a7/37/8d1746fcb58c37a9bd868fdca9a36c25b3c277bd764b7146419d11d2a58d/open3d-0.19.0-cp311-cp311-macosx_10_15_universal2.whl", hash = "sha256:117702467bfb1602e9ae0ee5e2c7bcf573ebcd227b36a26f9f08425b52c89929", size = 103098641 },
    { url = "https://files.pythonhosted.org/packages/bc/50/339bae21d0078cc3d3735e8eaf493a353a17dcc95d76bcefaa8edcf723d3/open3d-0.19.0-cp311-cp311-manylinux_2_31_x86_64.whl", hash = "sha256:678017392f6cc64a19d83afeb5329ffe8196893de2432f4c258eaaa819421bb5", size = 447683616 },
    { url = "https://files.pythonhosted.org/packages/a3/3c/358f1cc5b034dc6a785408b7aa7643e503229d890bcbc830cda9fce778b1/open3d-0.19.0-cp311-cp311-win_amd64.whl", hash = "sha256:02091c309708f09da1167d2ea475e05d19f5e81dff025145f3afd9373cbba61f", size = 69151111 },
]
```

I was expecting that with `fork-strategy = "fewest"`, that `open3d` would be limited to the fewest numbers with the required Python (3.11) and required environments (Linux ARM64 included). Open3D 0.18.0 is the only single version that satisfies all the required python versions of platforms, but `uv` still locked both `0.18.0` and `0.19.0`.

What we'd like when using `"fewest"` is that the locked versions would minimize to only 0.18 for all platforms so that every environment is running/testing on the same version and that this version is compatible on all required platforms.

### Platform

macOS 14 ARM64

### Version

uv 0.6.13 (a0f5c7250 2025-04-07)

### Python version

Python 3.11.11

---

_Label `bug` added by @johnthagen on 2025-04-09 14:11_

---

_Comment by @charliermarsh on 2025-04-10 14:04_

Yeah this makes sense. Thanks.

---
