```yaml
number: 12562
title: "`uv tree` doesn't display dependencies with conflict markers"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-03-30T18:57:19Z
updated_at: 2025-03-30T18:57:22Z
url: https://github.com/astral-sh/uv/issues/12562
synced_at: 2026-01-12T16:01:06Z
```

# `uv tree` doesn't display dependencies with conflict markers

---

_@charliermarsh_

Given:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = ["torch"]

[project.optional-dependencies]
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
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
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://astral-sh.github.io/pytorch-mirror/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://astral-sh.github.io/pytorch-mirror/whl/cu124"
explicit = true
```

The output of `uv tree` on macOS is:

```
project v0.1.0
├── torch v2.6.0
│   ├── filelock v3.18.0
│   ├── fsspec v2025.3.1
│   ├── jinja2 v3.1.6
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── setuptools v78.1.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.13.0
├── torch v2.6.0+cu124 (extra: cu124)
└── torchvision v0.21.0+cu124 (extra: cu124)
    ├── numpy v2.2.4
    ├── pillow v11.1.0
    └── torch v2.6.0+cu124
```

Notice that `torch v2.6.0+cu124` doesn't show any dependencies -- but it should? I mean, it should never be installed on macOS, but I think the dependencies are incorrectly excluded (like `{ name = "filelock", marker = "extra == 'extra-7-project-cu124'" }`).

In general, I don't think `uv tree` really supports conflict markers. It may not even be possible to do it properly with this interface? Since dependencies vary based on the installed extras, and so aren't consistent for a given package across different subtrees.


---

_Label `bug` added by @charliermarsh on 2025-03-30 18:57_

---
