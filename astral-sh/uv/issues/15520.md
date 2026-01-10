```yaml
number: 15520
title: "groups are not respected at sources' markers for `uv pip compile`"
type: issue
state: closed
author: ZzEeKkAa
labels:
  - question
assignees: []
created_at: 2025-08-25T18:16:53Z
updated_at: 2025-08-25T18:22:48Z
url: https://github.com/astral-sh/uv/issues/15520
synced_at: 2026-01-10T03:23:54Z
```

# groups are not respected at sources' markers for `uv pip compile`

---

_Issue opened by @ZzEeKkAa on 2025-08-25 18:16_

### Summary

When setting up sources with markers that depends on `dependency-group`, `uv pip compile` does not respect it with `--group` argument. For example for this `pyproject.toml`

```toml
[project]
name = "uv_test"
version = "0.0.1"

[dependency-groups]
torch-cu118 = ["torch"]
torch-cu126 = ["torch"]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", marker = "'torch-cu118' in dependency_groups"},
  { index = "pytorch-cu126", marker = "'torch-cu118' not in dependency_groups"},
]

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

I compile two locks:
```shell
uv pip compile -o pylock.cu118.toml --group torch-cu118 pyproject.toml
uv pip compile -o pylock.cu126.toml --group torch-cu126 pyproject.toml
```

But both environment locks to `pytorch-cu126` because passed `--group torch-cu*` is not considired in the `dependency_groups` marker, so for both cases `'torch-cu118' not in dependency_groups` condition is `True`.

### Platform

n/a

### Version

uv 0.8.13

### Python version

python 3.11.13

---

_Label `bug` added by @ZzEeKkAa on 2025-08-25 18:16_

---

_Comment by @charliermarsh on 2025-08-25 18:19_

I think you're looking for:
```toml
[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", group = "torch-cu118"},
  { index = "pytorch-cu126", group = "torch-cu126"},
]

[tool.uv]
conflicts = [
  [
    { group = "torch-cu118" },
    { group = "torch-cu126" },
  ],
]
```

We do _not_ support `dependency_groups` there. `dependency_groups` is only a valid marker specifically in the context of a PEP 751 lockfile.

---

_Label `bug` removed by @charliermarsh on 2025-08-25 18:19_

---

_Label `question` added by @charliermarsh on 2025-08-25 18:19_

---

_Comment by @ZzEeKkAa on 2025-08-25 18:22_

Thank you @charliermarsh ! That's exactly what I was looking for!

---

_Closed by @ZzEeKkAa on 2025-08-25 18:22_

---
