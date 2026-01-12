```yaml
number: 8358
title: Fail to install pytorch (cpu) on MacOS intel chip
type: issue
state: closed
author: fzyzcjy
labels: []
assignees: []
created_at: 2024-10-19T12:23:09Z
updated_at: 2024-10-20T15:07:54Z
url: https://github.com/astral-sh/uv/issues/8358
synced_at: 2026-01-12T15:59:24Z
```

# Fail to install pytorch (cpu) on MacOS intel chip

---

_@fzyzcjy_

Hi thanks for the tool! However I cannot do it and here are my attempts:

### version

```
uv --version
uv 0.4.24 (b9cd54913 2024-10-17)
```

### setup

```
(base) ➜  temp uv init hello_uv
Initialized project `hello-uv` at `/Users/tom/temp/hello_uv`
(base) ➜  temp cd hello_uv 
(base) ➜  hello_uv git:(main) ✗ ls
README.md      hello.py       pyproject.toml
```

### attempt 1

```
(base) ➜  hello_uv git:(main) ✗ uv add torch
Using CPython 3.9.12 interpreter at: /Users/tom/opt/anaconda3/bin/python
Creating virtual environment at: .venv
Resolved 24 packages in 44.33s
error: Distribution `torch==2.5.0 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

### attempt 2

```
(base) ➜  hello_uv git:(main) ✗ uv add torch --extra-index-url=https://download.pytorch.org/whl/cpu 
warning: Indexes specified via `--extra-index-url` will not be persisted to the `pyproject.toml` file; use `--index` instead.
Resolved 23 packages in 11.67s
error: Distribution `torch==2.5.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

### attempt 3

```
(base) ➜  hello_uv git:(main) ✗ uv add torch --index-url=https://download.pytorch.org/whl/cpu      
warning: Indexes specified via `--index-url` will not be persisted to the `pyproject.toml` file; use `--default-index` instead.
Resolved 11 packages in 9.48s
error: Distribution `torch==2.5.0+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

### attempt 4

```
[project]
name = "project"
version = "0.1.0"
dependencies = [
  "torch==2.1.2 ; platform_system != 'Windows'",
]
```

```
(base) ➜  hello_uv git:(main) ✗ uv sync            
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.11`.
  × No solution found when resolving dependencies for split (platform_system == 'Windows'):
  ╰─▶ Because there is no version of torch{platform_system == 'Windows'}==2.1.2+cu121 and your project depends on
      torch{platform_system == 'Windows'}==2.1.2+cu121, we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @FishAlchemist on 2024-10-20 10:13_

Try: ``uv add torch<2.3.0``
Starting from PyTorch 2.3.0, the minimum requirement for macOS is ``macOS 11.0+ ARM64``.
Therefore, you are restricted to installing PyTorch versions prior to 2.3.0.
https://pypi.org/project/torch/2.3.0/#files

---

_Comment by @fzyzcjy on 2024-10-20 13:29_

Oh I see, thank you so much!

---

_Closed by @fzyzcjy on 2024-10-20 13:30_

---

_Comment by @FishAlchemist on 2024-10-20 15:07_

@charliermarsh Is it normal to install for a PyTorch 2.5.0 wheel that wasn't built for Intel (x86-64) macOS?

The documentation states that macOS (x86_64) is supported, so UV should run correctly on that device. Otherwise, the documentation should be downgraded it.
https://github.com/astral-sh/uv/blob/7d883fc12f23ff271797e3769ae942c26a91b359/docs/reference/platforms.md?plain=1#L1-L8

---
