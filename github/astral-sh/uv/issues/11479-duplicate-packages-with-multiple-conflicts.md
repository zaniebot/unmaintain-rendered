---
number: 11479
title: Duplicate packages with multiple conflicts
type: issue
state: closed
author: ElliottKasoar
labels:
  - bug
assignees: []
created_at: 2025-02-13T14:44:39Z
updated_at: 2025-02-18T12:45:26Z
url: https://github.com/astral-sh/uv/issues/11479
synced_at: 2026-01-07T13:12:18-06:00
---

# Duplicate packages with multiple conflicts

---

_Issue opened by @ElliottKasoar on 2025-02-13 14:44_

### Summary

(Essentially a continuation of #11133, but with an additional conflicting pair)

For a pyproject.toml:

```
[project]
name = "test"
version = "0.0.1"
requires-python = ">=3.10"
dependencies = [
    "mace-torch==0.3.9",
]

[project.optional-dependencies]
chgnet = [
    "chgnet == 0.4.0",
]
sevennet = [
    "sevenn == 0.10.3",
]
all = [
    "chgnet == 0.4.0",
    "sevenn == 0.10.3",
]

# MLIPs with dgl dependency
alignn = [
    "alignn == 2024.5.27",
    "torch == 2.2",
    "torchdata == 0.7.1",
]
m3gnet = [
    "matgl == 1.1.3",
    "torch == 2.2",
    "torchdata == 0.7.1",
]

[tool.uv]
constraint-dependencies = [
    "dgl==2.1",
    "torch<2.6",
]
conflicts = [
    [
      { extra = "chgnet" },
      { extra = "alignn" },
    ],
    [
      { extra = "chgnet" },
      { extra = "m3gnet" },
    ],
    [
      { extra = "all" },
      { extra = "alignn" },
    ],
    [
      { extra = "all" },
      { extra = "m3gnet" },
    ],
]
```

Running `uv sync -p 3.12` now correctly installs only `torch==2.5.1`.

However, running `uv sync -p 3.12 --extra m3gnet` installs both `torch==2.2.0` and `torch==2.5.1`, in addition to two versions of `sympy`.

Similar to the previous example, it seems that in some cases, `torch==2.5.1` seems to be set unconditionally, e.g. for `e3nn` (a dependency which is always required due to `mace-torch`, but also an optional dependency of `sevenn`):

```
[[package]]
name = "e3nn"
version = "0.4.4"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "opt-einsum-fx" },
    { name = "scipy" },
    { name = "sympy", version = "1.13.1", source = { registry = "https://pypi.org/simple" } },
    { name = "sympy", version = "1.13.3", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-alignn' or extra == 'extra-4-test-m3gnet'" },
    { name = "torch", version = "2.2.0", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-alignn' or extra == 'extra-4-test-m3gnet'" },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" } },
]   
```

### Platform

macOS 15.2 arm64

### Version

uv 0.5.31 (e38ac4900 2025-02-12)

### Python version

Python 3.12.8

---

_Label `bug` added by @ElliottKasoar on 2025-02-13 14:44_

---

_Comment by @BurntSushi on 2025-02-13 15:21_

Thanks for the reproduction! I can confirm this is a bug.

---

_Referenced in [stfc/janus-core#415](../../stfc/janus-core/pulls/415.md) on 2025-02-14 11:28_

---

_Referenced in [astral-sh/uv#11513](../../astral-sh/uv/pulls/11513.md) on 2025-02-14 15:48_

---

_Closed by @BurntSushi on 2025-02-18 12:45_

---

_Closed by @BurntSushi on 2025-02-18 12:45_

---
