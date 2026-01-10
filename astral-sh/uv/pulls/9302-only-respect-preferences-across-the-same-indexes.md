```yaml
number: 9302
title: Only respect preferences across the same indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pref
created_at: 2024-11-21T01:49:13Z
updated_at: 2024-12-08T18:13:35Z
url: https://github.com/astral-sh/uv/pull/9302
synced_at: 2026-01-10T12:00:00Z
```

# Only respect preferences across the same indexes

---

_Pull request opened by @charliermarsh on 2024-11-21 01:49_

## Summary

The issue here is fairly complex. Consider the following:

```toml
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
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

When solving this project, we first pick a PyTorch version from PyPI, to solve the `cu124` extra, selecting `2.5.1`.

Later, we try to solve the `cpu` extra. In solving that extra, we look at the PyTorch CPU index. Ideally, we'd select `2.5.1+cpu`... But `2.5.1` is already a preference. So we choose that.

Now, we only respect preferences for explicit indexes if they came from the same index.

Closes https://github.com/astral-sh/uv/issues/9295.


---

_Label `bug` added by @charliermarsh on 2024-11-21 01:49_

---

_Comment by @charliermarsh on 2024-11-21 01:49_

I don't really know how to test this.

---

_Merged by @charliermarsh on 2024-11-21 03:26_

---

_Closed by @charliermarsh on 2024-11-21 03:26_

---

_Branch deleted on 2024-11-21 03:26_

---
