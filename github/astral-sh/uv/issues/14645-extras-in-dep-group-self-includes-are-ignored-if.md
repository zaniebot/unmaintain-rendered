---
number: 14645
title: Extras in dep group self includes are ignored if the extra is in a conflict group
type: issue
state: open
author: Dillonwong12
labels:
  - bug
assignees: []
created_at: 2025-07-16T04:02:21Z
updated_at: 2025-09-11T06:41:28Z
url: https://github.com/astral-sh/uv/issues/14645
synced_at: 2026-01-07T13:12:18-06:00
---

# Extras in dep group self includes are ignored if the extra is in a conflict group

---

_Issue opened by @Dillonwong12 on 2025-07-16 04:02_

### Summary

We have the following sample pyproject.toml file (it is part of a wider uv workspace). For managing optimal ci/cd run times, we introduced dependency-groups. However, when we run uv sync --package test-core --group ci it installs all the expected dependencies under the extras np & pd, but does not include llama-cpp-python which is defined under --extra cpu.

1. If I run `uv sync --package test-core --group ci --extra cpu` it functions as expected, installing all required dependencies.
2. The command  `uv sync --package test-core --group ci`  should install same set of dependencies as above, without having to define `--extra cpu`, as it is already defined in the dependency group within the pyproject.toml.

### Minimal reproducible example:

## 1. `pyproject.toml`
```
[project]
name = "test-core"
version = "0.0.0"
description = "Dummy workspce member"
requires-python = ">=3.13"

dependencies = [
  "pydantic",
]

[project.optional-dependencies]
np = [
  "numpy",
]
pd = [
  "pandas",
]
cpu = [
  "llama-cpp-python @ https://github.com/ChamalGomesHSO/artifacts/releases/download/v0.0.7-cpu/llama_cpp_python-0.3.9-cp313-cp313-linux_x86_64.whl",
]
gpu = [
  "llama-cpp-python @ https://github.com/ChamalGomesHSO/artifacts/releases/download/v0.0.7-cu126/llama_cpp_python-0.3.9-cp313-cp313-linux_x86_64.whl",
]

[dependency-groups]
ci = [ 
  "test-core[np,pd,cpu]",
]
cd = []

[build-system]
requires = ["uv_build"]
build-backend = "uv_build"

[tool.uv]
package = true
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]
```

## 2. src/test_core/__init__.py dummy source code
```
""" Test module."""
print("test module")
```

### Commands run:

<img width="1181" height="462" alt="Image" src="https://github.com/user-attachments/assets/70ddec19-ace4-4c65-8573-46ca343c7b1d" />

### Platform

Linux 6.8.0-1026-azure x86_64 GNU/Linux

### Version

uv 0.7.20

### Python version

Python 3.13.5

---

_Label `bug` added by @Dillonwong12 on 2025-07-16 04:02_

---

_Comment by @zanieb on 2025-07-16 04:19_

Can you share the logs as text instead of an image please?

Can you also share the `uv.lock` for the reproduction? (as a Gist if needed)

---

_Comment by @Dillonwong12 on 2025-07-16 05:13_

Please find the uv.lock gist [here](https://gist.github.com/Dillonwong12/5dc32696f3d602d8ab5f7ce2d02a5c6f), and the logs below:
```
(test-core) user@ubuntu:~/repos/test_core$ uv sync --package test-core 
Resolved 17 packages in 52ms
Audited 6 packages in 0.12ms
(test-core) user@ubuntu:~/repos/test_core$ uv sync --package test-core --group ci
Resolved 17 packages in 51ms
Installed 6 packages in 29ms
 + numpy==2.3.1
 + pandas==2.3.1
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + six==1.17.0
 + tzdata==2025.2
(test-core) user@ubuntu:~/repos/test_core$ uv sync --package test-core --group ci --extra cpu
Resolved 17 packages in 54ms
Installed 4 packages in 5ms
 + diskcache==5.6.3
 + jinja2==3.1.6
 + llama-cpp-python==0.3.9 (from https://github.com/ChamalGomesHSO/artifacts/releases/download/v0.0.7-cpu/llama_cpp_python-0.3.9-cp313-cp313-linux_x86_64.whl)
 + markupsafe==3.0.2
(test-core) user@ubuntu:~/repos/test_core$ uv sync --package test-core --group ci
Resolved 17 packages in 58ms
Uninstalled 4 packages in 2ms
 - diskcache==5.6.3
 - jinja2==3.1.6
 - llama-cpp-python==0.3.9 (from https://github.com/ChamalGomesHSO/artifacts/releases/download/v0.0.7-cpu/llama_cpp_python-0.3.9-cp313-cp313-linux_x86_64.whl)
 - markupsafe==3.0.2
```

---

_Comment by @charliermarsh on 2025-07-21 01:53_

This does look like a bug. I'm using this to reproduce (slightly simpler):
```toml
[project]
name = "test-core"
version = "0.0.0"
description = "Dummy workspce member"
requires-python = ">=3.13"

[project.optional-dependencies]
np = [
  "numpy",
]
pd = [
  "pandas",
]
cpu = [
  "flask==3.1.1",
]
gpu = [
  "flask==2.3.2",
]

[dependency-groups]
ci = [ 
  "test-core[np,pd,cpu]",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]
```

I'm not sure why it's omitting `cpu` yet, though.

---

_Renamed from "uv --extra with conflict not syncing in --group" to "Extras in dep group self includes are ignored if the extra is in a conflict group" by @konstin on 2025-07-21 10:09_

---

_Comment by @konstin on 2025-07-21 10:15_

The problem is that in project mode, we ignore extras included in a conflict when it is requested through a self-include from a dependencies group.

I trimmed it down a bit:

```toml
[project]
name = "mre"
version = "1.0.0"
requires-python = ">=3.13"

[project.optional-dependencies]
no_conflict = ["sniffio"]
conflict_foo = ["tqdm"]
conflict_bar = []

[dependency-groups]
ci = [
  "mre[no_conflict,conflict_foo]",
]

[tool.uv]
conflicts = [
  [
    { extra = "conflict_foo" },
    { extra = "conflict_bar" },
  ],
]

[build-system]
requires = ["uv_build>=0.8.0,<0.9.0"]
build-backend = "uv_build"
```

From `uv.lock`:

```toml
[[package]]
name = "mre"
version = "1.0.0"
source = { editable = "." }

[package.optional-dependencies]
conflict-foo = [
    { name = "tqdm" },
]
no-conflict = [
    { name = "sniffio" },
]

[package.dev-dependencies]
ci = [
    { name = "mre", extra = ["conflict-foo"], marker = "extra == 'extra-3-mre-conflict-foo'" },
    { name = "mre", extra = ["no-conflict"] },
]

[package.metadata]
requires-dist = [
    { name = "sniffio", marker = "extra == 'no-conflict'" },
    { name = "tqdm", marker = "extra == 'conflict-foo'" },
]
provides-extras = ["no-conflict", "conflict-foo", "conflict-bar"]

[package.metadata.requires-dev]
ci = [{ name = "mre", extras = ["no-conflict", "conflict-foo"] }]
```

This shows the difference between `uv pip install` (correct) and `uv sync` (bug):

```console
$ uv venv -q --clear && uv pip install --upgrade . --group ci
Resolved 3 packages in 14ms
Built mre @ file:///home/konsti/projects/uv/debug
Prepared 3 packages in 3ms
Installed 3 packages in 1ms
+ mre==1.0.0 (from file:///home/konsti/projects/uv/debug)
+ sniffio==1.3.1
+ tqdm==4.67.1
```

```console
$ uv venv -q --clear && uv sync --upgrade --group ci
Resolved 4 packages in 16ms
Built mre @ file:///home/konsti/projects/uv/debug
Prepared 2 packages in 3ms
Installed 2 packages in 0.50ms
+ mre==1.0.0 (from file:///home/konsti/projects/uv/debug)
+ sniffio==1.3.1
```

CC @BurntSushi 

---

_Comment by @charliermarsh on 2025-07-21 13:06_

Do you think this is a problem with the lockfile, or a problem at the install phase?

---

_Comment by @konstin on 2025-07-21 14:27_

I'm not clear on that yet.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-07-28 14:44_

---

_Referenced in [astral-sh/uv#15779](../../astral-sh/uv/issues/15779.md) on 2025-09-10 21:39_

---

_Unassigned @jtfmumm by @konstin on 2025-09-11 06:41_

---
