---
number: 9622
title: "Package dependencies not installed on first `uv run --extra` with conflicting extras"
type: issue
state: closed
author: eginhard
labels:
  - bug
assignees: []
created_at: 2024-12-03T22:00:51Z
updated_at: 2024-12-10T15:57:25Z
url: https://github.com/astral-sh/uv/issues/9622
synced_at: 2026-01-07T13:12:18-06:00
---

# Package dependencies not installed on first `uv run --extra` with conflicting extras

---

_Issue opened by @eginhard on 2024-12-03 22:00_

I can confirm that #9533 is fixed with uv 0.5.6, this is a follow-up for packages with dependencies in conflicting extras:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = []

[project.optional-dependencies]
cpu = ["torch>=2.0"]
cu118 = ["torch>=2.0"]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu118" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu118", extra = "cu118" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
```

To reproduce:
1. Ensure lock file and venv are **not** present: 
```bash
rm .venv/ uv.lock -rdf
```
2. On the first run only torch itself is installed:
```bash
$ uv run --extra cpu python -c "import torch; print(torch.__version__)"
Using CPython 3.13.0
Creating virtual environment at: .venv
Installed 1 package in 88ms
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import torch; print(torch.__version__)
    ^^^^^^^^^^^^
  File ".../.venv/lib/python3.13/site-packages/torch/__init__.py", line 37, in <module>
    from typing_extensions import ParamSpec as _ParamSpec, TypeGuard as _TypeGuard
ModuleNotFoundError: No module named 'typing_extensions'
```
3. The dependencies only get installed on the second run:
```bash
$ uv run --extra cpu python -c "import torch; print(torch.__version__)"
Installed 9 packages in 16ms
2.5.1+cpu
```

---

_Referenced in [astral-sh/uv#9640](../../astral-sh/uv/issues/9640.md) on 2024-12-04 14:15_

---

_Label `bug` added by @charliermarsh on 2024-12-06 00:16_

---

_Comment by @charliermarsh on 2024-12-06 00:16_

Should be fixed soon, sorry about that.

---

_Referenced in [astral-sh/uv#9370](../../astral-sh/uv/pulls/9370.md) on 2024-12-06 17:13_

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---

_Referenced in [stfc/janus-core#358](../../stfc/janus-core/pulls/358.md) on 2024-12-10 18:41_

---
