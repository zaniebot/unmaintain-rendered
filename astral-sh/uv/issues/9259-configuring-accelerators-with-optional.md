```yaml
number: 9259
title: Configuring accelerators with optional dependencies does not work with build system
type: issue
state: closed
author: eginhard
labels:
  - bug
assignees: []
created_at: 2024-11-20T01:18:08Z
updated_at: 2024-11-20T14:05:51Z
url: https://github.com/astral-sh/uv/issues/9259
synced_at: 2026-01-10T04:36:20Z
```

# Configuring accelerators with optional dependencies does not work with build system

---

_Issue opened by @eginhard on 2024-11-20 01:18_

Thank you for the improvements for installing Pytorch! The example at https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies works great. But it fails when I add a build system:

```shell
uv run --extra cpu python
  × Failed to build `project @ file:///home/.../project`
  ╰─▶ Source entry for `torch` only applies to extra `cpu`, but the `cpu` extra does not
      exist. When an extra is present on a source (e.g., `extra = "cpu"`), the relevant
      package must be included in the `project.optional-dependencies` section for that extra
      (e.g., `project.optional-dependencies = { "cpu" = ["torch"] }`).
```

Running on Ubuntu with uv 0.5.3. This is the `pyproject.toml`, with only the first 3 lines added to the example.

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-11-20 01:19_

Oh thanks, let me take a look…

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 01:19_

---

_Label `bug` added by @charliermarsh on 2024-11-20 01:19_

---

_Comment by @charliermarsh on 2024-11-20 04:21_

Thanks, it was an oversight on my part.

---

_Closed by @charliermarsh on 2024-11-20 14:05_

---
