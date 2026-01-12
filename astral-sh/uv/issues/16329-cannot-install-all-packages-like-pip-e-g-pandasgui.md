```yaml
number: 16329
title: Cannot install all packages like pip, e.g., PandasGUI
type: issue
state: closed
author: hoegge
labels:
  - question
assignees: []
created_at: 2025-10-16T09:44:43Z
updated_at: 2025-12-03T03:51:46Z
url: https://github.com/astral-sh/uv/issues/16329
synced_at: 2026-01-12T16:02:29Z
```

# Cannot install all packages like pip, e.g., PandasGUI

---

_@hoegge_

### Question

It seems impossible to install some packages with uv. E.g., `pandasgui` does not seem possible to install with uv but works fine using `pip`. I get this error on Windows (10) -see below. Have I misunderstood something? And if now, how do you handle packages in a project that uv cannot handle?
```
uv add pandasgui
Resolved 100 packages in 5.31s
error: Distribution `pyqt5-qt5==5.15.17 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `pyqt5-qt5` (v5.15.17) only has wheels for the following platforms: `manylinux2014_x86_64`, `macosx_10_13_x86_64`, `macosx_11_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels 
```

### Platform

Windows 10

### Version

uv 0.9.3 (83635a6c4 2025-10-15)

---

_Label `question` added by @hoegge on 2025-10-16 09:44_

---

_Comment by @AlmirNeeto99 on 2025-10-16 18:24_

Having the same problem trying to install torchvision on Linux!

Resolved 31 packages in 1.24s
error: Distribution `torchvision==0.24.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_35_x86_64`), but `torchvision` (v0.24.0) only has wheels for the following platforms: `manylinux_2_28_aarch64`, `macosx_12_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels

---

_Comment by @AlmirNeeto99 on 2025-10-16 18:29_

I was able to solve it by adding the following lines to my pyproject file:

[tool.uv]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
]

Maybe you should try adding:

[tool.uv]
required-environments = [
    "sys_platform == 'win32' and platform_machine == 'amd64'",
]

---

_Comment by @charliermarsh on 2025-12-03 03:51_

The reason this fails is fairly nuanced but there's an extensive write-up here (and the hint points you to this setting): https://docs.astral.sh/uv/concepts/resolution/#required-environments. I suggest adding:

```toml
[tool.uv]
required-environments = [
    "sys_platform == 'win32'"
]
```

---

_Closed by @charliermarsh on 2025-12-03 03:51_

---
