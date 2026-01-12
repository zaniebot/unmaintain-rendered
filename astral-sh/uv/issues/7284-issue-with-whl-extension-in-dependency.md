```yaml
number: 7284
title: "Issue with `.whl` extension in dependency."
type: issue
state: closed
author: EdgyEdgemond
labels:
  - bug
assignees: []
created_at: 2024-09-11T08:36:53Z
updated_at: 2024-09-12T08:18:23Z
url: https://github.com/astral-sh/uv/issues/7284
synced_at: 2026-01-12T15:59:12Z
```

# Issue with `.whl` extension in dependency.

---

_@EdgyEdgemond_

Following on from my questions in #7245, trying to pin pytorch for multiple architectures.

[edit] Have retried with 0.4.9 release and get same warnings. [/edit]

Minimum reproducible pyproject.toml
```
[project]
name = "uv-test"
version = "0.0.0"
requires-python = ">=3.10,<3.12"

dependencies = [
    "torch @ https://download.pytorch.org/whl/cpu/torch-2.0.1-cp310-none-macosx_11_0_arm64.whl ; sys_platform == 'darwin' and platform_machine == 'arm64'",
    "torch @ https://download.pytorch.org/whl/cpu/torch-2.0.1-cp310-none-macosx_10_9_x86_64.whl ; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch == 2.0.1+cpu ; sys_platform != 'darwin'",
]

[tool.uv]
extra-index-url = ["https://download.pytorch.org/whl/cpu"]
```

When locking on linux, all goes well
```
>> uv lock
Using Python 3.11.9 interpreter at: /usr/bin/python3.11
Resolved 11 packages in 1.22s
```

When syncing on linux, there is a warning (the root of my question), but installation continues successfully.
```
>> uv sync
Using Python 3.11.9 interpreter at: /usr/bin/python3.11
Creating virtualenv at: .venv
warning: Failed to validate existing lockfile: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
Resolved 11 packages in 754ms
Installed 8 packages in 79ms
 + filelock==3.13.1
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.0.1+cpu
 + typing-extensions==4.9.0
```

However when syncing on a mac.
```
>> uv sync
Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
warning: Failed to validate existing lockfile: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
Resolved 95 packages in 4.13s
error: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
  Caused by: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
```

Is this me failing to use `uv` correctly, or should a `.whl` be a viable source (lock command seems to think so)

```
[[package]]
name = "torch"
version = "2.0.1"
source = { url = "https://download.pytorch.org/whl/cpu/torch-2.0.1-cp310-none-macosx_10_9_x86_64.whl" }
resolution-markers = [
    "platform_machine == 'x86_64' and sys_platform == 'darwin'",
]
dependencies = [
    { name = "filelock", marker = "platform_machine == 'x86_64' and sys_platform == 'darwin'" },
    { name = "jinja2", marker = "platform_machine == 'x86_64' and sys_platform == 'darwin'" },
    { name = "networkx", marker = "platform_machine == 'x86_64' and sys_platform == 'darwin'" },
    { name = "sympy", marker = "platform_machine == 'x86_64' and sys_platform == 'darwin'" },
    { name = "typing-extensions", marker = "platform_machine == 'x86_64' and sys_platform == 'darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.0.1-cp310-none-macosx_10_9_x86_64.whl", hash = "sha256:567f84d657edc5582d716900543e6e62353dbe275e61cdc36eda4929e46df9e7" },
]
```

---

_Comment by @EdgyEdgemond on 2024-09-11 08:44_

For completeness, running lock and sync on a fresh directory on the mac.
```
>> uv lock
Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Resolved 95 packages in 4.10s
>> uv sync
Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
warning: Failed to validate existing lockfile: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
Resolved 95 packages in 4.39s
error: failed to parse file extension; expected one of: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
  Caused by: `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
```

---

_Comment by @charliermarsh on 2024-09-11 13:17_

That should work fine -- it must just be a bug? I'll take a look today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-11 13:18_

---

_Label `bug` added by @charliermarsh on 2024-09-11 13:18_

---

_Comment by @charliermarsh on 2024-09-11 18:36_

I believe you need `uv lock --python 3.10`... Those torch versions _only_ work on Python 3.10 (`torch-2.0.1-cp310-none-macosx_11_0_arm64.whl`), but you're resolving Python 3.11 based on the `requires-python`. `sync --python 3.10` works as expected for me while `uv sync` fails. That error message needs to be improved though.

---

_Closed by @charliermarsh on 2024-09-11 19:10_

---

_Comment by @EdgyEdgemond on 2024-09-11 21:52_

Ah yes, that makes a lot of sense. Thanks for the heads up. Will tighten that up in the morning and confirm it works as expected.

---

_Comment by @EdgyEdgemond on 2024-09-12 08:18_

@charliermarsh can confirm everything works as expected when used correctly.

---
